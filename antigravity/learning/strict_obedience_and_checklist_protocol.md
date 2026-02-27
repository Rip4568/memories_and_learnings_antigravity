# 2026-02-13 - Strict Obedience & Checklist Protocol

**Situation/Bug**:
O usuário reportou falha crítica na execução de instruções detalhadas, resultando em retrabalho e frustração. Instruções específicas (largura de botão, posição de ícone) foram perdidas ou mal interpretadas.

**Solution/Insight**:

1. **Checklist Granular Obrigatório**: Para CADA solicitação do usuário, crie um checklist passo-a-passo no `task.md` ANTES de alterar qualquer código.
2. **Confirmação Visual**: Se o usuário enviar um print (ou descrever visualmente), verifique o CSS correspondente (ex: `.hero-highlight::after`, `.btn`) antes de assumir que entendeu.
3. **Validação Estrita**: Nunca marque um item como feito sem verificar explicitamente o arquivo modificado.

**Reference**:
[Global Principles]
