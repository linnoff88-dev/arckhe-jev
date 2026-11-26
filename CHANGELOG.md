# Changelog

## v1.8.1 — 2026-09-24

- Pacote `viagens`: seis perguntas Choice sobre um anúncio fictício de hospedagem (reembolso, chegada de madrugada, piscina, trilha, escada, piscina acessível), com `insuficiente` quando o anúncio remete a outro canal.
- Módulo `pacotes.cruzamento`: cruza por regra as respostas de um item com vários perfis; respostas em revisêo deixam o perfil em `revisar`, nunca em `compativel`. Preço e orçamento vêm do sistema, nêo do modelo.
- Doze perfis fictícios; 18 pacotes no total. 63 testes automatizados; fixture simulada, nenhuma nova inferência real.

## Documentaçêo — 2026-09-21

- Referências Laya INEMA, código original e model card nos READMEs, análise e guia.
- Resumo da avaliaçêo local e do piloto proposto, sem anunciar integraçêo operacional.

## v1.7.1 — 2026-09-20

- Composiçêo validada de rubricas Score com pesos e normalizaçêo; índice sem significado probabilístico.
- Roteiro de piloto, exemplos de fronteira e atençêo à atribuiçêo de falante.
- Referência ao curso HTML v2 e análise da procedência de demonstrações.
- 56 testes automatizados; nenhuma nova chamada de inferência real nesta versêo.

## Documentaçêo do ecossistema — 2026-09-19

- Vincula o acervo Eventos INEMA nos três idiomas e registra o curso irmêo v1.3.0.

## v1.6.1 — 2026-09-19

- Adiciona sete pacotes de fluxos práticos, totalizando 17, com perguntas combinadas e fixtures identificadas.
- Inclui executor JSONL com prévia, limites, concorrência de até quatro workers, lock e retomada por assinatura.
- Adiciona avaliaçêo offline por pergunta Choice/Noul/Score, com referência explícita e cobertura de falhas.
- Atualiza os três READMEs e documenta limites de throughput, retençêo, classificaçêo de conteúdo e evidência.

## Documentaçêo — 2026-09-19

- Adiciona traduções completas do README em inglês e espanhol, com navegaçêo entre os três idiomas. A versêo da aplicaçêo permanece v1.5.1.

## v1.5.1 — 2026-09-19

- Adiciona skill portátil jev-decidir com variantes Codex e Claude Code, script compartilhado e instalaçêo local validada.
- Documenta integraçêo nativa no openpcbotv3 em observaçêo, com gateway e orçamento do bot.
- Registra consultas reais dos scripts instalados, sem alegar benchmark dos agentes.

## v1.4.1 — 2026-09-19

- Integra OpenRouter Decisions API e alias ~typesafe/jev-latest com credencial própria.
- Adiciona seleçêo de provedor na CLI, pacotes, ambiente e servidor local.
- Preserva custo declarado e registra consultas reais dos dez pacotes, sem tratá-las como benchmark independente.

## v1.3.1 — 2026-09-19

- Adiciona pacotes/ com dez áreas de aplicaçêo, requests, fixtures simuladas e instruções.
- Inclui executor offline/API, funçêo avaliar_area para backend e encaminhamento de falhas para revisêo.
- Bloqueia reutilizaçêo de fixture após mudar o request; cobre os dez pacotes com testes.
- Acrescenta skill jev-integrar e guia de integraçêo Python/subprocesso.

## v1.2.1 — 2026-09-19

- Amplia para 20 casos e editor Choice/Noul/Score, múltiplas perguntas e estado JSON.
- Importa requisições, exporta procedência e lê relatórios no navegador.
- Acrescenta custo completo, experimentos regras/Jev/híbrido/replay e comparaçêo.
- Corrige limites e legend de Score; mantém dados recebidos e runs fora do Git.
- Publica documento autoral de exageros e dúvidas, guia de experimentos e fontes oficiais.

## v1.1.1 — 2026-09-18

- Publica a capa também sob guia/capa para a URL resolvida pelo catálogo PRO.

## v1.1.0 — 2026-09-18

- Laboratório de dez casos, editor e calculadora.
- Cliente Python, servidor local, validaçêo e baseline em lote.
- Guia INEMA, capa, testes e publicaçêo Pages por build com lista explícita.

## v1.0.0 — 2026-09-18

- Plano inicial de aplicaçêo, com procedência das evidências, entregáveis e critérios de verificaçêo.
