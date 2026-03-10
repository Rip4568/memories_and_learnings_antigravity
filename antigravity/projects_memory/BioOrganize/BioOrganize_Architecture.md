# Bio-Organize: Arquitetura de Procedimentos e Galeria

Este documento descreve a lógica de funcionamento e as relações entre Agendamentos, Sessões de Procedimentos e o Sistema de Imagens.

## 1. Fluxo de Trabalho (Workflow)

O fluxo de um atendimento clínico segue esta ordem cronológica:

1.  **Appointment (Agendamento)**: Criado via calendário. Representa o horário reservado. Possui um ID próprio, mas ainda não contém dados clínicos ou fotos.
2.  **Início do Procedimento**: Quando o profissional clica em "Iniciar", o sistema cria uma `ProvidedSession` (Sessão Realizada).
3.  **ProvidedSession (Sessão de Procedimento)**: É a entidade principal para dados clínicos. Nela são salvos:
    *   Questionários preenchidos.
    *   Fotos de Antes e Depois.
    *   Status de execução (Em andamento, Finalizada, Paga).
4.  **ProvidedProcedure**: Uma única `ProvidedSession` pode conter múltiplos procedimentos (ex: Botox + Preenchimento). Cada um é um `ProvidedProcedure` vinculado à sessão pai.

## 2. Sistema de Galeria de Imagens

### Estrutura de Dados
As imagens são armazenadas na tabela `provided_procedure_images` e possuem as seguintes chaves:
- `providedSessionId`: Vínculo obrigatório com a sessão (não com o agendamento).
- `path`: Caminho relativo no storage.
- `isAfter`: Booleano (false = Antes, true = Depois).
- `beforeImageId`: ID da imagem de "Antes" correspondente (opcional, para montagem de comparativos).

### Regras de Negócio Implementadas
- **Trava de Início**: Fotos só podem ser adicionadas a sessões já iniciadas (`ProvidedSession`). Agendamentos puros não aceitam fotos.
- **Nomenclatura**: Os arquivos são salvos com nomes gerados pelo storage (UUID), mas podem ser buscados via metadados no banco.
- **Galeria Geral**: Existe um endpoint unificado (`GET /procedures/provided/all-images`) que permite filtrar fotos de toda a clínica por paciente, profissional, data ou nome do arquivo (via busca no path).

## 3. Armazenamento (Storage)
O sistema utiliza drivers intercambiáveis definidos no `.env`:
- `LOCAL`: Salva na pasta `./uploads` do servidor backend.
- `CLOUDFLARE/GCS`: Salva em buckets compatíveis com S3 ou Google Cloud Storage.

## 4. Dicas de Debugging
- **Erro 404 ao salvar foto**: Geralmente indica que o ID passado na URL é de um `Appointment` e não de uma `ProvidedSession`. Verifique se o parâmetro `?type=appointment` está presente na URL do frontend.
- **Erros de Tipagem**: O projeto utiliza Material UI v6, que requer a prop `size` em componentes `Grid` em vez de `xs`, `md` isolados ou as props `item`/`container`.
