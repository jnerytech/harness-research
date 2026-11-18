# Código legado

Como alterar código sem testes (ou cujos testes não cobrem o trecho a mudar): primeiro criar a rede de segurança, depois mudar.

## Princípios

- Legado é código sem testes, por melhor que esteja escrito. Mude sob testes (cover and modify), não editando com cuidado e torcendo.
- O risco está no comportamento que deve ser **preservado**, não na linha que muda.
- Para pôr o primeiro teste, mexa o mínimo possível no código. A limpeza vem depois, com os testes no lugar.
- O que o sistema faz hoje vale mais que o que a especificação diz que ele deveria fazer.

## Regras

### Sequência

- Para cada mudança: (1) identifique os pontos de mudança; (2) ache onde dá para observar o efeito; (3) quebre só as dependências que impedem o teste; (4) escreva os testes; (5) faça a mudança e refatore.
- Antes de usar um método que não tem testes, escreva os testes dele.
- Para entender um trecho obscuro, refatore livremente numa cópia descartável (branch/worktree temporário) e jogue fora. Nunca faça commit desse resultado.

### Characterization tests

- Fixe o comportamento atual: escreva uma asserção que vai falhar, rode, leia o valor real na falha e troque a asserção por ele. Repita até cobrir o que vai mudar.
- Confirme que os testes passam pelas linhas que vão mudar, inclusive branches e conversões de tipo (cobertura do trecho, debugger ou variável de sensing). Teste que passa sem exercitar o trecho não protege nada.
- Comportamento surpreendente ou aparentemente errado: mantenha o teste com o valor real, marque como suspeito e avise o usuário. Não corrija sem saber quem depende dele.
- Muitas classes envolvidas: comece por um teste no ponto estreito por onde passam os efeitos e desça para testes unitários depois.

### Quebrar dependências

- Identifique o motivo: não dá para **ver** o efeito → fake que registra o efeito; não dá para **rodar** o código → extraia interface ou injete a dependência.
- Prefira seams de objeto: parâmetro no construtor ou no método, extrair interface, extrair e sobrescrever a chamada/factory num subtipo de teste.
- Sem testes, edite com uma meta por vez e copie assinaturas inteiras em vez de redigitar.
- Em linguagem tipada, use o compilador/type checker para achar todos os pontos afetados (quebre a declaração de propósito e trate cada erro). Não confie nisso em hierarquias com override ou variáveis sombreadas.
- Singleton ou global: um setter estático para o teste é aceitável como passo provisório; registre que o objetivo é eliminar o global.
- Vontade de testar um método privado: extraia a lógica para uma classe nova em vez de afrouxar o acesso.

### Adicionar comportamento sem conseguir testar a classe

- Lógica nova como passo dentro de um método existente: crie um método ou classe nova, com testes, e só chame-a do código antigo (sprout).
- Comportamento antes ou depois do existente: envolva o método ou a classe antiga (wrap/decorator) em vez de inserir código no meio.
- Avise o usuário que o ponto de chamada continua sem teste.

## Evitar

- Mudar código legado sem nenhum teste cobrindo o trecho, contando só com revisão cuidadosa.
- Inserir a mesma lógica inline em vários pontos para ganhar tempo.
- Acrescentar mais uma responsabilidade numa classe que já é grande só porque os dados estão lá.
- Reescrever o módulo inteiro ou refatorar tudo de uma vez em vez de avançar por partes.
- Escrever os testes do legado a partir da especificação em vez do comportamento real.
- Corrigir bugs encontrados fora do escopo da tarefa.
- Seams de linkagem ou pré-processador quando existe seam de objeto.

## Quando parar e perguntar

- Characterization test revelou comportamento que parece bug.
- Quebrar a dependência exige mudar API pública, assinatura usada em muitos lugares ou mais de ~10 arquivos.
- Não há como observar o efeito da mudança sem infraestrutura que não existe no ambiente (banco, serviço externo).

## Ao terminar

Resuma: dependências quebradas (pela técnica), characterization tests criados e o que fixam, comportamentos suspeitos encontrados e trechos alterados que continuam sem teste.
