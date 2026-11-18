# Criação de rules

Como criar ou revisar arquivos em `rules/` deste repositório: rules do Claude Code derivadas da pesquisa, feitas para serem copiadas ou linkadas em `.claude/rules/` de outros projetos ou em `~/.claude/rules/`.

## Princípios

- Uma rule é instrução para um agente, não documento de pesquisa. Cada linha custa contexto sempre que ela carrega.
- A rule precisa funcionar sozinha, em qualquer projeto, sem este repositório por perto.
- Regra que não dá para verificar não muda comportamento.

## Regras

### Formato

Todo arquivo segue esta estrutura, nesta ordem:

````markdown
---
paths:            # opcional: omita para a rule carregar sempre
  - "<glob>"
---

# <Tema>

<Escopo em 1-2 linhas: o que a rule cobre e quando vale.>

## Princípios
<2-5 bullets: o critério que orienta decisões que as regras não cobrem.>

## Regras
<Bullets imperativos e verificáveis. Com mais de ~10, agrupe em ### por momento ou assunto.>

## Evitar
<Anti-patterns concretos.>

## Quando parar e perguntar    (opcional)
<Situações em que o agente pede confirmação ao usuário antes de seguir.>

## Ao terminar                 (opcional)
<O que o agente reporta no fim.>
````

- Seções obrigatórias: título com escopo, `Princípios`, `Regras`, `Evitar`. As opcionais entram só quando têm conteúdo.
- O único campo de frontmatter é `paths`. Não invente outros.

### Conteúdo

- **Um tema por arquivo**, nome em kebab-case (`testing.md`, `api-design.md`).
- **Autocontida:** sem links para docs do repo, sem seção de fontes, sem "Baseado em", sem marcação [fato]/[inferência]/[opinião]. A rastreabilidade fica no doc de pesquisa de origem.
- **Sem TL;DR:** a linha de escopo cumpre esse papel.
- **Imperativa e verificável:** "um Act por teste", não "escreva testes limpos". Quando a regra não for óbvia, dê o porquê em meia linha.
- **Impessoal:** fale com o agente. Refira-se a quem o usa como "o usuário", nunca "eu" ou "me".
- **Independente de stack:** sem linguagem, framework ou ferramenta específicos, exceto como exemplo entre parênteses.
- **Curta:** alvo de até ~100 linhas.

### `paths`

- Use só quando o tema se restringe a certos arquivos (ex.: testes). Cubra os padrões de nome comuns a várias linguagens.
- Rules de processo (ex.: refactoring) ficam sem `paths`.

### Processo

1. Leia as rules existentes em `rules/` e confira se o tema já está coberto. Se estiver, revise a rule existente em vez de criar outra.
2. Parta do doc de pesquisa correspondente em `software-engineering/` ou `patterns/`. Se não existir, crie o doc antes da rule.
3. Escreva a rule no formato acima.
4. Confira a lista de "Evitar" abaixo.
5. Commit na `main` com mensagem no imperativo (ex.: `adiciona rule de api-design`).

## Evitar

- Duplicar uma regra que já existe em outra rule. Cada regra vive em um só arquivo, e regras contraditórias fazem o agente escolher uma arbitrariamente.
- Copiar a explicação do doc de pesquisa. A rule leva só a instrução e, quando necessário, o porquê em meia linha.
- Instrução vaga ("seja cuidadoso", "siga boas práticas").
- Referências ao autor do repo, ao seu stack ou a projetos específicos.

## Quando parar e perguntar

- O tema contradiz uma rule existente.
- A rule precisaria passar de ~150 linhas: proponha dividir em mais de um arquivo.
