# Tests — Casos de Borda

## Tolerância (TOLERANCIA_MINUTOS = 15)

| Cenário              | Duração | valor_centavos | Explicação                       |
|----------------------|---------|----------------|----------------------------------|
| Tolerância exata     | 15 min  | 0              | Dentro da tolerância, grátis     |
| 1 min além           | 16 min  | 226            | ceil(16/15)=2 frações × 113     |
| Zero minutos         | 0 min   | 0              | Dentro da tolerância, grátis     |

## Frações (FRACAO_MINUTOS = 15)

| Cenário              | Duração | Frações | valor_centavos | Explicação                  |
|----------------------|---------|---------|----------------|-----------------------------|
| Fração exata         | 30 min  | 2       | 226            | 30/15 = 2 exato, cobra 2   |
| Fração +1 min        | 31 min  | 3       | 339            | ceil(31/15) = 3 frações     |
| Uma fração           | 15 min  | 0       | 0              | Dentro da tolerância        |
| Acima da tolerância  | 40 min  | 3       | 339            | ceil(40/15) = 3 frações     |

## Teto diário (TETO_DIARIO_CENTAVOS = 6000)

| Cenário              | Duração  | Cálculo              | valor_centavos | Explicação         |
|----------------------|----------|----------------------|----------------|--------------------|
| Abaixo do teto       | 120 min  | 8 frações × 113=904  | 904            | Não atinge teto    |
| Atinge o teto        | 810 min  | 54 frações × 113=6102| 6000           | Aplica teto        |
| Muito acima do teto  | 1440 min | 96 frações × 113     | 6000           | Aplica teto        |

## Tempo médio (relatório diário)

| Cenário                    | Durações     | Cálculo       | tempo_medio_minutos |
|----------------------------|-------------|---------------|---------------------|
| Média exata                | 20, 30 min  | (20+30)/2=25  | 25                  |
| Arredonda 0,5 pra cima    | 25, 30 min  | (25+30)/2=27,5| 28                  |
| Dia sem bilhetes encerrados| nenhum      | —             | 0                   |

## Placa duplicada

| Cenário                          | Resultado                          |
|----------------------------------|------------------------------------|
| Abrir 2x mesma placa (1 aberto)  | 409 {"erro": "bilhete_em_aberto"}  |
| Abrir após encerrar              | 201, funciona normalmente          |
| Abrir após cancelar              | 201, funciona normalmente          |

## Placa inválida

| Entrada     | Motivo             | Resultado                       |
|-------------|--------------------|---------------------------------|
| "AB"        | Muito curta        | 422 {"erro": "placa_invalida"}  |
| "ABC1D2!"   | Caractere especial | 422 {"erro": "placa_invalida"}  |
| "abc1d23"   | Minúscula          | 422 {"erro": "placa_invalida"}  |
| ""          | Vazia              | 422 {"erro": "placa_invalida"}  |
| "ABCD1234"  | 8 caracteres       | 422 {"erro": "placa_invalida"}  |

## Relatório dia vazio

| Cenário       | Resultado                                                     |
|---------------|---------------------------------------------------------------|
| Dia sem dados | {"data":"...","total_bilhetes":0,"faturamento_centavos":0,"tempo_medio_minutos":0} |
