# Bio-Organize - Memória do Projeto

## Tech Stack & Bibliotecas
### Frontend
- **Framework**: React 19 (Vite)
- **UI**: Material UI 7 (MUI)
- **State Management**: Redux Toolkit (UI/Auth), TanStack Query v5 (Data)
- **Form Handling**: Zod, React Hook Form (instalado, uso parcial)
- **API**: Axios
- **Payment**: Mercado Pago SDK, Stripe SDK

### Backend
- **Framework**: NestJS 11
- **ORM**: Prisma 6
- **Cache/Queue**: BullMQ, Redis
- **Database**: PostgreSQL
- **Security**: Passport JWT, Bcrypt
- **Email**: React Email, Resend

## Arquitetura e Padrões
- **Frontend**:
  - Padrão Mobile-First simulando Android em telas menores.
  - Componentização baseada em subpastas por domínio (ex: `professionals`, `appointments`).
  - Uso de `useMediaQuery` para alternar variantes de componentes MUI.
- **Backend**:
  - Arquitetura baseada em Domínios (`src/app/domains`).
  - Uso de Repositórios para abstração de acesso a dados.
  - Guardas JWT globais e Validation Pipe configurado.

## Principais Decisões & Trade-offs
- **Keep-Alive em Agendamentos**: Uso de `display: none` em vez de desmontagem condicional para preservar estado em fluxos complexos.
- **Divisão de Views de Calendário**: Componentes separados para Desk/Mob (`WeekViewDesktop` vs `WeekViewMobile`) para evitar complexidade excessiva de CSS.
- **TanStack + Redux**: Query para sincronização de dados do servidor, Redux para estado global de UI.

## Documentos de Referência
- [PADROES_MOBILE_WEBAPP.md](file:///c:/Users/user/Desktop/work/lintech/bio-organize/PADROES_MOBILE_WEBAPP.md)
- [project-agent.md](file:///c:/Users/user/Desktop/work/lintech/bio-organize/project-agent.md)
