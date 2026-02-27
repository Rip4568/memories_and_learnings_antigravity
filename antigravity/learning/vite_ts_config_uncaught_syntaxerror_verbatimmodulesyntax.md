# 2026-02-21 - Vite TS Config, Uncaught SyntaxError (VerbatimModuleSyntax)

**Situation/Bug**:
Ao inicializar o Vite com TS `template react-ts` atual, a regra `verbatimModuleSyntax` fica ativada no tsconfig. Isso causa `Uncaught SyntaxError: The requested module X does not provide an export named Y` se tentarmos importar um Type.

**Solution/Insight**:
Sempre use explícito `import type { ElementType }` quando for importar tipagens limpas em React/Vite. Modificar a sintaxe resolve imediatamente o bug de build.

**Reference**:
[CompanionWorld Project - Vite Setup]
