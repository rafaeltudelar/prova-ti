\# Constitution

\#\# Regra 1 — Valores monetários  
Todos os valores monetários são sempre em centavos  
inteiros (int), nunca em ponto flutuante.  
Exemplo: 1250 centavos, nunca 12.50.

\#\# Regra 2 — Data e hora  
Formato ISO-8601 com fuso \-03:00.  
Exemplo: 2026-10-12T08:30:00-03:00

\#\# Regra 3 — Porta do serviço  
O serviço escuta na porta 8002\.

\#\# Regra 4 — Ordem de validação  
Validação de formato (422) sempre vem ANTES de regra  
de negócio (409). Payload malformado nunca dispara  
conflito de estado.

\#\# Regra 5 — Formato de erro  
Todas as respostas de erro seguem a estrutura:  
{"erro": "nome\_do\_erro"}

\#\# Regra 6 — Identificadores  
IDs são únicos por bilhete, inteiros e sequenciais  
(1, 2, 3...). Um mesmo veículo pode ter vários  
bilhetes com IDs diferentes.

\#\# Regra 7 — Formato de placa  
7 caracteres alfanuméricos, sempre maiúsculos.  
Exemplo: ABC1D23

\#\# Regra 8 — Ordenação padrão  
Listagens retornam sempre do mais recente para  
o mais antigo.

\#\# Regra 9 — Container  
O serviço roda em container (Dockerfile).

