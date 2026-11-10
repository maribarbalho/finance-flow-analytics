# Finance Flow — Inteligência de Movimentações Financeiras

### Business Intelligence • Power BI • Análise Financeira • Data Storytelling

**Um case de Business Intelligence que transforma registros de recebimentos e pagamentos em análises de movimentação financeira, concentração por clientes e fornecedores, tendências e pontualidade.**

[**▶ Acessar o dashboard interativo no Power BI**](https://app.powerbi.com/view?r=eyJrIjoiNTViMTZlYzktNDZmZS00NzMyLTk5YTktNDc2Y2FkNWQwMDQxIiwidCI6IjY1OWNlMmI4LTA3MTQtNDE5OC04YzM4LWRjOWI2MGFhYmI1NyJ9)

> **Projeto educacional e de portfólio**, desenvolvido no curso *Power BI e Análise de Dados com Inteligência Artificial* do SENAI. Os indicadores representam apenas a base didática; não são resultados financeiros auditados de uma empresa real. Os dados brutos não são disponibilizados neste repositório.

---

## O desafio

Como acompanhar o volume de entradas e saídas financeiras, comparar períodos, identificar concentração por clientes e fornecedores e investigar movimentações registradas após o vencimento?

O **Finance Flow** integra duas tabelas de movimentação e os cadastros de apoio em um modelo dimensional, com indicadores DAX, filtros e navegação interativa. O projeto prioriza **clareza na definição das métricas** e uma experiência visual voltada a decisões de negócio.

## Principais resultados da base

Referências **sem filtros**, conferidas durante o desenvolvimento:

| Indicador | Valor |
|---|---:|
| Valor Recebido | **R$ 59.373.406,21** |
| Valor Pago | **R$ 24.016.307,52** |
| Movimentação Líquida (entradas − saídas) | **R$ 35.357.098,69** |
| Saídas / Entradas | **40,45%** |
| Registros de recebimento | **1.721** |
| Registros de pagamento | **1.004** |
| Clientes distintos com recebimentos | **44** |
| Fornecedores distintos com pagamentos | **231** |
| Período | **2018–2019** |

**Nota metodológica:** movimentação líquida é a diferença aritmética entre recebimentos e pagamentos registrados. **Não equivale a lucro contábil ou saldo bancário.** Quantidades de contrapartes com movimentações são diferentes dos totais de cadastros.

## O que foi desenvolvido

| Página | Análises |
|---|---|
| **Home** | Apresentação e navegação |
| **Panorama Financeiro** | KPIs, evolução mensal, comparativo anual, instituições e insights executivos |
| **Evolução & Tendências** | Comparação 2018 × 2019, variação mensal, acumulados, proporção trimestral e mapa de calor |
| **Inteligência de Recebimentos** | Concentração Top 5, Top 10 clientes, UF cadastral, prazo e matriz de clientes |
| **Inteligência de Pagamentos** | Concentração Top 5, Top 10 fornecedores, instituições, prazo e matriz de fornecedores |
| **Prazos & Diagnóstico** | Frequência e duração dos atrasos, faixas de dias e comparativo por instituição |

O dashboard contém navegação lateral, filtros contextuais, limpeza de filtros e **sete páginas personalizadas de tooltip**, reutilizadas conforme os gráficos.

## Ferramentas e competências

- **Power BI Desktop e Service** — desenvolvimento, validação e publicação.
- **Power Query (M)** — ingestão de Excel, transformações, atributos e flags de qualidade.
- **DAX** — medidas financeiras, rankings, percentuais, acumulados e comparação entre anos.
- **Modelagem dimensional** — duas tabelas fato e dimensões de calendário, cliente, fornecedor e conta bancária.
- **Data storytelling** — da visão executiva à análise de concentração e atrasos.
- **UX/UI** — tema escuro premium, paleta rosa/grafite, tooltips e navegação interativa.

**Fonte utilizada:** Base Financeiro.xlsx, com cinco abas: Cliente, Fornecedor, Banco, Pagamentos e Recebimentos. O Excel original não é publicado por precaução com informações cadastrais e bancárias.

## Arquitetura conceitual

~~~text
dCliente ────────> fRecebimentos <──── dContaBancaria
                         ^                    |
                         |                    |
                    dCalendario               |
                         |                    |
                         v                    v
dFornecedor ─────> fPagamentos <──────────────┘

dFaixaAtraso: tabela auxiliar desconectada
_Medidas: organização das medidas DAX
~~~

O calendário filtra as duas fatos pela **data da movimentação**; relacionamentos inativos permitem análises por emissão e vencimento. Ver [Modelagem e fontes](docs/modelagem-e-fontes.md).

## Identidade visual

| Elemento | Cor |
|---|---|
| Fundo escuro | #0D0D12 |
| Cartões grafite | #1B1B23 |
| Rosa principal | #FF169C |
| Texto principal | #FFFFFF |
| Texto secundário | #A7A7B8 |

Arquivos de tema em [themes/](themes/). Imagens de fundo e elementos estáticos foram construídos separadamente dos visuais dinâmicos do Power BI.

## Documentação técnica

| Documento | Conteúdo |
|---|---|
| [Contexto de negócio e storytelling](docs/visao-de-negocio.md) | Perguntas e percurso analítico |
| [Modelagem e fontes](docs/modelagem-e-fontes.md) | Tabelas, granularidade e relacionamentos |
| [Indicadores e DAX](docs/indicadores-e-dax.md) | Definições, regras e fórmulas de referência |
| [Qualidade e limitações](docs/qualidade-e-limitacoes.md) | Validações, cautelas e segurança |
| [Reprodução e publicação](docs/reproducao-e-publicacao.md) | Passo a passo para reproduzir o case |
| [Navegação e tooltips](docs/navegacao-e-tooltips.md) | Filtros e associação dos tooltips aos gráficos |

## Próximas evoluções

- Adicionar capturas das cinco páginas e do modelo de relacionamentos.
- Versionar PBIP/PBIR ou PBIX somente após revisar dados, caminhos e credenciais.
- Validar todos os filtros e tooltips na versão publicada.
- Criar base sintética redistribuível para reprodução integral.

## Autoria

**Mariana Barbalho** — Data Analytics

- [Portfólio](https://maribarbalho.github.io/)
- [Dashboard Finance Flow](https://app.powerbi.com/view?r=eyJrIjoiNTViMTZlYzktNDZmZS00NzMyLTk5YTktNDc2Y2FkNWQwMDQxIiwidCI6IjY1OWNlMmI4LTA3MTQtNDE5OC04YzM4LWRjOWI2MGFhYmI1NyJ9)

---

*Case educacional de Business Intelligence e Análise de Dados, baseado em registros financeiros de 2018–2019.*