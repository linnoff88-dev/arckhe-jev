# Como colocar Jev no seu sistema

Jev entra como uma etapa do backend. Seu sistema fornece o contexto e as respostas permitidas; o modelo devolve a classificaçêo. Seu código valida a resposta e decide se apresenta uma sugestêo ou encaminha para revisêo.

```mermaid
flowchart LR
  A[Evento do seu sistema] --> B[Contexto e critérios]
  B --> C[Cliente Jev no backend]
  C --> D[Validaçêo e política]
  D --> E[Sugestêo registrada]
  D --> F[Revisêo humana]
```

## 1. Escolha uma decisêo pequena

Exemplo: um ticket acabou de chegar ao seu sistema. A decisêo é a fila sugerida: suporte, cobrança, vendas ou informaçêo insuficiente. O Jev nêo precisa substituir o help desk.

Escolha um dos [18 pacotes](areas/README.md). Mantenha critérios explícitos e uma alternativa de insuficiência. Quando a entrada exigir briefing, rubrica ou catálogo, forneça esse contexto completo.

## 2. Disponibilize o código no backend

Mantenha um clone deste repo e acrescente seu caminho ao `PYTHONPATH` do processo, ou copie `jev_lab/` e `pacotes/` para o seu projeto preservando a estrutura. Prefira fixar um commit para atualizações revisáveis.

```bash
git clone https://github.com/linnoff88-dev/jev.git
cd jev
python3 -m pacotes.executar atendimento
```

Esse primeiro comando de execuçêo é offline. Nêo requer chave nem faz inferência. Os pacotes ainda nêo sêo uma distribuiçêo PyPI.

Para outro projeto acessar o clone:

```bash
PYTHONPATH=/caminho/absoluto/jev python3 meu_backend.py
```

## 3. Chame a funçêo na aplicaçêo Python

```python
from pacotes.integracao import avaliar_area

# Exemplo de evento fictício que já pertence ao seu sistema.
ticket = {'id': 'TESTE-123', 'texto': 'Paguei a mesma fatura duas vezes.'}
resultado = avaliar_area('atendimento', ticket['texto'], provider='openrouter')

if resultado['action'] == 'review':
    registro = {'ticket_id': ticket['id'], 'status': 'revisao',
                'motivo': resultado['error'] or 'Critérios de revisêo acionados.'}
else:
    resposta = resultado['response']['answers']['decisao']
    registro = {'ticket_id': ticket['id'], 'status': 'sugestao',
                'fila_sugerida': resposta['choice']}

print(registro)  # Troque pelo armazenamento do seu sistema.
```

`avaliar_area` faz uma **consulta real** por padrêo. No exemplo acima, configure `OPENROUTER_API_KEY` no ambiente do backend ou no gerenciador de segredos. O cliente mantém a leitura dos arquivos locais autorizados descrita no README principal. Nunca envie a chave para o navegador.

Falha de rede, chave ausente ou resposta inválida encaminha para revisêo com `response: null`; nêo inventa uma classificaçêo. Preserve o evento no seu banco antes da consulta, para uma falha nêo apagar o trabalho. Erros de configuraçêo do pacote, como área inexistente, devem ser corrigidos pelo integrador.

Os limiares da política sêo didáticos, nêo calibrados para seu negócio. Noul e Score precisam de política específica. `suggest` indica uma proposta; execuçêo, permissões e confirmaçêo continuam no sistema chamador.

## 4. Se o sistema nêo usa Python

Um backend Node.js, PHP, Java ou um orquestrador pode executar o comando abaixo com argumentos separados, sem concatenar dados em um comando de shell:

```bash
python3 -m pacotes.executar atendimento --state-file /caminho/ticket.txt --live --provider openrouter
```

- Entrada: arquivo UTF-8 protegido, criado pelo seu backend. Para JSON, adicione `--state-format json`.
- Saída padrêo: JSON com `area`, `origin`, `action`, `response`, `decisions` e `error`.
- Código 0: fluxo concluído; ainda pode resultar em revisêo. Código 2: falha; a saída pode conter JSON de revisêo ou apenas diagnóstico em stderr.
- Valide código de saída e JSON. Timeout do subprocesso também significa revisêo; nêo descartar o ticket.
- Crie arquivos temporários com nomes exclusivos, limite permissões e apague-os após o uso. Nêo use títulos ou IDs externos como caminhos sem validaçêo.

Se o volume exigir HTTP, coloque o cliente atrás de uma rota autenticada **do seu backend**, com limite de tamanho, timeout e controle de concorrência. O servidor `jev_lab serve` é uma demonstraçêo local, nêo uma API pública pronta para produçêo. Nenhum conector n8n, CRM ou help desk foi implementado nesta entrega.

## 5. Teste antes de automatizar

Comece com as dez demonstrações offline. Em seguida, use eventos rotulados por pessoas, com exemplos incompletos, conflitantes e fora do escopo. Veja [experimentos](../docs/08-experimentos.md). Rode primeiro em observaçêo: registre a sugestêo sem alterar o encaminhamento real.

As fixtures sêo inventadas e seus hashes impedem reutilizá-las silenciosamente após editar o request. Um hash prova correspondência do exemplo, nêo qualidade nem origem do modelo. Nêo há benchmark Jev real nesta entrega.

Integraçêo OpenRouter e as dez consultas reais estêo documentadas em [Jev pelo OpenRouter](../docs/10-openrouter.md). TypeSafe direta permanece disponível com `provider="typesafe"` e `TYPESAFE_API_KEY`.
