# User stories (Lee): escrita, critérios de aceitação, priorização e corte

## 1. TL;DR

*The Art of Crafting User Stories* (Christopher Lee, Packt, 2023) é um guia prático que junta as ferramentas usuais de user stories: o template persona/ação/benefício, os 3 Cs, INVEST, critérios de aceitação com casos negativos e de borda, priorização (MoSCoW, WSJF, RICE, Kano), estimativa relativa e quebra em fatias verticais. A tese central, reforçada nas entrevistas do cap. 8, é que a story é **um lembrete de conversa, não uma especificação**. A evidência empírica é fraca: percepção de praticantes, frameworks de qualidade sem validação de resultado e estudos mistos sobre planning poker. Para agentes, o que mais rende é tratar os critérios de aceitação como contrato verificável e quebrar o trabalho em fatias verticais. O livro clássico do tema, com as divergências em relação a este, está em [`cohn-user-stories-applied.md`](cohn-user-stories-applied.md).

## 2. Problema que resolve

- Requisito escrito como contrato fechado, por um intermediário, longe de quem implementa: gera "vocês deviam saber" / "como íamos saber".
- Itens de backlog grandes demais para estimar ou entregar numa iteração.
- "Pronto" indefinido: sem condição observável de término.
- Backlog sem ordem: o de menor valor sai primeiro.
- Funcionalidade construída e pouco usada. **[fato]** O livro cita (via Lyssa Adkins) que ~70% das funcionalidades são raramente ou nunca usadas; o número vem de relatos do Standish Group e é contestado (ver seção 4).

## 3. Abordagens e trade-offs

### 3.1 Anatomia e template (caps. 1, 3)

**[fato]** Toda story tem três partes: **persona** (quem), **ação** (o quê, em voz ativa, sem o como) e **benefício** (por quê). Template: `Como <persona>, quero <objetivo>, para <benefício>`.

| Sinal | Diagnóstico |
|---|---|
| Não dá para escrever o "para" | Não está centrada no usuário; reescreva |
| Cheia de detalhe de UI ou técnico | Perdeu o usuário; recomece pela persona |
| Mais de uma necessidade | Divida |

**[fato]** Variante: **job story** (`Quando <situação>, quero <motivação>, para <resultado>`), útil quando o contexto e a motivação importam mais que a persona. Vem do Jobs-to-be-Done (Christensen; Klement, Moesta).

### 3.2 Os 3 Cs e o "lembrete de conversa" (caps. 3, 8)

**[fato]** *Card* (o texto curto), *Conversation* (a discussão que gera o detalhe), *Confirmation* (os critérios de aceitação). No cap. 8, Bob Galen propõe que a story entre na iteração com só ~30-40% de definição e chegue a 100% ao final; o número não é medido, o ponto é "curta no início". **[inferência]** Isso pressupõe um cliente disponível para a conversa. Sem ele, a parte implícita da story vira suposição de quem implementa.

### 3.3 INVEST (caps. 1, 9)

| Letra | Passa se | Falha → ação |
|---|---|---|
| **I**ndependent | Entregável sozinha | Depende de outra → junte ou recorte |
| **N**egotiable | Escopo pode flexionar | Parece spec fechada → tire detalhe, deixe intenção |
| **V**aluable | Benefício claro ao usuário | Só beneficia o time → reformule para o usuário |
| **E**stimable | O time consegue dimensionar | Vaga ou grande → recorte ou faça um spike |
| **S**mall | Cabe numa iteração | É um epic → recorte |
| **T**estable | Tem critérios de aceitação | Sem condição de "pronto" → escreva os critérios |

**[fato]** As falhas mais comuns são Small e Testable.

### 3.4 Critérios de aceitação (caps. 3, 8)

**[fato]** Condições específicas, mensuráveis e observáveis pelo usuário, não de implementação. Incluem **casos negativos** (entrada inválida, erro) e **de borda** (limites válidos e incomuns). Exemplo do livro, reset de senha: e-mail inválido, e-mail sem conta, conta não verificada, conta bloqueada, senha fraca, tentativas repetidas, redirecionamento após sucesso. Requisitos não funcionais (desempenho, segurança, escala) entram quando se aplicam, para não serem esquecidos.

**[fato]** Galen: critérios orientados a valor dizem ao testador onde está o risco (teste baseado em risco). Anti-pattern: critério de checklist, que existe só porque o template pede, ou critérios demais, que escondem o que importa.

**[inferência]** Critérios observáveis no formato situação → ação → resultado são, na prática, os mesmos exemplos do Given/When/Then do Specification by Example; o livro não aprofunda a automação.

### 3.5 Quebra de stories (caps. 6, 7)

Eixos de corte, em ordem aproximada de preferência:

1. Passos do fluxo do usuário
2. Regras de negócio / condições
3. Variações de dados (tipos, faixas, locales)
4. Operações (CRUD)
5. Componentes de UI ou papéis de usuário

**[fato]** Cada fatia deve continuar entregando valor sozinha. **Nunca corte por camada técnica** (UI, API, banco). Story mapping (jornada no eixo horizontal × prioridade no vertical) mostra o MVP como a primeira linha que atravessa a jornada.

### 3.6 Priorização (cap. 4)

| Técnica | Quando | Regra |
|---|---|---|
| **MoSCoW** | Alinhar stakeholders rápido | Must / Should / Could / Won't |
| **WSJF** | Maximizar valor econômico | (valor + habilitação + urgência + redução de risco) / tamanho |
| **RICE** | Impacto e alcance decidem | Alcance × Impacto × Confiança / Esforço |
| **Kano** | Satisfação e diferenciação | Básicos primeiro; *delighters* viram básicos com o tempo |

**[fato]** O livro diz que no RICE os scores "são multiplicados"; a forma canônica da Intercom divide pelo esforço. **[fato]** No Kano, o livro põe *delight* como maior prioridade, mas também diz que o básico precisa existir; a leitura coerente é: básicos são pré-requisito, e entre o resto, *delighters* diferenciam.

### 3.7 Estimativa e prontidão (caps. 4, 6)

**[fato]** Story points (esforço relativo, Fibonacci), planning poker (voto privado, revelação simultânea, discussão dos extremos), velocidade confiável após ~3 iterações. Dispersão grande no voto indica desacordo de escopo escondido. **Definition of Ready**: INVEST, critérios escritos, dependências conhecidas, estimada. **Definition of Done**: codificada, testada, revisada, critérios atendidos. Refinamento contínuo com ~5-10% da capacidade do time.

### 3.8 Colaboração (caps. 5, 8)

**[fato]** Stories escritas a várias mãos: Three Amigos (produto, dev, teste; arquitetura/DevOps quando cabe). Mapa poder × interesse e modelo de saliência para decidir quem envolver. Anti-pattern: time como "tirador de pedido".

### 3.9 Anti-patterns

| Anti-pattern | Efeito |
|---|---|
| Epic escrito como story | Não cabe, não estima |
| Sem "para" | Ninguém sabe o valor |
| Sem critérios de aceitação | "Pronto" indefinido |
| Detalhe de implementação na story | Tira a decisão de design de quem implementa |
| Escrever demais no início | Bloqueia a conversa; herança de cascata |
| Escrita isolada (PO ou dev sozinho) | Desalinhamento |
| Corte por camada técnica | Fatias sem valor isolado |
| Persona baseada em estereótipo | Exclui usuários reais |

## 4. Evidência

- **Eficácia de user stories:** **[fato]** Lucassen, Dalpiaz, van der Werf e Brinkkemper (REFSQ 2016) observaram que não havia evidência científica sobre eficácia até então e fizeram survey com 182 praticantes e 21 entrevistas. O resultado é **percepção** de produtividade e qualidade, não medição. Não achei estudo que compare user stories com outras formas de requisito em resultado de projeto.
- **Qualidade de stories:** **[fato]** O framework QUS (Lucassen et al., 13 critérios sintáticos, semânticos e pragmáticos) é a régua mais usada na pesquisa; mede a qualidade do texto, não o efeito no software.
- **Planning poker:** **[fato]** Moløkken-Østvold, Haugen e Benestad (JSS, 2008) acharam precisão semelhante entre especialistas individuais e equipes com planning poker; outro estudo industrial dos mesmos autores viu planning poker pior em tarefas pouco familiares. A vantagem de precisão é inconsistente; o ganho mais defensável é revelar desacordo de escopo.
- **"70% das funcionalidades raramente usadas":** **[inferência]** o número parece vir de uma apresentação do Standish Group (Jim Johnson, XP 2002: 45% nunca usadas + 19% raramente), com amostra pequena e metodologia não publicada; citado de memória, não verifiquei a fonte original. Não use como dado.
- **WSJF, RICE, Kano, MoSCoW:** **[fato]** não achei estudo que compare as técnicas em resultado. São heurísticas de decisão.
- **Stories geradas por LLM:** **[fato]** "Can LLMs Generate User Stories and Assess Their Quality?" (arXiv 2507.15157, 2025) relata stories de LLM com boa qualidade sintática e de estilo, mas que atendem critérios de aceitação de forma menos consistente que as humanas; como avaliadores, alguns LLMs igualaram a concordância entre humanos usando um codebook detalhado. Um estudo com estudantes (arXiv 2502.02675) relata que LLMs ajudam a escrever critérios de aceitação, mas os estudantes acertam melhor o **escopo** sem LLM. Li só os resumos.

## 5. Aplicação prática

### 5.1 Em times

- Use INVEST como filtro de entrada no refinamento e DoR como portão da iteração.
- Exija casos negativos e de borda nos critérios; eles viram os testes de aceitação.
- Corte por fluxo ou regra de negócio, nunca por camada.
- Escolha uma técnica de priorização e mantenha; a consistência importa mais que a técnica.

### 5.2 Harnesses e agentes de IA

- **[opinião]** **A issue é o card; o usuário é a conversa.** O agente recebe stories com 30-40% de definição e não tem o cliente na sala o tempo todo. Antes de implementar, ele deve escrever os critérios de aceitação que vai usar e mostrar ao usuário quando a story não os tem. É o "Confirmation" feito explicitamente.
- **[opinião]** **Critérios → testes.** Cada critério vira pelo menos um teste; o agente reporta ao final quais critérios estão cobertos e quais não. Isso dá ao harness um critério de pronto verificável, melhor que "os testes passaram".
- **[opinião]** **Quebra vertical combate o diff gigante.** Fatias por fluxo/regra produzem PRs pequenos e revisáveis; cortes por camada produzem PRs que só funcionam juntos.
- **[opinião]** **Agente escrevendo stories:** a evidência sugere que LLMs escrevem stories bem formatadas mas erram escopo e cobertura de critérios. Use o agente para rascunhar e checar INVEST/QUS, não para fechar o escopo sozinho.
- **[opinião]** **"O quê, não como"** vale nos dois sentidos: o agente não deve tratar uma sugestão de implementação na story como requisito, nem acrescentar detalhe técnico ao reescrever uma story.

## 6. Fontes

- Lee, C. *The Art of Crafting User Stories*. Packt, 2023.
- Base de conhecimento extraída do livro (skill `crafting-user-stories`, 9 capítulos, glossário, padrões e cheatsheet). Consultada em 2026-11-18.
- Lucassen, G.; Dalpiaz, F.; van der Werf, J. M.; Brinkkemper, S. "The Use and Effectiveness of User Stories in Practice". REFSQ 2016, LNCS 9619, p. 205-222: https://webspace.science.uu.nl/~dalpi001/papers/luca-dalp-werf-brin-16-refsq.pdf. Consultado em 2026-11-18.
- Moløkken-Østvold, K.; Haugen, N. C.; Benestad, H. C. "Using planning poker for combining expert estimates in software projects". *Journal of Systems and Software*, 2008: https://web-backend.simula.no/sites/default/files/publications/Molokken-ostvold.2006.2.pdf. Consultado em 2026-11-18.
- "Can LLMs Generate User Stories and Assess Their Quality?" (arXiv 2507.15157, 2025): https://arxiv.org/pdf/2507.15157. Consultado em 2026-11-18 (resumo).
- "Exploring LLMs Impact on Student-Created User Stories and Acceptance Testing in Software Development" (arXiv 2502.02675, 2025): https://arxiv.org/pdf/2502.02675. Consultado em 2026-11-18 (resumo).
