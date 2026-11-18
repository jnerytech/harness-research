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
- Resistência a refactoring e manutenibilidade não se negociam. O ajuste fica entre proteção e velocidade.
- Verifique **o quê** (resultado observável), nunca **como** (passos internos). Todo teste deve rastrear até um requisito de negócio; se não rastreia, reescreva ou apague.
- O objetivo é sustentar mudanças no código com segurança, não atingir uma métrica. Teste é código e tem custo: só fica se pagar esse custo.

## Regras

### O que testar

- Teste **comportamento observável**, nunca detalhe de implementação. Se um refactoring sem mudança de comportamento quebraria o teste, reescreva o teste.
- "Unidade" é uma unidade de **comportamento**, não uma classe ou função. "Isolamento" significa testes independentes entre si.
- Prefira, nesta ordem: verificar a **saída** → verificar o **estado** final → verificar **interações**.
- Lógica de domínio e algoritmos: teste unitário, sem mocks. Orquestração (controllers, handlers): poucos testes de integração. Código trivial (getter, mapeamento direto): não teste.
- Código com lógica e I/O misturados: extraia a decisão para código puro (humble object / functional core) antes de testar. O núcleo recebe **valores**, não colaboradores (repositório, cliente HTTP).
- Integração: um teste para o happy path mais longo de cada caso de uso (o que toca mais dependências), mais os edge cases que testes unitários não alcançam.
- Edge case que derruba a operação na hora (entrada inválida, config ausente): prefira guard clause que lança exceção (fail fast) a um teste de integração.
- Log de suporte exigido pelo negócio (auditoria, evento de domínio) é comportamento observável: teste. Log de diagnóstico: não teste.

### Dependências e mocks

- Código in-process: use o colaborador **real**.
- Dependência out-of-process **gerenciada** (só esta aplicação acessa, ex.: o próprio banco): use a real em teste de integração e verifique o estado final. Não faça mock de repositório ou ORM.
- Dependência out-of-process **não gerenciada** (visível a outros sistemas, ex.: e-mail, message bus, API de terceiros): use mock, porque a interação faz parte do contrato.
- Banco compartilhado com outros sistemas: tabelas lidas por terceiros são não gerenciadas (verifique a interação); o resto é gerenciado (verifique o estado).
- Faça mock só de tipos próprios (adapter na borda), nunca do SDK de terceiro direto. Faça mock do **último** tipo antes do sistema externo, não de uma camada intermediária.
- Verifique o efeito colateral que o sistema externo recebe (payload, texto da mensagem), não a chamada a um método interno.
- **Stub** fornece entrada (query); **mock** verifica saída (command). Nunca verifique chamadas feitas a um stub.
- Ao verificar um mock, confira a contagem exata (uma vez) e a ausência de chamadas inesperadas.
- Prefira um spy escrito à mão, com assert reutilizável, a `verify` inline do framework, quando o mesmo adapter aparece em vários testes.
- Mocks pertencem a testes de integração. O número de mocks num teste é ditado pelo número de dependências não gerenciadas, não por uma regra de "um mock por teste".

### Banco de dados em integração

- Use o mesmo SGBD da produção (versão pode variar, fornecedor não). Não substitua por banco em memória.
- Limpe os dados residuais no **início** de cada teste, preservando dados de referência. Rode esses testes em sequência.
- Use uma transação / unit of work separada para Arrange, Act e Assert. No Assert, leia o estado de volta do banco, não do objeto em memória.
- Não teste repositório isoladamente: ele é coberto pelo teste de integração do caso de uso. Exija mais justificativa para testar leitura do que escrita.

### Estrutura

- Arrange / Act / Assert, com **um** Act de uma linha por teste. Act com várias linhas indica API que vaza invariante: corrija a API.
- Vários asserts são aceitáveis quando verificam o mesmo comportamento. Sem `if` nem laço no teste.
- Nome descreve o comportamento como fato, em linguagem do domínio, sem o nome do método (ex.: `delivery_with_a_past_date_is_invalid`).
- Reaproveite Arrange com factory methods privados, Object Mother ou Test Data Builder, não com campos mutáveis inicializados no construtor/setup.
- Use testes parametrizados para variações do mesmo comportamento.
- Valor esperado é **literal pré-calculado** (por especialista ou sistema legado), nunca recalculado com a lógica ou as constantes de produção.

## Evitar

- Testar método privado ou expor estado privado só para o teste. Método privado sem cobertura pela API pública é código morto (apague) ou abstração faltando (extraia uma classe).
- Ampliar a API pública só para facilitar o teste.
- Reimplementar o algoritmo de produção no assert.
- Código de produção que existe só para o teste (flag `isTestEnvironment`, branch de teste).
- Mock de classe concreta: sinal de classe com duas responsabilidades. Separe lógica e I/O.
- Interface com uma única implementação para classe de domínio, criada só para mockar.
- Mock que retorna mock (cadeia de mocks).
- Ler o relógio do sistema ou outro singleton estático diretamente: injete o tempo, de preferência como valor.
- Escrever teste para subir cobertura. Cobertura baixa é sinal de alerta; cobertura alta não prova qualidade.

## Quando parar e perguntar

- A única forma de testar a integração seria mockar uma dependência gerenciada (ex.: o banco não sobe no ambiente de teste). Nesse caso, prefira não escrever o teste de integração e confirme com o usuário.
- Não está claro se uma dependência é gerenciada ou não gerenciada (ex.: tabela de banco lida por outro sistema).
