# harness-research

Pesquisa sobre **harnesses de agentes de IA** e **engenharia de software** em geral.

Um *harness* é tudo o que envolve o modelo e o transforma em agente: o agent loop, o tool calling, a gestão de contexto, a memória, os subagentes, as permissões, o sandboxing, os hooks e a avaliação. Este repositório compara harnesses existentes, documenta padrões e trade-offs e serve de base para construir um harness próprio.

A segunda frente cobre engenharia de software mais ampla: arquitetura, testes, DevOps, confiabilidade, práticas, e como a IA muda o jeito de construir software. As duas frentes se cruzam o tempo todo, porque um bom harness é, antes de tudo, engenharia de software bem feita.

## Estrutura

```
harnesses/              uma análise por harness (Claude Code, Codex CLI, Aider, OpenHands...)
patterns/               padrões transversais de harness (context-compaction, subagent-delegation...)
software-engineering/
  architecture/
  testing/
  devops/
  reliability/
  practices/
  ai-assisted-dev/
references/             papers, livros, posts e repos, com resumo e por que importam
experiments/            testes práticos: AAAA-MM-DD-<tema>.md (hipótese, setup, resultado)
notes/                  rascunhos e ideias soltas
rules/                  rules do Claude Code prontas para copiar ou linkar em .claude/rules/
```

## Harnesses analisados

| Harness | Status | Última revisão |
|---|---|---|
| _(nenhum ainda)_ | | |

Status: `pendente` · `em andamento` · `revisado`

## Pesquisas de engenharia

| Doc | Área | Rule derivada | Última revisão |
|---|---|---|---|
| [Unit Testing (Khorikov)](software-engineering/testing/khorikov-unit-testing.md) | testing | [`rules/testing.md`](rules/testing.md) | 2026-11-18 |
| [TDD (Beck)](software-engineering/testing/beck-tdd.md) | testing | [`rules/tdd.md`](rules/tdd.md) | 2026-11-18 |
| [Refactoring, 2ª ed. (Fowler)](software-engineering/practices/fowler-refactoring-2ed.md) | practices | [`rules/refactoring.md`](rules/refactoring.md) | 2026-11-18 |

## Convenções

- **Fontes:** toda afirmação sobre ferramenta ou projeto específico leva link e data de consulta. Esses projetos mudam rápido; sem data, a informação não vale.
- **Grau de certeza:** o texto marca o que é **[fato]** (documentado ou visto no código), **[inferência]** (dedução a partir de evidências) e **[opinião]** (avaliação pessoal).
- **Idioma:** pt-BR. Termos técnicos ficam em inglês.
- **Formato:** todo documento começa com um TL;DR. Comparações vão em tabela e fluxos em Mermaid.

## Templates

### `harnesses/<nome>.md`

1. Resumo: o que é, quem mantém, licença, linguagem, versão consultada e data
2. Agent loop: como decide, chama tools e termina
3. Tools: quais são nativas e como se estende (MCP, plugins)
4. Contexto e memória: janela, compaction, memória persistente
5. Multiagente: subagentes, delegação, paralelismo
6. Segurança: permissões, sandbox, aprovação humana
7. Extensibilidade: hooks, skills, config
8. Pontos fortes, pontos fracos e ideias para roubar
9. Fontes

### `software-engineering/<área>/<tema>.md`

1. TL;DR
2. Problema que resolve
3. Abordagens e trade-offs
4. Evidência: o que sustenta e o que é controverso
5. Aplicação prática, inclusive em harnesses e agentes
6. Fontes

### `rules/<tema>.md`

Rules do Claude Code derivadas da pesquisa, para copiar ou linkar em `.claude/rules/` (projeto) ou `~/.claude/rules/` (usuário). O formato e as convenções estão em [`.claude/rules/rules-authoring.md`](.claude/rules/rules-authoring.md), que o Claude Code carrega ao trabalhar neste repo.

### `experiments/AAAA-MM-DD-<tema>.md`

1. Hipótese
2. Setup: ferramenta, versão, modelo, configuração
3. Execução
4. Resultado
5. Conclusão e próximos passos

## Fluxo de trabalho

A pesquisa acontece em um projeto do Claude. O conteúdo que vale guardar sai em Markdown, com o caminho sugerido em um comentário na primeira linha (`<!-- path: harnesses/claude-code.md -->`), e depois é commitado aqui.
