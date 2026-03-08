# 2026-03-08 - React & MUI: Pickers Imperativos com forwardRef

**Situation/Bug**: 
Ao migrar componentes de input nativos (como `<TextField type="date" />`) para custom pickers usando Material-UI (como `AppDatePicker` envolvendo `TextField` + `Dialog`/`Popover`), precisávamos expor uma API imperativa (`open()`) para que o componente-pai pudesse acionar a abertura do calendário/relógio diretamente, sem intermediários.
Após refatorar o Functional Component para `forwardRef`, surgiu o erro:
`TypeError: Component is not a function` no React Router (caso alguma inconsistência de export ou ciclo de vida acionasse um mal comportamento) e também alertas de TypeScript no `useImperativeHandle` reclamando de incompatibilidade de assinatura entre `MouseEvent<Element>` e `MouseEvent<HTMLElement>`.

**Solution/Insight**: 
1. **Tipagens Flexíveis em Eventos**: Quando o componente pai chama `ref.current?.open(e)` de um elemento indireto (ou stopPropagation é necessário), definir o parâmetro de evento do método exposto via `useImperativeHandle` como genérico ajuda a evitar bugs de compatibilidade estrita no React (ex: `open: (event?: React.MouseEvent<any>) => void;`).
2. **Integração Encapsulada Sem Modal Intermediário**: Colocamos os pickers ocultos (`display: "none"`) ou integrados na mesma árvore e chamamos o método `open()` diretamente da seção clicável (ex: `DetailSection`). Isso elimina fluxos com modais intermediários (ex: Modal de Detalhes -> Modal de "Deseja Reagendar?" -> Fecha Modal -> Abre Modal do DatePicker), substituindo por uma abertura in-loco (Modal de Detalhes -> Abre Dropdown/Popover do DatePicker). O design fica muito mais fluido.
3. **Erros de "Component is not a function"**: Este erro frequentemente acontece quando um componente React exportado como default é importado incorretamente, ou quando a conversão de um componente funcional simples para um componente envolvo em `forwardRef` sofre anomalias no *Fast Refresh* do Vite. Um *reload* completo muitas vezes clarifica se é só cache ou exportação inválida real.

**Reference**: 
- `AppDatePicker.tsx`, `AppTimePicker.tsx` (Bio-Organize Frontend)
- `AppointmentDetailsModal.tsx` (Bio-Organize Frontend)
