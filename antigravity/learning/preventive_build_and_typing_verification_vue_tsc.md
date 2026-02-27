# 2026-02-02 - Verificação Preventiva de Build & Tipagem

**Situation/Bug**:
Refatorações extensas (como troca de CSS para classes utilitárias ou migração de interfaces) podem introduzir erros de tipagem silenciosos ou regressões de propriedade em componentes Vue.

**Solution/Insight**:
Utilizar o comando `npx vue-tsc --noEmit > build_output.txt 2>&1` para capturar todos os erros de build e sintaxe em um arquivo de log. Isso permite uma análise sistemática e resolução em lote de erros de tipagem sem depender apenas da IDE.

**Reference**:
[WaiSports Refactor Phase 6 & 7]
