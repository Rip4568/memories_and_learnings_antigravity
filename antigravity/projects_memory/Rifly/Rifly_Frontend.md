# Rifly - Frontend Memory

## 🛠 Tech Stack
- **Framework**: Next.js 16.1 (App Router)
- **Language**: TypeScript / React 19
- **Styling**: Tailwind CSS v4
- **Key Libraries**:
  - `framer-motion`: Para animações e interações (Creative UI).
  - `lucide-react`: Iconografia.
  - `@radix-ui/react-*`: Componentes base acessíveis (Dialog, Label, Select, Slot, Toast).
  - `clsx` & `tailwind-merge`: Utilitários para classes condicionalmente unidas.

## 🏗 Architecture & Structure
- **Pattern**: Component-Based / App Router (Next.js)
- **Key Directories**:
  - `/src/app`: Rotas da aplicação (roteamento baseado em diretórios).
  - `/src/components/ui`: Componentes base de UI (provavelmente gerados por uma CLI como shadcn/ui).
  - `/src/context`: Providers globais de estado (ex: RaffleContext).
  - `/src/lib`: Utilitários gerais.

## 🧠 Key Decisions & Patterns
- **Estilização**: Uso forte de CSS variables no `globals.css` para tematização (Light/Dark mode) usando HSL.
- **Animações (UX/UI)**: Foco em animações de alto impacto ("Wow" moments), micro-interações ("Magnetic effect", offsets de scroll) seguindo as heurísticas da skill Creative_UI.
- **Layout**: Uso do Tailwind contêiner padrão e foco em responsividade (Mobile-first).

## ⚠️ Known Issues / Technical Debt
- [ ] Landing page inicial gerada sem estilização profunda (MVP). Precisa de reformulação visual para um "Premium Feel".
