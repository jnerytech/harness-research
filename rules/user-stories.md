# User stories

Como escrever, refinar e implementar a partir de user stories (ou issues no mesmo papel): formato, critérios de aceitação e quebra em fatias. Vale ao criar ou revisar itens de backlog e ao começar a implementar um.

## Princípios

- A story é um lembrete de conversa, não uma especificação. O detalhe que falta se resolve com o usuário, não por suposição.
- Diz **o quê** e **por quê**, nunca o **como**. Decisão de implementação fica fora da story.
- "Pronto" é o que os critérios de aceitação dizem, e nada mais.
- Cada fatia entregue precisa ter valor sozinha para o usuário.

## Regras

### Escrever ou revisar

- Formato: `Como <persona>, quero <objetivo>, para <benefício>`. Se o contexto pesa mais que a persona: `Quando <situação>, quero <motivação>, para <resultado>`.
- Persona concreta (papel e contexto), nunca "usuário" genérico quando há mais de um tipo de usuário.
- Sem benefício claro no "para": a story ainda não está pronta. Pergunte ao usuário em vez de inventar um.
- Uma necessidade por story. Duas ações ou um "e" no objetivo: divida.
- A story termina num objetivo completável ("rever os currículos recebidos"), não numa atividade contínua ("gerenciar anúncios").
- Escreva para um usuário, em voz ativa: "um cliente pode cancelar os próprios pedidos", não "pedidos podem ser cancelados". Isso expõe ambiguidades de posse e permissão.
- Detalhe só o que vai ser implementado em breve; stories distantes ficam grandes e vagas.
- Retire da story detalhe de UI, tecnologia ou implementação; se for restrição real (ex.: requisito legal), mova para os critérios.
- Confira INVEST: independente, negociável, com valor, estimável, pequena, testável. Reporte qual letra falha e a ação (recortar, juntar, escrever critérios, spike).

### Critérios de aceitação

- Escreva cada critério como comportamento observável pelo usuário (situação → ação → resultado), nunca como passo de implementação.
- Cubra o caminho feliz, os casos negativos (entrada inválida, erro, permissão) e os de borda (limites, repetição, vazio).
- Para achar os casos, pergunte: o que está sendo suposto sobre a implementação? Em que situação isto se comporta diferente? O que pode dar errado?
- Liste requisitos não funcionais (desempenho, segurança, acessibilidade) quando a story os toca, com valor mensurável. Requisito que vale para o sistema todo vira restrição separada, com teste automatizado, não critério repetido em cada story.
- Pare de acrescentar critérios quando o próximo não esclarece nada; detalhe miúdo fica para os testes de unidade. Critério genérico ("funciona corretamente") não conta.

### Quebrar

- Story que não cabe numa iteração ou não dá para estimar: corte por, nesta ordem de preferência, passos do fluxo, regras de negócio, variações de dados, operações (CRUD) ou papéis.
- Story grande por incerteza técnica ou de domínio: separe um spike com timebox (investigação descartável) da story de implementação, e faça o spike antes.
- Nunca corte por camada técnica (UI, API, banco): a fatia resultante não entrega valor sozinha.
- Cada fatia nova recebe os próprios critérios de aceitação.
- Itens pequenos demais para planejar sozinhos (ajustes, bugs rápidos): junte numa story com um objetivo comum.

### Implementar a partir de uma story

- Antes de codar, liste os critérios de aceitação que vai usar. Se a story não tem critérios ou eles são vagos, escreva-os e mostre ao usuário antes de implementar.
- Inclua cada critério na lista de testes; todo critério termina com pelo menos um teste automatizado ou com a justificativa de por que não tem.
- Trate sugestão de implementação na story como sugestão, não como requisito; se divergir dela, diga por quê.
- Comportamento não previsto pelos critérios: não implemente por conta própria; registre como pergunta ou story nova.
- Story pronta = todos os critérios atendidos e testados. Com critério pendente, ela não está pronta: reporte o que falta, não "quase pronto".

## Evitar

- Epic disfarçado de story.
- Story sem "para" ou com benefício só para o time ("refatorar X") apresentada como valor de usuário.
- Escrever a story inteira detalhada de antemão, como contrato.
- Critério de aceitação copiado do template, sem relação com a story.
- Priorizar no lugar do usuário. Quando ele pedir ajuda para priorizar, dê custo relativo e risco de cada item e deixe a decisão com ele.

## Quando parar e perguntar

- Critérios de aceitação ausentes ou contraditórios antes de implementar.
- O corte natural da story só sai por camada técnica.
- Um critério exige decisão de produto (texto, regra de negócio, limite numérico) que não está na story.

## Ao terminar

Resuma: critérios de aceitação atendidos (com os testes que os cobrem), critérios sem teste e por quê, tudo o que foi implementado além dos critérios, e perguntas ou stories novas que surgiram.
