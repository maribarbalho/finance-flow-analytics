# Contexto de negócio e storytelling

## Objetivo

O **Finance Flow** é um case de Business Intelligence, desenvolvido durante o curso *Power BI e Análise de Dados com Inteligência Artificial* do SENAI, com registros de recebimentos e pagamentos de **2018–2019**.

O desafio é transformar lançamentos financeiros e cadastros isolados em análises de tendências, concentração, perfil das contrapartes e pontualidade. As conclusões valem apenas para o conjunto de dados didático, não representam resultados auditados de uma empresa real.

## Perguntas de negócio

| Frente | Pergunta | Recursos do relatório |
|---|---|---|
| Panorama | Quanto foi registrado como entrada e saída? | KPIs e comparativos |
| Evolução | Como se comportaram os movimentos em 2018 e 2019? | Variações, acumulados e heatmap |
| Clientes | Quais clientes concentram mais recebimentos? | Top 5, Top 10 e matriz |
| Geografia | Em quais UFs estão os clientes associados às entradas? | Gráfico por UF cadastral |
| Fornecedores | Quem concentra os pagamentos? | Top 5, Top 10 e matriz |
| Bancos | Como os valores estão distribuídos entre instituições? | Comparativos por instituição |
| Prazos | Quantas operações ocorreram após o vencimento? | Taxas, médias e faixas de atraso |

## Percurso analítico

1. **Home:** acesso e navegação.
2. **Panorama Financeiro:** totais, evolução e insights executivos.
3. **Evolução & Tendências:** variações anuais e mensais, movimentos acumulados e proporções trimestrais.
4. **Inteligência de Recebimentos:** clientes, valores, distribuição geográfica e prazos.
5. **Inteligência de Pagamentos:** fornecedores, instituições e comportamentos de pagamento.
6. **Prazos & Diagnóstico:** frequência e duração dos atrasos por mês e instituição.

## Investigações que o painel apoia

- Identificar meses com maior oscilação nas entradas e saídas.
- Priorizar análise das contrapartes com alta concentração de valores.
- Avaliar a distribuição de valores entre instituições, sem atribuir causalidade.
- Investigar incidência de movimentos após o vencimento, observando contagens e dias médios.
- Diferenciar **volume de registros** e **valor financeiro** nas análises.

## Cuidados de interpretação

- **Movimentação líquida não equivale a lucro**; **valor recebido não equivale a faturamento contábil**.
- **Após vencimento** é uma movimentação posterior à data prevista; não significa necessariamente inadimplência ou título ainda aberto.
- A **UF** indica localização cadastral, não o local em que ocorreu a transferência.
- Taxas de atraso são baseadas em **quantidade de registros**, enquanto gráficos financeiros por prazo somam **valores monetários**.
- Rankings e percentuais dependem do universo selecionado pelos filtros; variações extremas requerem comparação com valores absolutos.

Veja [Modelagem](modelagem-e-fontes.md), [Indicadores](indicadores-e-dax.md) e [Qualidade](qualidade-e-limitacoes.md).