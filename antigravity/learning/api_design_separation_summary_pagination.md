# 2026-02-26 - API Design: Separação de Endpoints de Sumário e Paginação

**Situation/Bug**: 
Ao projetar interfaces de relatórios (como dashboards financeiros), ocorreu a tentativa de retornar na mesma requisição de `summary` (onde são caculados os totais, receitas por categoria e ticket médio) o objeto de `history` (extrato detalhado das transações) encapsulando também paginação (`filters`, `page`, `limit`). 
Isso gera alto acoplamento: o carregamento global do dashboard da clínica trava enquanto toda a base de transações é filtrada e paginada; e, ao navegar entre páginas do histórico ou aplicar filtros de pesquisa textuais ("search"), toda a estatística financeira precisaria ser recalculada ou cacheada complexamente dentro da request única.

**Solution/Insight**: 
Dividir estritamente as responsabilidades:
1. **Endpoint de Summary** (Ex: `GET /financial/summary`): Retorna exclusivamente as consolidações (somas, agrupamentos para gráficos) baseado num intervalo de período. Deve ser a requisição mais rápida e pesada para processamento.
2. **Endpoint Paginado/Listagem** (Ex: `GET /financial/history`): Recebe `page`, `limit` e `search` como Query Params. Retorna unicamente a listagem tabular e os metadados `{ data: [], total, page, limit }`. 

Ao implementar no Frontend, podemos criar dois containers lógicos separados (ou queries/thunks diferentes). Se a busca for alterada na tabela, a query de summary mantém seu cache e o front-end solicita dados na API apenas da nova página da tabela, economizando transferências e processamento de backend.

**Reference**: 
- *Contexto em BioOrganize* - Separação da Controller e Service em `Bio-Organize Backend`.
- Boas práticas de design de APIs REST orientadas a recursos analíticos.
