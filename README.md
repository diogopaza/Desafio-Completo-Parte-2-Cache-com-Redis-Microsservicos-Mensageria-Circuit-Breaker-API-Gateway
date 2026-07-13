# 🚀 Spring Boot Advanced — Parte 2 (Cache, Microsserviços, Mensageria, Resiliência, Gateway)

---

## 🔄 Recomeçando (2026-07-13) — revisão feita com Claude

Esse README foi originalmente escrito pelo ChatGPT (Partes 9–13, continuando a numeração da Parte 1) e ainda não foi implementado. A partir de agora a condução passa a ser com Claude, seguindo o mesmo padrão da Parte 1: eu aponto os requisitos, você escreve a solução, e toda correção "resolvida" é conferida direto no código antes de ser dada como fechada.

### O que muda nesse recomeço

* **Escopo desta Parte 2 fica só em sistemas distribuídos** (cache, microsserviços, mensageria, resiliência, gateway) — os temas de IA Generativa (RAG, MCP, LLMOps, Guardrails), motivados por uma vaga de interesse, viram uma **Parte 3 separada**, em outro repositório. Misturar os dois domínios aqui diluiria o foco e o prazo já apertado.
* **Java moderno entra a partir desta parte**: Streams e Lambdas, Records, Sealed Classes, Pattern Matching, Virtual Threads e noções de JVM/Java Memory Model — incorporados nas etapas onde fazem sentido natural (não como um módulo isolado), com uma etapa nova dedicada à concorrência real.
* **Testes dinâmicos reais**: a Parte 14 (nova) inclui teste de carga com múltiplos acessos concorrentes de verdade, não só teoria.

### Prazo

Prazo apertado — o objetivo é fechar essa Parte 2 em poucos dias, junto com a Parte 1, antes de migrar pra Parte 3 (GenAI/MCP/RAG).

---

## 🎯 OBJETIVO GERAL

Evoluir o sistema da Parte 1 aplicando:

* Cache com Redis
* Arquitetura de Microsserviços
* Mensageria
* Resiliência (Circuit Breaker)
* API Gateway
* Java moderno (Streams/Lambdas, Records, Sealed Classes, Pattern Matching, Virtual Threads, JVM/Memory Model)
* Testes reais de concorrência

---

## 🧱 PARTE 9 — CACHE COM REDIS

### 🎯 Objetivo

Implementar cache utilizando Redis para otimizar consultas e reduzir acesso ao banco.

### 🧪 Desafio

Você deve:

* Configurar Redis no projeto
* Cachear consultas de pagamento
* Definir tempo de expiração (TTL)
* Invalidar cache ao salvar/atualizar dados

### ❓ Perguntas

1. O que é cache e qual sua vantagem?
2. O que é TTL?
3. Quando o cache pode causar inconsistência?
4. Qual o impacto de não invalidar o cache?

### 🎯 Avaliação (0 a 10)

* Uso correto de `@Cacheable`
* Uso correto de `@CacheEvict`
* Entendimento de performance
* Controle de consistência

---

## 🌐 PARTE 10 — MICROSSERVIÇOS

### 🎯 Objetivo

Separar a aplicação em múltiplos serviços independentes utilizando Spring Boot.

### 🧪 Desafio

Criar:

* **🔹 pagamento-service** — responsável por pagamentos
* **🔹 notificacao-service** — responsável por notificações

### 🚨 Regras

* Cada serviço deve ser independente
* NÃO compartilhar banco
* Comunicação via HTTP ou mensageria

### ❓ Perguntas

1. O que são microsserviços?
2. Qual a vantagem em relação a monólitos?
3. Quais problemas podem surgir?
4. O que é acoplamento?

### 🎯 Avaliação (0 a 10)

* Separação correta dos serviços
* Independência entre sistemas
* Organização do código
* Comunicação funcional

---

## 📩 PARTE 11 — MENSAGERIA

### 🎯 Objetivo

Implementar comunicação assíncrona utilizando RabbitMQ ou Apache Kafka.

### 🧪 Desafio

Fluxo:

1. Pagamento é criado
2. Evento é enviado para a fila
3. Serviço de notificação consome o evento

### 🚨 Regras

* NÃO usar chamada direta via HTTP entre serviços para esse fluxo
* Comunicação deve ser via fila
* Sistema deve continuar funcionando mesmo com falhas

### ❓ Perguntas

1. O que é comunicação assíncrona?
2. Qual a vantagem da mensageria?
3. O que acontece se um serviço estiver fora do ar?
4. O que é desacoplamento?

### 🎯 Avaliação (0 a 10)

* Uso correto de filas
* Consumo de eventos
* Desacoplamento
* Entendimento do fluxo

---

## 🛡️ PARTE 12 — CIRCUIT BREAKER (+ Sealed Classes & Pattern Matching)

### 🎯 Objetivo

Implementar tolerância a falhas utilizando Resilience4j — e modelar o estado do circuito com Java moderno em vez de constantes soltas.

### 🧪 Desafio

* Simular falha em um serviço e implementar Circuit Breaker + Fallback
* Modelar o estado do circuito (`Fechado`, `Aberto`, `Meio-Aberto`) como uma **sealed interface/class**, com uma implementação por estado
* Usar **pattern matching** (`switch` com `sealed`) para decidir o comportamento de cada estado, sem `if`/`instanceof` encadeado

### 🚨 Regras

* O sistema NÃO pode quebrar em caso de falha
* Deve existir fallback
* A modelagem dos estados deve ser fechada (sealed) — o compilador deve acusar erro se um novo estado for criado e o `switch` não tratar ele

### ❓ Perguntas

1. O que é Circuit Breaker?
2. Qual problema ele resolve?
3. O que é fallback?
4. Quando utilizar esse padrão?
5. O que ganha ao modelar os estados como `sealed` em vez de um enum simples ou strings soltas?
6. Por que o `switch` sobre um tipo `sealed` é mais seguro que uma cadeia de `if/instanceof`?

### 🎯 Avaliação (0 a 10)

* Implementação correta do Circuit Breaker
* Resiliência do sistema
* Tratamento de falhas
* Uso de fallback
* Uso correto de sealed classes + pattern matching

---

## 🌍 PARTE 13 — API GATEWAY

### 🎯 Objetivo

Centralizar o acesso aos serviços utilizando Spring Cloud Gateway.

### 🧪 Desafio

Criar um gateway que:

* Roteie requisições
* Centralize endpoints
* Redirecione para os microsserviços

### 🚨 Regras

* O cliente só acessa o gateway
* Os serviços NÃO são acessados diretamente

### ❓ Perguntas

1. O que é API Gateway?
2. Qual problema ele resolve?
3. Quais responsabilidades ele pode ter?
4. Por que centralizar o acesso?

### 🎯 Avaliação (0 a 10)

* Roteamento correto
* Organização da arquitetura
* Centralização funcional
* Clareza na comunicação

---

## ⚡ PARTE 14 (NOVA) — JAVA MODERNO E CONCORRÊNCIA REAL

### 🎯 Objetivo

Aplicar Virtual Threads e entender o Java Memory Model na prática, com um teste de carga real — não só teoria — sobre os serviços já construídos.

### 🧪 Desafio

* Expor (ou reaproveitar) um endpoint que acesse o cache Redis e/ou um serviço protegido por Circuit Breaker
* Rodar esse endpoint sob **Virtual Threads** (Java 21+, `Executors.newVirtualThreadPerTaskExecutor()` ou o suporte nativo do Tomcat/Spring Boot 3.2+)
* Escrever um **cliente de teste** que dispare dezenas/centenas de requisições **simultâneas de verdade** (multithreading real, não sequencial) contra esse endpoint
* Observar e documentar o comportamento: throughput, race conditions (se houver estado compartilhado mal protegido), comportamento do cache sob concorrência, comportamento do circuit breaker sob carga
* Usar Streams/Lambdas para processar os resultados do teste de carga (agregações, contagens, latência média/p95)
* Usar Records para modelar o resultado de cada chamada do teste (ex: `record ChamadaResultado(int status, long latenciaMs, String erro)`)

### 🚨 Regras

* O teste de concorrência tem que ser **real** (threads/requisições concorrentes de verdade), não um loop sequencial disfarçado
* Pelo menos um cenário deve expor uma condição de corrida real (proposital) e a correção dela, pra provar entendimento — não só o caminho feliz

### ❓ Perguntas

1. O que são Virtual Threads e qual problema resolvem em relação às Platform Threads?
2. O que é o Java Memory Model, em termos simples? O que é uma *race condition*?
3. O que é *visibility* entre threads, e por que uma variável pode "não atualizar" pra outra thread sem sincronização adequada?
4. Por que Virtual Threads não eliminam a necessidade de pensar em concorrência (thread-safety continua sendo problema seu)?
5. Onde Streams/Lambdas ajudaram (ou atrapalharam) na análise dos resultados do teste de carga?
6. Por que Records são uma boa escolha pra modelar o resultado imutável de uma chamada de teste?

### 🎯 Avaliação (0 a 10)

* Teste de carga real implementado corretamente
* Identificação e correção de uma race condition real
* Entendimento conceitual de Virtual Threads e JMM
* Uso idiomático de Streams/Lambdas/Records

---

## 🧠 AVALIAÇÃO FINAL

Você deve se autoavaliar:

| Etapa | Nota (0–10) |
|---|---|
| Redis (Cache) | |
| Microsserviços | |
| Mensageria | |
| Circuit Breaker + Sealed/Pattern Matching | |
| API Gateway | |
| Java Moderno + Concorrência Real | |
| Integração geral | |

---

## 🏁 RESULTADO ESPERADO

Ao final, você terá demonstrado:

* ✔️ Arquitetura moderna e sistemas distribuídos
* ✔️ Uso de cache
* ✔️ Comunicação assíncrona
* ✔️ Resiliência
* ✔️ Escalabilidade
* ✔️ Java moderno aplicado com propósito (não decorativo)
* ✔️ Entendimento real de concorrência, não só de nome

---

## 💬 COMO VAMOS TRABALHAR

Ao concluir cada etapa:

* Me envie o código
* Responda as perguntas
* Explique suas decisões

Toda correção "resolvida" é conferida direto no código antes de ser dada como fechada — não só aceita pela explicação em texto.
