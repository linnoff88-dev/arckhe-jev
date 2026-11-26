# Marketing e conteúdo

Conferir se um texto respeita o briefing fornecido.

**Ponto de integraçêo:** Avalie o rascunho antes da etapa de aprovaçêo editorial.

**Uso da saída:** Marcar o texto para revisêo; nêo publicar nem comprovar fatos externos.

## Executar

Na raiz do clone Jev:

```bash
# Demonstraçêo sem rede nem chave
python3 -m pacotes.executar marketing

# Dados próprios em texto UTF-8; chamada real à API
python3 -m pacotes.executar marketing --state-file /caminho/entrada.txt --live --provider openrouter
```

Para estado estruturado, use `--state-format json` com um arquivo JSON. Para a consulta acima, configure `OPENROUTER_API_KEY` no backend. Revise os dados antes de enviá-los ao provedor. Nêo cole chaves no contexto.

## Arquivos

- [request.json](request.json): contexto fictício, instruções e alternativas editáveis.
- [fixture.json](fixture.json): resposta inventada para demonstrar o fluxo; nêo é benchmark.
- [pacote.json](pacote.json): metadados, rótulo esperado e exigência de supervisêo.

No exemplo, a classificaçêo esperada é **revisar**. O valor de confidence da fixture é inventado. A alternativa `revisar` e as alternativas de insuficiência continuam sob revisêo mesmo com confidence alta. Sugestões nêo executam ações por conta própria.

## Adaptar e avaliar

Troque o contexto e os critérios do request pelas regras reais da sua operaçêo. Mantenha uma saída para revisêo ou falta de informaçêo. O modo offline serve somente ao request original; arquivos alterados exigem validaçêo com uma nova referência, nêo o reaproveitamento silencioso da resposta inventada.

Monte um conjunto separado com exemplos claros, ambíguos, incompletos e fora do escopo. Compare com rótulos humanos antes de automatizar. Veja [integraçêo nos sistemas](../../INTEGRACAO.md) e [experimentos](../../../docs/08-experimentos.md). Nêo há conector pronto para plataformas comerciais neste pacote.
