# Bibit · Operação

Dashboard web do **DASHBOARD OPERAÇÃO - ACUMULADO** do ClickUp, com os mesmos 17 cards e filtro de período (Acumulado + mês a mês), na identidade visual Bibit.

## Deploy (Vercel)

1. Criar o repositório `qualidadebibitmkt/bibit-operacao` no GitHub e subir estes arquivos (`index.html`, `demo.html`, `api/clickup.js`, `README.md`).
2. No Vercel: **Add New → Project → Import** do repo. Framework: **Other** (site estático + função em `/api`). Sem build command.
3. Em **Settings → Environment Variables**, criar `CLICKUP_TOKEN` com o token da conta André Guedes (o mesmo dos outros dashboards). ⚠️ Ao regenerar o token, agora são **6 projetos** para atualizar: bibit-dashboard, bibit-closers, bibit-ranking, bibit-comissoes, individuais e **bibit-operacao**.
4. Diagnóstico: `https://<projeto>.vercel.app/api/clickup?diag=1` → deve responder `{"CLICKUP_TOKEN":"OK"}`.

`demo.html` abre sem deploy nenhum (dados de exemplo embutidos) — serve para validar visual e interações.

## Fontes de dados

| Card | Fonte |
|---|---|
| Clientes Ativos, MRR, Ticket Médio | Growth `901712531318` — status execução/atrasado, campo Valor Recorrente (Ticket = MRR ÷ Ativos) |
| Churn R$ / Nº / % | Growth — status churn + Data de Saída; % = Churn R$ ÷ MRR Acumulado (card 2026) |
| TMP (churn) / TMP (ativos) | Growth — campo TMP Individual (meses passados: recalculado pelas datas) |
| LTV | Growth — média do campo LTV dos clientes ativos |
| NRR, Produtividade, Expansão de Receita | Card **2026** (`86e1fwefa`) — NRR Anual/Mensal, % Produtividade Anual, Expansão Dash |
| NPS Geral, CSAT | CSAT - Envios `901713333681` — média de NPS Geral e das 4 notas CSAT, mês pelo campo Mês/Ano |
| Cross Sell | Cross-Sell `901713519081` — Σ Valor da Venda, mês pela Data do pagamento |
| Upsell / Down Sell | Acumulado: card 2026 (Soma Upsell/Downsell Anual); mês: Σ Upsell/Downsell Mês dos cards do Ranking `901713545639` |
| Produtividade mensal | Ranking — média de (Tarefas Totais − Atrasadas) ÷ Totais dos cards do mês |

## Filtro de mês

- **Eventos datados** (churn, cross sell, CSAT/NPS, produtividade, up/downsell) vêm direto dos registros do mês.
- **Clientes Ativos, MRR, Ticket e TMP** de meses passados são **reconstruídos** pelas datas de entrada em execução e saída (badge `reconstruído` no card). Aproximação: usa o Valor Recorrente atual de cada cliente.
- **NRR e LTV** só existem para o mês corrente (o ClickUp não guarda histórico) — meses passados mostram `—`.
- Histórico começa em **mai/2026**; meses anteriores ficam desabilitados.
