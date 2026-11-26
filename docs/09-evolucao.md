# Evoluçêo com simplicidade

## Disponível nesta versêo

- Vinte casos autorais agrupados, com busca por tema ou primitiva.
- Editor Choice/Noul/Score; múltiplas perguntas; estado textual ou JSON.
- Importaçêo de requisiçêo e exportaçêo do resultado com procedência explícita.
- Cálculo do custo de entrada e estimativa do processo completo.
- Validaçêo de Score, incluindo limite de níveis e `legend`.
- Experimentos por arquivo com regras, Jev, híbrido e replay; comparaçêo e leitura do relatório no navegador.
- Exemplos de skills, comentários, diffs, contexto, intenções, datas e logs, sem execuçêo de ações externas.
- Curso enriquecido em Markdown e laboratórios de aprofundamento.

O caminho inicial continua simples: `python3 -m jev_lab serve`. O site público funciona sem chave, instalaçêo ou servidor, com respostas didáticas. O núcleo Python continua usando só a biblioteca padrêo.

## O que depende de evidência futura

| Expansêo | Condiçêo de avanço |
|---|---|
| Triagem real de atendimento | Referência humana em português, resultado independente e observaçêo operacional |
| Integraçêo de skills ao agente | Demonstrar recall e qualidade final; nêo remover instruções obrigatórias |
| Revisêo automática de código | Medir falsos negativos também nos trechos descartados; manter testes e revisêo |
| Filtro de contexto em produçêo | Preservar evidências necessárias e contradições; medir resposta final |
| LLM ao vivo e fallback multicanal | Escolher provedor/contrato e avaliar custo total; hoje comparaçêo externa por replay |
| Navegador operacional | DOM atualizado, executor restrito, assertions e trace; escolher elemento nêo prova sucesso |
| Concorrência de chamadas | Medir demanda e taxa; servidor local atual serializa avaliaçêo por desenho |
| HTML do curso | Escolha do formato v5 ou v2; as aulas atuais estêo publicadas em Markdown |

Nenhuma promessa de retorno financeiro, autonomia física, imunidade a injeçêo ou substituiçêo universal de LLMs faz parte do produto. [Exageros e dúvidas](07-exageros-e-duvidas.md).
