# Ecommerce Supermarket - Project Memory

## 1. Tech Stack
- **Framework:** React (Vite)
- **Language:** TypeScript
- **Styling:** CSS Modules / Vanilla CSS (Scoped) with Design Tokens (Variables)
- **State Management:** React Context (for Cart/Admin state)
- **Routing:** React Router DOM
- **Icons:** Phosphor Icons / Lucide React
- **Charts:** Chart.js / Recharts (for Admin Dashboard) - *To be decided based on simplicity*

## 2. Architecture
- **Services:** Mocked services for Products, Sales, efficient data handling.
- **Components:** Atomic design inspiration (Atoms -> Molecules -> Organisms).
- **Layouts:** Separate layouts for Storefront and Admin.

## 3. Key Decisions
- **Mock Mode:** All data is local/mocked. No backend required.
- **Admin Access:** Direct link/button, no auth barrier (as per user request).
- **Aesthetics:** Premium, clean, "Supermarket" but modern (fresh colors, avoid generic blue).

## 4. Folder Structure
- `src/assets`: Images, global styles
- `src/components`: Shared components
- `src/layouts`: StoreLayout, AdminLayout
- `src/pages`: Home, Product, Cart, AdminDashboard, AdminProducts, etc.
- `src/services`: Mock data services
- `src/hooks`: Custom hooks
- `src/types`: TypeScript interfaces
