# Projeto: WaiSports - Backend

## Tech Stack & Libraries
- **Core:** PHP 8.x, Laravel 11
- **Database:** PostgreSQL
- **Auth:** Laravel Sanctum (Token based)
- **Payment:** Stripe (Webhooks e API)
- **Storage:** Local / Amazon S3

## Folder Structure & Architecture
- `app/Http/Controllers/Admin/`: Controladores centralizados por área administrativa.
- `app/Http/Requests/Admin/`: Validação estrita via FormRequests para garantir integridade de dados.
- `app/Models/`: Uso intensivo de `$casts` para converter enums de banco para tipos PHP nativos.

## Key Decisions & Trade-offs
### 1. Eloquent Strict Types
- **Decisão:** Usar `protected function casts(): array` nos modelos.
- **Racional:** Garante que flags como `blocked` ou `registration_completed` cheguem ao frontend como booleanos, independentemente de como o banco armazena.

### 2. UpdateOrCreate para Perfis de Treino
- **Decisão:** Usar a relação `trainingProfile()->updateOrCreate()` no `UsersController`.
- **Racional:** Evita erros de "member function update() on null" caso o perfil não tenha sido criado no onboarding inicial por alguma falha de conexão.

### 3. Filtro de Atributos Null/Empty
- **Decisão:** Filtrar o `training_profile_data` via `array_filter` antes da persistência.
- **Racional:** Permite que omissões do frontend não sobreponham valores default do banco de dados (ex: 'X' para sex).

## Aprendizados Recentes
- **Ambiguous Column Errors:** Ao usar `latestOfMany()` em joins, sempre referenciar a tabela explicitamente no Eloquent para evitar erros SQL de colunas ambíguas durante o `update`.
- **Enum Language Consistency:** Garantir que o banco aceite apenas enums em português (ex: 'iniciante') e que o frontend faça o mapeamento necessário caso os dados de seed estejam em inglês.
