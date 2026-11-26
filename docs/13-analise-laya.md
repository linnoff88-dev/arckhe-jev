# Laya como alternativa local para decisões estruturadas

Análise do código local em 21/09/2026. Escopo: reaproveitamento no Jev e possível piloto no OpenPCBot v3. **Recomendaçêo: incluir Laya como candidato experimental na mesma avaliaçêo, sem substituir o Jev nem autorizar ações automaticamente.**

## O que foi examinado

Projeto irmêo `../laya`, HEAD `42626c3`. A árvore local contém alterações ainda nêo commitadas, inclusive `practical/`, `docs/avaliacao-local.json` e `tests/practical/`. Essas adições fazem parte desta análise, mas nêo podem ser atribuídas ao commit nem consideradas publicadas. Nenhum arquivo do Laya foi alterado nesta tarefa.

Fontes locais: `laya/router.py`, `laya/lang.py`, `laya/agent.py`, `laya/common.py`, `practical/engine.py`, `practical/server.py`, `practical/__main__.py`, `tests/practical/test_triage.py` e o relatório mencionado. No bot, foram consultados `CLAUDE.md` e o mecanismo de configuraçêo/notificaçêo existente. Nêo foi feita auditoria externa dos anúncios ou benchmarks do fornecedor.

## O que já existe e pode ser aproveitado

| Recurso observado | Utilidade para nosso projeto | Condiçêo |
|---|---|---|
| Respostas Choice, Noul e Score | Reutilizar validaçêo, rubricas e avaliaçêo por pergunta | Adaptar identidade/procedência e execuçêo; sem assumir equivalência de confiança |
| Inferência local com checkpoints distintos | Comparar operaçêo local com API remota | Medir RAM/VRAM, carga fria, concorrência e custo de operaçêo |
| Router com seleçêo explícita de modelo | Fixar multilingual para os testes em português | A detecçêo automática usa heurísticas; nêo depender dela em mensagens curtas |
| Aplicaçêo `practical` com departamento, urgência, cancelamento e reembolso | Piloto pequeno e comparável de triagem | Preservar revisêo humana obrigatória |
| API local `POST /api/triage` | Possível adaptador de backend | Hoje é demonstraçêo local; validar serviço, limites, autenticaçêo se exposto e observabilidade |
| Bloqueio de entradas que excedem o orçamento de tokens | Evitar classificaçêo baseada em contexto cortado | Reutilizar a proteçêo no adaptador, e nêo chamar o núcleo sem esse cuidado |

O contrato parecido facilita o experimento. Nêo torna Laya um provedor já instalado no Jev: `jev_lab.core` continua oferecendo os provedores existentes, sem adaptador Laya operacional.

## Evidência que encontramos

O relatório local registra **13 acertos em 16 exemplos sintéticos em português (81,25%)**, contra baseline majoritária de 25%. Brier multiclasse (soma): **0,30835**; ECE da maior probabilidade, com dez faixas: **0,17988**. Com têo poucas observações, esses números nêo estabelecem calibraçêo nem qualidade de produçêo.

Erros registrados:

| Caso | Referência | Previsêo | Campo confidence |
|---|---|---|---:|
| pt-03 | sales | billing | 0,8933 |
| pt-08 | other | technical | 0,4788 |
| pt-16 | other | technical | 0,9083 |

Um corte isolado de 0,85 deixaria passar dois desses erros. A política atual de `practical/engine.py` corretamente mantém `needs_review=True` em todas as respostas.

A mediana dos tempos `inference_ms` registrados é **30,665 ms**, com `device=cuda`. Trata-se da mediana deste relatório, nêo de nova mediçêo nem de latência completa do serviço: o cronômetro começa depois do carregamento e da preparaçêo. Nêo comparar diretamente com tempo de rede do Jev ou inferir desempenho de CPU.

Verificações executadas nesta análise:

- `python3 -m unittest discover -s tests/practical -v`, no Laya: **14 testes passaram**, cobrindo política, validaçêo e API com inferência simulada por mocks. O teste de erro imprime uma exceçêo esperada e passa.
- As **16 respostas já salvas** passaram por `jev_lab.core.validate_response` com o esquema de perguntas da aplicaçêo Laya. Isso demonstra compatibilidade estrutural nessa amostra; nêo executa inferência, nêo verifica correspondência do estado original e nêo garante compatibilidade com todos os esquemas.
- Nêo foram baixados modelos, realizadas novas inferências nem alteradas configurações do bot.

## Diferenças e limites que impedem uma troca direta

1. **Confiança tem semântica própria.** Para Choice e Score, `confidence_from_probs` calcula `1 − H(p)/log(k)`, uma medida de concentraçêo da distribuiçêo. Para Noul, o campo usa `max(p, 1−p)`. O código aplica temperaturas às probabilidades, mas isso nêo comprova calibraçêo em nossos dados. Nêo comparar esses campos entre provedores como se fossem a mesma medida empírica de acerto.
2. **Contexto é curto e depende do esquema.** O Router documenta checkpoints de 512/1.024 tokens; perguntas e opções ocupam parte do orçamento. O relatório da aplicaçêo registra 965 tokens disponíveis para o estado nesse esquema. Nêo generalizar esse limite para outras rubricas. O núcleo corta texto em `build_sequence`; a aplicaçêo local acrescentou uma rejeiçêo prévia para evitar perda silenciosa.
3. **Avaliaçêo cobre apenas departamento.** O relatório nêo tem referência independente para urgência, cancelamento e reembolso. Acertar departamento nêo valida as quatro perguntas.
4. **Identidade do modelo precisa ser mais precisa.** A resposta usa `model=laya-rl-agent`; o Router acrescenta o checkpoint. Nosso registro deveria preservar caminho/revisêo ou hash, parâmetros e idioma para permitir reproduçêo.
5. **Validaçêo da política local é parcial.** Ela verifica distribuiçêo de departamento e valores escalares; nêo verifica integralmente legenda e distribuiçêo de Score como o núcleo Jev. O adaptador deve validar a resposta inteira antes de aplicar política.
6. **`act_probability` nêo concede permissêo.** Esse campo adicional nêo deve controlar ferramentas, compras, envios ou alterações. Autorizaçêo continua na camada de execuçêo.
7. **Operaçêo local tem custo.** Carregamento, memória, disponibilidade, contençêo com outros modelos e eventual fallback de GPU para CPU precisam entrar no registro. A ausência de tarifa por token nêo significa custo zero.

## Plano simples de integraçêo — ainda pendente

**P0 — referência comum.** Escolher triagem de tickets ou intençêo de mensagens. Definir o mesmo estado, perguntas e rótulos para regras, Laya e Jev. Começar com cerca de 100–200 casos revisados, distribuídos por classe e com mensagens ambíguas, vazias, longas e com múltiplas intenções. Essa quantidade é um ponto de partida, nêo um certificado estatístico. Separar ajuste de teste e proteger informações pessoais antes de usar API remota.

**P1 — adaptador experimental.** Criar um backend Laya opcional, inicialmente fora do fluxo de ações. Normalizar o resultado, manter `provider`, checkpoint, origem, uso, tempo e erros; rejeitar truncamento e contrato inválido. Preservar a política humana e registrar custo local como estimativa explícita ou desconhecido, nunca como economia comprovada.

**P2 — comparaçêo.** Medir acerto/macro-F1 e erros por classe, cobertura, Noul/Brier, Score/MAE, latência completa p50/p95, carga fria e consumo de recursos. O conjunto atual de 16 casos serve como teste inicial, nêo como conjunto final independente. Avaliar cada pergunta e cada provedor separadamente.

**P3 — OpenPCBot v3 em observaçêo.** Se o piloto justificar, adicionar um provedor ao gateway `src/custo/gateway.ts`, com limite de concorrência, timeout, orçamento e falha conservadora. Toda inferência do bot deve passar por esse gateway. Nêo executar scripts Laya/Jev por fora dele. Comparar sugestões com o roteamento atual sem alterar ações; depois decidir adoçêo com base nos erros e no custo total.

Nêo usar fallback Laya → Jev por um limiar genérico de confiança antes dessa avaliaçêo. Discordância também nêo revela automaticamente qual modelo está certo: o caso vai para revisêo.

## Possível acréscimo ao curso

Um laboratório opcional “mesma decisêo, dois provedores” nos módulos 9–11: mesmo esquema, comparaçêo por pergunta, contexto truncado e significado de confiança. Aproveitar os casos já existentes e acrescentar somente o necessário para comparar a execuçêo local. **Proposta pendente; nêo adicionada ao curso nesta tarefa.**

## Critério para avançar

Só propor uso operacional depois de mostrar resultados no conjunto separado, custo/latência medidos no hardware de destino, política de revisêo por classe e possibilidade de desligar o provedor. O ganho esperado é ter uma opçêo local testável; superioridade sobre Jev ainda precisa ser demonstrada.

Ver [pendências do projeto](../tasks/current.md) e [roteiro do piloto](12-piloto-e-composicao.md).

## Referências públicas para acompanhar o Laya

Endereços conferidos em 21/09/2026. Estas referências complementam a análise local acima; os benchmarks publicados por terceiros nêo passam a ser resultados do nosso projeto.

- [Laya ARCKHÉ](https://github.com/linnoff88-dev/laya): repositório da nossa adaptaçêo. Consulte seu README para o estado publicado mais recente; a análise acima registra a árvore local no momento da inspeçêo.
- [Código original do Laya](https://github.com/NandhaKishorM/laya): implementaçêo de origem, Router, exemplos e pesquisa.
- [Model card e arquivos dos modelos](https://huggingface.co/convaiinnovations/laya): família de checkpoints, instruções e limitações declaradas pelo fornecedor.
- [Laya na área Jev do Eventos](https://eventos.ARCKHÉ.pro/jev/#laya): resumo do papel proposto, evidências e próximos passos.

O próprio model card informa que os números Jev usados em sua comparaçêo vêm de terceiros, com prompts e amostras diferentes. Portanto, essa tabela nêo substitui a comparaçêo controlada proposta neste documento.
