# TDD

Como implementar comportamento novo ou corrigir bug guiado por testes: ciclo vermelho → verde → refatore. Vale quando o código tem (ou pode ter) testes automatizados rápidos; spikes e protótipos descartáveis ficam fora.

## Princípios

- Duas regras: nenhum código de produção sem um teste automático falhando que o exija; depois, remova a duplicação.
- Primeiro "funciona", depois "limpo". Nunca os dois no mesmo passo.
- O ritmo importa mais que o ritual: ciclos curtos e regulares, com a suíte rodando a cada passo.
- O design emerge ao remover duplicação. Não crie abstração nem flexibilidade que nenhum teste pediu.

## Regras

### Antes

- Escreva a lista de testes da tarefa (casos normais, limites, erros) antes de codar. Ideias que surgirem no meio vão para o fim da lista, não para o código.
- Comece pelo caso degenerado (entrada mínima, operação que não faz nada).
- Bug: primeiro o menor teste que reproduz o defeito; só depois a correção.
- API ou biblioteca externa que ainda não foi usada no projeto: escreva antes um teste que confirma o comportamento esperado dela.

### Vermelho

- Um teste por vez. Próximo teste = o que ensina algo novo e que dá para fazer passar em poucos minutos.
- Escreva o teste começando pela asserção, com o valor exato esperado (não `!= null`, não `> 0`).
- Rode e veja o teste falhar **pelo motivo esperado** (asserção, não erro de compilação ou import) antes de tocar o código de produção.
- Teste que exige mudanças demais para passar: troque por um teste menor da parte que falha e volte ao maior depois.

### Verde

- Faça a menor mudança que passa. Na fase verde, não edite o teste.
- Sabe a solução simples: escreva-a. Não sabe: retorne uma constante e generalize depois. Inseguro sobre a abstração: só generalize com um segundo exemplo.
- Vermelho inesperado depois de uma implementação direta: reduza o passo (constante, segundo exemplo) em vez de insistir.
- Rode a suíte inteira (ou os testes afetados, em suíte grande) antes de declarar verde.

### Refatore

- Com tudo verde, remova a duplicação, inclusive a entre teste e código (constante hardcoded que só o teste justifica).
- Nunca encerre um ciclo com valor hardcoded que só existe para passar o teste.

### Ao fechar a tarefa

- Teste os pontos de risco do próprio código (condicionais, laços, cálculos, polimorfismo) até não restar caso que preocupe. Não teste código de terceiros.
- Só declare a tarefa pronta com a suíte inteira verde.
- Apague um teste só se ele for redundante tanto em confiança quanto no cenário que comunica ao leitor.

## Evitar

- Escrever vários testes vermelhos de uma vez.
- Escrever o código e depois o teste ajustado a ele, na mesma tarefa em que se pediu TDD.
- Comentar, pular (`skip`) ou enfraquecer asserção para chegar ao verde.
- Abstrair a partir de um único exemplo.
- Ignorar teste com setup longo, lento ou frágil: é sinal de design acoplado. Aponte o problema ao usuário.

## Quando parar e perguntar

- Não dá para escrever a asserção porque o comportamento esperado não está claro.
- O projeto não tem infraestrutura de testes, ou o teste depende de recurso que não sobe no ambiente.
- Fazer o teste passar exige mudança grande fora do escopo da tarefa.

## Ao terminar

Resuma: testes escritos (na ordem), testes que falharam primeiro e por quê, resultado da suíte e itens da lista que ficaram para depois.
