# Qualidade dos dados, limitações e privacidade

## Origem e escopo

O Finance Flow usa uma base didática de **2018–2019**, importada de planilha Excel com cadastros de clientes, fornecedores e contas bancárias e registros de pagamentos e recebimentos.

A publicação do dashboard **não equivale** a uma auditoria financeira. A origem bruta e o arquivo Power BI não são disponibilizados no repositório por precaução com cadastros, identificadores bancários e caminhos locais.

## Decisões de qualidade

- **Datas:** conservar emissão, vencimento e movimentação; criar `DiasDesvioVencimento`, `SituacaoPrazo` e `QualidadeDatas`.
- **Valores:** preservar a precisão numérica original, inclusive frações de centavo, e formatar resultados com duas casas na interface.
- **Chaves:** usar IDs de cliente, fornecedor e conta para relacionamento 1:* com dimensões.
- **Cadastro x movimento:** distinguir 812 clientes e 630 fornecedores cadastrados de **44** e **231** contrapartes com registros, respectivamente.
- **Calendário:** os visuais padrão filtram a data de movimentação; outras datas usam relacionamentos inativos.
- **Contagens:** indicadores de atraso são baseados em número de registros, não em montantes financeiros.

## Limitações importantes

1. **Movimentação líquida ≠ lucro:** não estão presentes todos os componentes contábeis, nem saldos bancários iniciais e finais.
2. **Após vencimento ≠ inadimplência:** o registro tardio não informa se existem títulos pendentes.
3. **Concentração ≠ risco comprovado:** um Top 5 alto indica dependência relativa dos movimentos registrados, não risco de crédito comprovado.
4. **Geografia cadastral:** UF do cliente não demonstra onde a transação ocorreu.
5. **Instituições financeiras:** correlações entre banco e pontualidade não implicam responsabilidade ou causalidade do banco.
6. **Amostras pequenas:** variações extremas e taxas em segmentos com poucos registros devem ser lidas junto com suas contagens.
7. **Séries históricas:** os dados cobrem 2018–2019 e não mostram operações atuais.

## Cuidados de filtro e tooltip

- Usar `dCliente` com `fRecebimentos` e `dFornecedor` com `fPagamentos`, evitando repetir o total geral por categoria sem relação.
- Medidas de participação devem retirar o filtro da contraparte no **denominador**, respeitando a população de comparação pretendida.
- Tooltips com mês e ano devem receber o contexto correto do eixo e da legenda.
- Medidas em pontos percentuais devem ser formatadas como número com sufixo **p.p.**, não como porcentagem.

## Checklist de publicação

- [ ] Confirmar que a base e os nomes exibidos podem ser publicados publicamente.
- [ ] Revisar se informações pessoais, contas, IDs bancários, credenciais ou caminhos locais aparecem em tabelas, visuais e páginas de tooltip.
- [ ] Verificar se botões de navegação e de limpeza de filtros funcionam após publicação.
- [ ] Comparar os KPIs sem filtros com os valores validados da base.
- [ ] Testar percentuais de clientes e fornecedores em múltiplos exemplos.
- [ ] Validar o comportamento de zero e nulo nas séries de atraso.
- [ ] Registrar alterações futuras nas regras de negócio.

**Publicar na Web** torna o relatório acessível a qualquer pessoa com o link e potencialmente descobrível. Use somente bases e campos apropriados à divulgação pública.