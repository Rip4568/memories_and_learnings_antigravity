# 2026-02-13 - Redux Toolkit: Separação de Actions (Thunks) e Slices

**Situation/Bug**:
Em PRs e revisões de código, foi identificado que manter `createAsyncThunk` dentro do mesmo arquivo do `createSlice` torna o arquivo excessivamente grande, dificulta a leitura e mistura responsabilidades de lógica de API com gerenciamento de estado.

**Solution/Insight**:
**Diretriz Global**: Sempre mover `createAsyncThunk` para um arquivo separado (ex: `whatsappActions.ts`) e importar as actions no slice (ex: `whatsappSlice.ts`).

1. O slice deve conter apenas a estrutura do estado, reducers síncronos e o tratamento dos `extraReducers`.
2. As actions devem focar na lógica de requisição (API), tratamentos de erro e payloads.
3. Isso melhora a modularidade, facilita testes unitários de lógica de API e mantém os arquivos concisos.

**Reference**:
[whatsappSlice.ts](file:///C:/Users/user/Desktop/work/lintech/bio-organize/bio-organize-frontend/src/stores/whatsapp/whatsappSlice.ts)
