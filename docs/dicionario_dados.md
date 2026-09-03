# Dicionário de dados — `reports/AP_Aging_Nordex_4690_08_2026_FINAL_VALIDADO.xlsx`

Workbook com 9 abas, todas visíveis:

| Aba | Dimensão | Conteúdo |
|---|---|---|
| `Resumo Executivo` | A1:V30 | Cartões gerenciais (exposição total, vencido, intercompany x terceiros), 3 gráficos nativos (composição por categoria, antiguidade por ano, concentração por fornecedor) |
| `Base` | A1:R5129 | Um documento em aberto por linha — dado granular, já classificado |
| `Aging por Categoria` | A1:I22 | Saldo por categoria x ano do documento (pivot da `Base`) |
| `Aging por Conta` | A1:J14 | Saldo por conta contábil x ano do documento |
| `Posição por Fornecedor` | A1:K677 | Saldo por fornecedor x ano do documento, com categoria |
| `De-Para Fornecedores` | A1:D4404 | Cadastro fornecedor → categoria (1.328 fornecedores preenchidos) |
| `Parâmetros` | A1:H33 | Configuração da extração, reconciliação, escopo de contas |
| `Detalhe` | A1:AA3002 | Drill-down por filtro (categoria/conta/ano) + cadastro automático de fornecedores novos |
| `Pendências` | A1:I51 | Fornecedores sem categoria ainda; lista de categorias válidas |

## Colunas da aba `Base`

| Coluna | Campo | Descrição |
|---|---|---|
| A | Conta | Conta contábil do Razão |
| B | Fornecedor | Código do fornecedor (SAP) |
| C | Nome do fornecedor | Razão social |
| D | Tipo doc | Tipo de documento SAP (ex.: `AB`, `RE`) |
| E | Documento | Número do documento contábil |
| F | Referência | Referência externa (nota fiscal etc.) |
| G | Moeda | Moeda da transação |
| H | Montante | Valor em aberto na moeda do documento |
| I | Data documento | Data do documento contábil de origem — base do eixo de envelhecimento |
| J | Data vencimento | Data de vencimento do item |
| K | Texto | Texto do item |
| L | Atribuição | Campo "Atribuição" do SAP |
| M | Ano | Ano da `Data documento` — balde do aging |
| N | Categoria | Categoria de negócio (via De-Para Fornecedores) |
| O | Dias atraso | Dias entre `Data vencimento` e a data de corte |
| P | Vencido | Flag 1/0 — vencimento já expirado na data de corte |
| Q | Match | Flag de conferência/match do item |
| R | Rank | Ordenação auxiliar |

Ver `docs/regras_classificacao.md` para os parâmetros de extração, o escopo
de contas e a lógica de categorização.
