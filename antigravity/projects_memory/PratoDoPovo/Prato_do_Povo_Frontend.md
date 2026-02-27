# Prato_do_Povo_Frontend

## Tech Stack & Libraries

- **Core**: React 19, Vite, TypeScript
- **Styling**: Tailwind CSS v4, `clsx`, `tailwind-merge`
- **Animations**: Framer Motion
- **Icons**: Lucide React
- **Routing**: React Router DOM v7

## Folder Structure & Architecture

- `src/components/ui`: Componentes genéricos e reutilizáveis (Button, Section, Card, Badge).
- `src/components/layout`: Componentes estruturais (Header).
- `src/pages`: Páginas da aplicação (Ex: Landing.tsx).
- `src/lib/utils.ts`: Funções utilitárias como `cn` para mesclar classes do Tailwind.
- O projeto usa uma abordagem moderna e modularizada com componentes funcionais e hooks.

## Design Patterns Used

- Componentização baseada no padrão Radix UI / shadcn/ui (revelado pelo uso de `cn` com `clsx` e `tailwind-merge` em componentes da pasta `ui`).
- Animações baseadas em scroll e interações via Framer Motion, criando uma experiência "Premium" sugerida no guideline.

## Key Decisions & Trade-offs

- Uso de design "Glassmorphism" e temas escuros (dark mode parcial/híbrido).
- Responsividade via classes utilitárias do Tailwind.
- Interface orientada a dados públicos (Prefeitura/CadÚnico), focada em acessibilidade e facilidade de entendimento.
