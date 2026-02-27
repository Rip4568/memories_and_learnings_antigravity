# Golembrar - Backend

## Tech Stack & Libraries
- **Framework**: NestJS (v11)
- **Database ORM**: Prisma (v6.16)
- **Database**: PostgreSQL
- **Message Broker**: RabbitMQ (`amqplib`, `amqp-connection-manager`)
- **Cache / External Sync**: Redis (`ioredis`)
- **WhatsApp Integration**: Baileys (`@whiskeysockets/baileys`)
- **Authentication**: JWT, Passport (`@nestjs/jwt`, `@nestjs/passport`)
- **Other Key Libs**: Swagger for API Docs, Pino for Logging, RxJS, Bcrypt.

## Folder Structure & Architecture
- **Monolítico Modular**: O backend segue a estrutura padrão do NestJS focada em domínios (`user`, `auth`, `message`, `wallet`, `contact`, `tasks`, etc.).
- **Worker & Filas**: Existe uma pasta `worker` que escuta as filas (RabbitMQ) e interage com os serviços de mensageria (WhatsApp).
- **Tasks (Cron Jobs)**: A pasta `tasks` concentra as rotinas agendadas (ex: leitura de cache/banco a cada minuto para disparo de lembretes).

## Design Patterns Used
- **Dependency Injection / IoC**: Nativo do NestJS.
- **Repository Pattern / ORM**: Centralizado nas chamadas ao Prisma.
- **Publisher/Subscriber (Filas)**: Desacoplamento do envio de mensagens pesadas (WhatsApp) via RabbitMQ. A API publica na fila e o `WhatsappWorkerService` consome de forma assíncrona.
- **Scheduled Tasks (Cron)**: Lógica de agendamento usando `@nestjs/schedule` para controlar o tempo exato de envio de mensagens do módulo de `reminder`.

## O Módulo de Reminder / Mensagens (SUMA IMPORTÂNCIA)
- O agendamento de **"Reminders"** é centralizado nas entidades `Message` e `MessageDispatch`. 
- **Fluxo de Disparo**:
  1. A `TasksService` aciona um cron job `everyMinute()` (`@Cron(CronExpression.EVERY_MINUTE)`).
  2. A tarefa busca no banco/cache as mensagens cujo tempo de envio já chegou.
  3. Estas mensagens são enviadas como um payload (incluindo `message_id` e `identity`) para uma fila RabbitMQ mapeada como `whatsapp_queue`.
  4. O `WhatsappWorkerService` (que implementa `OnModuleInit`) fica escutando a fila `whatsapp_queue`.
  5. Ao consumir um item, o worker aciona o `WhatsappService` (Baileys) para realizar o disparo real no WhatsApp do cliente, lidando com os *acks/nacks* na fila em caso de erro.

## Key Decisions & Trade-offs
- **Filas com RabbitMQ**: Permite a escalabilidade horizontal dos workers de WhatsApp caso haja pico de lembretes (reminders). O fluxo do NestJS não fica travado esperando a rede do WhatsApp responder.
- **Redis vs Database para Lembretes**: O worker utiliza caches para facilitar a consulta rápida das mensagens agendadas e otimizar acessos a cada minuto sem sobrecarregar o banco de dados principal no PostgreSQL.
- **Baileys (WebSockets)**: Gerencia o WhatsApp localmente na aplicação, gerando a necessidade de gerenciar sessões explicitamente no banco de dados (`WhatsappSession`), evitando qrcode scans repetitivos.
