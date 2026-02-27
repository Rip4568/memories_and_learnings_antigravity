# Mattus Móveis Planejados - Memória do Projeto

## 1. Visão Geral
**Tipo**: Landing Page Institucional
**Status**: Concluído (v1.0)
**Data**: Fevereiro 2026

## 2. Tech Stack
- **Framework**: React 18 + Vite
- **Estilização**: Tailwind CSS v4 + Motion (Framer Motion)
- **Roteamento**: React Router Dom v7
- **Ícones**: Lucide React

## 3. Arquitetura e Decisões
- **Design System "Dark & Gold"**:
  - Cores: `#0a0a0a` (Background), `#d4af37` (Gold Accent).
  - Tipografia: *Montserrat* (Sans) + *Playfair Display* (Serif).
- **Estrutura de Pastas**:
  - `components/layout`: Navbar, Footer.
  - `components/sections`: Hero, About, Projects, etc.
  - `components/ui`: Componentes atômicos (Button).
  - `pages/`: Páginas legais (Privacy, Terms).

## 4. Soluções Específicas
### Tailwind v4
O projeto utiliza a versão 4 do Tailwind. A configuração é feita exclusivamente via plugin Vite (`@tailwindcss/vite`) e CSS variables no `@theme` block do `index.css`.
*Atenção*: Não adicionar `postcss.config.js` ou `tailwind.config.js` legados.

### Responsividade e Mobile
- **Full Bleed Controlado**: Imagens de projetos têm padding lateral no mobile para evitar corte visual abrupto.
- **Elementos Flutuantes**: Cards de citação (`About.tsx`) mudam de `absolute` para `relative` no mobile para evitar sobreposição de conteúdo.

### SEO e Social
- Implementação de **Open Graph** e **Twitter Cards**.
- **Schema Markup**: `LocalBusiness` JSON-LD para indexação local.
- **Correção de Domínio**: Meta tags configuradas para `mattusmoveis.vercel.app` para garantir preview correto no WhatsApp.
