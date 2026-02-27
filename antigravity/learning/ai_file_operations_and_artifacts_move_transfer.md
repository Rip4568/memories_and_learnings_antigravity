# 2026-02-15 - AI File Operations & Artifacts (Move/Transfer)

**Situation/Bug**:
Ao gerar ativos (como imagens) usando ferramentas de IA, os arquivos são salvos no diretório de artefatos da conversa. Tentar mover esses arquivos para o projeto pode falhar se não forem usados caminhos absolutos ou se houver restrições de permissão do sistema operacional (Windows/Linux).

**Solution/Insight**:

1. **Sempre use Caminhos Absolutos**: O agente deve identificar o caminho completo do arquivo no diretório de artefatos e o caminho de destino absoluto no projeto.
2. **Permissões de Escrita**: Garantir que o processo do agente tenha permissão de escrita no diretório `public/` do projeto.
3. **Fluxo Recomendado**: O agente gera o arquivo -> Notifica o usuário com o caminho absoluto -> Oferece comando de cópia via `run_command` se o usuário preferir automação.
4. **Resumo Técnico**: Erros de "File Not Found" ou "Permission Denied" ao mover arquivos via IA geralmente são resolvidos validando se o `Cwd` do comando está correto e se os caminhos não são relativos.

**Reference**:
[Global Skills - Build Optimization]
