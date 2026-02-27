# Projeto: WaiSports - Frontend

## Tech Stack & Libraries
- **Core:** Vue 3, TypeScript, Vite
- **Styling:** Vuetify 3 (Atomic CSS / Utility Classes focus)
- **State Management:** Pinia
- **Payment:** Stripe.js
- **API:** Axios (com interceptors para Auth e Refresh Token)

## Folder Structure & Architecture
- `src/models/`: Centralização de todas as interfaces e tipos de dados do domínio.
- `src/utils/`: Utilitários globais (e.g., `api.ts`, `date.ts`).
- `src/stores/`: Lógica de estado persistente por módulo (admin, auth, subscription).
- `src/views/admin/`: Componentes e telas do painel administrativo.

## Key Decisions & Trade-offs
### 1. Refatoração de CSS para Vuetify Utility Classes
- **Decisão:** Substituir blocos `<style scoped>` por classes como `ga-n`, `ma-n`, `rounded-lg`, etc.
- **Racional:** Redução de código boilerplate, consistência visual imediata com o framework e facilidade de manutenção de temas (dark/light mode).

### 2. Centralização de Modelos
- **Decisão:** Extinguir a pasta `interfaces/` e mover tudo para `models/`.
- **Racional:** Eliminar confusão entre definições parciais e garantir que componentes e stores usem a mesma fonte de verdade tipada.

### 3. Verificação de Build em Massa
- **Prática:** Usar o log do `vue-tsc` para resolver erros de tipagem após mudanças estruturais.

### 4. Gestão de Scroll e UX
- **Decisão:** Implementar `window.scrollTo({ top: 0, behavior: 'smooth' })` ao navegar de volta para o menu principal no chat.
- **Racional:** Evitar que o usuário inicie um novo fluxo de chat no final da página, garantindo que o context menu esteja visível no topo.

### 5. UI Premium e Padronização de Tabelas
- **Decisão:** Padronizar headers de tabelas administrativas com `text-transform: uppercase`, `font-weight: 700` e fundo sutil `#f8fafc`.
- **Racional:** Proporcionar uma estética "SaaS Premium" e melhorar a escaneabilidade de dados complexos.

## Aprendizados Recentes
- **FormatDate:** Sempre centralizar formatação de datas em `src/utils/date.ts` para evitar variações regionais e redundância de código.
- **Double Scrollbar:** Corrigido via `overflow: hidden !important` em `AdminLayout.vue` para forçar o scroll nativo do container principal.
- **Data Mapping (Legacy Enums):** Mapear enums antigos (como 'beginner' vindo de seeds) para os novos valores em português ('iniciante') no `populateFormData` para evitar erros de validação do backend.
- **Date Inputs:** Uso de `.substring(0, 10)` ao popular campos de data editáveis (como expiração de assinatura) para compatibilizar o formato ISO do backend com o formato `YYYY-MM-DD` exigido pelos inputs HTML.
