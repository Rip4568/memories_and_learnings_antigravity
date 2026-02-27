# Mattus Móveis Planejados - Landing Page

## 1. Tech Stack & Libraries
- **Framework**: React 18 + Vite
- **Styling**: Tailwind CSS (via PostCSS)
- **Animation**: Framer Motion
- **Icons**: Lucide React
- **Language**: TypeScript

## 2. Folder Structure & Architecture
- `src/components/layout`: Componentes estruturais (Navbar, Footer).
- `src/components/sections`: Seções da landing page (Hero, About, Values, Projects, Contact).
- `src/components/ui`: Componentes reutilizáveis (Button).
- `src/index.css`: Definições globais de estilo e variáveis CSS para cores premium.

## 3. Design Decisions & Trade-offs
- **Estética Premium**: Optou-se por um fundo dark (`#0a0a0a`) com acentos dourados (`#d4af37`) para transmitir sofisticação, alinhado com o mercado de móveis planejados de alto padrão.
- **Performance vs Visual**: Uso extensivo de `framer-motion` para micro-interações e scroll reveal. trade-off aceitável para uma landing page de impacto visual, mas requer cuidado com bundle size (otimizado via tree-shaking do Vite).
- **Responsividade**: Mobile-first, com menu hambúrguer animado e grids flexíveis (Masonry layout adaptado via CSS Grid/Flex).

## 4. Key Learnings
- A integração de `framer-motion` com componentes puramente funcionais requer atenção na passagem de props (`forwardRef` ou filtragem de props não-DOM em wrappers).
- O uso de `backdrop-blur` na Navbar deve ser acompanhado de um fundo semi-transparente para legibilidade em todas as seções.
