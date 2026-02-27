# 2026-02-13 - NestJS + Baileys (WhatsApp) Architecture Conflict

**Situation/Bug**:
Ao separar a API e o Worker de filas em processos distintos (arquitetura de microsserviços/worker padrão), o `@whiskeysockets/baileys` falha com `Connection Closed (440)` ou `Stream Errored`. Isso ocorre porque o Baileys não suporta múltiplas conexões simultâneas com a mesma sessão (state) sem um gerenciamento de estado externo robusto (Redis/DB sync complexo).

**Solution/Insight**:
Para arquiteturas monolíticas ou simples, a solução mais estável é **unificar o Worker no processo principal da API**.

1. Crie um `QueueModule` que inicialize o worker no `onModuleInit`.
2. Injete o `WhatsappService` (Singleton) nos Jobs.
3. Isso garante que o envio de mensagens use a conexão TCP/Socket já aberta pela API, sem tentar criar uma nova sessão concorrente.

**Reference**:
[BioOrganize Project - Queue Refactor]
