# 2026-03-09 - Node.js Version Conflict & Vite MIME Type Error

**Situation/Bug**: 
O frontend do projeto Bio-Organize (React 19 + Vite) começou a apresentar erros de "Failed to load module script: Expected a JavaScript-or-Wasm module script but the server responded with a MIME type of 'text/html'". Esse erro ocorria em diversos arquivos `.tsx`, `.ts` e `.schema.ts`, indicando que o servidor de desenvolvimento do Vite estava retornando o `index.html` (fallback 404) para arquivos de script válidos.

**Solution/Insight**: 
O problema foi causado pelo uso de uma versão incompatível do Node.js. O projeto foi configurado para rodar no **Node 20**, mas o ambiente estava utilizando o **Node 22**. Ao retornar para a versão 20 (LTS recomendada no `package.json`), o Vite voltou a processar e servir os módulos corretamente.

**Reference**: 
`package.json` (engines: { "node": ">=20" }) e Logs do Navegador.
