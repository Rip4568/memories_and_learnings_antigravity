---
name: git-standards
description: DIRETRIZES PARA GERAÇÃO DE COMMITS HUMANIZADOS. Siga sempre este padrão ao sugerir ou realizar commits no Git.
---

## 📝 Padrões de Commit Humanizados

Os commits devem ser escritos em português (pt-br), de forma humanizada, clara e direta. **Não utilize prefixos de Conventional Commits (feat:, fix:, chore:)** a menos que explicitamente solicitado.

### Diretrizes
1.  **Idioma**: Sempre em **pt-br**.
2.  **Tom**: Humanizado, explicativo e direto.
3.  **Estilo**: Inicie com o verbo no presente (ex: adiciona, corrige, otimiza, configura).
4.  **Tamanho**: Poucas linhas, mesmo para alterações complexas. Seja sintético.
5.  **Agrupamento**: Para conjuntos massivos de mudanças (como um CRUD inteiro), descreva o domínio e a lógica de negócio envolvida de forma resumida.

### Exemplos de Mensagens
-   `adiciona logs para depurar bugs`
-   `otimiza consultas e problema de query n+1`
-   `configura appserviceprovider para fazer uso do singleton e deixar as instancias das classes mais otimizadas`
-   `CRUD completo de clientes e suas devidas logicas e regras de negocios`

### Regras de Otimização de Mensagem
-   Evite gerúndio ("adicionando", "corrigindo"). Use o presente do indicativo ("adiciona", "corrige").
-   Se houver mudanças em múltiplos arquivos, foque na intenção e no resultado para o negócio ou performance.
-   Mantenha a mensagem em letras minúsculas (opcional, mas preferível conforme exemplos do usuário).
