# Modelagem de dados e fontes

## Fonte

A origem do case é a planilha **Base Financeiro.xlsx**, com cinco abas (contagens de linhas de dados sem cabeçalho):

| Aba | Linhas | Exemplos de atributos |
|---|---:|---|
| Cliente | 812 | ID, razão social/nome fantasia, tipo, município, UF |
| Fornecedor | 630 | ID, razão social/nome fantasia, tipo, município, UF |
| Banco | 24 | ID de banco/conta, nome da instituição, município e UF |
| Pagamentos | 1.004 | Fornecedor, conta, datas de emissão/vencimento/movimentação, valor |
| Recebimentos | 1.721 | Cliente, conta, datas de emissão/vencimento/movimentação, valor |

No conjunto completo, apenas **44 clientes** e **231 fornecedores** têm movimentações. Cadastros totais e contrapartes movimentadas são medidas distintas. **A planilha não é redistribuída neste repositório.**

## Camadas

| Papel | Tabela | Granularidade / função |
|---|---|---|
| Fato | `fRecebimentos` | Uma linha por movimentação de recebimento |
| Fato | `fPagamentos` | Uma linha por movimentação de pagamento |
| Dimensão | `dCliente` | Cliente e atributos cadastrais |
| Dimensão | `dFornecedor` | Fornecedor e atributos cadastrais |
| Dimensão | `dContaBancaria` | Conta e instituição financeira |
| Dimensão | `dCalendario` | Datas e atributos de período |
| Auxiliar desconectada | `dFaixaAtraso` | Intervalos de atraso usados como eixo |
| Medidas | `_Medidas` | Organização dos cálculos DAX |
| Staging | consultas `stg_...` | Preparação das cinco abas originais |

### Atributos derivados

As tabelas fato incluem `DataEmissao`, `DataVencimento`, `DataMovimentacao`, `ValorMovimentacao`, IDs de contraparte e conta, além de:

- `DiasDesvioVencimento`: diferença de dias entre movimentação e vencimento.
- `SituacaoPrazo`: antes, no dia ou após vencimento.
- `QualidadeDatas` e `QualidadeValor`: marcadores auxiliares de consistência.
- `FracaoCentavo`: identificação de precisão além de dois decimais.

**Precisão monetária:** preservar valores originais no ETL; formatar em duas casas apenas na exibição.

## Relacionamentos

**Ativos (6), dimensão → fato, cardinalidade 1:*:**

1. `dCliente` → `fRecebimentos`.
2. `dFornecedor` → `fPagamentos`.
3. `dContaBancaria` → `fRecebimentos`.
4. `dContaBancaria` → `fPagamentos`.
5. `dCalendario[Data]` → `fRecebimentos[DataMovimentacao]`.
6. `dCalendario[Data]` → `fPagamentos[DataMovimentacao]`.

**Inativos (4):** calendário → `DataEmissao` e → `DataVencimento` de cada tabela fato. As análises temporais padrão são realizadas pela **data da movimentação**. Medidas específicas podem empregar `USERELATIONSHIP` quando apropriado.

## Calendário e faixas

`dCalendario` inclui `Data`, `Ano`, `Trimestre`, `NumeroMes`, `MesAno`, `AnoMesOrdem`, `InicioMes` e `MesAbreviado`.

- `MesAno`: classificado por `AnoMesOrdem`, para a série cronológica de 24 meses.
- `MesAbreviado`: classificado por `NumeroMes`, para a comparação entre anos.
- `dFaixaAtraso` é **desconectada**, com faixas 1–7, 8–15, 16–30 e acima de 30 dias; medidas aplicam os limites diretamente nas duas fatos.

## Diagrama conceitual

```text
dCliente ─────────> fRecebimentos <────── dContaBancaria
                           ^                      |
                           |                      |
                      dCalendario                 |
                           |                      |
                           v                      v
dFornecedor ───────> fPagamentos <────────────────┘

dFaixaAtraso (desconectada)       _Medidas
```

A representação é conceitual. Para uma reprodução fiel dos relacionamentos é necessário validar o modelo no arquivo Power BI, que não está publicado neste repositório.