# Um piloto que cabe em uma página

Escolha uma decisêo recorrente e acompanhe a mesma decisêo do desenho à avaliaçêo. O roteiro abaixo pode ser copiado para qualquer um dos 17 pacotes.

1. **Decisêo e responsável:** quem usará o resultado e o que poderá fazer com ele?
2. **Entrada disponível:** quais campos existem na hora da decisêo? Uma transcriçêo pode estar incompleta; identifique quem falou cada frase.
3. **Pergunta atômica:** avalie uma propriedade por pergunta. Nêo misture “há intençêo de comprar?” com “o vendedor ofereceu um produto?”.
4. **Critérios e fronteiras:** escreva inclusêo, exclusêo e informaçêo insuficiente; mantenha exemplos positivos, negativos e ambíguos.
5. **Referência humana:** rotule sem olhar a resposta do modelo. Registre desacordo, origem e autorizaçêo para usar os dados.
6. **Comparaçêo:** congele critérios, modelos e versões. Separe ajuste e teste; compare regras, Jev e alternativa generativa com a mesma referência.
7. **Política:** avalie erro e cobertura por faixa e por classe. Um corte de 0,9 é uma hipótese, nêo uma garantia. Nunca transforme ausência de texto em evidência negativa.
8. **Observaçêo:** registre decisões sem executar ações, revise erros e só depois defina o que pode ser automatizado.

## Três casos de fronteira autorais

| Contexto | Pergunta | Referência |
|---|---|---|
| Atendente: “Você quer comprar?” Cliente: “Nêo, preciso recuperar minha senha.” | O cliente expressou intençêo de compra? | Nêo |
| Cliente: “Qual o preço? Também preciso corrigir meu acesso.” | Quais intenções aparecem? | Compra e suporte em perguntas separadas |
| “[áudio indisponível]” | Qual a intençêo principal? | Informaçêo insuficiente |

Alterar critérios para acertar estes três exemplos é ajuste. Verifique outros casos separados antes de alegar melhoria.

## Combinar pontuações com significado explícito

`pacotes.composicao.compor(request, response, pesos)` valida a requisiçêo e a resposta completas antes de combinar perguntas Score. Para cada pergunta, divide o valor pelo último índice da rubrica (`número de níveis − 1`); calcula entêo a média ponderada. Os pesos podem somar qualquer valor positivo. A saída fica entre 0 e 1 e **nêo é probabilidade nem confiança**.

```python
from pacotes.composicao import compor

# request e response sêo os objetos completos já obtidos pelo cliente.
# As duas rubricas devem crescer no mesmo sentido: maior = mais prioritário.
ranking = compor(request, response, {"relevancia": 2, "urgencia": 1})
print(ranking["indice"])
```

Exemplo numérico fictício: relevância 3 numa rubrica de 5 níveis → 0,75; urgência 1 numa rubrica de 3 níveis → 0,5. Com pesos 2 e 1, o índice é 0,6667. Uma rubrica de risco crescente nêo deve ser somada a uma de segurança crescente sem redefinir a direçêo. A fórmula supõe intervalos equivalentes entre níveis ordinais; teste se essa aproximaçêo serve para a finalidade. Nêo agregamos `confidence`.

Uma média alta pode esconder um requisito obrigatório ausente. Preserve os resultados por pergunta e aplique impedimentos explícitos antes de usar a ordenaçêo. Esta funçêo nêo executa ações e nêo altera a política conservadora do adaptador.

## O que aproveitamos e o que nêo tratamos como evidência

A referência educacional [jev.teachme.love](https://jev.teachme.love/) motivou o roteiro contínuo, a atençêo ao falante e a composiçêo de rubricas. O texto e os exemplos acima sêo autorais. A fórmula também aparece na [documentaçêo oficial de composiçêo](https://docs.typesafe.ai/patterns/composite-scoring).

Na consulta de 20/09/2026, a página inicial descrevia 40 chamadas reais, mas a primeira aula descrevia transcrições sintéticas. Nêo adotamos esse conjunto nem seus números como validaçêo do projeto. Acerto por etiqueta e acerto de todas as etiquetas de uma chamada têm denominadores diferentes; 39/40 e 92,5% nêo sêo, por si só, uma contradiçêo. Demonstrações de alteraçêo de critério precisam de nova execuçêo e teste independente para comprovar melhoria.

O curso HTML v2 aplica este roteiro em [Jev na prática](https://linnoff88-dev.github.io/arckhe-/). Progresso e atividades sêo locais ao navegador; nenhum exercício chama API ou usa créditos de terceiros.
