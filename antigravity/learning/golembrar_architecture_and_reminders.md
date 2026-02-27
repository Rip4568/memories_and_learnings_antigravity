# 2026-02-25 - Arquitetura de Módulo de Lembretes Em Fila (Golembrar)

**Situation/Bug**:
O projeto Golembrar possui um módulo central para o agendamento e envio de lembretes (Mensagens) que precisam ser entregues rigorosamente no horário previsto via WhatsApp, sem gargalar a aplicação ou realizar acessos custosos repetidamente em sincronia.

**Solution/Insight**:
A arquitetura consolidada divide os processamentos através de Message Brokers no ecossistema NestJS + Prisma. O envio baseia-se num fluxo assimétrico:
1. **Schedule Service (`TasksService`)**: Uma CRON `@Cron(CronExpression.EVERY_MINUTE)` busca via Node periodicamente na base de dados (ou camada cache) quais mensagens (`Message`/`MessageDispatch`) encontram-se no horário "agora" ou atrasadas para enviar.
2. **Producer**: O `TasksService` dispara os registros correspondentes para o RabbitMQ (numa fila chamada `whatsapp_queue`).
3. **Consumer Worker (`WhatsappWorkerService`)**: O worker consome as transações dessa fila à parte e realiza a integração pesada (via WebSocket do pacote `@whiskeysockets/baileys`). O RabbitMQ faz todo o tracking de ACKs/NACKs para garantia de entrega em caso de falha da engine do WhatsApp.

Esse padrão elimina congelamentos de threads no processo principal, lida de forma robusta e otimizada com tentativas de retries (através de NACKs) e possibilita o ganho com escalabilidade horizontal dos workers de WhatsApp.

**Reference**:
[Golembrar Backend Memory](file:///C:/Users/user/.gemini/antigravity/projects_memory/Golembrar/Golembrar_Backend.md)
[Whatsapp Worker Source](file:///c:/Users/user/Desktop/work/code/golembrar/golembrar-api/src/worker/whatsapp-worker.service.ts)
