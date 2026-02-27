# Ecommerce Frontend Memory

## Tech Stack
- **Framework**: React + Vite
- **Styling**: Tailwind CSS v4 (configured via `@tailwindcss/vite`)
- **Icons**: Lucide React
- **Router**: React Router DOM v6+

## Architecture & Configuration
### Tailwind CSS v4
- **Configuration**: Uses `@tailwindcss/vite` plugin in `vite.config.ts`.
- **CSS Entry**: `src/index.css` with `@import "tailwindcss";`.
- **Note**: No `tailwind.config.js` or `postcss.config.js` is needed for this setup.

## Key Decisions
- **Styling Strategy**: Utility-first with Tailwind. Custom CSS variables in `index.css` for theme colors.
