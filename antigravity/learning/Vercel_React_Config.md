# Configuração Vercel + React (Vite/Next.js)

## Visão Geral

A Vercel utiliza uma abordagem "Zero Config" para a maioria dos frameworks, mas projetos React modernos (especialmente com **Vite** ou **React 19**) exigem configurações específicas para garantir o funcionamento correto de rotas, build e performance.

---

## 1. Importância da Versão

### Node.js

A Vercel utiliza versões LTS por padrão. Para evitar discrepâncias entre o ambiente local e a produção, defina explicitamente no `package.json`:

```json
"engines": {
  "node": ">=20.x"
}
```

### React 19

Se estiver usando **React 19**, certifique-se de que todas as dependências são compatíveis.

- **Peer Dependencies**: Em alguns casos, o build da Vercel pode falhar por conflitos de versões. Se necessário, altere o "Install Command" no painel da Vercel para:
  `npm install --legacy-peer-deps`

---

## 2. Configuração Vite (SPA)

Diferente do Next.js, o Vite gera um SPA (Single Page Application). Sem uma configuração de redirecionamento, atualizar a página em uma rota como `/dashboard` resultará em um erro **404**.

**Solução**: Crie um arquivo `vercel.json` na raiz do projeto:

```json
{
  "rewrites": [
    { "source": "/(.*)", "destination": "/index.html" }
  ]
}
```

---

## 3. Otimização de Build

### Pasta de Saída (Output Directory)

- **Vite**: O padrão é `dist`.
- **Next.js**: O padrão é `.next`.
- **CRA**: O padrão é `build`.

Se você alterou o `outDir` no `vite.config.ts`, deve atualizar o "Output Directory" nas configurações do projeto na Vercel.

### Scripts de Build

Garanta que seu `package.json` tenha os comandos corretos:

```json
"scripts": {
  "dev": "vite",
  "build": "tsc && vite build",
  "preview": "vite preview"
}
```

> [!TIP]
> O comando `tsc && vite build` garante que o build falhe se houver erros de tipagem, prevenindo deploys quebrados.

---

## 4. Variáveis de Ambiente

- **Vite**: Apenas variáveis prefixadas com `VITE_` são expostas ao código do cliente.
  - Ex: `VITE_API_URL`
- **Next.js**: Use o prefixo `NEXT_PUBLIC_`.

Ao configurar na Vercel, você pode adicionar a variável e ela será injetada em tempo de build.

---

## 5. Troubleshooting Comum

- **Erro 404 em rotas dinâmicas**: Falta do `vercel.json` com `rewrites`.
- **Build lento**: Verifique se a pasta `node_modules` ou `dist` não está sendo enviada para o Git (use `.gitignore`).
- **Imagens não carregam**: No Vite, use a pasta `public/` para ativos estáticos e referencie-os com caminhos absolutos (ex: `/logo.png`).
