# Prato do Povo - Frontend Memory

## Tech Stack
- **Framework**: React (Vite)
- **Language**: TypeScript
- **Styling**: Tailwind CSS (PostCSS)
- **Animation**: Framer Motion
- **Icons**: Lucide React
- **Hosting**: Vercel (Optimized)

## Architecture
- `src/components/ui`: Generic, reusable UI components (Buttons, Cards, Inputs).
- `src/components/features`: Specific feature components (QR Scanner simulation, Meal Card).
- `src/pages`: Page layouts (Landing, About, Demo).
- `src/assets`: Static assets (Images, Icons).

## Design System
- **Colors**:
  - Primary: State/Government colors (usually Blue/Green/Yellow mix, but will aim for a modern "Clean Governance" look - Emerald Green + Slate Blue).
  - Accent: Vibrant Orange for "Warmth/Food".
- **Typography**: Inter (Clean, Modern, Legible).
- **Vibe**: Trustworthy, Accessible, Modern, Efficient.

## Key Decisions
- **SPA on Vercel**: Uses `vercel.json` rewrites to handle client-side routing.
- **Micro-interactions**: Every interactive element must have a hover/active state.
- **Motion**: Use Framer Motion for scroll reveals and layout transitions to create a "premium" feel.
