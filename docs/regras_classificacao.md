# Regras de classificação — AP Aging (Nordex Energy Brasil, empresa 4690)

Fonte: extraída da aba `Parâmetros` do relatório
`reports/AP_Aging_Nordex_4690_08_2026_FINAL_VALIDADO.xlsx` (posição em 31/08/2026).

## Escopo e critério de extração

| Parâmetro | Valor |
|---|---|
| Empresa | 4690 — Nordex Energy Brasil LTDA |
| Data de corte | 31/08/2026 |
| Relatório-fonte | Administrar itens fornecedor (SAP Fiori) |
| Critério de extração | Partidas em aberto — aberto na data fixada = data de corte — itens normais + transações do Razão Especial |
| Eixo de envelhecimento | Idade do documento contábil de origem (não é a data de vencimento) |

A automação usa **um único relatório-fonte**: "Administrar itens fornecedor"
(Fiori), que já traz data do documento e data de vencimento na mesma
extração. A controladoria também extrai pelo Razão (FAGLL03, layout
"Aging AP") apenas quando precisa de campos contábeis/de projeto que o
relatório de fornecedor não tem (Elemento PEP, Definição do projeto, Centro
de custo, Centro de lucro, Segmento, Grupo de contas) — o eixo de
envelhecimento é equivalente nos dois (validado item a item). Conclusão: para
o aging puro, o relatório de fornecedor basta; se a análise exigir abertura
por projeto/PEP, o Razão entra como segunda fonte.

## Contas do Razão no escopo

Ver `config/contas_escopo.csv`. Contas incluídas: fundo fixo de caixa,
adiantamentos (viagem, fornecedores terceiros e partes relacionadas),
fornecedores nacionais, contas a pagar — países estrangeiros,
responsabilidades com empregados, fornecedores — partes relacionadas.

## Eixo de antiguidade (aging)

O envelhecimento é agrupado **por ano do documento contábil de origem**
(coluna `Ano` na aba `Base` / `Data documento`), não por faixas de dias em
atraso (30/60/90...). Os baldes usados nos relatórios são:

`Até 2020 | 2021 | 2022 | 2023 | 2024 | 2025 | 2026`

A aba `Base` também carrega `Dias atraso` e `Vencido` (flag 0/1, vencimento
já expirado na data de corte) por documento, para quem precisar de uma visão
por dias em vez de por ano — mas os relatórios consolidados (Resumo
Executivo, Aging por Categoria, Aging por Conta, Posição por Fornecedor)
usam o eixo por ano.

## Classificação por categoria de fornecedor

Cada fornecedor é mapeado para uma categoria de negócio via
`config/categorias_fornecedores.csv` (De-Para, 1.328 fornecedores
cadastrados). Categorias em uso:

- Intercompany
- Construction Services – Under Litigation
- Operational Suppliers (Wind Farms)
- Customs Brokerage – Overall
- SINOMA
- Other Suppliers
- Legal Services Suppliers
- Nordex Employees
- Corporate Card Payments
- Facilities
- Insurance
- Taxes Payable
- Employee-Related Liabilities
- Social charges
- Aeris
- Consumables Suppliers
- Audit Services
- LDs Service Payments
- Suppliers Under Litigation
- Vehicle Rental Services

Fornecedor sem categoria cadastrada cai em **"(sem categoria)"** e aparece na
aba `Pendências` do relatório — fluxo esperado: preencher a categoria na
aba `Detalhe`, e o fornecedor some das pendências na próxima geração.

## Reconciliação

O total da base (soma de todos os documentos em aberto) é conferido contra o
total de referência do Razão. No relatório de 08/2026 a diferença foi de
`6.68e-06` (arredondamento), considerada OK.

## Observações de qualidade de dados corrigidas nesta versão

- Corrigidos mismatches de `dimension`/`interval` no XML do `.xlsx` gerado
  (causavam aviso de arquivo corrompido ao abrir no Excel).
- Gráficos nativos do Excel (3, na aba `Resumo Executivo`) aplicados e
  validados.
