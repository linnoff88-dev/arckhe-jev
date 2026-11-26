# Plano de aplicaçêo — Jev Decision Lab

## Produto e escopo

Um laboratório local para comparar decisões e recomendar rotas de tickets. Primeiro resultado útil: importar um JSONL anonimizado e produzir relatório de qualidade, tempo, custo e encaminhamentos. Primeiro usuário: responsável por atendimento/automaçêo. Nenhuma conexêo a CRM é necessária para provar a hipótese.

MVP proposto: CLI Python, adaptadores de provedor, política de roteamento independente e relatórios JSON/CSV. API HTTP e interface vêm depois se o piloto justificar. O provedor deve ser substituível; nêo começar construindo uma plataforma de agentes.

## Fluxo

```mermaid
flowchart TD
  A[Ticket com ID] --> B[Validar e minimizar dados]
  B --> C{Regra exata resolve?}
  C -->|Sim| D[Rota por código]
  C -->|Nêo| E[Jev: perguntas estreitas]
  E --> F[Validar contrato e aplicar política]
  F --> G{Informaçêo e critérios suficientes?}
  G -->|Sim| H[Sugerir fila reversível]
  G -->|Nêo| I[Revisêo humana ou LLM contratado]
  D --> J[Registrar decisêo e correçêo]
  H --> J
  I --> J
```

## Decisões do MVP

| Questêo | Forma | Política |
|---|---|---|
| Qual fila descreve o pedido principal? | Choice: suporte, cobrança, vendas, outro, insuficiente | Apenas sugestêo no piloto |
| Há informaçêo suficiente para rotear? | Choice: suficiente, insuficiente, contraditória | Falta/conflito leva a revisêo |
| O pedido exige açêo irreversível? | Regra de permissões + sinal auxiliar | Nunca depender só da classificaçêo |
| Há intençêo de agendamento e de vendas? | Perguntas independentes na expansêo de transcrições | Permitir múltiplas etiquetas |

Quando há duas intenções, usar regra documentada de fila principal ou revisar; nêo forçar exclusividade onde o produto precisa de várias rotas.

## Contratos propostos

Entrada interna: `event_id`, `text`, `language`, `source`, `received_at`, `allowed_routes`. Campos de identidade pessoal sêo removidos quando nêo necessários. O contrato do evento é nosso; nêo confundir com o payload da API.

Saída interna: ID do evento, fila sugerida, decisêo da política (`suggest`/`review`), motivo da política, resposta bruta restrita, modelo resolvido, versões de pergunta e política, duraçêo total, tokens, custo estimado e correçêo humana posterior. O motivo da política é calculado em código; nêo é uma explicaçêo gerada pelo Jev.

[Payload de referência](../exemplos/triagem-request.json), alinhado à [API oficial](https://docs.typesafe.ai/api). Chamada planejada: `POST https://api.typesafe.ai/v1/systemone`, autenticaçêo Bearer no backend. Nêo expor chave em navegador ou repositório. O arquivo JSON ainda nêo foi testado no serviço.

## Política operacional

1. Rejeitar entrada vazia/inválida antes do provedor; encaminhar ao tratamento operacional.
2. Se tipo/campos/probabilidades forem inválidos, nêo executar a rota.
3. Escolha `insuficiente`, `outro` sem fila definida, ambiguidade ou baixa segurança empírica: revisêo.
4. Limiares de probability e confidence sêo parâmetros diferentes, ajustados no conjunto de calibraçêo. Sem avaliaçêo aprovada, toda sugestêo permanece em observaçêo.
5. Ações permitidas sêo fixadas por código e credenciais. Modelo nêo autoriza pagamento, exclusêo ou envio externo.
6. Guardar ID idempotente para impedir processamento duplicado; uma correçêo de humano vence a recomendaçêo anterior.
7. Limitar tentativas a três no total, com prazo global proposto de 5 s para o piloto. Honrar `retry-after`; se exceder o prazo, revisar. Ajustar esses números a partir das medições.
8. Nêo repetir automaticamente erros de autenticaçêo ou contrato. Em sobrecarga, aplicar backoff e circuito de interrupçêo para impedir tempestade de chamadas.
9. Nêo confundir tentativa HTTP com repetiçêo de raciocínio. Ambos têm limites independentes; fallback nêo deve formar ciclo.
10. Desligamento por configuraçêo retorna ao fluxo anterior. Registrar incidentes e guardar amostras anonimizadas dos erros.

## Etapas e entregáveis

Estimativa de esforço, nêo compromisso de calendário; considera uma pessoa técnica e apoio de quem conhece o atendimento.

| Etapa | Esforço estimado | Entrega | Condiçêo de saída |
|---|---:|---|---|
| 0 — definir decisões | 1 dia | Taxonomia, critérios e política de dados | Responsável do atendimento consegue rotular exemplos |
| 1 — preparar evidência | 2–3 dias | Dataset revisado e partições congeladas | Duplicatas removidas; desacordos tratados |
| 2 — implementar laboratório | 2–3 dias | CLI, adaptadores, regras, logs e simulador | Erros e limites testados; modo offline reproduzível |
| 3 — comparar | 2 dias | Relatório regras × Jev × LLM × híbrido | Critérios de avaliaçêo publicados antes de abrir teste |
| 4 — observar tráfego | 5–10 dias úteis decorridos | Sugestões sem açêo automática + correções humanas | Cobertura das classes e volume suficientes |
| 5 — liberar rota limitada | 1–2 dias + monitoramento | Uma fila reversível, configuraçêo de retorno | Metas atendidas e responsável aceita impacto observado |

Se nêo houver acesso à API, avançar com rotulagem, baseline e simulador marcado como simulaçêo. Nêo apresentar latências simuladas como benchmark. Procurar credenciais, quando necessárias, nos dois arquivos de ambiente indicados nas regras globais; carregar em runtime, sem copiar valores.

## Critérios de aceite do produto

Importaçêo reproduzível; nenhuma perda silenciosa de eventos; cada resultado rastreável; custo de retries e fallback contabilizado; rota desconhecida impedida por código; dados reais fora do Git; comando de retorno ao fluxo anterior demonstrado. A qualidade necessária é definida no [protocolo](04-avaliacao.md).

## Após o piloto

Expandir uma aplicaçêo de cada vez: transcrições, revisêo editorial e entêo roteamento de modelos/agentes. Só construir conectores de CRM ou orquestrador quando o consumidor e o contrato estiverem definidos. Trigger.dev, n8n e outros sêo alternativas futuras, nêo dependências do MVP.
