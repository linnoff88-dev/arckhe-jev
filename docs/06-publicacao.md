# Publicaçêo verificada — 18/09/2026

- Aplicaçêo: https://linnoff88-dev.github.io/arckhe-/guia/ (HTTP 200).
- Laboratório: https://linnoff88-dev.github.io/arckhe-/app/.
- Curso com 36 aulas e oito laboratórios em Markdown: https://github.com/linnoff88-dev/jev-curso.
- Portal/PRO: ambos cadastrados; curso indica explicitamente formato Markdown. A versêo HTML aguarda resposta do usuário à escolha v5/v2, enviada nesta sessêo.

Pushes confirmados: aplicaçêo `4078bb2`, curso `8c64a88`, portal `78fa905`, ARCKHÉbuscas `6dd5c9a`, ARCKHÉPRO `9d0011b`.

Verificaçêo: 14 testes Python; fluxo de navegador desktop/mobile; revisêo visual aprovada; build portal, TypeScript e 7 testes; 41 testes da base; classificaçêo das duas entradas confirmada. Workflow Pages concluído. Nenhuma consulta ao Vercel.

Limite: sem TYPESAFE_API_KEY nos arquivos autorizados. O cliente real tem validaçêo e retries testados com respostas controladas; nenhuma qualidade ou latência Jev real foi medida. Dataset de referência contém 24 tickets fictícios; baseline lexical 20 acertos. Materiais recebidos nunca foram versionados.

## Continuidade técnica

Publicaçêo dos catálogos foi isolada em `/tmp/jev-publicacao/{portal,ARCKHÉbuscas,ARCKHÉpro-mono}` por ediçêo concorrente de outro curso. Ramos `publish-jev-20260918` foram enviados ao main remoto com integraçêo das atualizações concorrentes. Os diretórios originais com alterações existentes foram preservados.

ATENÇêO: no ARCKHÉbuscas, o remote `origin` aponta para outro projeto (ARCKHÉPRO). O destino correto é o remote `ARCKHÉbuscas`, upstream `ARCKHÉbuscas/main`. Nêo usar origin nesse repo.

Ao escolher a versêo visual do curso: ler a skill correspondente, gerar HTML a partir de `conteudo/curso.json`, verificar aprendizagem/navegaçêo e publicar Pages. Atualizar a URL do card e migrar enrichment de `curso:linnoff88-dev-jev-curso` para o ID derivado da nova URL, preservando relacionados.

## Verificaçêo da atualizaçêo v1.2.1 — 19/09/2026

Vinte casos validados; 27 testes Python passaram. Teste de navegador passou para os 20 casos, perguntas combinadas, importaçêo/exportaçêo, relatórios, temas, custos e layouts desktop/mobile sem overflow ou erros JavaScript. Build mantém somente app, guia, capa e entrada. Banner dos recursos adicionado ao README. Curso v1.2.0 validado: 12 módulos, 36 aulas e 12 laboratórios; casos sincronizados com o app.

Mantida a identidade visual existente; o detector de estilo apontou avisos de tipografia e tokens, sem impedir os testes funcionais. A geraçêo do banner usou a ferramenta integrada imagegen, com composiçêo de prisma de decisões, fundo escuro e acentos âmbar. Nêo houve inferência Jev real.

### Pushes e disponibilidade confirmados nesta atualizaçêo

- Aplicaçêo e banner: `a0dc4de` em `linnoff88-dev/jev`; Pages concluído com sucesso. Laboratório v1.2.1, guia e banner retornaram HTTP 200.
- Curso: `b37d99a` em `linnoff88-dev/jev-curso`.
- Portal: `000325b` em `NeiMaldaner/portal`.
- Busca: `b93348d` em `linnoff88-dev/ARCKHÉbuscas`.
- PRO: `ef6b5bd` em `linnoff88-dev/ARCKHÉpro`.

Verificações dos catálogos: 7 testes do portal, TypeScript sem erros e 41 testes da base. Acervo final preservado com 18.276 IDs, incluindo 277 cursos, 234 projetos e 12.312 clipes; 100% dos cursos/projetos classificados. Conflito JSON legado resolvido sem descartar classificações. Fontes de clipes sincronizadas antes da verificaçêo final de IDs. Correções registradas no FALHAS.md do PRO. Nenhuma consulta ao Vercel.

Trabalho dos catálogos isolado em `/tmp/jev-update-20260919/` para preservar alterações preexistentes nos diretórios originais.

## Atualizaçêo de 20/09/2026

Jev v1.7.1: funçêo de composiçêo de Score e roteiro de piloto; 56 testes Python passaram e build público concluído. Curso v1.4.0: 18 páginas HTML v2 responderam HTTP 200, com 36 aulas e 72 etapas. Portal, Eventos e catálogos receberam o destino HTML; README em PT/EN/ES atualizado. Nenhuma nova inferência real foi feita nesta atualizaçêo.
