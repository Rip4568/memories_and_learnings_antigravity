# Construction Simulator 3D - NextJS

## Tech Stack & Libraries
- **Frontend Core**: Next.js (App Router), React, TypeScript.
- **3D Engine**: Three.js, @react-three/fiber, @react-three/drei.
- **Global State / Game State**: Zustand.
- **Styling**: Tailwind CSS, Framer Motion (micro-interaçoes UI).
- **Embalagem Multiplataforma**: Arquitetura pensada primariamente como PWA responsiva ou futura embalagem em Capacitor.js para Android/iOS nativo.

## Folder Structure & Architecture (Planejada)
- `src/components/3d/`: Lógica da Cena, Componentes 3D, Terrenos, Câmera e Modelos.
- `src/components/ui/`: Overlays HTML/Tailwind sobre o canvas 3D (Menus, HUD de dinheiro, Paineis de Compra).
- `src/store/`: `useGameStore.ts` para dados persistentes do jogador e lista de propriedades renderizadas.
- `src/lib/economy/`: Regras de negócio, cálculos baseados no nível e rentabilidade passiva.

## Key Decisions & Trade-offs
1. **React Three Fiber (R3F)**: Escolhido por ser declarativo e integrável perfeitamente ao Next.js, permitindo renderização fácil dos estados do Zustand vinculados à malha.
2. **Zustand vs Redux Toolkit**: Estado do jogo requer mutações rapidas com menor boilerplate possível, podendo também ler states fora do loop do React (ex: hooks transientes ou listeners de tick globais para gerar renda a cada segundo).
3. **Claude Sonnet 4.6 Delegation**: Funcionalidades complexas (App Store Integrations e Matemática Econômica avançada) terão seus skeletons/interfaces preparadas aqui para posterior implementação pela IA supracitada.

## Padrões Adotados (Baseado nos Learnings Globais)
- Criação de Task lists sistemáticas, revisões estritas antes e após entregas de códigos.
- Design responsivo de alto padrão visual (Premium e Moderno, sem "coringa" puro como azul ou vermelho primário, usando harmonização HSL ou dark modes refinados).
- Código autoexplicativo usando pt-br na documentação da arquitetura e em logs da memória.
