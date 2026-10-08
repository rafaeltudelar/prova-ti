\# Tasks — Decomposição de Tarefas

\#\# Tarefa 1 — Criar estrutura do projeto  
\- Inicializar projeto Python com Flask  
\- Criar requirements.txt com dependências  
\- Criar estrutura de pastas

\#\# Tarefa 2 — Implementar modelo de dados  
\- Definir estrutura do bilhete (id, placa, entrada,  
  saida, status, minutos, valor\_centavos)  
\- Implementar armazenamento em memória  
\- IDs inteiros sequenciais começando em 1

\#\# Tarefa 3 — Implementar endpoints de bilhete  
\- UC1: POST /bilhetes (abrir)  
\- UC2: POST /bilhetes/{id}/encerramento (encerrar)  
\- UC5: POST /bilhetes/{id}/cancelamento (cancelar)  
\- Incluir lógica de cálculo de valor com frações,  
  tolerância e teto

\#\# Tarefa 4 — Implementar endpoints de consulta  
\- UC3: GET /bilhetes/ativos (listar ativos)  
\- UC6: GET /bilhetes?placa=X (histórico por placa)  
\- UC4: GET /relatorios/diario?data=X (relatório)

\#\# Tarefa 5 — Implementar validações e erros  
\- Validação de placa (7 chars alfanuméricos maiúsculos)  
\- Validação de entrada (ISO-8601)  
\- Validação de data (AAAA-MM-DD)  
\- Ordem: validação de formato (422) antes de regra  
  de negócio (409)

\#\# Tarefa 6 — Infraestrutura e documentação  
\- Criar Dockerfile expondo porta 8002  
\- Escrever README com instruções de build e execução  
\- Gerar testes automatizados cobrindo UCs e casos  
  de borda  
