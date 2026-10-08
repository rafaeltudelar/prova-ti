\# Spec — Zona Azul Digital

\#\# UC1 — Abrir bilhete

\*\*Rota:\*\* POST /bilhetes

\*\*Entrada:\*\*  
\- \`placa\` (obrigatório): 7 caracteres alfanuméricos maiúsculos  
\- \`entrada\` (opcional): ISO-8601 com fuso \-03:00. Ausente \= hora atual do servidor

\*\*Resposta de sucesso (201):\*\*  
| Campo   | Tipo    | Exemplo                     |  
|---------|---------|-----------------------------|  
| id      | inteiro | 1                           |  
| placa   | string  | "ABC1D23"                   |  
| entrada | string  | "2026-10-07T14:00:00-03:00" |  
| status  | string  | "aberto"                    |

\*\*Erros:\*\*  
| Situação                    | Status | Body                           |  
|-----------------------------|--------|--------------------------------|  
| Placa ausente ou inválida   | 422    | {"erro": "placa\_invalida"}     |  
| Entrada fora de ISO-8601    | 422    | {"erro": "entrada\_invalida"}   |  
| Placa já com bilhete aberto | 409    | {"erro": "bilhete\_em\_aberto"}  |

\*\*Critério de aceite:\*\*  
POST /bilhetes com body vazio retorna 422 com {"erro": "placa\_invalida"}.  
POST /bilhetes com placa que já tem bilhete aberto retorna 409  
com {"erro": "bilhete\_em\_aberto"}.

\---

\#\# UC2 — Encerrar bilhete

\*\*Rota:\*\* POST /bilhetes/{id}/encerramento

\*\*Entrada:\*\* \`id\` do bilhete na URL

\*\*Resposta de sucesso (200):\*\*  
| Campo          | Tipo    | Exemplo                     |  
|----------------|---------|-----------------------------|  
| id             | inteiro | 1                           |  
| placa          | string  | "ABC1D23"                   |  
| entrada        | string  | "2026-10-07T14:00:00-03:00" |  
| saida          | string  | "2026-10-07T14:40:00-03:00" |  
| minutos        | inteiro | 40                          |  
| valor\_centavos | inteiro | 339                         |

\*\*Regra de cálculo:\*\*  
1\. Frações \= ceil(minutos / 15\)  
2\. Valor fração \= ceil(450 / 4\) \= 113 centavos  
3\. Total \= frações x 113  
4\. Se total \> 6000, aplica teto: valor \= 6000

\> \[\!WARNING\]  
\> Valor da fração (450/4 \= 112,5) é arredondado para cima: 113 centavos.  
\> Teto diário de 6000 centavos nunca pode ser ultrapassado.

\*\*Erros:\*\*  
| Situação              | Status | Body                              |  
|-----------------------|--------|-----------------------------------|  
| Bilhete inexistente   | 404    | {"erro": "bilhete\_nao\_encontrado"}|  
| Bilhete já encerrado  | 409    | {"erro": "bilhete\_ja\_encerrado"}  |

\*\*Critério de aceite:\*\*  
Bilhete com 40 min de permanência (acima da tolerância de 15 min):  
ceil(40/15) \= 3 frações x 113 \= 339 centavos.  
Retorna 200 com valor\_centavos \= 339\.

\---

\#\# UC3 — Listar ativos

\*\*Rota:\*\* GET /bilhetes/ativos

\*\*Entrada:\*\* nenhuma

\*\*Resposta de sucesso (200):\*\* array de bilhetes abertos,  
mais recentes primeiro. Se não houver bilhetes abertos,  
retorna array vazio \[\].

\*\*Erros:\*\* nenhum erro específico.

\*\*Critério de aceite:\*\*  
Após abrir 2 bilhetes, GET /bilhetes/ativos retorna array  
com 2 bilhetes ordenados do mais recente pro mais antigo.

\---

\#\# UC4 — Relatório diário

\*\*Rota:\*\* GET /relatorios/diario?data=AAAA-MM-DD

\*\*Entrada:\*\* query param \`data\` (obrigatório, formato AAAA-MM-DD)

\*\*Resposta de sucesso (200):\*\*  
| Campo                 | Tipo    | Exemplo      |  
|-----------------------|---------|--------------|  
| data                  | string  | "2026-10-05" |  
| total\_bilhetes        | inteiro | 12           |  
| faturamento\_centavos  | inteiro | 8400         |  
| tempo\_medio\_minutos   | inteiro | 47           |

\- total\_bilhetes: conta todos os bilhetes do dia  
\- faturamento\_centavos: soma dos valores cobrados  
  (cancelados não entram)  
\- tempo\_medio\_minutos: média apenas dos encerrados  
  no dia, arredondando 0,5 para cima  
\- Dia sem dados retorna JSON com valores zero:  
  total\_bilhetes=0, faturamento\_centavos=0,  
  tempo\_medio\_minutos=0

\*\*Erros:\*\*  
| Situação             | Status | Body                      |  
|----------------------|--------|---------------------------|  
| Data fora do formato | 422    | {"erro": "data\_invalida"} |

\*\*Critério de aceite:\*\*  
No dia 2026-10-07, com 2 bilhetes encerrados de 339 e 113  
centavos, retorna total\_bilhetes=2, faturamento\_centavos=452,  
tempo\_medio\_minutos=28.

\---

\#\# UC5 — Cancelar bilhete

\*\*Rota:\*\* POST /bilhetes/{id}/cancelamento

\*\*Entrada:\*\* \`id\` do bilhete na URL

\*\*Resposta de sucesso (200):\*\*  
| Campo   | Tipo    | Exemplo                     |  
|---------|---------|-----------------------------|  
| id      | inteiro | 1                           |  
| placa   | string  | "ABC1D23"                   |  
| entrada | string  | "2026-10-07T14:00:00-03:00" |  
| status  | string  | "cancelado"                 |

\> \[\!NOTE\]  
\> Campos \`saida\` e \`valor\_centavos\` NÃO aparecem  
\> na resposta de cancelamento.

\*\*Erros:\*\*  
| Situação           | Status | Body                              |  
|--------------------|--------|-----------------------------------|  
| Bilhete inexistente| 404    | {"erro": "bilhete\_nao\_encontrado"}|  
| Bilhete não aberto | 409    | {"erro": "bilhete\_nao\_aberto"}    |

\*\*Critério de aceite:\*\*  
POST /bilhetes/1/cancelamento de bilhete aberto retorna 200  
com status "cancelado", sem campos saida e valor\_centavos.

\---

\#\# UC6 — Histórico por placa

\*\*Rota:\*\* GET /bilhetes?placa=ABC1D23

\*\*Entrada:\*\* query param \`placa\`

\*\*Resposta de sucesso (200):\*\* array de todos os bilhetes  
da placa (qualquer status: aberto, encerrado, cancelado),  
mais recentes primeiro. Placa que nunca estacionou retorna  
array vazio \[\].

\*\*Erros:\*\*  
| Situação       | Status | Body                       |  
|----------------|--------|----------------------------|  
| Placa inválida | 422    | {"erro": "placa\_invalida"} |

\*\*Critério de aceite:\*\*  
GET /bilhetes?placa=BRA2E19 com placa que tem 2 bilhetes  
retorna array com 2 registros, mais recente primeiro.

\---

\#\# UC7 — Tolerância gratuita (regra de valor)

Primeiros 15 minutos de um bilhete são grátis:  
\- Duração \<= 15 min: valor\_centavos \= 0  
\- Duração \> 15 min (mesmo 16 min): cobra INTEGRAL  
  desde o minuto zero. A tolerância NÃO é descontada.

\---

\#\# UC8 — Uma vaga por placa (regra de negócio)

Uma placa só pode ter UM bilhete aberto por vez.  
\- POST /bilhetes com placa que já tem bilhete aberto  
  retorna 409 {"erro": "bilhete\_em\_aberto"}  
\- Após encerrar ou cancelar, a placa pode abrir  
  novo bilhete.  
