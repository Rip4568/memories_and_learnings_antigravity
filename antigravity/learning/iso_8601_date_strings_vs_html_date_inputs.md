# 2026-02-03 - ISO 8601 Date Strings vs HTML Date Inputs

**Situation/Bug**:
Ao carregar datas de uma API (ex: Laravel/PostgreSQL), o formato costuma ser ISO 8601 completo (`YYYY-MM-DDTHH:mm:ss.sssZ`). No entanto, o elemento `<input type="date">` do HTML (incluindo o `v-text-field` do Vuetify com esse type) exige o formato estrito `YYYY-MM-DD`. Se for passado o formato completo, o input permanece vazio ou inválido.

**Solution/Insight**:
Ao popular formulários com dados vindos do backend, sempre trate a string da data usando `.substring(0, 10)` or `.split('T')[0]`. Isso garante que apenas a parte da data seja enviada ao input, mantendo a compatibilidade e permitindo a edição correta pelo usuário.

**Reference**:
[UserEditModal.vue](file:///c:/Users/user/Desktop/work/lintech/wai-sports/wai-sports-frontend/src/components/admin/users/UserEditModal.vue)
