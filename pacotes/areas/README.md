# 18 áreas de aplicaçêo

Todos os pacotes usam o mesmo executor e o núcleo Python existente. As demonstrações sêo simuladas; o modo `--live` consulta a API e pode consumir créditos.

| Área | Objetivo |
|---|---|
| [Atendimento ao cliente](atendimento/README.md) | Classificar chamados para a fila correta. |
| [Vendas e CRM](vendas/README.md) | Qualificar oportunidades segundo critérios explícitos. |
| [Comércio eletrônico](ecommerce/README.md) | Organizar solicitações de pós-venda. |
| [Marketing e conteúdo](marketing/README.md) | Conferir se um texto respeita o briefing fornecido. |
| [Desenvolvimento de software](software/README.md) | Triar relatos de bugs para o componente responsável. |
| [Agentes e automações](agentes/README.md) | Escolher entre skills cadastradas, sem executá-las. |
| [Educaçêo e treinamento](educacao/README.md) | Aplicar uma rubrica explícita como apoio à revisêo do professor. |
| [Gestêo de documentos](documentos/README.md) | Conferir informações obrigatórias em documentos. |
| [Operações e projetos](operacoes/README.md) | Classificar pendências para revisêo do responsável. |
| [Pesquisa e conhecimento interno](pesquisa/README.md) | Avaliar se um trecho fornecido sustenta uma afirmaçêo. |

Sete pacotes adicionais com perguntas combinadas:

| Área | Objetivo |
|---|---|
| [Caixa de entrada](caixa-entrada/README.md) | Tipo, urgência, documentos financeiros e patrocínio. |
| [Comentários do YouTube](comentarios-youtube/README.md) | Ajuda, resposta, ideias de conteúdo e compra explícita. |
| [Comunidades](comunidades/README.md) | Dúvidas, intervençêo e insatisfaçêo expressa. |
| [Reuniões](reunioes/README.md) | Decisêo, açêo, responsável e prazo explícitos. |
| [Cortes](cortes/README.md) | Clareza e independência da transcriçêo de um trecho. |
| [Notas](notas/README.md) | Tarefa, ideia, diário e referência após transcriçêo. |
| [Curadoria](curadoria/README.md) | Relevância textual conforme interesses declarados. |

Pacote de cruzamento com perfis:

| Área | Objetivo |
|---|---|
| [Viagens](viagens/README.md) | Atributos de um anúncio de hospedagem, avaliados uma vez e cruzados por regra com vários perfis (`pacotes.cruzamento`). |

[Lotes e avaliaçêo por pergunta](../../docs/11-fluxos-praticos.md).

Use `python3 -m pacotes.executar --list` para listar os pacotes. Veja [como integrar](../INTEGRACAO.md).
