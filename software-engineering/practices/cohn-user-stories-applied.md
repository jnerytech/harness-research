# User Stories Applied (Cohn): papéis, testes de aceitação e planejamento por velocidade

## 1. TL;DR

*User Stories Applied* (Mike Cohn, Addison-Wesley, 2004) é a referência clássica de user stories: 3 Cs, INVEST, modelagem de papéis, quebra em fatias verticais ("slice the cake"), testes de aceitação escritos pelo cliente, estimativa em story points e planejamento por velocidade. Comparado ao livro do Lee ([`lee-crafting-user-stories.md`](lee-crafting-user-stories.md)), Cohn é mais rigoroso em **quem faz o quê** (cliente prioriza e escreve testes, time estima), traz um **catálogo de smells** com remédio e trata requisitos não funcionais como **constraint cards** com teste automático. A evidência empírica é a mesma, e fraca: percepção de praticantes e estudos mistos sobre estimativa.

## 2. Problema que resolve

- Requisito como documento ("o sistema deve...") que vira lista de atributos sem objetivo do usuário e permite construir a coisa errada (cap. 12).
- Tudo escrito para um "usuário" genérico, escondendo papéis com necessidades diferentes (cap. 3).
- Planejamento que promete data fixa sem medir a capacidade real do time (caps. 9, 11).
- "Pronto" decidido por quem implementa, não por quem pediu (cap. 6).

**[fato]** A tese do cap. 13: stories mudam o foco de um **documento compartilhado** para um **entendimento compartilhado**. São compreensíveis por todos, têm o tamanho certo para planejar e permitem adiar o detalhe até ele importar.

## 3. Abordagens e trade-offs

### 3.1 Story, 3 Cs e INVEST (caps. 1, 2, 7)

**[fato]** Template Connextra: `Como <papel>, quero <função>, para <valor>`. Os 3 Cs (Ron Jeffries): *Card* (lembrete de uma linha), *Conversation* (detalhe adiado até importar), *Confirmation* (testes de aceitação no verso do card). O card é "promessa de conversa, nunca contrato". INVEST (Bill Wake) como checklist, com remédio por letra:

| Letra | Falha | Remédio |
|---|---|---|
| I | Não dá para planejar sem arrastar outras | Juntar ou recortar como fatia vertical |
| N | Card superespecificado | Reduzir a lembrete; detalhe decidido vai para os testes |
| V | Só o time se importa | Reformular em valor para usuário/cliente |
| E | Time não consegue dimensionar | Spike (lacuna de domínio/técnica) ou recortar (grande demais) |
| S | Não cabe na iteração | Recortar (composta → CRUD/dados; complexa → spike + story) |
| T | "Rápido", "fácil", "nunca" | Testes de aceitação mensuráveis |

**[fato]** Diretrizes do cap. 7:
- **Goal stories:** comece pelos objetivos de cada papel e derive as stories deles.
- **Closed stories (Lauesen):** a story termina num objetivo completável ("rever currículos dos candidatos"), não numa atividade contínua ("gerenciar anúncios").
- **Size to the horizon:** detalhe o que está perto de ser feito; deixe grande e vago o que está longe.
- **UI o mais tarde possível:** detalhe de tela em story inicial é contaminação.
- Escreva para **um** usuário e em voz ativa: "Um candidato pode remover os **próprios** currículos" expõe a ambiguidade de posse que "candidatos podem remover currículos" esconde.

### 3.2 Modelagem de papéis e coleta (caps. 3-5)

**[fato]** Antes de escrever stories: brainstorm de papéis em cards → organizar espacialmente → consolidar (juntar equivalentes, descartar irrelevantes) → refinar com atributos. Promova 1-2 papéis vitais a **personas** (com pesquisa); use **personagens extremos** com parcimônia para achar stories esquecidas.

**[fato]** Requisitos são **pescados** (*trawling*, Robertson & Robertson), não "capturados": várias passadas, rede grossa primeiro. Técnicas: entrevistas (padrão), observação (revela a necessidade por trás da declarada), workshops com protótipo de baixa fidelidade (maior volume), questionários (só para priorizar o que já existe). Perguntas **abertas e livres de contexto**: "que tipo de desempenho é necessário?", não "a busca precisa ser rápida?".

**[fato]** Usuário real vence proxy sempre. Desenvolvedores costumam ser **piores** proxies que os próprios proxies. Proxies têm viés previsível (gerente amplifica, marketing quer quantidade de features, suporte otimiza para o suporte); combine tipos diferentes para os vieses se cancelarem.

### 3.3 Quebrar e juntar (caps. 2, 7)

**[fato]** *Slice the cake*: cada pedaço atravessa todas as camadas e é usável sozinho. Errado: "preencher formulário de currículo" + "gravar formulário no banco" (nenhum serve sozinho). Certo: "enviar currículo só com dados básicos" + "enviar currículo completo".

| Tipo de epic | Corte |
|---|---|
| **Composto** (várias stories embrulhadas) | Por operação (criar/editar/excluir) ou fronteira de dados |
| **Complexo** (incerteza intrínseca) | **Spike** com timebox + story de desenvolvimento; o spike vai antes |

**[fato]** O oposto também vale: stories pequenas demais (bugs, ajustes de UI) se juntam numa story de meio dia a alguns dias. Bug que leva o tempo de uma story típica é uma story.

### 3.4 Testes de aceitação (caps. 6, 21)

**[fato]** O **cliente** escreve os testes, no verso do card, antes ou durante a codificação; eles definem "pronto". Processo em dois passos: (1) anote lembretes de teste no verso sempre que alguém pensar em um; (2) no início da iteração, transforme-os em testes executáveis. Perguntas para gerar testes:

- O que mais os programadores precisam saber?
- O que estou supondo sobre a implementação?
- Em que circunstâncias isto se comporta diferente?
- O que pode dar errado?

**[fato]** Cubra caminho feliz, borda e falhas explícitas (cartão expirado, acima do limite, usuário sem permissão). Pare quando um teste novo não esclarece nada; detalhes miúdos ficam para os testes de unidade. Escrever os testes **é** conversa de requisitos e pode simplificar o design (cap. 21).

**[fato]** Requisitos não funcionais viram **constraint cards** (Newkirk & Martin): card anotado "Constraint", com teste automático rodando pelo menos diariamente, estimado em 0 pontos porque molda as outras stories.

### 3.5 Estimativa e planejamento (caps. 8-11)

**[fato]** Story points relativos na escala ½, 1, 2, 3, 5, 8, 13, 20, 40, 80. Planning poker com no máximo ~3 rodadas, convergência "razoável". **Triangulação**: compare cada story nova com as já estimadas. Quem faz o trabalho estima; o cliente responde perguntas mas **nunca estima**.

**[fato]** "Custo muda prioridade": o cliente prioriza, mas só com a estimativa na mão; em conflito, o cliente decide. Release: faixa de data alvo → prioridade (valor + risco + estimativa, MoSCoW) → iterações = pontos ÷ velocidade. Comunique "pronto em ~5-7 iterações", nunca uma data fixa. Ajuste escopo, não data nem qualidade.

**[fato]** Velocidade conta só stories **terminadas** (testes de aceitação passando); parcial vale zero, nunca re-estime story feita. Não confie antes de 2-3 iterações. Burndown subindo significa escopo crescendo, não time lento.

**[fato]** Iteração: story → tarefas (desagregação) → um desenvolvedor aceita cada tarefa → estima em horas ideais → soma para detectar excesso de compromisso.

### 3.6 Catálogo de smells (cap. 14)

| Sintoma | Smell | Remédio |
|---|---|---|
| Re-estimativa constante | Stories pequenas demais | Juntar para planejar; quebrar na iteração |
| Não dá para planejar sem cadeias | Interdependência | Juntar ou recortar como fatia vertical |
| Aparecem features não pedidas | **Goldplating** | Visibilidade diária, revisão no fim da iteração |
| Escreve-se mais sobre stories do que se conversa | Detalhe demais | Cards menores; conversar |
| Cards iniciais citam telas e botões | UI cedo demais | Reescrever sem suposição de UI |
| Pedem template e estimativa em horas | Pensando longe demais | Relembrar por que se escolheu stories |
| Cliente não consegue priorizar | Stories grandes ou sem valor | Menores e com valor explícito |

### 3.7 O que stories não são (cap. 12)

**[fato]** Story ≈ um cenário de use case; story + testes de aceitação ≈ um use case, mas menor, menos completo e transitório. Diferente de especificação IEEE 830: foco no **objetivo** do usuário, não numa lista de "o sistema deve". Lista de atributos permite entregar tudo e ainda errar o objetivo.

### 3.8 Divergências com o Lee

| Tema | Cohn (2004) | Lee (2023) | Fica valendo |
|---|---|---|---|
| Quem escreve critérios/testes | Cliente escreve; time ajuda | Colaborativo (Three Amigos) | **[opinião]** Colaborativo, com o cliente aprovando |
| Corte por UI/papel | UI o mais tarde possível; smell "UI cedo demais" | Último eixo de corte, mas aceito | **[opinião]** Cohn: evitar corte por tela |
| Escala de pontos | ½ a 80 | Fibonacci 1-13 | Indiferente; o que importa é ser relativa |
| Cliente estima? | Nunca | Não trata | Cohn |
| Priorização | MoSCoW + valor/risco + custo | MoSCoW, WSJF, RICE, Kano | **[opinião]** Cohn acrescenta o essencial: custo muda prioridade |
| Spike | Explícito para epic complexo | Mencionado em INVEST | Cohn |
| Requisito não funcional | Constraint card com teste automático | Critério de aceitação quando se aplica | **[opinião]** Os dois: critério na story e constraint global com teste |

## 4. Evidência

- **Eficácia de user stories e planning poker:** ver a seção 4 de [`lee-crafting-user-stories.md`](lee-crafting-user-stories.md). Em resumo: a eficácia medida é **percebida** (Lucassen et al., REFSQ 2016), e planning poker não supera consistentemente estimativas individuais de especialistas (Moløkken-Østvold et al., 2008).
- **Velocidade como previsão:** **[fato]** não achei estudo que valide a regra das 2-3 iterações ou a precisão de previsão por velocidade. É heurística de praticante.
- **Clientes escrevendo testes de aceitação (FIT/FitNesse):** **[fato]** não achei evidência de adoção sustentada de clientes escrevendo testes executáveis; o FitNesse perdeu tração e a prática migrou para BDD/Specification by Example, em geral com testes escritos por devs/QA a partir de exemplos do negócio. **[inferência]** O princípio (exemplos concretos definem "pronto") sobreviveu; o mecanismo (cliente escreve o teste) não.
- **Goldplating:** **[fato]** não achei medição. **[inferência]** Para agentes de código, gerar mais do que foi pedido é um comportamento recorrente em relatos de uso; a regra contra goldplating é mais relevante para agentes do que para humanos.

## 5. Aplicação prática

### 5.1 Em times

- Faça a modelagem de papéis (uma hora) antes do primeiro workshop de stories.
- Use o catálogo de smells nas retrospectivas de refinamento.
- Requisitos não funcionais: constraint com teste automático, não só um parágrafo no wiki.
- Prometa faixa de iterações, não data.

### 5.2 Harnesses e agentes de IA

- **[opinião]** **As perguntas de Cohn viram checklist do agente** antes de implementar: o que estou supondo sobre a implementação? quando isto se comporta diferente? o que pode dar errado? As respostas viram critérios de aceitação propostos ao usuário.
- **[opinião]** **Goldplating é o smell número um de agente.** O remédio de Cohn (visibilidade e revisão) mapeia para: o agente reporta tudo o que implementou além dos critérios, para o usuário aceitar ou remover.
- **[opinião]** **"Pronto" = testes de aceitação passando, parcial vale zero.** O agente não declara a story feita com critério pendente; reporta o que falta.
- **[opinião]** **Spike antes de story complexa.** Diante de incerteza técnica, o agente faz uma investigação com timebox (código descartável, sem commit na branch da story) e só depois implementa.
- **[opinião]** **Custo muda prioridade.** Quando o usuário pede para escolher entre itens, o agente dá a estimativa relativa e o risco de cada um e deixa a decisão com ele.
- **[opinião]** **Escrever para um usuário, em voz ativa,** é uma checagem barata que o agente pode aplicar ao revisar issues: expõe ambiguidades de posse e permissão.

## 6. Fontes

- Cohn, M. *User Stories Applied: For Agile Software Development*. Addison-Wesley, 2004. ISBN 9780321205681.
- Base de conhecimento extraída do livro (skill `cohn-user-stories`, 22 arquivos de capítulo, glossário, padrões e cheatsheet). Consultada em 2026-11-18.
- Lucassen, G. et al. "The Use and Effectiveness of User Stories in Practice". REFSQ 2016: https://webspace.science.uu.nl/~dalpi001/papers/luca-dalp-werf-brin-16-refsq.pdf. Consultado em 2026-11-18.
- Moløkken-Østvold, K.; Haugen, N. C.; Benestad, H. C. "Using planning poker for combining expert estimates in software projects". *Journal of Systems and Software*, 2008: https://web-backend.simula.no/sites/default/files/publications/Molokken-ostvold.2006.2.pdf. Consultado em 2026-11-18.
