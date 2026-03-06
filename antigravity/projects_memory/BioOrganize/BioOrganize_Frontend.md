# Bio-Organize - Frontend

## Tech Stack & Libraries
- **Core**: React (v19), Vite
- **UI**: Material UI (MUI v7), Emotion
- **State Management**: Redux Toolkit (Thunks)
- **Forms**: React Hook Form + Zod
- **API**: Axios

## Design Patterns Used
- **Slices por Domínio**: Organização do estado em `src/stores/[domain]`.
- **Global Files Store**: `filesSlice.ts` para gerenciar uploads globais.
- **WhatsApp Store**: `whatsappSlice.ts` para gerenciar status de conexão e QR Code globalmente.
- **Interfaces e Tipagens**: As definições de Types e Interfaces devem ser armazenadas de forma autônoma na pasta `src/interfaces` ao invés de acopladas fortemente nas Actions/Thunks (ex: `src/interfaces/financial.interfaces.ts`). Isso reduz warnings de importação (type-only) e clareia o código das slices globais e hooks.

## Key Decisions & Trade-offs

### 🚀 Fluxo de Upload em Dois Passos
**Padrão**: Nunca enviar arquivos e dados de formulário no mesmo request `multipart`.
**Fluxo**:
1. **Upload**: Disparar `dispatch(uploadFile(file))` para `/files`.
2. **URL**: Obter a URL de retorno e atualizar o estado do formulário (`setValue('image', url)`).
3. **Persistência**: Enviar o form completo via JSON para a rota de destino (ex: `updateClinic`).

**Benefício**: Evita problemas de serialização, falhas parciais complexas e simplifica o backend.

### 🛠️ Limpeza de Configuração (React vs Vue)
**Situação**: O projeto React continha dependências e scripts do Vue (`vue-tsc`), herdados de templates incorretos, causando confusão e checks desnecessários.
**Decisão**: Remoção total de `vue-tsc` e padronização dos scripts de build.
**Padrão**:
2. Scripts de lint/build não devem referenciar extensões `.vue`.

### 🧩 Redux State e Comportamento de Interface
**Situação**: O uso de Redux Toolkit (Thunks) estava resultando em variáveis globais de `loading` e `error` que bloqueavam a UI inteira devido ao compartilhamento do estado entre múltiplos componentes, além de causar condições de corrida entre actions pendentes.
**Decisão**: 
1. Redux Slices do projeto armazenam APENAS o estado de negócio do lado do cliente.
2. Não adicionamos `.pending` e `.rejected` aos `extraReducers`. O estado local de carregamento e as tratativas de erro são gerenciados **direto nos componentes** (usando `useState` e resolvendo as Promises do RTK via `unwrapResult()`).
**Próximo Passo Futuro**: Migrar inteiramente chamadas de API do RTK para React Query (`useQuery` / `useMutation`).

### 🚫 Arquivos Protegidos (NÃO MODIFICAR)
- **`src/hooks/usePagination.ts`**: Este hook é considerado estável e **não deve ser alterado**. Para forçar refetch após criação/edição, use `useQueryClient` diretamente no componente consumidor e chame `invalidateQueries({ queryKey: [key] })` com a mesma key passada para o hook.

### 🔔 Módulo de Notificações (TK-1462)
- **Backend**: Domínio `notifications` criado em `src/app/domains/notifications/` seguindo o padrão de módulo NestJS (controller + service + module + DTO).
  - O `NotificationHttpService` usa `RepositoryFactory` direto (sem injeção de dependência de repositórios), seguindo o mesmo padrão do `DashboardService`.
- **Frontend**: `notificationActions.ts` usa `createAsyncThunk` para integrar com `usePagination` (que exige um AsyncThunk conforme seu tipo). Funções utilitárias (mark, count) são funções simples com `apiClient`.
- **Sidebar**: Badge de não lidas carrega via `fetchUnreadCount` no mount — não usa Redux para evitar estado global de loading desnecessário.

### 📄 Detalhes do Procedimento (Híbrido)
- **Flexibilidade**: A página `ProvidedProcedureDetails.tsx` foi refatorada para carregar tanto Agendamentos (`Appointment`) quanto Execuções (`ProvidedProcedure`), detectando o tipo via `location.state` ou fallback automático.
- **UX de Reagendamento**: Implementado o sistema de arraste vertical no calendário com Snap de 30min e ativação por Long Press (500ms).

---
*Atualizado em: 2026-03-05*
