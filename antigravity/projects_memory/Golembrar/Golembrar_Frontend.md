# Golembrar - Frontend

## Tech Stack & Libraries
- **Framework**: Angular 17
- **Workspace Tooling**: NX (Monorepo architecture tooling)
- **UI Components**: PrimeNG 17 / PrimeIcons 7 / PrimeFlex 3
- **State Management / Data Fetching**: `@ngneat/query` (TanStack Query adaptation for Angular) e RxJS
- **Styling**: SCSS (em conjunto com classes utilitárias do Tailwind/PrimeFlex)
- **Analytics & Tracking**: PostHog JS
- **Testes**: Jest e Cypress

## Folder Structure & Architecture
- **Estrutura Core**: O código principal reside em `src/app/core/`, dividindo responsabilidades através de pastas bem definidas:
  - `pages/`: Componentes que representam telas inteiras (Smart Components / Routable Components).
  - `components/`: Componentes reutilizáveis de UI (Dumb Components).
  - `services/`: Serviços injetáveis para requisições HTTP, integrações de APIs e regras de negócio.
  - `models/` e `@types/`: Tipagens TypeScript, interfaces genéricas e DTOs.
  - `guards/` e `interceptors/`: Controle de rota, injeção de tokens de autenticação via HTTP.
  - `constants/` e `utils/`: Funções auxiliares sem estado e variáveis globais estáticas.

## Design Patterns Used
- **Smart / Dumb Components**: Separação clara entre as `pages` (que chamam serviços de dados e mantêm o estado principal da rota) e os `components` base.
- **Reactive Programming**: Uso extenso de RxJS para streams de eventos e respostas assíncronas assossiado à arquitetura de Componentes Standalone (Angular 17+).
- **Server State Management**: Utilização do `@ngneat/query` para abstrair as requisições, realizando fetch, cacheamento, background updates e hooks otimizados, o que reduz bastante a quantidade de código manual no RxJS genérico para controle de loading e erros.

## Key Decisions & Trade-offs
- **Uso do NX no Frontend**: Promove facilidade de build, execução de testes focados e linting apenas nos componentes que foram iterados, aumentando drasticamente a escala do frontend.
- **PrimeNG como Base UI**: Fornece componentes ricos ("Out of the Box") de forma madura para Angular, permitindo que o time foque apenas na refinação / personalização (temas).
- **Arquitetura Standalone**: Total flexibilidade provida pela versão 17 do Angular, evitando arquivos `*.module.ts` complexos onde não é necessário e melhorando o lazy-loading da aplicação.
