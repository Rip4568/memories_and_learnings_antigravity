# Hytale Modding - Aprendizados

## 2026-02-11 - InteractionVars e "Missing Interaction"
**Situation/Bug**:
Ao definir variáveis de interação personalizadas para um item (ex: uma espada com `WeaponID` customizado), o jogo reporta erros de "Missing interaction" se as chaves em `InteractionVars` não seguirem estritamente o padrão exigido pelo motor.

**Solution/Insight**:
Se o item tem um `Id` ou `WeaponID` customizado (ex: `Weapon_Longsword_OP`), as chaves dentro de `InteractionVars` DEVEM começar com esse ID seguido de `_InteractionVars_`.
Exemplo Correto: `"Weapon_Longsword_OP_InteractionVars_Longsword_Swing_Left_Damage": { ... }`

**Reference**:
[items.Weapon_Longsword_Void.json]

---
## 2026-02-11 - Solução Definitiva: Dano Customizado em Armas (KISS Principle)
**Situation/Bug**:
Tentativas de criar uma arma "Void OP" com múltiplos tipos de dano (Físico + Void), Knockback customizado e ID próprio falharam repetidamente.
- Com ID próprio -> Erro de "Missing Interaction" ou dano travado em 5.
- Com ID removido mas propriedades complexas -> Erro ou comportamento padrão.

**Solution/Insight (O que funcionou)**:
A abordagem "Menos é Mais" foi a vencedora.
1. **Sem ID**: Não declare `"Id": "..."` no JSON. Deixe o jogo identificar o item pelo nome do arquivo. Isso permite usar as chaves de interação padrão (`Longsword_Swing_Left_Damage`).
2. **Estrutura Simples de Dano**: Ao invés de misturar tipos de dano ou adicionar efeitos complexos (Knockback, AoE) que podem estar mal formatados ou não suportados no override simples:
   - Use apenas `BaseDamage` com `Physical`.
   - Use `Type: Absolute`.
   - Adicione `RandomPercentageModifier` se quiser variação.
   
**JSON Vencedor**:
```json
"InteractionVars": {
    "Longsword_Swing_Left_Damage": {
      "Interactions": [
        {
          "Parent": "Longsword_Swing_Left_Damage",
          "DamageCalculator": {
            "Type": "Absolute",
            "BaseDamage": {
              "Physical": 500  <-- Valor alto funciona aqui
            },
            "RandomPercentageModifier": 0.15
          }
        }
      ]
    }
}
```

**Conclusão**: O motor do Hytale parece ser sensível a propriedades extras em overrides. Comece pelo básico (Dano Físico puro) e adicione complexidade *após* validar que o básico funciona.

**Reference**:
[items.Weapon_Longsword_Void.json](file:///c:/Users/user/Desktop/Things/hytale_mods/assets/items/weapons/items.Weapon_Longsword_Void.json)
