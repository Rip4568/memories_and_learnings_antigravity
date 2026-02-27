# Lições de Refatoração: Tailwind v4 + Vuetify + SASS

Este arquivo documenta os desafios técnicos e as soluções encontradas durante a tentativa de migração para Tailwind v4 em um projeto híbrido.

## 1. Conflito entre Tailwind v4 e Compiladores SASS
**Problema:** Incluir `@import "tailwindcss";` e blocos `@theme` diretamente em arquivos `.scss` que contêm lógica SASS (variáveis, mixins ou imports de terceiros como Vuetify) causa erros de compilação. O compilador SASS tenta resolver variáveis antes que o Tailwind processe as diretivas, resultando em "Undefined variable".

**Solução:** 
- Separe o Tailwind v4 em um arquivo `.css` puro (ex: `index.css`).
- Use o plugin `@tailwindcss/vite` no `vite.config.ts`.
- Importe o arquivo CSS no ponto de entrada (`main.ts`) antes ou depois do SCSS, dependendo da precedência desejada.

## 2. Sincronização de Temas (Dark/Light)
**Problema:** Classes utilitárias do Tailwind com cores fixas (ex: `bg-white`, `text-slate-900`) quebram a responsividade ao tema do Vuetify, especialmente em Dashboards que alternam para Dark Mode.

**Solução:** Mapeie os tokens do Tailwind para as variáveis CSS expostas pelo Vuetify:
```css
@theme {
  --color-primary: var(--v-theme-primary);
  --color-surface: var(--v-theme-surface);
  --color-background: var(--v-theme-containerBg);
  --color-dark: var(--v-theme-darkText);
  --color-light: var(--v-theme-lightText);
}
```

## 3. Fragilidade de Ferramentas de Edição Automatizada
**Insight:** Edições massivas e não-contíguas via ferramentas de "replace" em templates Vue complexos podem causar corrupção de tags (malformed XML/HTML) se não houver verificação rigorosa entre os blocos. 
**Best Practice:** Para refatorações estruturais pesadas, prefira reescrever o arquivo completo ou validar a sintaxe após cada bloco de edições contíguas.

## 4. Ordem de Instalação de Plugins Vite
**Observação:** O plugin `@tailwindcss/vite` deve ser invocado como uma função `tailwindcss()` na lista de plugins, e o pacote deve ser instalado como `devDependencies`.
