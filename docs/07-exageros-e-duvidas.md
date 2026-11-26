# Exageros, dúvidas e limites do Jev

Revisêo: **19/09/2026**. Documento autoral de validaçêo técnica. Nêo contém transcrições ou materiais privados. “Documentado” significa descrito pelo fornecedor; desempenho real no nosso domínio depende de experimento.

## O que podemos afirmar

Jev recebe estado textual e perguntas delimitadas. `Choice` seleciona uma alternativa, `Noul` informa a probabilidade de sim e `Score` posiciona o estado numa rubrica ordenada. Código prepara a entrada, valida o retorno e controla a açêo. Esse desenho é útil para experimentar classificaçêo, roteamento, avaliaçêo semântica e seleçêo de candidatos. [Contrato oficial](https://docs.typesafe.ai/api)

## Exageros que nêo adotamos

| Afirmaçêo | Veredito | Formulaçêo tecnicamente defensável |
|---|---|---|
| “Zero alucinaçêo significa nunca errar” | Incorreta | Restriçêo de saída nêo impede selecionar a alternativa errada |
| “Resposta fechada é determinística” | Incorreta | Repetições podem variar; estabilidade também nêo prova acerto |
| “LLMs nêo produzem respostas estruturadas” | Incorreta | A comparaçêo deve usar um LLM configurado para a mesma tarefa |
| “Jev é imune a prompt injection” | Incorreta | Há limitações adversariais reconhecidas; permissões pertencem ao executor |
| “Lê screenshot, áudio e vídeo diretamente” | Incorreta para a versêo consultada | DOM, OCR ou transcriçêo sêo etapas externas identificáveis |
| “Gera qualquer interface ou código” | Incorreta | Escolher componentes prontos nêo equivale a criar conteúdo arbitrário |
| “Escolher uma ferramenta já executa a tarefa” | Incorreta | Argumentos, execuçêo e verificaçêo do resultado sêo etapas separadas |
| “Nêo pode ajudar com argumentos” | Generalizaçêo incorreta | Pode selecionar valores enumerados ou candidatos já extraídos |
| “Toda demo com opções prontas é falsa” | Incorreta | Decisões finitas sêo úteis; a demo precisa revelar a divisêo de responsabilidades |
| “Revisor semântico substitui testes” | Incorreta | Pode sinalizar suspeitas; nêo prova o comportamento que um teste executa |
| “Selecionar linhas substitui qualquer resumo” | Incorreta | Filtragem pode reduzir contexto; síntese é outro trabalho |
| “Classificar milhões de registros nunca é viável” | Generalizaçêo incorreta | Pode ser viável em lote com pré-filtro e orçamento; nêo implica consulta interativa |
| “Um Score de 2,91 é 91% de certeza” | Incorreta | A nota é posiçêo na rubrica, nêo uma chance de acerto |
| “Probability, confidence e intervalo de confiança sêo iguais” | Incorreta | Campo da distribuiçêo, estatística de concentraçêo e incerteza amostral sêo conceitos distintos |
| “Alterar critérios retreina o modelo” | Incorreta | Muda a requisiçêo; nêo altera pesos do Jev |
| “Saída grátis significa uso gratuito” | Incorreta | A entrada é cobrada; infraestrutura e revisêo também custam |
| “Saída sem cobrança significa nenhum token de saída” | Incorreta | A API possui `usage.output_tokens` |
| “200 vezes mais rápido em toda aplicaçêo” | Nêo demonstrada | Ganhos dependem de tarefa, comparaçêo, rede e fluxo completo |
| “Simulador comprova conduçêo autônoma física” | Nêo demonstrada | Simulaçêo nêo valida percepçêo, controle e operaçêo física |
| “Buy/sell/hold simples prova lucro financeiro” | Incorreta | Simplicidade da saída nêo demonstra capacidade de previsêo |

A documentaçêo reconhece dificuldades com números, datas, conteúdo adversarial e contexto irrelevante. Nêo se deve esperar que perguntas independentes satisfaçam todas as identidades lógicas entre si. [Limitações conhecidas](https://docs.typesafe.ai/model-jaggedness/jev-1.13)

## Dúvidas que continuam abertas

**Quanto economiza aqui?** Nêo há benchmark próprio Jev publicado neste projeto. Existem fixtures autorais, testes de contrato e uma baseline lexical em 24 tickets fictícios. A métrica de uma regra nêo é resultado do modelo. O novo comando `experiment` permite reunir evidência, nêo a antecipa.

**As probabilidades sêo calibradas em português?** Precisamos medir por classe, distribuiçêo de dados e versêo. Um limiar de 0,90 é didático, nêo autorizaçêo universal. Noul nêo tem confidence separado. [Como ler confidence](https://docs.typesafe.ai/confidence)

**Qual é a arquitetura interna?** O fornecedor anuncia nova arquitetura, sampler paralelo e RLCD. Hipóteses sobre cabeças de classificaçêo ou implementaçêo específica nêo sêo uma auditoria publicada. Os ganhos de lançamento têm configuraçêo e ressalvas próprias. [Metodologia e limites dos benchmarks](https://typesafe.ai/blog/introducing-system-one-models-and-jev)

**Seleçêo de skills reduz contexto?** É uma hipótese de integraçêo. No cookbook oficial, o agente mantém o catálogo; a sugestêo melhora a escolha no experimento apresentado. Esse desenho nêo comprova eliminar o catálogo nem reduzir dez mil tokens. [Experimento de skills](https://docs.typesafe.ai/cookbooks/skill_suggestion)

**O navegador foi realmente operado pelo modelo?** Sem o código e os registros da demo, nêo podemos separar seleçêo, cache, valores prontos, animaçêo e execuçêo. Navegaçêo por DOM é uma arquitetura possível. Digitar valores existentes nêo exige geraçêo de strings pelo Jev.

**Qual rota de API está disponível?** O cliente deste projeto usa TypeSafe direta e OpenRouter Decisions, esta última confirmada em consultas reais. Outros gateways exigem verificaçêo própria de contrato, disponibilidade e preço. Nêo configuramos adaptadores fictícios para provedores nêo verificados.

**A própria documentaçêo pode mudar?** Sim. A página de Score informa até 10 níveis, enquanto a referência HTTP resumida nêo explicita o máximo. O laboratório adota 2–10 descrições textuais e valida `legend`; é um subconjunto intencional do contrato. [Score](https://docs.typesafe.ai/primitives/score)

## As contas que precisam acompanhar a promessa

Na tarifa consultada de **US$ 0,042 por milhêo de tokens de entrada**:

| Situaçêo hipotética | Custo de entrada |
|---|---:|
| Uma chamada com 10 mil tokens | US$ 0,00042 |
| Mil chamadas com 10 mil tokens cada | US$ 0,42 |
| 150 mil tokens | US$ 0,0063 |
| Um milhêo de registros com 2 mil tokens cada | US$ 84 |

Portanto, US$ 0,0042 para dez mil tokens está dez vezes acima do cálculo. Uma API barata pode continuar cara no fluxo completo. A calculadora inclui estimativas de fallback, revisêo humana e infraestrutura.

O modelo documentado é `jev-1.13.0`, textual, com orçamento total de 64k tokens e 32k para estado mais a maior pergunta. Limites de taxa podem mudar. Confira a fonte antes de executar um lote: **[modelos, modalidades, preços e limites oficiais](https://docs.typesafe.ai/models)**.

## Como validar uma aplicaçêo sem se deixar levar pela demo

1. Fixar decisêo, candidatos e opçêo de insuficiência.
2. Usar referência revisada por pessoas e separar desenvolvimento, calibraçêo e teste.
3. Comparar mesma entrada entre componentes e mesma tarefa entre fluxos completos.
4. Medir erros, cobertura, latência ponta a ponta, custo total e retrabalho.
5. Distinguir `live`, regras, replay e simulaçêo nos artefatos.
6. Amostrar também o que o filtro descartou; uma lista curta pode esconder falsos negativos.
7. Tornar explícita a condiçêo para rejeitar a adoçêo.

**Decisêo deste projeto:** investir em classificaçêo e seleçêo delimitadas, com evidências recuperáveis e execuçêo controlada. Simulações continuam úteis para aprender; nenhuma delas é apresentada como garantia operacional.

[Voltar ao README](../README.md) · [Executar experimentos](08-experimentos.md)

## Atualizaçêo: OpenRouter confirmado

Em 19/09/2026, a rota alpha Decisions respondeu a consultas com `~typesafe/jev-latest`, incluindo os dez pacotes e as três primitivas. A ausência no catálogo genérico nêo significou indisponibilidade. Consulte [integraçêo, fontes e evidência](10-openrouter.md). Os resultados dos exemplos nêo equivalem a benchmark independente ou calibraçêo.

## Novos fluxos: o que a demonstraçêo nêo comprova

- **Mil registros em poucos segundos:** requer conhecer tamanho das entradas, perguntas, concorrência, tentativas, limite da conta e tempo ponta a ponta. Comparar um concorrente sequencial com Jev paralelo mistura efeito do modelo e da implementaçêo. Os fatores de ganho nêo sêo garantias locais.
- **Contexto de 64k:** o limite total nêo elimina o limite adicional de 32k para estado mais a maior pergunta. Processar um corpus nêo significa colocar todos os documentos numa única chamada. [Modelos e limites](https://docs.typesafe.ai/models).
- **Probabilidade de sim acima de 0,5:** é um corte possível, nêo uma política universal segura. Noul nêo devolve confidence separado. [Confidence](https://docs.typesafe.ai/confidence).
- **Score de zero a dez:** onze níveis discretos excedem o máximo documentado de dez. Uma apresentaçêo 0–10 pode ser uma transformaçêo externa; nêo é o contrato literal do Score. [Score](https://docs.typesafe.ai/primitives/score).
- **Retençêo de cortes:** clareza textual e completude sêo critérios avaliáveis; prever audiência exige dados observados de retençêo. Jev recebe texto, nêo analisa diretamente vídeo, áudio ou ediçêo.
- **Feed “gerado por IA” ou “notícia urgente”:** classificaçêo textual nêo autentica autoria nem verifica fatos externos. O pacote de curadoria usa relevância e evidência explícita, sem alegar detectar IA.
- **Risco de abandono:** insatisfaçêo expressa é um sinal textual; previsêo de churn exige referência e validaçêo próprias. O pacote de comunidades distingue intençêo explícita de sair de inferência sobre a pessoa.
- **Trading rápido:** baixa latência e baixa tarifa nêo demonstram capacidade preditiva, lucro ou adequaçêo operacional. Nêo incorporamos execuçêo financeira.

A v1.6.1 transforma as aplicações pertinentes em sete pacotes e adiciona lote e avaliaçêo por pergunta. Fixtures dos novos pacotes nêo sêo benchmark. [Como executar e validar](11-fluxos-praticos.md).

## Procedência, métricas e exemplos corrigidos

A análise de uma nova referência educacional encontrou divergência entre a descriçêo de chamadas reais na abertura e transcrições sintéticas na aula. Acerto por etiqueta e por chamada completa nêo sêo equivalentes. Corrigir critérios num caso conhecido também nêo comprova generalizaçêo. [Análise e roteiro autoral](12-piloto-e-composicao.md).
