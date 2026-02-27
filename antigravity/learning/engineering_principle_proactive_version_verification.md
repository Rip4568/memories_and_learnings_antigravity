# Princípio de Engenharia: Verificação Proativa de Versões

**Situation/Bug**:
O uso de configurações, comandos ou bibliotecas sem validar a versão exata (ex: v3 vs v4) é uma das causas principais de erros imprevistos.

**Solution/Insight**:
**Antes de qualquer implementação**, valide sempre as versões no `package.json`. Busque documentação **específica para a versão instalada** e nunca assuma que a sintaxe de versões anteriores funcionará em novas.

**Reference**:
[Global Principles]
