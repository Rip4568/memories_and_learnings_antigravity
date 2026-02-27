# 2026-02-03 - Double Scrollbar Bug (Nested Vuetify Components)

**Situation/Bug**:
Aparecimento de duas barras de rolagem (uma global e uma no sidebar/content) em layouts administrativos complexos no Vuetify. Isso geralmente ocorre quando componentes de layout (`v-navigation-drawer`, `v-main`) competem pelo controle do overflow.

**Solution/Insight**:

1. Aplicar `overflow: hidden !important` no container de conteúdo problemático (ex: `.v-navigation-drawer__content`).
2. Utilizar `v-container fluid` em modais e páginas internas para garantir que o conteúdo se ajuste ao viewport sem disparar scroll horizontal.
3. Centralizar o controle de scroll no elemento pai e evitar regras de `height: 100vh` em componentes filhos.

**Reference**:
[AdminLayout.vue](file:///c:/Users/user/Desktop/work/lintech/wai-sports/wai-sports-frontend/src/layouts/admin/AdminLayout.vue)
