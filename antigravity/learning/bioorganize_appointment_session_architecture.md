# 08/03/2026 - Arquitetura de Agendamentos e Sessões (Bio-Organize)

**Situation/Bug**:
O usuário precisou adaptar o sistema de agendamento do Bio-Organize para suportar a seleção de múltiplos procedimentos (ex: Botox e Peeling) no mesmo horário, sem que o sistema criasse múltiplos agendamentos independentes que causassem problemas de sincronia, exibição no calendário e validação de conflitos. Além disso, o calendário (React) tinha dificuldades para agrupar e exibir um "card" único com a soma total do tempo das sessões se fossem entidades separadas.

**Solution/Insight**:
A solução projetada junto ao time de engenharia foi adotar a separação hierárquica `Appointment` -> `ProvidedSession` -> N `ProvidedProcedures`.
- **Backend**: O endpoint de `create` gera **apenas 1** `Appointment` "âncora" (usando o primeiro procedimento da lista por obrigatoriedade de schema), atrela **1** `ProvidedSession`, e vincula todos os serviços solicitados como `ProvidedProcedure` filhos da sessão criada.
- **Listagem e Conflitos**: A verificação de bloqueio de horários (conflitos) e a listagem de agendamentos (`list`) foram refatoradas para, ao invés de ler apenas `procedure.duration`, iterarem sobre a relation `providedSessions -> providedProcedures` (via Prisma nested includes) e somarem as durações reativas do `ClinicProcedure` associado a cada filho, obtendo o tempo global ocupado na agenda.
- **Frontend (Calendário em React)**: Em vez do frontend tentar agrupar as coisas pelo `patientId`, o backend passa a emitir uma propriedade `.session` contendo o formato aninhado. O calendário (`useDayViewLayout`) agora extrai a propriedade especial exportada `_layout.duration` do agendamento somado para gerar um Card maior (calc CSS de altura) no `DayView`/`WeekView`, enquanto ações de exclusão e edição operam repassando somente o `appointmentId` Pai para que o Prisma acione o `onDelete: Cascade`.

**Reference**:
- [appointment.service.ts](file:///c:/Users/user/Desktop/work/lintech/bio-organize/bio-organize-backend/src/app/domains/appointments/services/appointment.service.ts)
- [schema.prisma](file:///c:/Users/user/Desktop/work/lintech/bio-organize/bio-organize-backend/prisma/schema.prisma)
- [DayView.tsx](file:///c:/Users/user/Desktop/work/lintech/bio-organize/bio-organize-frontend/src/components/appointments/DayView.tsx)
