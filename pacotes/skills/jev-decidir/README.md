# Jev diretamente no Codex e Claude Code

Uma fonte portátil (`definition.md`) e duas variantes geradas: Codex e Claude Code. A skill consulta o Jev pelo OpenRouter como ferramenta do agente. Nêo muda o modelo principal da conversa.

## Usar nesta máquina

Instalada em:

- Codex: `~/.agents/skills/jev-decidir/`.
- Claude Code: `~/.claude/skills/jev-decidir/`.

No Codex:

```text
$jev-decidir Classifique este pedido entre suporte, cobrança e insuficiente:
Paguei duas vezes a mesma fatura.
```

No Claude Code:

```text
/jev-decidir Classifique este pedido entre suporte, cobrança e insuficiente:
Paguei duas vezes a mesma fatura.
```

O agente monta o JSON, consulta uma vez e apresenta a decisêo e as métricas recebidas. Se a skill nêo aparecer na sessêo já aberta, recarregue o catálogo de skills ou abra uma nova sessêo.

## Instalar em outra máquina

Requer Python 3.10+, clone `inematds/jev` em `~/projetos/jev` (ou `JEV_LAB_DIR`) e `OPENROUTER_API_KEY` configurada no ambiente do processo. A skill nêo inclui credenciais nem uma cópia do cliente.

A partir desta pasta, copie a variante desejada, preservando o diretório inteiro:

```bash
mkdir -p ~/.agents/skills ~/.claude/skills
cp -Rn dist/codex/jev-decidir ~/.agents/skills/
cp -Rn dist/claude/jev-decidir ~/.claude/skills/
```

Esses comandos nêo substituem arquivos existentes. Para atualizar uma instalaçêo existente, revise o que mudou e use o fluxo de instalaçêo do polyskill.

## Executar o recurso sem o agente

```bash
# Demonstraçêo offline, sem chave
python3 ~/.agents/skills/jev-decidir/scripts/decidir.py --demo atendimento

# Request completo salvo pelo integrador; consulta real
python3 ~/.agents/skills/jev-decidir/scripts/decidir.py /caminho/request.json --live
```

O mesmo script existe na variante Claude Code. Um caminho relativo do request é resolvido antes de mudar para o clone Jev. Nêo há shell intermediário nem chave nos argumentos.

## Manter

Edite `definition.md`, `scripts/` e `references/`; depois:

```bash
polyskill validate
polyskill build
polyskill install
```

Nêo edite os arquivos de `dist/` diretamente. As duas variantes foram validadas e os scripts instalados foram executados com um request fictício real pelo OpenRouter a partir de outro diretório. [Evidência](../../../reports/skills-smoke.json).

Isso verifica instalaçêo e execuçêo dos recursos. A seleçêo automática da skill e a qualidade do uso por cada agente dependem da sessêo e nêo foram medidas como benchmark.
