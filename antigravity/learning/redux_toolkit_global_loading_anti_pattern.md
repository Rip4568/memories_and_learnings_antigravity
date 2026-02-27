# 2026-02-24 - Redux Toolkit: Anti-pattern de Loading/Error Global

**Situation/Bug**: 
A aplicação possuía um anti-pattern no gerenciamento de estado global com Redux Toolkit. As Slices estavam gerenciando seus próprios estados de `loading`, `error` e realizando side-effects como redirecionamentos nos `extraReducers`. Isso causava sérios problemas de UX:
1. Telas inteiras eram recarregadas ou bloqueadas por spinners mesmo quando uma única sub-requisição estava carregando, devido ao estado global de `loading` compartilhado por múltiplos componentes consumindo a mesma Slice.
2. Comportamentos indesejados e gargalos, pois múltiplas chamadas à mesma API sobrescreviam o estado de `loading` ou `error` umas das outras (race conditions visuais).

**Solution/Insight**: 
**Diretriz Global**: Redux Slices devem armazenar APENAS o estado dos dados reais (o banco de dados do lado do cliente). Comportamentos e estados dependentes do ciclo de vida da UI (`loading`, `error`, `success`, side-effects) devem ser mantidos sempre **mais próximos de onde são usados**, no estado local dos componentes React.

1. Remover `.pending` e `.rejected` dos `extraReducers` das Slices.
2. Em chamadas com Redux Thunks, utilizar o `unwrapResult` do `@reduxjs/toolkit` diretamente nos componentes (ou custom hooks).
3. Gerenciar estados de `loading` com `useState(false)` ou, de modo mais moderno, transicionar chamadas assíncronas para bilbiotecas voltadas para gerenciamento de dados de servidor como **React Query** (TanStack Query) ao invés de usar Redux Thunks para API calls.

**Reference**: 
Refatorações realizadas em múltiplos Slices (ex: `authSlice.ts`, `clinicSlice.ts`) e componentes que consumiam `loading` global (ex: `Login.tsx`, `Profile.tsx`, `Onboarding.tsx`).
