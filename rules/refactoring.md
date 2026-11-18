# Refactoring

Como refatorar código: mudar a estrutura interna **sem alterar o comportamento observável**, em passos pequenos, com o código funcionando entre eles.

## Princípios

- **Dois chapéus:** em cada passo, ou se adiciona comportamento ou se refatora. Nunca os dois no mesmo passo.
- Clareza primeiro. Otimização só com medição apontando o hotspot.
- Refatore o que a tarefa atual toca ou lê (preparatory / comprehension refactoring), não o repositório inteiro.

## Regras

### Antes

- Rode os testes do trecho. Se estiverem vermelhos ou não existirem, pare (ver "Quando parar e perguntar").
- Cobertura fraca: proponha primeiro um characterization test, que fixa o comportamento atual, e só depois refatore.

### Durante

- Um passo pequeno por vez, com o nome do catálogo quando houver (Extract Function, Rename Variable, Move Function, Split Phase, Replace Loop with Pipeline, Introduce Parameter Object...). Rode os testes depois de cada passo.
- Prefira a refatoração automática (LSP/IDE: rename, extract, move) a editar texto com `sed`/regex. Se editar texto, revise o diff de cada arquivo.
- Não altere testes durante um refactoring. Teste que quebra sem mudança de comportamento está acoplado à implementação: aponte o teste, não "conserte" o assert.
- APIs públicas, contratos e schemas: use parallel change (expand → migrar chamadores → contract), nunca troca de assinatura de uma vez.
- Se, durante uma feature, algo precisar de refactoring: termine o passo atual, refatore com os testes verdes e volte para a feature.
- Commits separados: `refactor:` (só estrutura) e `feat:`/`fix:` (comportamento). Um commit de refactoring não muda o resultado de nenhum teste.

### Prioridade

- Priorize o que **muda com frequência e é difícil de ler**: arquivo grande, função longa, lógica duplicada no caminho da tarefa. Smell em código estável que ninguém toca não é prioridade.
- Arquivos muito grandes custam contexto a cada leitura. Quebrar em módulos coesos é uma das refatorações de maior retorno.
- Lógica misturada com I/O: aplique Split Phase / functional core, deixando a decisão pura e testável, com os efeitos colaterais nas bordas.
- Regra de três: na terceira duplicação, extraia.

## Evitar

- Juntar refactoring e correção de bug no mesmo passo. Bug encontrado no caminho vira um passo de `fix` separado.
- Deixar o código obscuro em nome de performance sem medição.
- Refatorar código fora do escopo da tarefa por conta própria.

## Quando parar e perguntar

- Testes do trecho vermelhos ou inexistentes antes de começar.
- Reestruturação grande (vários módulos, mudança de arquitetura, mais de ~10 arquivos): apresente o plano em passos e espere confirmação do usuário.
- Bug encontrado durante o refactoring: avise o usuário antes de tratar.

## Ao terminar

Resuma em poucas linhas: refatorações aplicadas (pelo nome), testes rodados e resultado, testes frágeis ou bugs encontrados.
