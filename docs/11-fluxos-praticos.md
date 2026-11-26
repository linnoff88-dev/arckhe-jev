# Fluxos práticos: sete novos pacotes e um executor comum

Disponível em **v1.6.1**. Os 20 casos do laboratório visual continuam disponíveis; os pacotes reutilizáveis passam de dez para **17**. Os sete novos pacotes têm perguntas combinadas e exemplos autorais fictícios. Nêo sêo conectores de plataformas nem modelos treinados para cada área.

## Escolha pelo trabalho

| Pacote | Perguntas e aplicaçêo | Limite essencial |
|---|---|---|
| [Caixa de entrada](../pacotes/areas/caixa-entrada/README.md) | Tipo, documento financeiro, patrocínio, sinal de phishing, urgência e aderência ao perfil | Nêo abre anexos, detecta todo golpe ou move mensagens |
| [Comentários do YouTube](../pacotes/areas/comentarios-youtube/README.md) | Ajuda, resposta, ideia de conteúdo, compra e contexto necessário | Nêo coleta nem responde comentários |
| [Comunidades](../pacotes/areas/comunidades/README.md) | Ajuda, intervençêo, insatisfaçêo explícita e intençêo declarada de saída | Nêo prevê abandono ou atributos pessoais |
| [Reuniões](../pacotes/areas/reunioes/README.md) | Decisêo, açêo, responsável e prazo explícitos | Nêo extrai automaticamente responsáveis nem resolve calendário |
| [Cortes](../pacotes/areas/cortes/README.md) | Ideia completa, dependência de contexto, frase destacável e clareza textual | Nêo estima retençêo real, analisa vídeo ou publica mídia |
| [Notas](../pacotes/areas/notas/README.md) | Tarefa, ideia, diário, referência, múltiplas intenções e prazo mencionado | Nêo cria compromissos ou resolve datas relativas |
| [Curadoria](../pacotes/areas/curadoria/README.md) | Relevância, exemplo concreto, alegaçêo de novidade e utilidade | Nêo comprova notícia ou identifica autoria por IA |

Os casos anteriores de `comentarios` e `emails` continuam distintos: comentários de código e mudanças em uma conversa por e-mail. Os novos pacotes nêo substituem esses exemplos.

## Um minuto, sem chave

```bash
python3 -m pacotes.executar --list
python3 -m pacotes.executar reunioes
python3 -m pacotes.qualidade reunioes
```

O primeiro lista os 18 pacotes. O segundo demonstra uma resposta inventada. O terceiro compara a fixture com rótulos autorais: serve para aprender o relatório, **nêo comprova qualidade do Jev**. Noul e Score continuam em revisêo na política existente, mesmo que a fixture pareça confiante.

## Processar eventos sem montar outro sistema

Formato de entrada JSONL, um evento completo por linha:

```json
{"id":"reuniao-001","state":"Decidimos publicar o tutorial. Ana revisará o roteiro até sexta."}
{"id":"reuniao-002","state":"Precisamos melhorar o tutorial. Voltamos a conversar depois."}
```

Exemplo fictício em [data/reunioes-eventos.jsonl](../data/reunioes-eventos.jsonl).

```bash
# Prévia: valida TODOS os eventos, nêo chama API e nêo cria saída
python3 -m pacotes.lote reunioes data/reunioes-eventos.jsonl

# API real: uma consulta por evento, com várias perguntas
python3 -m pacotes.lote reunioes data/reunioes-eventos.jsonl --live --provider openrouter --workers 2 --interval 1 --out runs/reunioes.jsonl

# Repetir o mesmo comando retoma os resultados já salvos
```

Características:

- Um worker por padrêo, máximo quatro. Intervalo mínimo entre inícios de consultas: um segundo por padrêo; configurável entre 0,1 e 60 segundos.
- O intervalo controla inícios no executor, **nêo cada tentativa HTTP interna**. O cliente já trata retries e `Retry-After`. Isso nêo substitui um limitador distribuído por tokens ou garante atender a cotas compartilhadas com outros processos.
- Teto padrêo de 100 eventos, ajustável com `--max-events` até 10 mil; arquivo limitado a 10 MB. O teto nêo é orçamento em dólares e cada consulta pode fazer até três tentativas HTTP.
- O arquivo de saída associa ID, assinatura do lote, resposta, política, latência da consulta, tentativas e custo conhecido. Nêo copia o estado original; IDs e resultados ainda podem conter dados sensíveis. Use `runs/`, ignorada pelo Git.
- Retomada exige os mesmos dados, ordem, template, provedor e modo. ID repetido na entrada é recusado antes de qualquer chamada. Alterar configuraçêo do modelo/template exige uma saída nova.
- Um lock impede dois escritores no mesmo arquivo. Resultados podem ser gravados fora da ordem de entrada; associe por ID.
- Erros do provedor viram revisêo, com custo desconhecido. Também sêo preservados na retomada; para tentar novamente, prepare um lote separado dos erros e outra saída, conscientemente.
- Queda após uma consulta e antes da gravaçêo pode levar a repetir a consulta na retomada. **Nêo é garantia de exatamente uma cobrança.** Um alias também pode mudar de versêo entre execuções; fixe o modelo no template para avaliações comparáveis.
- Um lock residual após encerramento abrupto deve ser removido somente depois de verificar que o processo anterior terminou. Linha incompleta é recusada: preserve o arquivo para investigaçêo, remova apenas o fragmento final comprovadamente incompleto e retome ciente da possível repetiçêo.
- Custos após retries sêo `null`: o custo da resposta final nêo revela necessariamente o consumo das tentativas anteriores. Latência por item exclui espera para iniciar no executor; nêo representa o tempo completo do lote.

Nêo há webhook, banco de eventos ou coleta de plataforma nesta entrega. Seu sistema exporta os eventos para JSONL; este executor classifica e registra resultados. Deduplicaçêo vale para a saída do mesmo lote, nêo globalmente entre diferentes arquivos.

## Avaliar cada pergunta com referência humana

O avaliador original `jev_lab experiment` permanece focado em uma Choice, com métricas detalhadas. O novo `pacotes.qualidade` complementa esse fluxo:

| Tipo | Métricas |
|---|---|
| Choice | Acurácia com erros operacionais no denominador |
| Noul | Acurácia com corte 0,5 e Brier binário nas respostas válidas |
| Score | Erro absoluto médio e erro normalizado pela amplitude da rubrica |
| Todos | Total, respostas válidas, erros e cobertura |

Corte 0,5 é uma definiçêo da métrica, nêo autorizaçêo para agir. Uma resposta com contrato inválido invalida todo o registro. MAE/Brier ignoram registros inválidos, mas o relatório mostra sua ausência pela cobertura. Nêo há métricas de consistência lógica conjunta, calibraçêo de Score, seleçêo automática de limiar ou comparaçêo LLM ao vivo.

Cada linha de referência exige `id`, `state`, `request_sha256`, `expected` para **todas** as perguntas, `origin` (`simulation` ou `measured`) e `response`. `expected` usa rótulo textual em Choice, booleano em Noul e número dentro da rubrica em Score. O hash corresponde ao request com o estado daquele evento, calculado por `pacotes.integracao.fingerprint`.

Para montar um arquivo a partir de um lote, mantenha os eventos originais e um arquivo `runs/rotulos.json` revisado por pessoas, no formato `{"id-do-evento":{"decisao":"...","tem_decisao":true,...}}`. Exemplo de junçêo:

```python
import json
from pathlib import Path
from pacotes.integracao import carregar, fingerprint

_, _, template = carregar('reunioes')
events = {r['id']: r for r in map(json.loads, Path('data/reunioes-eventos.jsonl').read_text().splitlines())}
labels = json.loads(Path('runs/rotulos.json').read_text())
with Path('runs/reunioes-rotuladas.jsonl').open('w') as out:
    for line in Path('runs/reunioes.jsonl').read_text().splitlines():
        row = json.loads(line)
        assert row['result']['origin'] == 'api'
        state = events[row['id']]['state']
        record = dict(id=row['id'], state=state,
                      request_sha256=fingerprint(dict(template, state=state)),
                      expected=labels[row['id']], origin='measured',
                      response=row['result']['response'])
        out.write(json.dumps(record, ensure_ascii=False) + '\n')
```

Use o mesmo template e os mesmos eventos da consulta. Preserve falhas com `response: null`; nêo as elimine para melhorar as métricas. Nêo copie as previsões para os rótulos esperados. A declaraçêo `measured` e o hash nêo autenticam a origem nem comprovam revisêo humana.

```bash
python3 -m pacotes.qualidade reunioes --input runs/reunioes-rotuladas.jsonl --out runs/qualidade.json
```

Esse relatório por pergunta é exibido na CLI/JSON; o leitor visual do app continua dedicado ao formato de `jev_lab experiment`.

## Como conectar aos seus projetos

- **OpenPCBot:** depois de transcrever uma nota, o bot pode consultar o template de `notas` pelo seu gateway existente e mostrar sugestões. A integraçêo de notas ainda precisa ser implementada no bot; nêo invoque este executor para contornar o gateway de custos.
- **inemavox:** transcriçêo e segmentaçêo continuam no inemavox. Jev recebe apenas texto e tempos dos candidatos; o editor confirma o corte.
- **Cursos e comunidades:** a aplicaçêo coleta eventos autorizados, exporta o JSONL e usa os resultados para montar uma fila de revisêo.
- **LLM gerador:** pode receber os itens selecionados para redigir rascunhos. Amostre também os descartados para encontrar falsos negativos. Nêo há geraçêo ou publicaçêo automática aqui.

## Evidência e próximo passo

Os sete novos pacotes foram validados com fixtures e testes controlados. O relatório OpenRouter anterior cobre somente os dez pacotes originais. Nêo extrapole sua evidência para estes sete. Antes de adoçêo operacional, separe exemplos reais para desenvolvimento, calibraçêo e teste independente, medindo erros por classe e custo completo.

A entrada do modelo é textual, com 64k tokens totais e 32k para estado mais a maior pergunta na versêo consultada. Saída sem cobrança nêo significa ausência de tokens. [Modelos oficiais](https://docs.typesafe.ai/models) · [Confidence](https://docs.typesafe.ai/confidence) · [Score](https://docs.typesafe.ai/primitives/score).

[Voltar ao README](../README.md)
