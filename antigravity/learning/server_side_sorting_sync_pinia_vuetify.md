# 2026-02-03 - Server-Side Sorting Sync (Pinia + Vuetify)

**Situation/Bug**:
Ao implementar ordenação no backend, a UI pode ficar dessincronizada se o evento `@update:options` do `v-data-table-server` não for corretamente mapeado para os filtros da store antes do fetch.

**Solution/Insight**:
Sempre capturar `options.sortBy` no evento de atualização, extrair a chave e a ordem (asc/desc), e atualizar o estado da store **antes** de disparar a requisição de novos dados. Isso garante que a paginação e a ordenação sejam tratadas como um único conjunto de filtros atômicos.

**Reference**:
[UserList.vue](file:///c:/Users/user/Desktop/work/lintech/wai-sports/wai-sports-frontend/src/components/admin/users/UserList.vue)
