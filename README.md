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

## 🌐 PARTE 10 — MICROSSERVIÇOS (agora poliglota: Java + Clojure)

### 🎯 Objetivo

Separar a aplicação em múltiplos serviços independentes. **Decisão tomada em 2026-07-13:** o `pagamento-service` fica em Spring Boot (como já vínhamos fazendo); o `notificacao-service` vira uma oportunidade de aprender **Clojure** na prática, já que roda na JVM e se comunica com o resto do sistema pela mesma fila que a Parte 11/11B já constroem. Isso também é currículo real — arquitetura poliglota (Java + Clojure conversando via mensageria) é exatamente como bancos como o Nubank operam.

Como você nunca programou em Clojure, essa etapa foi dividida em três partes: a base em Java (10), os fundamentos da linguagem do zero (10B) e o serviço de fato (10C). Não pula a 10B — sem ela, a 10C vai ser só copiar código sem entender.

### 🧪 Desafio (pagamento-service)

* **🔹 pagamento-service** — responsável por pagamentos, em Spring Boot

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

## 🟣 PARTE 10B — FUNDAMENTOS DE CLOJURE (do zero)

### 🎯 Objetivo

Antes de escrever um serviço, entender a linguagem — sintaxe, imutabilidade, REPL. Sem framework nenhum ainda, só a linguagem pura.

### 🧪 Desafio

* Instalar a ferramenta oficial (`clj`/Clojure CLI, via `deps.edn`) e abrir um REPL
* No REPL, praticar até se sentir confortável com:
  * **Notação prefixa**: `(+ 1 2)` em vez de `1 + 2` — o operador vem primeiro, dentro dos parênteses
  * **Estruturas de dados**: listas `()`, vetores `[1 2 3]`, mapas `{:nome "Diogo" :idade 30}`, keywords `:tipo`
  * **`def`** (define um valor) e **`defn`** (define uma função)
  * **Imutabilidade**: não existe variável que muda de valor — "alterar" um mapa cria um mapa novo (`assoc`, `update`)
  * **A macro de threading `->`**: encadeia chamadas de função de forma legível, parecido com um `.method().method()` fluente do Java — `(-> pagamento :valor (* 1.1))`
* Escrever, em Clojure puro (sem framework), a mesma lógica de **cálculo de multa por atraso** que você já implementou em Java na Parte 6 do outro repositório (Strategy Pattern) — agora como uma função pura

### ❓ Perguntas

1. Por que Clojure não tem `for` (i=0; i<10; i++) do jeito que Java tem? O que substitui os loops imperativos?
2. O que significa "dado imutável", na prática? Se você não pode alterar um mapa, como um programa "muda de estado"?
3. Compara a função de multa que você escreveu em Clojure com a versão Java (Strategy Pattern) — o que ficou mais simples? O que ficou mais estranho?
4. O que é o REPL, e por que desenvolvimento "REPL-driven" é uma cultura tão forte em Clojure (diferente do ciclo compilar→rodar→testar do Java)?

### 🎯 Avaliação (0 a 10)

* Conforto real com a sintaxe (não só copiar exemplo)
* Função de multa reescrita corretamente em Clojure
* Entendimento de imutabilidade

---

## 🟣 PARTE 10C — NOTIFICACAO-SERVICE EM CLOJURE

### 🎯 Objetivo

Construir o `notificacao-service` de verdade: um microsserviço HTTP em Clojure que consome mensagens da fila (Parte 11 ou 11B) e "envia" notificações (pode ser só um log — não precisa de e-mail/SMS real).

### 🧪 Desafio

* Adicionar as bibliotecas **Ring** (abstração HTTP de base, equivalente ao Servlet API) e **Reitit** (roteamento, equivalente ao `@RequestMapping`) ao `deps.edn`
* Criar um handler Ring mínimo: uma função que recebe um mapa de request e devolve um mapa de response (`{:status 200 :body "ok"}`) — sem anotação, sem reflection, só função pura
* Expor uma rota de health-check (`GET /health`) via Reitit
* Consumir mensagens da fila de pagamento criada nas Partes 11/11B, e logar uma "notificação" pra cada pagamento processado
* Rodar o `pagamento-service` (Spring) e o `notificacao-service` (Clojure) ao mesmo tempo, provando que os dois se comunicam só pela fila — nenhuma chamada HTTP direta entre eles

### 🚨 Regras

* `notificacao-service` não pode ter acesso direto ao banco do `pagamento-service`
* A única comunicação entre os dois é via mensageria

### ❓ Perguntas

1. Um handler Ring é só uma função (request → response), sem classe, sem anotação. Compara isso com um `@RestController` do Spring — o que se perde, o que se ganha?
2. Por que não faz sentido o `notificacao-service` compartilhar o banco do `pagamento-service`?
3. O que aconteceria com o `notificacao-service` se a fila estivesse fora do ar? E se fosse uma chamada HTTP direta em vez de fila?

### 🎯 Avaliação (0 a 10)

* Serviço Clojure funcional, consumindo a fila de verdade
* Uso correto de Ring/Reitit
* Comunicação exclusivamente via mensageria (sem atalho HTTP direto)
* Entendimento do contraste com o modelo Spring

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

## ☁️ PARTE 11B — MENSAGERIA COM AWS SQS (baseado no padrão real do Educabiz)

### 🎯 Objetivo

A Parte 11 usou Kafka/RabbitMQ (mensageria "genérica"). Essa parte é focada especificamente em **AWS SQS**, porque é o que se usa de verdade em produção no seu trabalho (o fluxo de boletos do Educabiz roda inteiro em cima disso) — e é diretamente relevante pra feature de assinatura digital que você está construindo.

Essa etapa nasceu de uma análise do código real do Educabiz (`GenericQueue`, `BoletoIssueRequestQueue`, `BoletosQueue`, `BoletoIssuePDFJob`), que revelou um padrão funcional mas com pontos datados: SDK v1 da AWS (já em manutenção), polling curto em vez de long-polling, sem Dead Letter Queue configurada, sem proteção visível contra reprocessamento (SQS é *at-least-once*, não *exactly-once*), credenciais estáticas em vez de IAM Role. O objetivo aqui é implementar a versão **corrigida** desses pontos.

### 💰 Sobre custo — não precisa de conta AWS real

Você mencionou não ter muita experiência com AWS e não querer o risco de esquecer de apagar algo e ter gasto. Solução: **use o LocalStack** (`docker.io/localstack/localstack`), que emula SQS localmente via Docker — sem conta AWS, sem cartão de crédito, sem "máquina" nenhuma pra lembrar de desligar. Sobe o container, cria a fila local, testa à vontade, derruba o container quando terminar (`docker compose down`). Zero risco de cobrança, porque não existe recurso real da AWS envolvido. Isso também é uma prática a mais de Docker, que você já vai precisar pra entrevista.

Se mais pra frente você quiser testar com AWS de verdade: SQS não tem "máquina" (é serverless — você paga por chamada de API, não por hora ligado), e o free tier cobre 1 milhão de requisições/mês. Ainda assim, pra esse exercício, LocalStack é mais simples e 100% seguro.

### 🧪 Desafio

* Suba um SQS local via LocalStack (Docker) com uma fila principal + uma Dead Letter Queue associada (`RedrivePolicy` com `maxReceiveCount`)
* Implemente um **Producer** (envia mensagem) usando o **AWS SDK v2** (`software.amazon.awssdk`, não o v1 usado no Educabiz)
* Implemente um **Consumer** que:
  * Usa **long-polling de verdade** (`WaitTimeSeconds` > 0 no `ReceiveMessageRequest`)
  * É **idempotente** — processa a mesma mensagem duas vezes (simule reenvio) sem duplicar o efeito
  * Deleta a mensagem só depois de processar com sucesso
* Simule uma mensagem "envenenada" (que sempre falha ao processar) e prove que ela migra pra DLQ depois de N tentativas, sem ficar em loop infinito

### 🚨 Regras

* Nenhuma credencial real da AWS no código — só as credenciais fake do LocalStack
* Tem que existir um teste real provando a idempotência (mandar a mesma mensagem 2x, mostrar que só processou uma vez)
* Tem que existir um teste real provando a DLQ (mensagem que falha sempre acaba lá, não fica reprocessando pra sempre)

### ✅ Resultado esperado

* Producer/Consumer funcionando contra o SQS local
* DLQ configurada e testada de verdade
* Idempotência provada com teste, não só implementada "na fé"

### ❓ Perguntas

1. Qual a diferença entre entrega *at-least-once* e *exactly-once*? Por que o SQS é *at-least-once*?
2. O que é long-polling, e por que ele é melhor que o polling curto que o Educabiz usa hoje (sem `WaitTimeSeconds`)?
3. Por que uma Dead Letter Queue importa? O que aconteceria com uma mensagem envenenada sem ela?
4. Que estratégia você usou pra garantir idempotência no consumidor?
5. Comparando com o código real do Educabiz que analisamos: das melhorias que você implementou aqui (SDK v2, long-polling, DLQ, idempotência, sem credenciais estáticas), quais você aplicaria lá se pudesse, e por quê?

### 🎯 Avaliação (0 a 10)

* Producer/Consumer funcionando via LocalStack
* DLQ configurada e testada
* Idempotência provada com teste real
* Entendimento conceitual (at-least-once, long-polling)
* Conexão clara com o código real do Educabiz

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
| Microsserviços (pagamento-service) | |
| Fundamentos de Clojure | |
| Microsserviços (notificacao-service em Clojure) | |
| Mensageria | |
| Mensageria com AWS SQS (LocalStack) | |
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
