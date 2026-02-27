# Companion World - Architecture & Memory

## Tech Stack & Libraries
- **Framework**: Vite + React + TypeScript 
- **Styling**: Tailwind CSS v4 (Glassmorphism e paletas premium estritas baseadas em HSL e tons vibrantes).
- **Global State**: Zustand. Utilizado por dispensar Providers rígidos e otimizar re-renders para stores lógicos de games.
- **Renderer 3D**: React Three Fiber (R3F) e Drei.
- **Animações / UX**: Framer Motion (para transições UI fluídas) + Three.js Animations para a cena.

## Folder Structure & Architecture
O projeto segue uma hierarquia híbrida (UI vs Engine):
- `src/components/ui`: Tudo o que é DOM, HTML, divs, Tailwind e Framer Motion. Sobreposto ao canvas em absolutismo.
- `src/components/canvas`: Exclusivo para código Three.js (`<mesh>`, `<group>`, shaders).
- `src/store`: Fatias de domínio usando Zustand (`useGameStore` para player stats, `useCompanionStore` para os monstros).
- `src/utils/gameLogic`: Funções puras que lidam com cálculo de breeding, algoritmos de mutação probabilística de stats.

## Design Patterns Used
- **Dumb/Smart Components (UI)**: Formulários de game e modais não possuem lógica de estado, eles apenas chamam métodos da store (ex: `breed(id1, id2)`).
- **Billboarding / 2.5D**: Devido à não utilização inicial de modelos GLTF animados complexos, o jogo utilizará artes 2D geradas por IA aplicadas como texturas em um plano 3D que "segue" constantemente a câmera, garantindo alta qualidade gráfica e vibe retrô-moderna 2.5D.
- **Partículas Elementais**: Ao invés de redesenhar os sprites 2D para efeitos, cada `<CompanionSprite />` injeta sistemas simples de partículas 3D no Three.js baseados no `element` associado (ex: Gelo emite partículas esbranquiçadas para baixo).

## Key Decisions & Trade-offs
1. **Zustand ao invés de Redux/Contexto**: Context API em React causa re-renders massivos difíceis de controlar em cenários de Canvas e Games tick-based. Zustand previne isso.
2. **Sprites 2D ao invés de GLTF**: Um GDD focado em dezenas de combinações (Fogo+Gelo=Lava) exigiria dezenas de modelos de rigging complexos. Utilizar spritesheet/artes individuais e magia 3D resolve o MVP mantendo a alta estética.
3. **Tailwind v4 Setup Híbrido**: Como o UI requer aparência moderna de jogo, estilizaremos componentes para parecer flutuantes, utilizando suporte CSS in-js minimalista quando necessário, mas preservando utilities em sua maioria. Evitando at-rules legados, acompanhando lições no LEARNING.md.

---

## IA Asset Generation Prompts (NanoBanana Style Guide)
Para manter a total **coerência e estética premium** entre todos os monstros gerados no jogo, adotamos o seguinte template paramétrico para o prompt de imagem:

**Template Base**:
> "A high quality 3D isometric render of a cute `[ELEMENT/CONCEPT]` monster creature. `[VISUAL FEATURES]`. Axie Infinity and Pokemon style, cute eyes, premium game design aesthetics, vibrant colors, dark background."

**Game Design: Diferença entre Lava e Magma**:
- **Magma (Fogo + Terra)**: Focado em peso (tank), defesa massiva e brutalidade. Visualmente: rocha vulcânica extremamente escura e sólida, com rachaduras profundas que emitem um brilho laranja sombrio. Ele pertence ao ambiente subterrâneo lento e imovel.
- **Lava (Fogo + Gelo)**: Focado em velocidade térmica, agressão e puro dano explosivo (glass cannon). Resultado de um forte choque térmico vulcânico. Visualmente: estado puramente líquido, incandescente, ágil, gotejante e altamente instável.
