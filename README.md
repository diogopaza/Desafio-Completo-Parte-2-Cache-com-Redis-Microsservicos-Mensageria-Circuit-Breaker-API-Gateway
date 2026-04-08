🚀 Spring Boot Advanced — Parte 2
🎯 OBJETIVO GERAL

Evoluir o sistema da Parte 1 aplicando:

Cache com Redis
Arquitetura de Microserviços
Mensageria
Resiliência
API Gateway
🧱 PARTE 9 — CACHE COM REDIS
🎯 Objetivo

Implementar cache utilizando Redis para otimizar consultas e reduzir acesso ao banco.

🧪 Desafio

Você deve:

Configurar Redis no projeto
Cachear consultas de pagamento
Definir tempo de expiração (TTL)
Invalidar cache ao salvar/atualizar dados
❓ Perguntas
O que é cache e qual sua vantagem?
O que é TTL?
Quando o cache pode causar inconsistência?
Qual o impacto de não invalidar o cache?
🎯 Avaliação
Uso correto de @Cacheable
Uso correto de @CacheEvict
Entendimento de performance
Controle de consistência
🌐 PARTE 10 — MICROSSERVIÇOS
🎯 Objetivo

Separar a aplicação em múltiplos serviços independentes utilizando Spring Boot

🧪 Desafio

Criar:

🔹 pagamento-service
Responsável por pagamentos
🔹 notificacao-service
Responsável por notificações
🚨 Regras
Cada serviço deve ser independente
NÃO compartilhar banco
Comunicação via HTTP ou mensageria
❓ Perguntas
O que são microserviços?
Qual a vantagem em relação a monólitos?
Quais problemas podem surgir?
O que é acoplamento?
🎯 Avaliação
Separação correta dos serviços
Independência entre sistemas
Organização do código
Comunicação funcional
📩 PARTE 11 — MENSAGERIA
🎯 Objetivo

Implementar comunicação assíncrona utilizando:

👉 RabbitMQ
ou
👉 Apache Kafka

🧪 Desafio

Fluxo:

Pagamento é criado
Evento é enviado para a fila
Serviço de notificação consome o evento
🚨 Regras
NÃO usar chamada direta via HTTP entre serviços
Comunicação deve ser via fila
Sistema deve continuar funcionando mesmo com falhas
❓ Perguntas
O que é comunicação assíncrona?
Qual a vantagem da mensageria?
O que acontece se um serviço estiver fora do ar?
O que é desacoplamento?
🎯 Avaliação
Uso correto de filas
Consumo de eventos
Desacoplamento
Entendimento do fluxo
🛡️ PARTE 12 — CIRCUIT BREAKER
🎯 Objetivo

Implementar tolerância a falhas utilizando:

👉 Resilience4j

🧪 Desafio

Simular falha em um serviço e implementar:

Circuit Breaker
Fallback
🚨 Regras
O sistema NÃO pode quebrar em caso de falha
Deve existir fallback
❓ Perguntas
O que é Circuit Breaker?
Qual problema ele resolve?
O que é fallback?
Quando utilizar esse padrão?
🎯 Avaliação
Implementação correta
Resiliência do sistema
Tratamento de falhas
Uso de fallback
🌍 PARTE 13 — API GATEWAY
🎯 Objetivo

Centralizar o acesso aos serviços utilizando:

👉 Spring Cloud Gateway

🧪 Desafio

Criar um gateway que:

Roteie requisições
Centralize endpoints
Redirecione para os microserviços
🚨 Regras
O cliente só acessa o gateway
Os serviços NÃO são acessados diretamente
❓ Perguntas
O que é API Gateway?
Qual problema ele resolve?
Quais responsabilidades ele pode ter?
Por que centralizar o acesso?
🎯 Avaliação
Roteamento correto
Organização da arquitetura
Centralização funcional
Clareza na comunicação
🧠 AVALIAÇÃO FINAL

Você deve se autoavaliar:

Etapa	Nota (0–10)
Redis (Cache)	
Microserviços	
Mensageria	
Circuit Breaker	
API Gateway	
Integração geral	
🏁 RESULTADO ESPERADO

Ao final, você terá demonstrado:

✔️ Arquitetura moderna
✔️ Sistemas distribuídos
✔️ Uso de cache
✔️ Comunicação assíncrona
✔️ Resiliência
✔️ Escalabilidade

💬 INSTRUÇÃO FINAL

👉 Ao concluir cada etapa:

Me envie o código
Responda as perguntas
Explique suas decisões

👉 Eu vou corrigir como uma entrevista técnica real (com nota e feedback detalhado)
