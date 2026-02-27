---
name: general-project-standards
description: DIRETRIZES OBRIGATÓRIAS DE DESENVOLVIMENTO. Ativar SEMPRE que houver solicitação de análise de código, implementação, refatoração ou criação de funcionalidades (Frontend/Backend). Contém as "Leis do Projeto".
---

## 🛡️ Regras Gerais (Backend & Processo)

**Consistência:** Mantenha consistência e padrões no código. Priorize reutilizar componentes e bibliotecas.
**Esclarecimento:** Caso fique em dúvida sobre algum aspecto, peça esclarecimentos antes de prosseguir. Crie um documento `question-*.md` com suas dúvidas e espere a resposta.

---

## 🎨 Regras de Frontend (Obrigatório)

Ao trabalhar com UI/Web, siga estritamente:
**Reutilização:** Sempre verificar e utilizar componentes já existentes no projeto antes de criar novos.
**Tipagem:** Sempre fazer uso de `models*.ts` ou `types.ts` para tipagem de dados.
**Ferramentas Internas:** Garantir uso das ferramentas já instaladas (ex: se tiver `react-query`, use-o).
**Data Fetching:** Evitar `fetch` nativo caso o projeto tenha **Axios** ou outra lib configurada.
**Design Tokens:** Sempre fazer uso de variáveis globais (CSS/SASS/Theme) para cores, fontes e espaçamentos. Não "hardcoded" valores mágicos.
**Componentes Globais:** Use componentes globais para botões, inputs, modais, tabelas. Se não existir, crie um componente global (ex: Modal de delete genérico).
**Generics e Interfaces:**
    * Reaproveitar tipagens existentes (interfaces, enums).
    * Atualizar tipagens se necessário.
    * Garantir genéricos. Exemplo:
        ```typescript
        export interface PaginatedResponse<T> { data: T[]; total: number; page: number; limit: number; }
        ```
**Vue.js (se aplicável):** Estrutura: `<script>` em cima, `<template>` embaixo.
**Fidelity (Figma/Design):** Se fornecido print ou link, siga fielmente design, cores e fontes. O espaçamento pode ser adaptado para harmonia visual, mas a estética deve ser idêntica.