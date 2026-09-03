# Aging_AP

Análise de aging de Contas a Pagar (AP) da Nordex Energy Brasil (empresa
4690), a partir de exports do SAP ("Administrar itens fornecedor" — Fiori).
Este repositório centraliza o relatório consolidado, as regras de
classificação já validadas e o cadastro de fornecedores por categoria, para
reprodução mês a mês.

## Conteúdo

- `reports/AP_Aging_Nordex_4690_08_2026_FINAL_VALIDADO.xlsx` — relatório
  consolidado (posição em 31/08/2026): Resumo Executivo com gráficos, Aging
  por Categoria, Aging por Conta, Posição por Fornecedor, Base de documentos,
  Detalhe (drill-down) e Pendências de classificação.
- `config/categorias_fornecedores.csv` — De-Para fornecedor → categoria de
  negócio (1.328 fornecedores cadastrados).
- `config/contas_escopo.csv` — contas do Razão incluídas no escopo do aging.
- `docs/regras_classificacao.md` — critério de extração, eixo de
  envelhecimento (por ano do documento), categorias e reconciliação.
- `docs/dicionario_dados.md` — estrutura das abas e colunas do relatório.

## Como o aging é calculado

1. **Fonte**: um único export do SAP Fiori ("Administrar itens fornecedor"),
   partidas em aberto na data de corte.
2. **Eixo de antiguidade**: ano da `Data documento` de origem (não faixas de
   dias) — baldes `Até 2020` a `2026`.
3. **Classificação**: cada fornecedor recebe uma categoria de negócio via
   `config/categorias_fornecedores.csv`. Fornecedor novo sem categoria cai em
   "(sem categoria)" e aparece na aba `Pendências` do relatório até ser
   classificado.
4. **Reconciliação**: o total da base é conferido contra o Razão; a
   diferença deve ficar em arredondamento (~1e-05 ou menor).

Detalhes completos em `docs/regras_classificacao.md`.

## Status

Relatório de 08/2026 gerado e validado: gráficos nativos aplicados na aba
`Resumo Executivo`, problema de corrupção do `.xlsx` (mismatch de
`dimension`/`interval` no XML) corrigido, reconciliação Base x Razão OK.
