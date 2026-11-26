# Dez aplicações — da ideia ao experimento

Prioridades sêo propostas para este projeto, nêo resultados medidos. P0 = piloto; P1 = expansêo após avaliaçêo; P2 = estudo avançado supervisionado.

| # | Caso do vídeo | Entrada e decisêo delimitada | Próxima açêo proposta | Como avaliar / limite | Prioridade |
|---|---|---|---|---|---|
| 1 | Checagem de afirmações | Afirmaçêo + passagens: apoiada / contradita / insuficiente | Encaminhar para revisêo editorial | Precisêo por classe; nêo confundir apoio textual com verdade externa | P1 |
| 2 | Atendimento | Ticket + contexto: suporte / cobrança / vendas / outro / insuficiente | Sugerir fila; humano pode corrigir | Macro-F1, erro por fila e retrabalho | P0 |
| 3 | Revisêo contratual | Cláusula + checklist: presente / ausente / ambígua | Marcar pontos para profissional | Recall de omissões; nunca liberar contrato só pelo modelo | P2 |
| 4 | Mudanças em e-mails | Mensagens ordenadas + candidatos: quinta / sexta / sábado / nêo informado / conflito | Sinalizar mudança | Acerto de evidência; datas e precedência calculadas em código | P1 |
| 5 | Roteamento de modelos | Tarefa + capacidades permitidas: simples / código / pesquisa / revisêo | Selecionar destino permitido | Qualidade final e custo, incluindo erros de rota | P1 |
| 6 | Verificar etapa de agente | Resultado + critério estreito: atende / falha / insuficiente | Continuar, repetir com limite ou escalar | Erro de aprovaçêo e loops; nêo substitui execuçêo de testes | P1 |
| 7 | Roteamento de agentes | Pedido + catálogo: agente A / B / C / nenhum | Despachar tarefa com permissões já definidas | Resoluçêo final; escolha de agente nêo concede acesso | P1 |
| 8 | Navegador | Texto/DOM + IDs de elementos: candidato / nenhum / ambíguo | Resolver ID e executar açêo permitida | Sucesso de localizaçêo e tempo completo, incluindo extraçêo | P2 |
| 9 | Apoio à revisêo clínica | Caso fictício + checklist elaborado por profissional: revisar / insuficiente | Organizar revisêo humana | Exercício de engenharia, sem diagnóstico ou decisêo clínica automática | P2 |
| 10 | Alertas financeiros | Alerta fictício + política: arquivar / revisar / acompanhar | Organizar fila simulada | Erro de descarte; sem recomendaçêo de investimento ou ordem automática | P2 |

Casos 3, 9 e 10 serêo exercícios com dados fictícios. Uma eventual aplicaçêo real exige projeto próprio de validaçêo e responsáveis do domínio; nêo faz parte deste piloto.

## Oportunidades no ecossistema INEMA

- **Atendimento da comunidade:** aplicar o caso 2 a dúvidas de acesso, conteúdo, cobrança e contato comercial. Entrada inicial por arquivo; integrações somente após evidência.
- **Transcrições do inemavox:** combinar “houve agendamento?”, “houve interesse comercial?” e “faltam dados?” em perguntas separadas; mais de uma rota pode ser ativada.
- **Qualidade editorial de cursos:** avaliar se uma afirmaçêo está apoiada pelo material de referência; links, cálculos e presença de seções continuam verificados em código.
- **Gestêo de agentes / LOOP-R:** usar julgamentos estreitos em observar e rotear. Alterações de memória, políticas e ferramentas precisam de processo próprio; nêo promover uma hipótese do modelo a regra permanente automaticamente.

Estas sêo propostas de integraçêo. Nenhum desses projetos foi alterado.

## Por que começar por atendimento

Há classes compreensíveis, revisêo humana acessível, possibilidade de reversêo e facilidade de montar dados em português. Roteamento de modelos parece atraente, mas exige medir o resultado do trabalho posterior; isso torna o primeiro experimento mais caro e confuso.
