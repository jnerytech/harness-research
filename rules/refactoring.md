# Refactoring discipline

Baseado em *Refactoring*, 2ª ed. (Fowler, 2018). Refactoring = mudar a estrutura interna **sem alterar o comportamento observável**, em passos pequenos, com o código funcionando entre eles.

## Duas hats: nunca misture

- Em cada passo, ou você está **adicionando comportamento** ou **refatorando**. Nunca os dois no mesmo passo.
- Se, durante uma feature, aparecer algo que precisa ser refatorado: termine o passo atual, refatore com os testes verdes e volte para a feature.
- Commits separados: `refactor: ...` (só estrutura) e `feat:`/`fix:` (comportamento). Um commit de refactoring não muda o resultado de nenhum teste.

## Antes de refatorar

1. Rode os testes relevantes. Se estiverem vermelhos ou não existirem, **pare e me avise** antes de mexer na estrutura.
2. Se a cobertura do trecho for fraca, proponha primeiro um characterization test (fixa o comportamento atual) e só depois refatore.
3. Refatore apenas código que a tarefa atual toca ou lê (preparatory / comprehension refactoring). Não saia refatorando o repositório por conta própria.

## Durante

- Um passo pequeno por vez, com o nome do catálogo quando houver (Extract Function, Rename Variable, Move Function, Split Phase, Replace Loop with Pipeline, Introduce Parameter Object...). Rode os testes depois de cada passo.
- Prefira a refatoração automática (LSP/IDE: rename, extract, move) a editar texto com `sed`/regex. Se precisar editar texto, revise o diff de cada arquivo.
- **Não altere testes durante um refactoring.** Se um teste quebrar sem mudança de comportamento, ele está acoplado à implementação: pare e aponte o teste, não "conserte" o assert.
- APIs públicas, contratos e schemas: use parallel change (expand → migrar chamadores → contract), nunca uma troca de assinatura de uma vez.
- Performance não é justificativa para deixar o código obscuro sem medição. Clareza primeiro; otimização só com profiler apontando o hotspot.

## Quando vale a pena

- Priorize o que **muda com frequência e é difícil de ler** (arquivo grande, função longa, lógica duplicada no caminho da tarefa). Smell em código estável que ninguém toca não é prioridade.
- Arquivos muito grandes custam contexto a cada leitura. Quebrar em módulos coesos é uma das refatorações de maior retorno.
- Lógica misturada com I/O: aplique Split Phase / functional core, deixando a decisão pura e testável, com os efeitos colaterais nas bordas.
- Regra de três: na terceira duplicação, extraia.

## Limites

- Reestruturação grande (vários módulos, mudança de arquitetura, mais de ~10 arquivos): apresente o plano em passos e espere meu ok antes de começar.
- Nunca junte um refactoring a uma correção de bug sem avisar. Se achar um bug no caminho, registre-o e trate em um passo de `fix` separado.

## Ao terminar

Resuma em poucas linhas: quais refatorações aplicou (pelo nome), os testes rodados e o resultado, e qualquer teste frágil ou bug encontrado.
