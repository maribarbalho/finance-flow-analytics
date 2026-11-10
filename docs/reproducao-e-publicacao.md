# Reprodução, manutenção e publicação

## Pré-requisitos

- Power BI Desktop com Power Query e DAX.
- Acesso autorizado à fonte `Base Financeiro.xlsx` (não incluída neste repositório).
- Acesso ao Power BI Service para publicar e validar.
- Temas em [themes/](../themes/), se desejar reaproveitar a identidade visual.

O [dashboard público](https://app.powerbi.com/view?r=eyJrIjoiNTViMTZlYzktNDZmZS00NzMyLTk5YTktNDc2Y2FkNWQwMDQxIiwidCI6IjY1OWNlMmI4LTA3MTQtNDE5OC04YzM4LWRjOWI2MGFhYmI1NyJ9) permite visualizar os resultados, mas não fornece a fonte, o PBIX nem o código completo das consultas M.

## Fluxo de reprodução

1. **Conferir a origem:** cinco abas `Cliente`, `Fornecedor`, `Banco`, `Pagamentos` e `Recebimentos`.
2. **Ingerir no Power Query:** criar consultas de staging `stg_`, revisar tipos, separadores numéricos, datas, IDs e registros nulos.
3. **Transformar:** gerar dimensões e fatos e atributos de prazo/qualidade, preservando os valores monetários originais.
4. **Criar dimensões:** `dCliente`, `dFornecedor`, `dContaBancaria`, `dCalendario`.
5. **Criar fatos:** `fRecebimentos` e `fPagamentos`.
6. **Relacionar:** seis caminhos ativos de dimensão para fato e quatro inativos de calendário para emissão/vencimento.
7. **Modelar faixas:** criar `dFaixaAtraso` desconectada e medidas para filtrar intervalos de dias.
8. **Criar medidas:** valor, quantidade, participações, concentração Top 5, comparação anual, acumulado e indicadores de atraso.
9. **Construir as páginas:** Panorama, Evolução, Recebimentos, Pagamentos e Prazos; incluir Home se aplicável.
10. **Configurar navegação:** segmentações, limpeza de filtros e tooltips report-page.
11. **Validar:** confrontar indicadores, matrizes e totais por instituição/ano.
12. **Publicar:** enviar ao Power BI Service e validar a experiência publicada.

## Referências para conferência

| Verificação | Esperado sem filtros |
|---|---:|
| Valor Recebido | R$ 59,37 mi |
| Valor Pago | R$ 24,02 mi |
| Movimentação Líquida | R$ 35,36 mi |
| Saídas / Entradas | 40,45% |
| Qtd. Recebimentos | 1.721 |
| Qtd. Pagamentos | 1.004 |
| Clientes com movimentações | 44 |
| Fornecedores com movimentações | 231 |

## Versionamento e segurança

- Prefira **PBIP/PBIR** para versionamento quando a instalação oferecer suporte.
- Revise o conteúdo dos arquivos de projeto antes de torná-los públicos.
- Nunca versionar credenciais, chaves, tokens, caminhos privados ou planilhas contendo cadastros sensíveis.
- A documentação de DAX nesta versão apresenta **exemplos de referência**, não uma exportação integral das medidas do modelo publicado.

## Estrutura

```text
finance-flow-analytics/
├── README.md
├── docs/
│   ├── visao-de-negocio.md
│   ├── modelagem-e-fontes.md
│   ├── indicadores-e-dax.md
│   ├── qualidade-e-limitacoes.md
│   ├── reproducao-e-publicacao.md
│   └── navegacao-e-tooltips.md
├── themes/
│   ├── Finance_Flow_Dark.json
│   └── Finance_Flow_Light_Corrigido.json
├── assets/
│   └── README.md
└── .gitignore
```