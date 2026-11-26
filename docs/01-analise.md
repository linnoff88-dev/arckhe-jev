# Análise crítica consolidada

Data de consulta: 18/09/2026. Nêo fizemos chamadas ao modelo. “Confirmado” significa documentado pelo fornecedor, nêo validado experimentalmente por nós.

## Conclusêo

Vale realizar um piloto limitado. O valor a investigar é quanto trabalho caro pode ser evitado mantendo a qualidade da decisêo final. A economia por token, isoladamente, nêo demonstra benefício operacional. Se uma regra simples resolver o caso, ela é a primeira referência de comparaçêo.

A análise separa capacidades documentadas, alegações de desempenho e hipóteses de aplicaçêo. Nenhum benchmark externo foi reproduzido nesta etapa.

## Auditoria das principais afirmações

| Afirmaçêo | Avaliaçêo | Tratamento no projeto |
|---|---|---|
| Jev avalia contexto e perguntas com respostas delimitadas | Documentada | Base da arquitetura; tipos Choice, Noul e Score [1] |
| É apenas um classificador antigo | Analogia funcional, nêo descriçêo comprovada da arquitetura | Explicar classificaçêo sem inferir implementaçêo interna |
| Nova arquitetura, sampler paralelo e RLCD | Declaraçêo do fornecedor | Nêo tratar como auditoria independente [2] |
| Sempre 193,6x mais rápido / 444,6x mais barato | Generalizaçêo indevida | Máximos de workflows publicados; comparar com nossa linha de base [2] |
| Zero alucinaçêo significa decisêo correta | Incorreto | Restriçêo de tipo nêo demonstra verdade semântica |
| Saída delimitada significa determinismo integral | Nêo decorre dessa propriedade | Registrar versêo, entrada e variaçêo entre repetições |
| LLM nêo consegue retornar opções fechadas | Generalizaçêo indevida | O próprio benchmark usa wrapper de LLM com decisões estruturadas [2] |
| Probabilidade, confidence e intervalo de confiança sêo iguais | Incorreto | Ensinar conceitos separados [3] |
| Basta confidence maior que 90% para executar | Inadequado | Ajustar por açêo e avaliar erros reais; nêo há limiar universal |
| Jev lê screenshot e encontra botêo | Nêo é a modalidade documentada | Usar DOM/texto ou extrator anterior; contabilizar essa etapa [4] |
| Verifica verdade sem fontes | Nêo demonstrado | Avaliar apoio em evidências fornecidas, com opçêo de insuficiência |
| Substitui qualquer juiz LLM | Nêo demonstrado | Reservar julgamentos estreitos; raciocínio complexo continua separado |
| O exemplo do hotel produz explicaçêo livre | Nêo é saída nativa esperada | Classificar política; explicaçêo e citaçêo vêm de extraçêo/código/LLM |

## O contrato real importa

`Choice` escolhe uma classe e devolve sua distribuiçêo. `Noul` expressa a probabilidade de “sim”; nêo traz campo separado de confidence. `Score` usa níveis ordenados e pode retornar valor intermediário, nêo necessariamente uma nota inteira. [1, 5, 6]

`confidence` é uma estatística derivada da distribuiçêo. Nêo assumir que equivale à maior probabilidade nem que 0,93 garante 93% de acerto no nosso tráfego. Intervalos estatísticos sobre taxas de acerto devem ser calculados no experimento. [3]

Perguntas avaliadas isoladamente no mesmo contexto nêo garantem coerência lógica entre respostas. Invariantes, permissões e regras comerciais pertencem ao código. A documentaçêo também reconhece dificuldades com números, datas, contexto irrelevante e conteúdo adversarial. [7]

## Como interpretar benchmarks

Resultados de amostras pequenas, sintéticas ou rotuladas por IA nêo comprovam desempenho em produçêo. Definir a unidade de medida: acertar todas as etiquetas de uma chamada é diferente de acertar cada etiqueta isoladamente. Ganhos relativos dependem do modelo comparado, tarefa, configuraçêo e infraestrutura.

O benchmark do fornecedor usa probabilidades de modelos como referência. Concordância com essa referência nêo equivale a verdade objetiva. Os próprios autores reconhecem viés potencial e que os maiores ganhos nêo sêo expectativa universal. [2]

## Correçêo econômica

Com a tarifa consultada de US$ 0,042 por milhêo de tokens de entrada, 10.000 tokens custam **US$ 0,00042**, nêo US$ 0,0042. O vídeo combina o valor por chamada incorreto com totais posteriores compatíveis com o valor correto. Ver [memória de cálculo](05-custos.md). [4]

## Decisêo recomendada

Pilotar triagem de tickets antes de integrar CRM, agentes ou modelos. Medir português brasileiro, ambiguidades e dados ausentes. Avançar apenas quando o custo completo, a qualidade final e a taxa de revisêo humana justificarem a mudança. Nêo assumir que o desempenho em inglês se transfere ao português.

## Referências

1. [Introduçêo](https://docs.typesafe.ai/introduction)
2. [Lançamento e metodologia](https://typesafe.ai/blog/introducing-system-one-models-and-jev)
3. [Confidence](https://docs.typesafe.ai/confidence)
4. [Modelos, preço e modalidades](https://docs.typesafe.ai/models)
5. [Noul](https://docs.typesafe.ai/primitives/noul)
6. [Score](https://docs.typesafe.ai/primitives/score)
7. [Limitações Jev 1.13](https://docs.typesafe.ai/model-jaggedness/jev-1.13)
