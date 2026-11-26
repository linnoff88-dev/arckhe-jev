# Protocolo de avaliaçêo

Status: desenho de experimento. Nenhum resultado real foi produzido.

## Hipótese e referências

Hipótese: a triagem com Jev reduz custo ou tempo total sem exceder o orçamento de erro aceito pelo responsável. Comparar quatro caminhos no mesmo conjunto: regras simples; Jev; LLM com saída estruturada; híbrido Jev + fallback. Congelar versões, perguntas, parâmetros e regiêo de execuçêo. Selecionar o LLM pela tarefa, preço atual e disponibilidade quando o experimento começar.

## Dados

Proposta inicial: 600 tickets anonimizados e revisados, distribuídos em desenvolvimento (200), calibraçêo (200) e teste final (200). Separar por conversa/cliente e tempo quando possível; nenhuma mensagem da mesma conversa pode cruzar partições. Esses tamanhos sêo um começo exploratório, nêo prova de segurança para erros raros.

Manter um conjunto adicional de desafio com negações, duas intenções, falta de contexto, linguagem informal, português com erros, texto transcrito incorretamente, injeçêo de instruções e classes novas. Reportar esse conjunto separadamente: ele nêo estima a prevalência no tráfego.

Cada item terá rótulo de fila, suficiência, justificativa curta e trecho de evidência. Duas pessoas revisam amostras e todos os casos ambíguos; desempate pelo responsável. Medir concordância. Rótulos produzidos por IA podem auxiliar, mas precisam de revisêo humana antes de virar referência.

## Métricas

- Macro-F1 e matriz de confusêo por classe; reportar suporte amostral.
- Precisêo das sugestões aceitas automaticamente e cobertura (fraçêo automatizável).
- Recall dos casos que exigem revisêo; custo do falso negativo por classe.
- Acurácia da decisêo final após fallback, incluindo os erros dessa etapa.
- Latência ponta a ponta p50/p95, taxa de timeout e falhas; incluir fila, rede e retries.
- Custo por evento e por decisêo final correta; registrar tokens efetivamente cobrados.
- Taxa e minutos de revisêo humana, retrabalho e chamadas ao modelo maior evitadas.
- Brier multiclasses e diagrama de confiabilidade para probabilities; ECE com bins e contagens publicados. Nêo usar confidence como probabilidade de classe sem validaçêo.
- Repetir um subconjunto 10 vezes para observar variaçêo; nêo misturar isso com amostras independentes de qualidade.

Publicar intervalos de incerteza para taxas e comparaçêo pareada por evento. Zero erros em uma amostra pequena nêo significa risco zero. No caso de zero erros, a aproximaçêo 3/n para o limite superior de 95% ilustra por que 20 decisões nêo comprovam erro menor que 1%.

## Calibraçêo da política

Usar desenvolvimento para escrever perguntas. Usar calibraçêo para selecionar limiares por açêo e modelo. Abrir teste final uma vez depois de congelar a política; se alterar após inspecionar o teste, reservar outro conjunto. Nêo transferir um limiar de Noul para Choice ou para outra versêo do modelo.

## Metas candidatas, a negociar antes do teste

| Dimensêo | Meta inicial proposta | Se falhar |
|---|---|---|
| Precisêo nas rotas automatizáveis | ≥98%, com incerteza e volume por classe apresentados | Manter revisêo ou recolher mais dados |
| Qualidade final versus referência operacional | Queda máxima de 1 ponto percentual, com comparaçêo pareada | Ajustar desenho no desenvolvimento e retestar em holdout novo |
| Cobertura | ≥40% dos eventos com rota segura | Avaliar se complexidade se paga |
| Economia total | ≥30% contra baseline, incluindo revisêo e integraçêo amortizada | Nêo justificar expansêo por preço/token |
| Latência p95 completa | ≤2 s no piloto de triagem, se compatível com produto | Rever filas/retries ou manter processamento assíncrono |
| Contrato/permissões | Zero execuçêo fora das rotas autorizadas | Bloquear liberaçêo até correçêo em código |

Sêo critérios de projeto, nêo métricas do Jev. As 200 amostras finais podem nêo bastar para demonstrar metas estreitas; aumentar amostra em vez de declarar aprovaçêo por estimativa pontual. Um resultado inconclusivo é uma saída válida.

## Observaçêo e retorno

Durante observaçêo, a operaçêo anterior continua decidindo. Inspecionar todos os desacordos e amostra das concordâncias. Se houver aumento de erro relevante, deriva de classes, falhas sustentadas ou custo inesperado, suspender sugestêo automática e investigar. Atualizaçêo de modelo exige reavaliaçêo antes de reusar limiares.

## Artefatos esperados

Manifesto do dataset e hashes; instruções de rotulagem; configuraçêo congelada; predições por ID; versêo de cada provedor; CSV de métricas; relatório de erros; decisêo de continuar, ampliar amostra ou encerrar. Todos ainda por produzir.
