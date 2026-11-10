# Navegação, filtros e tooltips

## Navegação

O Finance Flow utiliza menu lateral com **Home**, **Panorama Financeiro**, **Evolução & Tendências**, **Recebimentos**, **Pagamentos** e **Prazos & Diagnóstico**.

Os botões são discretos no estado padrão, destacam a página ativa com rosa `#FF169C` e apresentam realce suave ao passar o mouse. Botões de limpeza de filtros são específicos por página.

## Segmentações

| Página | Filtros principais |
|---|---|
| Panorama Financeiro | Ano e instituição financeira |
| Evolução & Tendências | Período e instituição financeira |
| Recebimentos | Período, UF cadastral e cliente |
| Pagamentos | Período, instituição e fornecedor |
| Prazos & Diagnóstico | Período e instituição financeira |

## Mapa de associação dos tooltips

Foram planejadas **sete páginas personalizadas de tooltip**, reutilizadas em diferentes gráficos:

| Página | Visual | Tooltip |
|---|---|---|
| Panorama | Evolução mensal das movimentações | **Tooltip - Evolução Mensal** |
| Panorama | Comparativo anual | Padrão |
| Panorama | Movimentações por instituição | Padrão |
| Evolução | Movimentação Líquida Mensal | **Tooltip - Evolução Mensal** (reutilizado; validar contexto) |
| Evolução | Variação Mensal (%) | **Tooltip - Variação Mensal** |
| Evolução | Evolução Acumulada 2018 × 2019 | **Tooltip - Acumulado** |
| Evolução | Saídas / Entradas por Trimestre | **Tooltip - Saídas / Entradas por Trimestre** |
| Evolução | Mapa de Calor Mensal | Padrão |
| Recebimentos | Top 10 Clientes | **Tooltip - Recebimentos** |
| Recebimentos | UF, prazos e matriz | Padrão; matriz pode ser personalizada |
| Pagamentos | Top 10 Fornecedores | **Tooltip - Detalhamento do fornecedor** |
| Pagamentos | Instituições, prazos e matriz | Padrão; matriz pode ser personalizada |
| Prazos | Evolução dos Atrasos | **Tooltip - Diagnóstico de Prazos** |
| Prazos | Faixas de Atraso e matriz | Padrão |
| Prazos | Pontualidade por Instituição | **Tooltip - Diagnóstico de Prazos** |

## Indicadores dos tooltips

| Tooltip | Indicadores |
|---|---|
| Evolução Mensal | Valor Recebido, Valor Pago, Movimentação Líquida |
| Variação Mensal | Movimentação Atual, Anterior, Variação Mensal (%) |
| Acumulado | Acumulado 2018, Acumulado 2019, Diferença entre anos |
| Saídas / Entradas por Trimestre | Recebido, Pago, Saídas / Entradas (%) |
| Recebimentos / Cliente | Recebido, quantidade, participação |
| Fornecedor | Pago, quantidade, participação |
| Diagnóstico de Prazos | Recebimentos Após Prazo %, Pagamentos Após Prazo %, Diferença em p.p. |

## Recomendações de validação no Service

- [ ] Verificar cada rota de navegação e o link do LinkedIn.
- [ ] Conferir estados padrão, hover e seleção do menu.
- [ ] Testar os botões de limpar filtros, evitando bookmarks herdados de páginas duplicadas.
- [ ] Confirmar que tooltips não aparecem como páginas de navegação normais.
- [ ] Conferir valores de tooltip ao alterar mês/ano, instituição e contraparte.
- [ ] Comparar participações do tooltip com as respectivas matrizes, observando o denominador.
- [ ] Checar que a diferença de atrasos é exibida em **pontos percentuais**.
- [ ] Remover associações de tooltip inadequadas que tenham sido herdadas ao duplicar páginas.

**Atenção:** o tooltip de Evolução Mensal foi construído para explicar recebimentos, pagamentos e líquido. Ao ser usado na movimentação líquida mensal, deve receber corretamente o contexto do ponto e do ano; caso contrário, usar tooltip específico.