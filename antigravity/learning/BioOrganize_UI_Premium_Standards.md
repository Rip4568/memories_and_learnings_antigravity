# Aprendizado: Padronização de Modais e Botões Premium (Bio-Organize)

## Contexto
Durante a implementação da Task-1792 (Logout), identificou-se a necessidade de rigorosa padronização visual para componentes de diálogo e botões, garantindo que a experiência mobile (simulando app nativo) e desktop sejam consistentes e de alta qualidade.

## Aprendizados & Regras de Ouro

### 1. Estrutura de Dialogs (Modais)
- **Mobile**: Deve ser `fullScreen={isMobile}`. O título deve ficar em um `Box` com borda inferior (simulando Header) e os botões de ação devem ocupar `flex: 1` para preencher a largura da tela.
- **Desktop**: Deve ser centralizado, com `maxWidth` definido (ex: `xs` ou `sm`) e bordas arredondadas (`borderRadius: 3`).
- **Comportamento**: O `PaperProps` deve ajustar o `borderRadius` dinamicamente entre 0 (mobile) e 3 (desktop).

### 2. Botões Premium
- **Animações**: Botões em desktop devem ter translação no hover (`transform: translateY(-2px)`) e sombras dinâmicas.
- **Hierarquia de Intenção**: 
    - Ações de **baixa intenção** ou perigosas (como Logout) devem ficar em locais de mais difícil acesso no mobile (final da página).
    - No desktop, devem seguir a ordem lógica de leitura, geralmente à esquerda de ações primárias.
- **Estilo**: Evitar botões simples do MUI sem estilização. Usar `alpha` para cores de fundo suaves no hover e `boxShadow` para profundidade.

### 3. Evitar Erros Comuns
- **ReferenceError (alpha)**: Sempre garantir a importação de `alpha` de `@mui/material` ao usar cores com transparência.
- **Visibilidade**: Testar sempre em modo responsivo (ex: iPhone 12 no Chrome) para garantir que o `Dialog` não fique escondido ou mal posicionado.

### 4. Lógica de Drag-and-Drop no Calendário
- **Ativação**: O tempo ideal para Long Press é **500ms**. Mais que isso gera percepção de lentidão; menos gera ativações acidentais.
- **Prevenção de Cliques**: Use um `wasDragging` ref para impedir que o `onClick` (abrir modal) dispare logo após o término de um arraste (drag).
- **Snap**: Em grades de tempo, use unidades fixas (ex: 40px para 30min) para o cálculo de "saltos" de horário.

### 5. Agrupamento de Datas e Timeline
- **Chave de Agrupamento**: Nunca agrupar por objetos `Date` ou instâncias diretas. Use `dayjs(date).format("YYYY-MM-DD")` como chave para garantir consistência no fuso horário local e evitar que agendamentos "pulem" de dia.
- **Labels**: Converta a chave string de volta para DayJS apenas no momento de exibir o título da seção.

### 6. Páginas de Detalhes Híbridas
- **Contexto**: Páginas como `ProvidedProcedureDetails` devem ser resilientes. 
- **Estratégia**: Detectar via `location.state` ou via tentativa/erro (fallback) se o ID pertence a um agendamento puro ou a um procedimento realizado, adaptando a busca (API) e a UI (Galeria/Preços) dinamicamente.

---
*Atualizado em 05/03/2026*
