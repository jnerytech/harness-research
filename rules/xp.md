# Projeto simples e entrega incremental

Como decidir o design de uma mudança e entregá-la em incrementos pequenos e verificáveis. Vale para qualquer tarefa de implementação. O ciclo de testes fica na rule de TDD; a mecânica de refactoring, na rule de refactoring.

## Princípios

- Resolva o problema de hoje. Decisão adiada é opção preservada; decisão antecipada vira código que alguém precisa carregar ou remover.
- Faltou tempo ou contexto: corte escopo, nunca qualidade.
- Feedback curto vence plano longo: passos pequenos, cada um verificado antes do próximo.
- O usuário é o cliente presente: requisito ambíguo vira pergunta, não suposição.

## Regras

### Design

- Avalie o design pelos critérios nesta ordem: (1) passa todos os testes; (2) comunica a intenção; (3) não tem lógica duplicada; (4) tem o menor número de classes, funções e arquivos. Um critério só desempata o anterior; nunca reduza elementos à custa de legibilidade.
- Primeiro uso: solução específica e direta. Generalize no segundo uso real.
- Antes de entregar, releia o diff e apague o que não tem propósito na tarefa: parâmetro sem chamador, configuração sem uso, camada que só repassa a chamada, tratamento de caso que não pode ocorrer.
- Decida agora só o que seria caro mudar depois (formato de dado persistido, contrato público, schema). Para esses, apresente a opção ao usuário antes de implementar.
- Escreva como o código vizinho: nomes, estrutura, tratamento de erro e estilo de teste do próprio repo, mesmo que outro jeito pareça melhor.

### Incrementos

- Quebre a tarefa em passos que deixam o sistema funcionando e a suíte verde; cada passo deve poder virar um commit isolado.
- Ordene os passos pelo de maior valor ou maior risco primeiro, para que uma interrupção deixe algo útil.
- Rode os testes a cada passo. Passo que não chega ao verde em pouco tempo: descarte as mudanças dele e recomece com um passo menor, em vez de acumular correções.
- Não deixe trabalho parcial sem integrar: termine o passo ou reverta-o antes de mudar de assunto.

### Escopo

- A tarefa não cabe (tempo, contexto, dependência faltando): entregue a parte coerente que cabe, testada, e liste explicitamente o que ficou de fora.
- Problema encontrado fora do escopo (código ruim, bug, teste frágil): registre e aponte ao usuário; não corrija sem pedido.

## Evitar

- Entregar "tudo" com qualidade menor: teste pulado, erro engolido, `TODO` escondido, caso de borda ignorado sem aviso.
- Minimizar linhas ou arquivos a ponto de o código ficar críptico.
- Um único diff grande que só funciona no fim.
- Inventar regra de negócio para preencher lacuna do requisito.
- Diagrama ou documento de design mantido em paralelo ao código para algo que o código e os testes já descrevem.

## Quando parar e perguntar

- O requisito admite mais de uma interpretação com resultados diferentes.
- A tarefa não cabe e é preciso escolher o que fica de fora.
- A solução simples exige mudar algo caro de reverter (contrato público, dado persistido, dependência nova).

## Ao terminar

Resuma: passos entregues (na ordem), estado da suíte, o que ficou de fora do escopo e por quê, e problemas encontrados fora do escopo.
