# Build Optimization Skill (Node.js/Vite/Vue)

Este documento serve como guia de melhores práticas e contexto para otimização de builds no projeto Wai-Sports.

## 1. Diretrizes de Build (VPS & Local)
- **Ignorar Type-Check no Build:** O comando `npm run build` deve rodar apenas `vite build`. A checagem de tipos (`vue-tsc`) deve ser feita localmente no VS Code ou em um pipeline de CI separado, nunca travando o deploy na VPS.
- **Incremental Builds:** Priorizar ferramentas que suportem cache de build (Turborepo ou Nx) para evitar reprocessar módulos inalterados.
- **Docker Cache (se aplicável):** Sempre separar a instalação de dependências (`COPY package.json`) da cópia do código fonte para aproveitar as camadas de cache.

## 2. Estratégia de Assets (Performance)
- **Pasta /public:** Arquivos que não precisam de transformação (Fontes, Ícones de PWA, Imagens de Carrossel > 500kb) devem residir na pasta `/public` para evitar o processamento do Rollup.
- **Code Splitting:** O arquivo principal `index.js` deve ser monitorado. Caso ultrapasse 2MB, aplicar `manualChunks` no `vite.config.ts` para separar fornecedores (vendor).

## 3. Comandos Otimizados
| Objetivo | Comando |
| :--- | :--- |
| Build Rápido (VPS) | `vite build` |
| Build com Cache | `turbo run build` |
| Analisar Bundle | `npm run preview` (com plugin de visualização) |

## 4. Stack de Otimização Sugerida
- **Engine:** esbuild (nativo do Vite).
- **Cache:** Turborepo.
- **Deployment:** Git Pull -> NPM Install (se package.json mudou) -> Vite Build -> Rsync/Serve.

## 5. Notas Específicas do Projeto
- O projeto possui ~4000 módulos.
- O tempo base de build sem otimização é de ~31s. 
- Meta de build incremental: < 5s.