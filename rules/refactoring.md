# Refactoring

Como refatorar código: mudar a estrutura interna **sem alterar o comportamento observável**, em passos pequenos, com o código funcionando entre eles.

## Princípios

- **Dois chapéus:** em cada passo, ou se adiciona comportamento ou se refatora. Nunca os dois no mesmo passo.
- A justificativa é econômica: refatore o que torna a mudança atual ou a próxima mais barata, não por estética.
- Clareza primeiro. Otimização só com medição apontando o hotspot.
- Refatore o que a tarefa atual toca ou lê (preparatory / comprehension refactoring), não o repositório inteiro.
- Se o código fica quebrado por horas, não é refactoring: volte e use passos menores.

## Regras

### Antes

- Rode os testes do trecho. Se estiverem vermelhos ou não existirem, pare (ver "Quando parar e perguntar").
- Cobertura fraca: proponha primeiro um characterization test, que fixa o comportamento atual, e só depois refatore. Escreva com valor esperado provisório, rode, troque pelo valor real e injete uma falha temporária para confirmar que o teste fica vermelho.

### Durante

- Um passo pequeno por vez, com o nome do catálogo quando houver (Extract Function, Rename Variable, Move Function, Split Phase, Replace Loop with Pipeline, Introduce Parameter Object...). Rode os testes depois de cada passo.
- Teste vermelho depois de um passo: reverta o passo em vez de depurar e refaça em passos menores.
- Prefira a refatoração automática (LSP/IDE: rename, extract, move) a editar texto com `sed`/regex. Se editar texto, revise o diff de cada arquivo.
- Mover função ou campo: copie para o destino, adapte, valide, transforme o original em delegação e só então remova. Nunca recorte e cole direto em código não trivial.
- Dado de escopo amplo (global, campo muito usado): encapsule atrás de funções antes de renomear ou mover.
- Só reordene instruções (Slide Statements) depois de confirmar que os efeitos colaterais não colidem.
- Extração com muitas variáveis mutadas: antes, simplifique com Split Variable ou Replace Temp with Query.
- Substituir algoritmo: isole o antigo numa função testada e compare as saídas antes de trocar.
- Não altere testes durante um refactoring. Teste que quebra sem mudança de comportamento está acoplado à implementação: aponte o teste, não "conserte" o assert.
- APIs públicas, contratos e schemas: use parallel change (expand → migrar chamadores → contract), nunca troca de assinatura de uma vez. Troca de biblioteca: branch by abstraction.
- Se, durante uma feature, algo precisar de refactoring: termine o passo atual, refatore com os testes verdes e volte para a feature.
- Commits separados: `refactor:` (só estrutura) e `feat:`/`fix:` (comportamento). Um commit de refactoring não muda o resultado de nenhum teste.

### Prioridade

- Priorize o que **muda com frequência e é difícil de ler**: arquivo grande, função longa, lógica duplicada no caminho da tarefa. Smell em código estável que ninguém toca não é prioridade.
- Arquivos muito grandes custam contexto a cada leitura. Quebrar em módulos coesos é uma das refatorações de maior retorno.
- Lógica misturada com I/O: aplique Split Phase / functional core, deixando a decisão pura e testável, com os efeitos colaterais nas bordas.
- Regra de três: na terceira duplicação, extraia.

### Escolhas frequentes

- Bloco que precisa de comentário para ser entendido: Extract Function com o nome do comentário.
- Caso excepcional que impede o resto: guard clause. Dois caminhos igualmente normais: `if/else`.
- `switch` sobre tipo repetido em vários lugares: Replace Conditional with Polymorphism. Se aparece uma vez, deixe.
- Mesmo valor especial (`null`, `"unknown"`) tratado igual em vários lugares: Introduce Special Case.
- Função que retorna valor e altera estado: Separate Query from Modifier.
- Argumento booleano/enum que só escolhe o ramo: Remove Flag Argument (uma função por caso).
- Parâmetros que sempre andam juntos: Introduce Parameter Object ou Preserve Whole Object.
- Valor derivado guardado e sincronizado à mão: Replace Derived Variable with Query.
- Subclasse que recusa a interface do pai ou precisa trocar de tipo em runtime: substitua herança por delegação.

## Evitar

- Juntar refactoring e correção de bug no mesmo passo. Bug encontrado no caminho vira um passo de `fix` separado.
- Deixar o código obscuro em nome de performance sem medição (ex.: juntar laços sem profiler).
- Refatorar código fora do escopo da tarefa por conta própria.
- Flexibilidade especulativa (parâmetro, hook ou abstração "para o futuro"): refatore quando a necessidade aparecer.
- Tratar a lista de smells como checklist ou métrica: smell é heurística, a decisão depende do contexto.
- Polimorfismo ou objeto-comando onde `if/else` ou função simples bastam.
- Comentar código morto "para depois": apague, o histórico guarda.

## Quando parar e perguntar

- Testes do trecho vermelhos ou inexistentes antes de começar.
- Reestruturação grande (vários módulos, mudança de arquitetura, mais de ~10 arquivos): apresente o plano em passos e espere confirmação do usuário.
- Bug encontrado durante o refactoring: avise o usuário antes de tratar.

## Ao terminar

Resuma em poucas linhas: refatorações aplicadas (pelo nome), testes rodados e resultado, testes frágeis ou bugs encontrados.
