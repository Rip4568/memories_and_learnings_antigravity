# 2026-02-21 - Rules of Hooks em React-Three-Fiber

**Situation/Bug**:
Tentar invocar o hook `useTexture` do `@react-three/drei` de forma condicional (`if (element === 'lava) { useTexture(...) }`) quebra seriamente as regras do React, causando loops ou crashes.

**Solution/Insight**:
Crie um sub-componente estrito (ex. `<TexturedSprite url={url} />`) que faça o call do `useTexture` isolado e incondicional, e apenas chame o sub-componente via uma expressão condicional no JSX parent. JSX respeita a montagem de componentes.

**Reference**:
[CompanionWorld Project - WorldCanvas]
