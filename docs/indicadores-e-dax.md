# Indicadores e exemplos DAX

> Exemplos baseados nas medidas utilizadas/discutidas no desenvolvimento, **não** exportação automática do PBIX publicado. Conferir nomes de colunas e filtros ao reproduzir.

## Valores de referência sem filtros

| Indicador | Valor | Regra |
|---|---:|---|
| Recebido | R$ 59.373.406,21 | Soma das movimentações recebidas |
| Pago | R$ 24.016.307,52 | Soma das movimentações pagas |
| Líquido | R$ 35.357.098,69 | Recebido − pago |
| Saídas / Entradas | 40,45% | Pago ÷ recebido |
| Recebimentos | 1.721 | Registros |
| Pagamentos | 1.004 | Registros |
| Clientes com recebimentos | 44 | IDs distintos da fato |
| Fornecedores com pagamentos | 231 | IDs distintos da fato |
| Média por recebimento | R$ 34.499,36 | Recebido ÷ 1.721 |
| Média por pagamento | R$ 23.920,63 | Pago ÷ 1.004 |

## Medidas básicas

```dax
Valor Recebido = SUM(fRecebimentos[ValorMovimentacao])
Valor Pago = SUM(fPagamentos[ValorMovimentacao])

Movimentação Líquida =
[Valor Recebido] - [Valor Pago]

% Saídas sobre Entradas =
DIVIDE([Valor Pago], [Valor Recebido])

Qtd Recebimentos = COUNTROWS(fRecebimentos)
Qtd Pagamentos = COUNTROWS(fPagamentos)

Clientes com Recebimentos =
DISTINCTCOUNT(fRecebimentos[IdCliente])

Fornecedores com Pagamentos =
DISTINCTCOUNT(fPagamentos[IdFornecedor])

Média por Registro Recebido =
DIVIDE([Valor Recebido], [Qtd Recebimentos])

Média por Pagamento =
DIVIDE([Valor Pago], [Qtd Pagamentos])
```

## Concentração

O Top 5 identifica contrapartes de maior valor dentro do contexto de seleção e calcula sua representatividade no universo analisado.

```dax
Concentração Top 5 Clientes % =
VAR RankingClientes =
    ADDCOLUMNS(
        ALLSELECTED(dCliente[IdCliente]),
        "@Valor", [Valor Recebido]
    )
VAR Top5 =
    TOPN(5, RankingClientes, [@Valor], DESC,
         dCliente[IdCliente], ASC)
RETURN DIVIDE(
    SUMX(Top5, [@Valor]),
    CALCULATE(
        [Valor Recebido],
        ALLSELECTED(dCliente[IdCliente])
    )
)
```

### Participação no tooltip de fornecedor

```dax
Participação Fornecedor Tooltip % =
VAR ValorFornecedor = [Valor Pago]
VAR ValorTotal =
    CALCULATE(
        [Valor Pago],
        REMOVEFILTERS(dFornecedor),
        REMOVEFILTERS(fPagamentos[IdFornecedor])
    )
RETURN DIVIDE(ValorFornecedor, ValorTotal)
```

O denominador ignora o fornecedor, mas preserva filtros de data e instituição financeira. Por isso, pode diferir de uma participação calculada apenas dentro da seleção de fornecedores.

## Acumulado mensal

```dax
Movimentação Líquida Acumulada =
VAR AnoAtual = SELECTEDVALUE(dCalendario[Ano])
VAR MesAtual = SELECTEDVALUE(dCalendario[NumeroMes])
RETURN IF(
    ISBLANK(AnoAtual) || ISBLANK(MesAtual),
    BLANK(),
    CALCULATE(
        [Movimentação Líquida],
        FILTER(
            ALL(dCalendario),
            dCalendario[Ano] = AnoAtual
                && dCalendario[NumeroMes] <= MesAtual
        )
    )
)
```

Em tooltip comparativo, o acumulado de 2018 e o de 2019 usam medidas separadas até o **mesmo mês**. A diferença entre anos precisa ser **medida DAX**, não coluna calculada.

## Indicadores de atraso

Atraso = registro com `SituacaoPrazo = "Após vencimento"`. Os percentuais são baseados em **contagem de movimentações**; médias são dos registros tardios.

```dax
Recebimentos Após Prazo % =
VAR Total = [Qtd Recebimentos]
VAR Atrasados =
    CALCULATE(
        [Qtd Recebimentos],
        KEEPFILTERS(fRecebimentos[SituacaoPrazo] = "Após vencimento")
    )
RETURN IF(Total > 0, DIVIDE(COALESCE(Atrasados, 0), Total))

Pagamentos Após Prazo % =
VAR Total = [Qtd Pagamentos]
VAR Atrasados =
    CALCULATE(
        [Qtd Pagamentos],
        KEEPFILTERS(fPagamentos[SituacaoPrazo] = "Após vencimento")
    )
RETURN IF(Total > 0, DIVIDE(COALESCE(Atrasados, 0), Total))

Atraso Médio Recebimentos =
CALCULATE(
    AVERAGE(fRecebimentos[DiasDesvioVencimento]),
    KEEPFILTERS(fRecebimentos[SituacaoPrazo] = "Após vencimento")
)

Atraso Médio Pagamentos =
CALCULATE(
    AVERAGE(fPagamentos[DiasDesvioVencimento]),
    KEEPFILTERS(fPagamentos[SituacaoPrazo] = "Após vencimento")
)
```

**Diferença de atrasos em pontos percentuais:**

```dax
Diferença de Atrasos p.p. =
VAR Rec = [Recebimentos Após Prazo %]
VAR Pag = [Pagamentos Após Prazo %]
RETURN
IF(ISBLANK(Rec) || ISBLANK(Pag), BLANK(), (Pag - Rec) * 100)
```

Formato sugerido: `+0.0" p.p.";-0.0" p.p.";0.0" p.p."`. Valores positivos indicam maior frequência de pagamentos tardios.

## Testes de sanidade

1. `[Valor Recebido] - [Valor Pago]` deve reproduzir `[Movimentação Líquida]`.
2. As matrizes devem totalizar **1.721** recebimentos e **1.004** pagamentos (sem filtros).
3. Participações dos tooltips devem variar por contraparte; um 100% repetido pode indicar filtro indevido no denominador.
4. Distinguir **0% de atrasos** (há registros sem atraso) de **em branco** (não há registros no contexto).
5. Conferir limites e elegibilidade das faixas de atraso.