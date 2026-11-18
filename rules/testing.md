---
paths:
  - "**/{test,tests,__tests__,spec,specs}/**"
  - "**/*{Test,Tests,Spec,Specs}.*"
  - "**/*.{test,spec}.*"
  - "**/*_{test,spec}.*"
  - "**/test_*.*"
---

# Testes

Como escrever e revisar testes automatizados (unidade e integração), em qualquer linguagem.

## Princípios

- Um teste vale o **produto** de 4 atributos: proteção contra regressões, resistência a refactoring, feedback rápido e manutenibilidade. Um atributo zerado zera o teste.
- Resistência a refactoring não se negocia. O ajuste fica entre proteção e velocidade.
- O objetivo é sustentar mudanças no código com segurança, não atingir uma métrica.

## Regras

### O que testar

- Teste **comportamento observável**, nunca detalhe de implementação. Se um refactoring sem mudança de comportamento quebraria o teste, reescreva o teste.
- "Unidade" é uma unidade de **comportamento**, não uma classe ou função. "Isolamento" significa testes independentes entre si.
- Prefira, nesta ordem: verificar a **saída** → verificar o **estado** final → verificar **interações**.
- Lógica de domínio e algoritmos: teste unitário. Orquestração (controllers, handlers): poucos testes de integração. Código trivial: não teste.
- Código com lógica e I/O misturados: extraia a decisão para código puro (humble object / functional core) antes de testar.
- Integração: um teste para o happy path mais longo de cada caso de uso, mais os edge cases que testes unitários não alcançam.

### Dependências e mocks

- Código in-process: use o colaborador **real**.
- Dependência out-of-process **gerenciada** (só esta aplicação acessa, ex.: o próprio banco): use a real em teste de integração. Não faça mock de repositório ou ORM.
- Dependência out-of-process **não gerenciada** (visível a outros sistemas, ex.: e-mail, message bus, API de terceiros): use mock, porque a interação faz parte do contrato.
- Faça mock só de tipos próprios (adapter na borda), nunca do SDK de terceiro direto.

### Estrutura

- Arrange / Act / Assert, com **um** Act por teste. Sem `if` nem laço no teste.
- Nome descreve o comportamento em linguagem do domínio, não o nome do método.
- Use testes parametrizados para variações do mesmo comportamento.

## Evitar

- Testar método privado ou expor estado privado só para o teste.
- Reimplementar o algoritmo de produção no assert.
- Código de produção que existe só para o teste.
- Mock de classe concreta.
- Ler o relógio do sistema diretamente: injete o tempo como valor ou dependência.
- Escrever teste para subir cobertura. Cobertura baixa é sinal de alerta; cobertura alta não prova qualidade.
