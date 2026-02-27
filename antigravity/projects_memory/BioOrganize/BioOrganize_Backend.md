# Bio-Organize - Backend

## Tech Stack & Libraries
- **Core**: NestJS (v11+)
- **ORM**: Prisma (v6+)
- **Database**: PostgreSQL (implícito via Prisma)
- **File Storage**: Google Cloud Storage (`@google-cloud/storage`) / Local Storage
- **Validation**: `class-validator`, `class-transformer`
- **Security**: Passport, JWT
- **Logging**: Pino (necessário para `@whiskeysockets/baileys`), Winston (geral)

## Folder Structure & Architecture
- `src/app/domains/`: Divisão por domínios de negócio (ex: `clinics`, `users`, `files`).
- `src/infrastructure/`: Implementações de baixo nível (ex: `file`, `queue`, `log`).
- `src/repositories/`: Padrão Repository para abstração do banco de dados.

## Key Decisions & Trade-offs

### 📂 Padrão Global de Upload de Arquivos (IMPORTANTE)
**Situação**: Antigamente, rotas de domínio (ex: `PUT /clinics/:id`) recebiam arquivos via `multipart/form-data`.
**Decisão**: Todos os uploads agora são centralizados na rota única `POST /files`.
**Padrão**:
1. O backend de domínio **não deve** usar `FileInterceptor` ou receber arquivos.
2. O controlador de domínio deve aceitar apenas JSON contendo a URL do arquivo já upado.
3. Isso desacopla o processamento de binários da lógica de negócio.

### 📊 Rotas Analíticas: Overview vs History
**Situação**: Rotas de dashboard misturavam totais gerais (`summary`) com tabelas listadas completas (`history`) dentro do mesmo retorno do controlador, dificultando a paginação e tornando os cálculos pesados.
**Decisão**: O `history` (que engloba listas baseadas em queries de `page`, `limit` e `search`) e o `summary` (totais estáticos do período) foram separados em rotas diferentes.
**Padrão**:
1. Páginas de Dashboard devem consumir dados estatísticos paralelos às listagens transacionais puras em endpoints REST distintos.

### 🤖 Integração WhatsApp (Baileys)
**Situação**: A biblioteca `@whiskeysockets/baileys` exige um logger compatível com Pino (com método `.child()`). O `Logger` padrão do NestJS não atende a esse contrato, causando `TypeError: logger.child is not a function`.
**Decisão**: Adicionado `pino` como dependência direta e configurado no `WhatsappService` especificamente para o socket do Baileys.

### 🔄 Arquitetura de Filas (Worker Unificado)
**Situação**: O uso de um processo worker separado (`npm run worker:dev`) causava conflito de sessão no WhatsApp (Erro 440), pois ambos os processos tentavam conectar simultaneamente usando as mesmas credenciais do Baileys.
**Decisão**: O Worker foi integrado à aplicação principal via `QueueModule`.
**Padrão**:
1. O worker roda no mesmo processo do NestJS (`WorkerService`).
2. Compartilha a instância singleton do `WhatsappService`, aproveitando a conexão ativa.
3. `QueueProcessor` e Jobs devem usar Injeção de Dependência (DI) e não instanciar services manualmente.

---
*Atualizado em: 2026-02-24*
