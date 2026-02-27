# 2026-02-10 - Tailwind v4 Config Conflict (Vite)

**Situation/Bug**:
Ao usar Tailwind CSS v4 com Vite, configurações legadas (postcss.config.js, tailwind.config.js) ou imports complexos no CSS podem impedir o carregamento dos estilos base (Preflight), quebrando layout e cores.

**Solution/Insight**:
Para v4:

1. Remova `postcss.config.js` e `tailwind.config.js`.
2. Use `@tailwindcss/vite` no `vite.config.ts`.
3. No CSS, use APENAS `@import "tailwindcss";` e `@theme { ... }`. Evite misturar com `@tailwind base/components/utilities` antigos a menos que estritamente necessário e suportado.
4. Reinicie o servidor de dev para limpar cache.

**Reference**:
[Mattus Móveis Project - Index.css Fix]
