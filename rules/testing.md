---
paths:
  - "**/{test,tests,__tests__,spec,specs}/**"
  - "**/*{Test,Tests,Spec,Specs}.*"
  - "**/*.{test,spec}.*"
  - "**/*_{test,spec}.*"
  - "**/test_*.*"
---

# Testes

Baseado em *Unit Testing Principles, Practices, and Patterns* (Khorikov).

## Critério de valor

Todo teste é avaliado por 4 atributos, e o valor é o produto deles:

1. **Proteção contra regressões**: exercita lógica que importa.
2. **Resistência a refactoring**: não quebra se o comportamento não mudou.
3. **Feedback rápido**
4. **Manutenibilidade**: curto, legível, poucas dependências para rodar.

Resistência a refactoring não se negocia. O ajuste fica entre proteção e velocidade.

## Regras

- Teste **comportamento observável**, nunca detalhe de implementação. Se um refactoring sem mudança de comportamento quebraria o teste, reescreva o teste.
- "Unidade" é uma unidade de **comportamento**, não uma classe ou função. "Isolamento" significa testes independentes entre si.
- Prefira, nesta ordem: verificar a **saída** → verificar o **estado** final → verificar **interações**.
- Use o colaborador **real** para código in-process.
- Dependência out-of-process **gerenciada** (só esta aplicação acessa, ex.: o próprio banco): use a real em teste de integração. Não faça mock de repositório ou ORM.
- Dependência out-of-process **não gerenciada** (visível a outros sistemas, ex.: e-mail, message bus, API de terceiros): aqui o mock é correto, porque a interação faz parte do contrato.
- Faça mock só de tipos próprios (adapter na borda), nunca do SDK de terceiro direto.
- Não teste código trivial (getters, mapeamento óbvio).
- Para código com lógica e I/O misturados, extraia a lógica de decisão para código puro (humble object / functional core) antes de testar.
- Integração: um teste para o happy path mais longo de cada caso de uso, mais os edge cases que testes unitários não alcançam.

## Estrutura

- Arrange / Act / Assert, com **um** Act por teste. Sem `if` nem laço no teste.
- Nome descreve o comportamento em linguagem do domínio, não o nome do método.
- Testes parametrizados para variações do mesmo comportamento.

## Anti-patterns

- Testar método privado ou expor estado privado só para o teste.
- Reimplementar o algoritmo de produção no assert.
- Código de produção que existe só para o teste.
- Mock de classe concreta.
- Ler relógio do sistema diretamente: injete o tempo como valor ou dependência.
- Escrever teste para subir cobertura. Cobertura baixa é sinal de alerta; cobertura alta não prova nada.
