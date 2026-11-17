# Jev Decision Lab

**Idiomas:** [PortuguÃªs](README.md) Â· [English](README.en.md) Â· [EspaÃ±ol](README.es.md)

![Jev Decision Lab â€” mais recursos, uso simples: 20 casos prÃ¡ticos, experimentos e comparaÃ§Ã£o](capa/novos-recursos-v1.2.1.png)

**v1.8.1** Â· Um laboratÃ³rio para formular, testar e comparar decisÃµes de IA, com uma interface simples e recursos avanÃ§ados quando vocÃª precisar.

[Abrir laboratÃ³rio](https://inematds.github.io/jev/app/) Â· [Guia de uso](https://inematds.github.io/jev/guia/) Â· [Curso em portuguÃªs](https://inematds.github.io/jev-curso/)

[Acervo Jev no Eventos INEMA](https://eventos.inema.pro/jev/) â€” projeto, curso e pacotes reunidos em uma pÃ¡gina, com apresentaÃ§Ã£o em portuguÃªs, inglÃªs e espanhol.

**ReferÃªncia oficial de modelos, preÃ§os, modalidades e limites:** https://docs.typesafe.ai/models

**Leia tambÃ©m: [Exageros, dÃºvidas e limites do Jev](docs/07-exageros-e-duvidas.md).** O documento separa capacidade documentada, hipÃ³tese, extrapolaÃ§Ã£o e evidÃªncia ainda necessÃ¡ria.

## Leve para seu projeto

A pasta **[pacotes/](pacotes/README.md)** reÃºne **18 pacotes por Ã¡rea**, uma integraÃ§Ã£o Python, a skill `jev-integrar` e receitas de uso. Comece offline com `python3 -m pacotes.executar atendimento`. Para seu backend, veja [como integrar Jev nos sistemas](pacotes/INTEGRACAO.md). Veja em cada pacote o que jÃ¡ funciona e o que ainda depende de integraÃ§Ã£o.

## Novos fluxos prÃ¡ticos

Agora sÃ£o **18 pacotes reutilizÃ¡veis**: os dez originais mais **caixa de entrada, comentÃ¡rios do YouTube, comunidades, reuniÃµes, cortes por transcriÃ§Ã£o, notas, curadoria de feed e hospedagem Ã— perfis de viajantes**. Os novos pacotes combinam Choice, Noul e Score; suas fixtures sÃ£o fictÃ­cias, sem benchmark real. O pacote de viagens avalia cada anÃºncio uma vez e cruza as respostas por regra com doze perfis (`python3 -m pacotes.cruzamento viagens --preco 420`).

```bash
python3 -m pacotes.executar reunioes
python3 -m pacotes.qualidade reunioes
python3 -m pacotes.lote reunioes data/reunioes-eventos.jsonl
```

Os dois primeiros comandos demonstram o exemplo e suas mÃ©tricas simuladas. O terceiro faz uma prÃ©via sem API. Com `--live`, o executor processa JSONL, permite atÃ© quatro workers e retoma resultados salvos. A avaliaÃ§Ã£o por pergunta mede acurÃ¡cia, Brier ou erro de Score conforme o tipo. **[InstruÃ§Ãµes, integraÃ§Ã£o e limites](docs/11-fluxos-praticos.md)**. NÃ£o hÃ¡ coleta de plataformas ou aÃ§Ãµes automÃ¡ticas.

## Jev real pelo OpenRouter

Suporte a **`~typesafe/jev-latest`**, confirmado na Decisions API. Configure `OPENROUTER_API_KEY` no backend e execute:

```bash
python3 -m pacotes.executar atendimento --live --provider openrouter
```

Os dez pacotes originais foram testados com inferÃªncia real: dez respostas sem falha e dez classificaÃ§Ãµes esperadas nos exemplos fictÃ­cios. Isso Ã© um teste de integraÃ§Ã£o, nÃ£o benchmark independente. [ConfiguraÃ§Ã£o e cÃ³digo para seu sistema](docs/10-openrouter.md) Â· [RelatÃ³rio medido](reports/openrouter-smoke.json).

## Usar no Codex, Claude Code e openpcbotv3

- **Codex:** `$jev-decidir` seguido do contexto e das alternativas.
- **Claude Code:** `/jev-decidir` seguido do contexto e das alternativas.
- **openpcbotv3:** `/jev observar` compara rota, agente e skill; `/jev` mostra o resultado, `/jev off` desliga. IntegraÃ§Ã£o nativa via gateway, com orÃ§amento e custos registrados.

[InstalaÃ§Ã£o e exemplos da skill](pacotes/skills/jev-decidir/README.md) Â· [IntegraÃ§Ã£o no bot](https://github.com/inematds/openpcbotv3/blob/main/docs/JEV.md). A consulta direta usa o cliente deste repo; o bot conserva seu prÃ³prio gateway e seu roteador em modo de observaÃ§Ã£o.

## Uma porta sÃ³ para as consultas: jev-gw

Quando a consulta ao Jev sai de vÃ¡rios pontos do mesmo sistema, o **[jev-gw](https://github.com/inematds/jev-gw)** concentra tudo numa porta: teto de gasto diÃ¡rio conferido antes de cada consulta, cache por pedido idÃªntico, registro de custo e latÃªncia, e falha conservadora â€” Jev fora do ar, chave errada ou teto estourado devolvem revisÃ£o em vez de quebrar quem chamou. Biblioteca Python, serviÃ§o HTTP e CLI, sÃ³ biblioteca padrÃ£o. Usa o cliente deste repositÃ³rio, vendorizado.

[Guia](https://inematds.github.io/jev-gw/guia/) Â· [Arquitetura](https://github.com/inematds/jev-gw/blob/main/ARQUITETURA.md)

## Comece em um minuto

No site pÃºblico, escolha um dos **20 casos autorais**, examine contexto e critÃ©rios e explore a resposta didÃ¡tica. VocÃª pode editar as perguntas, importar/exportar JSON e estimar custos sem cadastrar uma chave.

Para abrir no computador:

```bash
git clone https://github.com/inematds/jev.git
cd jev
python3 -m jev_lab serve
```

Abra `http://127.0.0.1:8765`. Requer Python 3.10+; o nÃºcleo usa apenas a biblioteca padrÃ£o. NÃ£o precisa instalar um framework, banco ou serviÃ§o de filas.

## O que estÃ¡ disponÃ­vel

| Recurso | O que vocÃª faz |
|---|---|
| 20 casos com busca | Explora atendimento, evidÃªncias, agentes, skills, cÃ³digo, logs e mais |
| Choice, Noul e Score | Escolhe alternativas, mede probabilidade de sim ou usa rubricas ordenadas |
| VÃ¡rias perguntas no mesmo contexto | Separa fila, urgÃªncia, suficiÃªncia e intenÃ§Ãµes |
| Contexto textual ou JSON | Representa documentos, catÃ¡logos e estados estruturados |
| ImportaÃ§Ã£o e exportaÃ§Ã£o | Leva requisiÃ§Ãµes para a CLI e salva resultados com procedÃªncia |
| PolÃ­tica didÃ¡tica | Observa como limiares e abstenÃ§Ã£o mudam a revisÃ£o; nenhuma aÃ§Ã£o externa Ã© executada |
| Custo completo estimado | Acrescenta fallback, revisÃ£o humana e infraestrutura ao custo de entrada |
| Experimentos em lote | Executa regras, Jev direto, hÃ­brido ou replay em dataset rotulado |
| ComparaÃ§Ã£o e mÃ©tricas | Examina macro-F1, confusÃ£o, cobertura, precisÃ£o aceita, Brier/ECE, latÃªncia e custo desconhecido |
| Leitor de relatÃ³rios | Abre report.json no navegador e identifica erros para investigar |

As respostas do site sÃ£o **simulaÃ§Ãµes autorais**, claramente identificadas. Editar contexto, tipo, opÃ§Ãµes ou pergunta desativa a fixture anterior. A pÃ¡gina pÃºblica nÃ£o consulta uma API; no servidor local, a consulta real precisa de chave.

## Os 20 casos

Os dez iniciais: afirmaÃ§Ãµes, atendimento, contratos, mudanÃ§as em e-mails, modelos, verificaÃ§Ã£o de etapas, agentes, navegador por candidatos, revisÃ£o clÃ­nica fictÃ­cia e alertas financeiros fictÃ­cios.

Os dez adicionais: **skills, qualidade de comentÃ¡rios, filtro de evidÃªncias, triagem composta, intenÃ§Ãµes simultÃ¢neas, datas por candidatos, gravidade de logs, dados pessoais minimizados, assunto versus intenÃ§Ã£o e revisÃ£o de diff**.

Cada caso possui estado, perguntas, fixture, explicaÃ§Ã£o e prÃ³ximo passo. Casos de ferramentas/navegador sugerem candidatos; nÃ£o controlam agentes, navegadores ou dispositivos. Os exemplos sensÃ­veis permanecem supervisionados.

## Terminal: do exemplo ao experimento

```bash
# Listar e exportar um exemplo
python3 -m jev_lab cases
python3 -m jev_lab cases --id triagem-composta --out exemplos/minha-requisicao.json

# Verificar contrato, sem consumir API
python3 -m jev_lab validate exemplos/triagem-composta-request.json

# Consultar Jev quando houver acesso
python3 -m jev_lab ask exemplos/triagem-composta-request.json

# Experimento reproduzÃ­vel, sem chave
python3 -m jev_lab experiment data/tickets-sinteticos.jsonl --out runs/regras

# Experimento com Jev real
python3 -m jev_lab experiment data/tickets-sinteticos.jsonl --provider jev --out runs/jev

# Comparar duas execuÃ§Ãµes do mesmo dataset
python3 -m jev_lab compare runs/regras/report.json runs/jev/report.json
```

O comando anterior `batch` continua disponÃ­vel. Para templates de outras tarefas, repetibilidade, hÃ­brido e importaÃ§Ã£o de resultados de LLM, veja **[Experimentos reproduzÃ­veis](docs/08-experimentos.md)**. NÃ£o hÃ¡ adaptador LLM ao vivo: a comparaÃ§Ã£o externa usa replay com hash da requisiÃ§Ã£o e origem declarada.

## ConfiguraÃ§Ã£o local

O cliente usa `OPENROUTER_API_KEY` com OpenRouter e `TYPESAFE_API_KEY` com TypeSafe direta. Carrega a chave do ambiente ou, em runtime, de `~/projetos/openpcbotv2/.env` e `~/projetos/wifi/.env`. NÃ£o copia a chave e nÃ£o a envia ao navegador. Sem acesso, exemplos, exportaÃ§Ã£o, calculadora e regras continuam funcionando; chamadas reais registram falha explÃ­cita.

O servidor escuta somente loopback, valida origem e serve arquivos permitidos. Serializa as consultas ao provedor. NÃ£o Ã© um backend pÃºblico multiusuÃ¡rio. A aplicaÃ§Ã£o aceita descriÃ§Ãµes textuais, atÃ© 30 perguntas e 100 KB por payload; esses dois Ãºltimos limites sÃ£o locais, nÃ£o limites anunciados do Jev. Score usa 2â€“10 nÃ­veis e exige legend correspondente na resposta.

## EvidÃªncia e limites

- A baseline foi reexecutada: **20/24 acertos (83,33%)**, macro-F1 **0,84235**, em tickets fictÃ­cios. [RelatÃ³rio de regras](reports/experimento-regras/report.json).
- HÃ¡ um [replay didÃ¡tico](data/replay-didatico.jsonl) com erros intencionais e custo/latÃªncia desconhecidos. A [comparaÃ§Ã£o de exemplo](reports/comparacao-didatica.json) Ã© um exercÃ­cio, nÃ£o benchmark Jev.
- Os testes verificam contratos, orÃ§amento, falhas, duplicatas, replay, repetibilidade e polÃ­ticas. O navegador verifica os 20 casos, ediÃ§Ã£o, tipos, importaÃ§Ã£o/exportaÃ§Ã£o e leitura de relatÃ³rio.
- **HÃ¡ teste de integraÃ§Ã£o real pelo OpenRouter, mas nÃ£o benchmark independente.** A suÃ­te automatizada usa respostas controladas; o relatÃ³rio separado registra as dez consultas reais. Qualidade em portuguÃªs e calibraÃ§Ã£o ainda precisam de avaliaÃ§Ã£o com referÃªncia humana em dados independentes.
- NÃ£o hÃ¡ envio de mensagens, pagamentos, merge, diagnÃ³stico ou execuÃ§Ã£o de ferramentas. A classificaÃ§Ã£o nÃ£o concede permissÃ£o.

## DocumentaÃ§Ã£o

- [Usar Jev pelo OpenRouter](docs/10-openrouter.md)

- [Exageros, dÃºvidas e limites](docs/07-exageros-e-duvidas.md)
- [Experimentos e formato de replay](docs/08-experimentos.md)
- [O que foi agregado e o que depende de evidÃªncia](docs/09-evolucao.md)
- [AnÃ¡lise conceitual](docs/01-analise.md) Â· [Dez aplicaÃ§Ãµes iniciais](docs/02-aplicacoes.md)
- [Plano original](docs/03-plano-aplicacao.md) Â· [Protocolo de avaliaÃ§Ã£o](docs/04-avaliacao.md) Â· [Custos](docs/05-custos.md)
- [Curso: 36 aulas e 12 laboratÃ³rios](https://inematds.github.io/jev-curso/)
- [Modelos oficiais](https://docs.typesafe.ai/models) Â· [API oficial](https://docs.typesafe.ai/api)

## Desenvolvimento e publicaÃ§Ã£o

```bash
python3 -m unittest discover -s tests -v
python3 scripts/build_site.py
```

Os testes de navegador em `tests/browser.cjs` usam Playwright somente no desenvolvimento. Instale-o no ambiente de testes com `npm install --no-save --package-lock=false playwright` e `npx playwright install chromium`; a aplicaÃ§Ã£o nÃ£o precisa dessa dependÃªncia. `BASE_URL`, `GUIDE_URL` e `SCREENSHOT_DIR` permitem apontar o teste para os servidores e diretÃ³rio de capturas escolhidos. O build publica uma lista explÃ­cita: `app/`, `guia/`, `capa/` e a entrada do site. DocumentaÃ§Ã£o pÃºblica fica no repositÃ³rio.

**TranscriÃ§Ãµes e materiais recebidos permanecem somente locais**, ignorados pelo Git e excluÃ­dos do build. RelatÃ³rios operacionais vÃ£o em `runs/`, tambÃ©m ignorada. Os documentos pÃºblicos sÃ£o autorais, com referÃªncias oficiais. Confira o [changelog](CHANGELOG.md).

## Roteiro de piloto e composiÃ§Ã£o

[Planeje uma decisÃ£o, escreva casos de fronteira e combine rubricas Score](docs/12-piloto-e-composicao.md). A funÃ§Ã£o `pacotes.composicao.compor` valida os dados e calcula um Ã­ndice ponderado; nÃ£o representa confianÃ§a nem autoriza aÃ§Ãµes. [Curso HTML v2](https://inematds.github.io/jev-curso/) com 36 aulas, progresso e roteiro pessoal.

## PrÃ³ximos passos e alternativa local

[PendÃªncias confirmadas](tasks/current.md#pendÃªncias-confirmadas-em-21092026) Â· [AnÃ¡lise do Laya para comparaÃ§Ã£o com Jev e piloto no bot v3](docs/13-analise-laya.md). A integraÃ§Ã£o Laya Ã© proposta, ainda nÃ£o implementada.

### ReferÃªncias Laya

Laya Ã© um candidato local para o piloto comparativo. A anÃ¡lise e as referÃªncias jÃ¡ estÃ£o disponÃ­veis; o adaptador no Jev e a integraÃ§Ã£o no bot continuam pendentes.

[Laya INEMA](https://github.com/inematds/laya) Â· [CÃ³digo original](https://github.com/NandhaKishorM/laya) Â· [Modelos e model card](https://huggingface.co/convaiinnovations/laya) Â· [Laya no Eventos](https://eventos.inema.pro/jev/#laya)

---

Projeto adaptado da comunidade INEMA TDS - Marca Arckhe