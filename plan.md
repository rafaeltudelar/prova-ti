\# Plan — Decisões Técnicas

\#\# Stack  
\- Linguagem: Python com Flask  
\- Justificativa: ecossistema maduro, Flask é leve  
  e rápido de configurar para API REST

\#\# Persistência  
\- Armazenamento em memória (dicionário/lista Python)  
\- Justificativa: não requer banco externo, simplifica  
  o container. O enunciado não exige persistência  
  entre reinícios.

\#\# Valores monetários  
\- Todos os valores em centavos inteiros (int), nunca  
  ponto flutuante  
\- Justificativa: ponto flutuante causa erro de  
  arredondamento (0.1 \+ 0.2 \!= 0.3 em Python).  
  Centavos inteiros eliminam essa classe de bug.

\> \[\!WARNING\]  
\> Valor da fração \= 450/4 \= 112,5 — não é inteiro.  
\> Decisão: arredondar para cima (ceil) \= 113 centavos  
\> por fração.

\#\# Relógio  
\- Quando o campo \`entrada\` não é enviado, usar hora  
  do sistema com fuso America/Sao\_Paulo (-03:00)

\#\# Container  
\- Gerar Dockerfile com imagem Python  
\- Expor porta 8002  
\- Incluir README com instruções de execução

\#\# Testes  
\- Gerar testes automatizados cobrindo todos os UCs  
  e casos de borda  
