ï»¿# Jev Decision Lab

**Idiomas:** [PortuguêÂªs](README.md) êÂ· [English](README.en.md) êÂ· [EspaêÂ±ol](README.es.md)

![Jev Decision Lab ê¢â¬â mais recursos, uso simples: 20 casos prêÂ¡ticos, experimentos e comparaêÂ§êÂ£o](capa/novos-recursos-v1.2.1.png)

**v1.8.1** êÂ· Um laboratêÂ³rio para formular, testar e comparar decisêÂµes de IA, com uma interface simples e recursos avanêÂ§ados quando vocêÂª precisar.

[Abrir laboratêÂ³rio](https://inematds.github.io/jev/app/) êÂ· [Guia de uso](https://inematds.github.io/jev/guia/) êÂ· [Curso em portuguêÂªs](https://inematds.github.io/jev-curso/)

[Acervo Jev no Eventos INEMA](https://eventos.inema.pro/jev/) ê¢â¬â projeto, curso e pacotes reunidos em uma pêÂ¡gina, com apresentaêÂ§êÂ£o em portuguêÂªs, inglêÂªs e espanhol.

**ReferêÂªncia oficial de modelos, preêÂ§os, modalidades e limites:** https://docs.typesafe.ai/models

**Leia tambêÂ©m: [Exageros, dêÂºvidas e limites do Jev](docs/07-exageros-e-duvidas.md).** O documento separa capacidade documentada, hipêÂ³tese, extrapolaêÂ§êÂ£o e evidêÂªncia ainda necessêÂ¡ria.

## Leve para seu projeto

A pasta **[pacotes/](pacotes/README.md)** reêÂºne **18 pacotes por êÂ¡rea**, uma integraêÂ§êÂ£o Python, a skill `jev-integrar` e receitas de uso. Comece offline com `python3 -m pacotes.executar atendimento`. Para seu backend, veja [como integrar Jev nos sistemas](pacotes/INTEGRACAO.md). Veja em cada pacote o que jêÂ¡ funciona e o que ainda depende de integraêÂ§êÂ£o.

## Novos fluxos prêÂ¡ticos

Agora sêÂ£o **18 pacotes reutilizêÂ¡veis**: os dez originais mais **caixa de entrada, comentêÂ¡rios do YouTube, comunidades, reuniêÂµes, cortes por transcriêÂ§êÂ£o, notas, curadoria de feed e hospedagem êâ perfis de viajantes**. Os novos pacotes combinam Choice, Noul e Score; suas fixtures sêÂ£o fictêÂ­cias, sem benchmark real. O pacote de viagens avalia cada anêÂºncio uma vez e cruza as respostas por regra com doze perfis (`python3 -m pacotes.cruzamento viagens --preco 420`).

```bash
python3 -m pacotes.executar reunioes
python3 -m pacotes.qualidade reunioes
python3 -m pacotes.lote reunioes data/reunioes-eventos.jsonl
```

Os dois primeiros comandos demonstram o exemplo e suas mêÂ©tricas simuladas. O terceiro faz uma prêÂ©via sem API. Com `--live`, o executor processa JSONL, permite atêÂ© quatro workers e retoma resultados salvos. A avaliaêÂ§êÂ£o por pergunta mede acurêÂ¡cia, Brier ou erro de Score conforme o tipo. **[InstruêÂ§êÂµes, integraêÂ§êÂ£o e limites](docs/11-fluxos-praticos.md)**. NêÂ£o hêÂ¡ coleta de plataformas ou aêÂ§êÂµes automêÂ¡ticas.

## Jev real pelo OpenRouter

Suporte a **`~typesafe/jev-latest`**, confirmado na Decisions API. Configure `OPENROUTER_API_KEY` no backend e execute:

```bash
python3 -m pacotes.executar atendimento --live --provider openrouter
```

Os dez pacotes originais foram testados com inferêÂªncia real: dez respostas sem falha e dez classificaêÂ§êÂµes esperadas nos exemplos fictêÂ­cios. Isso êÂ© um teste de integraêÂ§êÂ£o, nêÂ£o benchmark independente. [ConfiguraêÂ§êÂ£o e cêÂ³digo para seu sistema](docs/10-openrouter.md) êÂ· [RelatêÂ³rio medido](reports/openrouter-smoke.json).

## Usar no Codex, Claude Code e openpcbotv3

- **Codex:** `$jev-decidir` seguido do contexto e das alternativas.
- **Claude Code:** `/jev-decidir` seguido do contexto e das alternativas.
- **openpcbotv3:** `/jev observar` compara rota, agente e skill; `/jev` mostra o resultado, `/jev off` desliga. IntegraêÂ§êÂ£o nativa via gateway, com orêÂ§amento e custos registrados.

[InstalaêÂ§êÂ£o e exemplos da skill](pacotes/skills/jev-decidir/README.md) êÂ· [IntegraêÂ§êÂ£o no bot](https://github.com/inematds/openpcbotv3/blob/main/docs/JEV.md). A consulta direta usa o cliente deste repo; o bot conserva seu prêÂ³prio gateway e seu roteador em modo de observaêÂ§êÂ£o.

## Uma porta sêÂ³ para as consultas: jev-gw

Quando a consulta ao Jev sai de vêÂ¡rios pontos do mesmo sistema, o **[jev-gw](https://github.com/inematds/jev-gw)** concentra tudo numa porta: teto de gasto diêÂ¡rio conferido antes de cada consulta, cache por pedido idêÂªntico, registro de custo e latêÂªncia, e falha conservadora ê¢â¬â Jev fora do ar, chave errada ou teto estourado devolvem revisêÂ£o em vez de quebrar quem chamou. Biblioteca Python, serviêÂ§o HTTP e CLI, sêÂ³ biblioteca padrêÂ£o. Usa o cliente deste repositêÂ³rio, vendorizado.

[Guia](https://inematds.github.io/jev-gw/guia/) êÂ· [Arquitetura](https://github.com/inematds/jev-gw/blob/main/ARQUITETURA.md)

## Comece em um minuto

No site pêÂºblico, escolha um dos **20 casos autorais**, examine contexto e critêÂ©rios e explore a resposta didêÂ¡tica. VocêÂª pode editar as perguntas, importar/exportar JSON e estimar custos sem cadastrar uma chave.

Para abrir no computador:

```bash
git clone https://github.com/inematds/jev.git
cd jev
python3 -m jev_lab serve
```

Abra `http://127.0.0.1:8765`. Requer Python 3.10+; o nêÂºcleo usa apenas a biblioteca padrêÂ£o. NêÂ£o precisa instalar um framework, banco ou serviêÂ§o de filas.

## O que estêÂ¡ disponêÂ­vel

| Recurso | O que vocêÂª faz |
|---|---|
| 20 casos com busca | Explora atendimento, evidêÂªncias, agentes, skills, cêÂ³digo, logs e mais |
| Choice, Noul e Score | Escolhe alternativas, mede probabilidade de sim ou usa rubricas ordenadas |
| VêÂ¡rias perguntas no mesmo contexto | Separa fila, urgêÂªncia, suficiêÂªncia e intenêÂ§êÂµes |
| Contexto textual ou JSON | Representa documentos, catêÂ¡logos e estados estruturados |
| ImportaêÂ§êÂ£o e exportaêÂ§êÂ£o | Leva requisiêÂ§êÂµes para a CLI e salva resultados com procedêÂªncia |
| PolêÂ­tica didêÂ¡tica | Observa como limiares e abstenêÂ§êÂ£o mudam a revisêÂ£o; nenhuma aêÂ§êÂ£o externa êÂ© executada |
| Custo completo estimado | Acrescenta fallback, revisêÂ£o humana e infraestrutura ao custo de entrada |
| Experimentos em lote | Executa regras, Jev direto, hêÂ­brido ou replay em dataset rotulado |
| ComparaêÂ§êÂ£o e mêÂ©tricas | Examina macro-F1, confusêÂ£o, cobertura, precisêÂ£o aceita, Brier/ECE, latêÂªncia e custo desconhecido |
| Leitor de relatêÂ³rios | Abre report.json no navegador e identifica erros para investigar |

As respostas do site sêÂ£o **simulaêÂ§êÂµes autorais**, claramente identificadas. Editar contexto, tipo, opêÂ§êÂµes ou pergunta desativa a fixture anterior. A pêÂ¡gina pêÂºblica nêÂ£o consulta uma API; no servidor local, a consulta real precisa de chave.

## Os 20 casos

Os dez iniciais: afirmaêÂ§êÂµes, atendimento, contratos, mudanêÂ§as em e-mails, modelos, verificaêÂ§êÂ£o de etapas, agentes, navegador por candidatos, revisêÂ£o clêÂ­nica fictêÂ­cia e alertas financeiros fictêÂ­cios.

Os dez adicionais: **skills, qualidade de comentêÂ¡rios, filtro de evidêÂªncias, triagem composta, intenêÂ§êÂµes simultêÂ¢neas, datas por candidatos, gravidade de logs, dados pessoais minimizados, assunto versus intenêÂ§êÂ£o e revisêÂ£o de diff**.

Cada caso possui estado, perguntas, fixture, explicaêÂ§êÂ£o e prêÂ³ximo passo. Casos de ferramentas/navegador sugerem candidatos; nêÂ£o controlam agentes, navegadores ou dispositivos. Os exemplos sensêÂ­veis permanecem supervisionados.

## Terminal: do exemplo ao experimento

```bash
# Listar e exportar um exemplo
python3 -m jev_lab cases
python3 -m jev_lab cases --id triagem-composta --out exemplos/minha-requisicao.json

# Verificar contrato, sem consumir API
python3 -m jev_lab validate exemplos/triagem-composta-request.json

# Consultar Jev quando houver acesso
python3 -m jev_lab ask exemplos/triagem-composta-request.json

# Experimento reproduzêÂ­vel, sem chave
python3 -m jev_lab experiment data/tickets-sinteticos.jsonl --out runs/regras

# Experimento com Jev real
python3 -m jev_lab experiment data/tickets-sinteticos.jsonl --provider jev --out runs/jev

# Comparar duas execuêÂ§êÂµes do mesmo dataset
python3 -m jev_lab compare runs/regras/report.json runs/jev/report.json
```

O comando anterior `batch` continua disponêÂ­vel. Para templates de outras tarefas, repetibilidade, hêÂ­brido e importaêÂ§êÂ£o de resultados de LLM, veja **[Experimentos reproduzêÂ­veis](docs/08-experimentos.md)**. NêÂ£o hêÂ¡ adaptador LLM ao vivo: a comparaêÂ§êÂ£o externa usa replay com hash da requisiêÂ§êÂ£o e origem declarada.

## ConfiguraêÂ§êÂ£o local

O cliente usa `OPENROUTER_API_KEY` com OpenRouter e `TYPESAFE_API_KEY` com TypeSafe direta. Carrega a chave do ambiente ou, em runtime, de `~/projetos/openpcbotv2/.env` e `~/projetos/wifi/.env`. NêÂ£o copia a chave e nêÂ£o a envia ao navegador. Sem acesso, exemplos, exportaêÂ§êÂ£o, calculadora e regras continuam funcionando; chamadas reais registram falha explêÂ­cita.

O servidor escuta somente loopback, valida origem e serve arquivos permitidos. Serializa as consultas ao provedor. NêÂ£o êÂ© um backend pêÂºblico multiusuêÂ¡rio. A aplicaêÂ§êÂ£o aceita descriêÂ§êÂµes textuais, atêÂ© 30 perguntas e 100 KB por payload; esses dois êÂºltimos limites sêÂ£o locais, nêÂ£o limites anunciados do Jev. Score usa 2ê¢â¬â10 nêÂ­veis e exige legend correspondente na resposta.

## EvidêÂªncia e limites

- A baseline foi reexecutada: **20/24 acertos (83,33%)**, macro-F1 **0,84235**, em tickets fictêÂ­cios. [RelatêÂ³rio de regras](reports/experimento-regras/report.json).
- HêÂ¡ um [replay didêÂ¡tico](data/replay-didatico.jsonl) com erros intencionais e custo/latêÂªncia desconhecidos. A [comparaêÂ§êÂ£o de exemplo](reports/comparacao-didatica.json) êÂ© um exercêÂ­cio, nêÂ£o benchmark Jev.
- Os testes verificam contratos, orêÂ§amento, falhas, duplicatas, replay, repetibilidade e polêÂ­ticas. O navegador verifica os 20 casos, ediêÂ§êÂ£o, tipos, importaêÂ§êÂ£o/exportaêÂ§êÂ£o e leitura de relatêÂ³rio.
- **HêÂ¡ teste de integraêÂ§êÂ£o real pelo OpenRouter, mas nêÂ£o benchmark independente.** A suêÂ­te automatizada usa respostas controladas; o relatêÂ³rio separado registra as dez consultas reais. Qualidade em portuguêÂªs e calibraêÂ§êÂ£o ainda precisam de avaliaêÂ§êÂ£o com referêÂªncia humana em dados independentes.
- NêÂ£o hêÂ¡ envio de mensagens, pagamentos, merge, diagnêÂ³stico ou execuêÂ§êÂ£o de ferramentas. A classificaêÂ§êÂ£o nêÂ£o concede permissêÂ£o.

## DocumentaêÂ§êÂ£o

- [Usar Jev pelo OpenRouter](docs/10-openrouter.md)

- [Exageros, dêÂºvidas e limites](docs/07-exageros-e-duvidas.md)
- [Experimentos e formato de replay](docs/08-experimentos.md)
- [O que foi agregado e o que depende de evidêÂªncia](docs/09-evolucao.md)
- [AnêÂ¡lise conceitual](docs/01-analise.md) êÂ· [Dez aplicaêÂ§êÂµes iniciais](docs/02-aplicacoes.md)
- [Plano original](docs/03-plano-aplicacao.md) êÂ· [Protocolo de avaliaêÂ§êÂ£o](docs/04-avaliacao.md) êÂ· [Custos](docs/05-custos.md)
- [Curso: 36 aulas e 12 laboratêÂ³rios](https://inematds.github.io/jev-curso/)
- [Modelos oficiais](https://docs.typesafe.ai/models) êÂ· [API oficial](https://docs.typesafe.ai/api)

## Desenvolvimento e publicaêÂ§êÂ£o

```bash
python3 -m unittest discover -s tests -v
python3 scripts/build_site.py
```

Os testes de navegador em `tests/browser.cjs` usam Playwright somente no desenvolvimento. Instale-o no ambiente de testes com `npm install --no-save --package-lock=false playwright` e `npx playwright install chromium`; a aplicaêÂ§êÂ£o nêÂ£o precisa dessa dependêÂªncia. `BASE_URL`, `GUIDE_URL` e `SCREENSHOT_DIR` permitem apontar o teste para os servidores e diretêÂ³rio de capturas escolhidos. O build publica uma lista explêÂ­cita: `app/`, `guia/`, `capa/` e a entrada do site. DocumentaêÂ§êÂ£o pêÂºblica fica no repositêÂ³rio.

**TranscriêÂ§êÂµes e materiais recebidos permanecem somente locais**, ignorados pelo Git e excluêÂ­dos do build. RelatêÂ³rios operacionais vêÂ£o em `runs/`, tambêÂ©m ignorada. Os documentos pêÂºblicos sêÂ£o autorais, com referêÂªncias oficiais. Confira o [changelog](CHANGELOG.md).

## Roteiro de piloto e composiêÂ§êÂ£o

[Planeje uma decisêÂ£o, escreva casos de fronteira e combine rubricas Score](docs/12-piloto-e-composicao.md). A funêÂ§êÂ£o `pacotes.composicao.compor` valida os dados e calcula um êÂ­ndice ponderado; nêÂ£o representa confianêÂ§a nem autoriza aêÂ§êÂµes. [Curso HTML v2](https://inematds.github.io/jev-curso/) com 36 aulas, progresso e roteiro pessoal.

## PrêÂ³ximos passos e alternativa local

[PendêÂªncias confirmadas](tasks/current.md#pendêÂªncias-confirmadas-em-21092026) êÂ· [AnêÂ¡lise do Laya para comparaêÂ§êÂ£o com Jev e piloto no bot v3](docs/13-analise-laya.md). A integraêÂ§êÂ£o Laya êÂ© proposta, ainda nêÂ£o implementada.

### ReferêÂªncias Laya

Laya êÂ© um candidato local para o piloto comparativo. A anêÂ¡lise e as referêÂªncias jêÂ¡ estêÂ£o disponêÂ­veis; o adaptador no Jev e a integraêÂ§êÂ£o no bot continuam pendentes.

[Laya INEMA](https://github.com/inematds/laya) êÂ· [CêÂ³digo original](https://github.com/NandhaKishorM/laya) êÂ· [Modelos e model card](https://huggingface.co/convaiinnovations/laya) êÂ· [Laya no Eventos](https://eventos.inema.pro/jev/#laya)

---

Projeto  - ARCKHÉe