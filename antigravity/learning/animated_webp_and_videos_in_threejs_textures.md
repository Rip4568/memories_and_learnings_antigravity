# 2026-02-22 - WebP Animados e Vídeos em Texturas Three.js (React Three Fiber)

**Situation/Bug**:
O usuário tentou importar uma animação `.webp` gerada por AI (Veo3 -> EzGif) diretamente no `useTexture` do Three.js em um componente Billboard (`<meshBasicMaterial map={tex} />`). O resultado foi uma textura estática travada no primeiro frame, e o fundo "transparente" apareceu com ruídos/preto.

**Solution/Insight**:
Motores WebGL não renderizam GIFs ou WebPs animados nativamente. Eles leem o buffer do primeiro frame. Além disso, IA's de vídeo como o Veo3 não exportam em Alpha real (são fundos sólidos). Para contornar e animar em WebGL:

1. **Vídeo Nativ**: Renderizar via `VideoTexture` passando um `HTMLVideoElement` (quebra a transparência nativa se for renderizado num Plano em Z-index).
2. **Spritesheets (A Opção Ideal Indie):** O usuário deve converter o vídeo IA para uma folha de Sprites contínua (SpriteSheet.png) e no React-Three-Fiber, devemos fatiar a imagem atualizando o `.offset.x` e `.offset.y` do shader ou usar bibliotecas como `@react-three/drei` -> `<meshBasicMaterial map={texture} />` alterando a propriedade `{repeat}` e atualizando frame a frame no hook `useFrame`.
3. Evitar geradores de GIF para assets de jogo WebGL, optar por Grid Spritesheets de PNG puros sem fundo.

**Reference**:
[CompanionWorld Project - BattleArena / assets.ts]
