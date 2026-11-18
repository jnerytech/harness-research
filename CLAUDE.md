# CLAUDE.md

Repositório de pesquisa sobre **harnesses de agentes de IA** (foco principal) e **engenharia de software** em geral. Não é código de produção: o produto aqui são documentos Markdown bem fundamentados. O `README.md` explica a estrutura e traz os templates.

## Público

Leitor técnico. Não explique o básico. O conteúdo é independente de linguagem e stack: use exemplos de uma tecnologia só quando ilustrarem o ponto, sem assumir uma específica.

## Regras de pesquisa

- **Verifique antes de afirmar.** Harnesses e ferramentas mudam toda semana. Antes de descrever o estado atual de um projeto, consulte a fonte: web, docs ou o próprio código-fonte (clone em `tmp/` na raiz do repo e leia).
- **Fonte primária primeiro:** código, docs oficiais, posts de engenharia dos autores, papers, livros de referência.
- **Toda afirmação sobre ferramenta ou projeto específico leva link e data de consulta** (`AAAA-MM-DD`).
- **Marque o grau de certeza:**
  - **[fato]**: documentado ou visto no código
  - **[inferência]**: dedução a partir de evidências
  - **[opinião]**: avaliação ou recomendação
- Em engenharia de software, separe o que tem evidência do que é dogma ou moda.
- Se não achar evidência, escreva que não achou. Não preencha lacuna com suposição.

## Onde cada coisa vai

| Conteúdo | Caminho |
|---|---|
| Análise de um harness | `harnesses/<nome>.md` (kebab-case: `claude-code.md`, `codex-cli.md`) |
| Padrão transversal de harness | `patterns/<tema>.md` |
| Pesquisa geral de engenharia | `software-engineering/<área>/<tema>.md`, onde área é `architecture`, `testing`, `devops`, `reliability`, `practices` ou `ai-assisted-dev` |
| Referência (paper, livro, post, repo) | `references/<tipo>.md` ou `references/<tema>.md`, com resumo de 2-3 linhas e por que importa |
| Experimento prático | `experiments/AAAA-MM-DD-<tema>.md` |
| Rascunho ou ideia solta | `notes/` |
| Rule do Claude Code derivada da pesquisa | `rules/<tema>.md`, com frontmatter `paths` quando fizer sentido, linkando o doc de origem |

Siga os templates do `README.md` para `harnesses/`, `software-engineering/` e `experiments/`. Se uma seção do template não se aplica, mantenha o título e escreva "n/a" com o motivo. Não remova a seção.

## Escrita

- pt-BR. Termos técnicos ficam em inglês (tool calling, context window, subagent...).
- Todo documento começa com um **TL;DR** de 2-3 linhas.
- Comparações vão em tabela; fluxos e arquiteturas, em Mermaid.
- Seja direto: frases curtas, sem enchimento.
- Os comentários `<!-- path: ... -->` vindos do projeto Claude web indicam onde o arquivo deve ficar. Remova o comentário ao salvar o arquivo no lugar certo.

## Manutenção

- Ao criar ou revisar uma análise em `harnesses/`, atualize a tabela "Harnesses analisados" do `README.md` (status e data).
- Ao citar um padrão que já tem arquivo em `patterns/`, linke o arquivo em vez de repetir o conteúdo.
- Ao criar o primeiro arquivo real de uma pasta, apague o `.gitkeep` dela.

## Repos estudados

- Clone os repositórios que for estudar em `tmp/<nome>` na raiz deste repo (ex.: `git clone --depth 1 https://github.com/openai/codex tmp/codex`).
- `tmp/` está no `.gitignore`: nada ali é versionado. Antes de clonar, confira se o repo já está em `tmp/`; se estiver, rode `git -C tmp/<nome> pull` em vez de clonar de novo.
- Ao citar código de um repo estudado, aponte o link permanente no GitHub (com o hash do commit), não o caminho em `tmp/`.

## Git

- **Trabalhe sempre na `main`.** É um repo de pesquisa: não crie branches nem abra PRs. Faça commit e push direto na `main`.
- Commits pequenos, mensagens em pt-BR no imperativo: `adiciona análise do Aider`, `revisa context-compaction com fontes de nov/2026`.
