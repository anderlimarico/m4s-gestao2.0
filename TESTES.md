# Verificação · M4S GESTÃO 2.1

## Executado nesta entrega

- 92 testes automatizados, sem instalar dependências: `node tests.mjs`.
- Verificação sintática dos módulos JavaScript.
- Chrome desktop com cenários em iframes de largura 320, 360 e 412 px: início, Full, pagamentos, produção, corte, estoque, produtos, futuros, compras, tecidos, fornecedor, margem, relatórios, alertas, backup e ajustes. A medição não encontrou scroll horizontal da página. A área útil fica menor quando o Chrome desktop reserva espaço para a barra de rolagem vertical.
- Ampliação de texto a 200% em início, pagamentos, estoque, Full e produtos. A grade adapta para uma coluna quando necessário. Foram corrigidos encaixe de marca, menu e faixa de pendências após a inspeção.
- Carga de exemplos, selos e contadores no navegador.
- Baixa de ciclo fictício: a pagar diminuiu, pago no mês aumentou, histórico apareceu e pendência sumiu.
- Cadastro de produto unitário com preço usando vírgula e conta selecionada; pendência de custo ausente apareceu.
- Relatório completo com pagamentos, histórico e links numerados para WhatsApp.

A instalação nativa no Android, funcionamento em modo avião real, compartilhamento nativo e impressão em PDF não foram executados neste ambiente. O envio no WhatsApp não foi feito: apenas os links e o texto foram conferidos.

## Checklist manual — pelo menos 30 cenários

Os itens abaixo são o roteiro de aceitação, com o resultado esperado. Não representam a afirmação de que todos foram executados manualmente. Vários também têm cobertura nos testes automatizados.

1. Início em 320 px: duas colunas, nomes completos e sem scroll horizontal.
2. Repetir em 360 px: selos sem cobrir nome/ícone.
3. Repetir em 412 px: último módulo ocupa a largura toda.
4. Fonte ampliada: grade passa a uma coluna se necessário; nomes não somem.
5. Fonte ampliada a 200%: marca, faixa de pendências e menu continuam utilizáveis.
6. Último card de cada tela: não fica escondido permanentemente pelo menu inferior.
7. Sem dados: aparece estado vazio e ação de adicionar, sem gráfico quebrado.
8. Alternar telas e filtros rapidamente: uma instância de gráfico por tela, sem canvas duplicado.
9. Adicionar registro: toast Salvo e KPIs atualizados.
10. Selo com contagem zero: não aparece.
11. Criar mais de 99 pendências: selo 99+; contador não invade o nome.
12. Urgente e atenção no mesmo módulo: selo vermelho prevalece.
13. Movimento reduzido: sem pulso dos selos.
14. Resolver uma entrega: o mesmo item sai de Costureiras, Futuros e Central, sem divergência.
15. Ocultar alerta comum: some também do selo correspondente.
16. Backup vencido: selo ! e alerta fixo; Lembrar depois só recolhe o aviso grande.
17. Cadastrar SKU vazio: erro claro e nenhuma gravação.
18. Cadastrar SKU repetido, variando maiúsculas: rejeitado.
19. Produto unitário: campos de kit ficam ocultos.
20. Selecionar kit: base e quantidade aparecem; base precisa ser unitária.
21. Preço por conta: só os campos das contas selecionadas ficam visíveis.
22. Kit 3, envio de 100: baixa exatamente 300 peças físicas.
23. Dividir origem do envio: cada origem baixa apenas a sua parcela.
24. Origem com saldo insuficiente: sem baixa parcial.
25. Estoque total insuficiente: fica um plano e a necessidade chega ao corte, sem criar trânsito.
26. Chegada ao canal: trânsito diminui e estoque do canal aumenta uma única vez.
27. Vendas 7 > 15 ou 15 > 30: rejeitado.
28. Vendas zeradas: sem divisão por zero.
29. SKU em duas contas disputando estoque: o detalhe avisa e mostra divisão proporcional indicativa.
30. Cortar sem tecido suficiente: bloqueia operação inteira.
31. Corte válido: kg diminuem, cortadas aumentam e rendimento aparece.
32. Envio à costureira: cortadas e insumos diminuem; cria Futuro.
33. Envio sem cortadas: vira plano e não baixa insumos.
34. Recebimento parcial: entra só o real e saldo é reprogramado.
35. Recebimento externo excedente sem motivo: rejeitado.
36. Recebimento excedente com motivo: entra quantidade real e o pagamento acompanha.
37. Etapa 2 sem etapa 1: rejeitada.
38. Lançar 20 e depois 30: soma 50, sem apagar o primeiro registro.
39. Botões +10/+20/+50: somam ao campo, sem gravar até confirmar.
40. Perda por etapa: consome a peça, sem criar pronta.
41. Etapa 4: entra em Interna e atende a previsão interna sem duplicar.
42. Corrigir produção: estorno e novo registro preservados.
43. Estorno com saldo já consumido: bloqueia sem alterar estoque.
44. Dias de pagamento 15 e 30 em janeiro: dois ciclos, sem incluir dia 31 indevido.
45. Dia 30 em fevereiro: cai no dia 28, ou 29 em ano bissexto.
46. Dias 30, 31 e Último em fevereiro: não duplica a mesma data.
47. Fim de semana Manter: data original preservada.
48. Sexta anterior: sábado/domingo ajustados corretamente, inclusive mudança de mês.
49. Segunda seguinte: sábado/domingo ajustados corretamente, inclusive mudança de mês.
50. Confirmação no dia 15: entra no ciclo 15; no dia 16: entra no ciclo 30.
51. Mudança de preço no meio do ciclo: cada recebimento mantém o preço da sua confirmação.
52. Apenas envio previsto: valor confirmado zero; previsão separada, sem gerar cobrança real.
53. Desconto com motivo: reduz o total; não pode exceder o valor confirmado e deixar saldo negativo.
54. Bônus com motivo: cria ajuste positivo separado.
55. Marcar pago: pede data real, confirmação e forma opcional; cria evento e fecha ciclo.
56. Data de pagamento futura ou anterior às confirmações: rejeitada.
57. Confirmação depois de um fechamento antecipado: vai ao próximo ciclo aberto.
58. Pagar duas vezes o mesmo ciclo: bloqueado.
59. Desfazer pagamento: pede motivo/confirmação e o saldo volta a pagar; registro original permanece.
60. Pagar após estorno: permitido uma vez, sem duplicar peças.
61. Alterar calendário com reagendamento: abertos mudam e guardam de/para; baixas pagas não mudam.
62. Pagamento vence em 2 dias: atenção; hoje/atrasado e valor positivo: vermelho.
63. Pagamento sem valor confirmado: não cria pendência falsa de cobrança.
64. Texto de pagamento: negrito WhatsApp com asterisco simples, valores em R$, sem tabela ou barras separadoras.
65. Copiar/compartilhar/baixar resumo só de uma costureira: não inclui as outras.
66. Compra pronta não recebida: não cria estoque de Fornecedor.
67. Confirmar compra recebida: entrada única; botão deixa de oferecer nova confirmação.
68. Parcelas com centavos: soma confere com total.
69. Marcar parcela paga: devedor diminui; evento de pagamento fica no histórico.
70. Custo do rolo por peça: usa kg reais e valor do rolo usado.
71. Comparar interna x costureira: vencedor estimado identificado.
72. Margem negativa: vermelho; abaixo da meta: laranja; acima: verde.
73. Taxas que tornam preço mínimo inviável: mensagem clara, sem infinito.
74. Relatório Hoje/Ontem/Semana/Mês: filtros de movimentos corretos; saldos atuais identificados.
75. Seção Pagamentos no relatório: lê ciclos da camada única, incluindo datas, confirmados, previstos e pagos.
76. Relatório sem dados: omite seções vazias.
77. Relatório longo: links numerados; preferencialmente divide entre linhas; preserva todos os caracteres.
78. Cópia bloqueada: oferece campo selecionável.
79. Navegador sem Web Share: botão não aparece; copiar e baixar continuam disponíveis.
80. PDF: abre impressão do navegador; escolher Salvar como PDF.
81. Telefone da esposa com letras: rejeitado; DDD + número: aceito; DDI 55 opcional normalizado.
82. Exportar backup: conferir Downloads; data/hora e selo atualizam.
83. Compartilhar backup cancelado: data não atualiza.
84. Importar JSON truncado, de outro app ou com saldo adulterado: rejeitado antes de substituir.
85. Restaurar backup antigo válido: mantém saldos e migra pagamentos já feitos sem duplicar.
86. Reimportar/revalidar a mesma migração: não duplica baixas.
87. Backup 2.1: contém pagamentos, estornos, ajustes e relatórios.
88. Apagar tudo: exige confirmação e digitação APAGAR; cancelar preserva.
89. Armazenamento cheio/bloqueado: erro claro, sem meia operação salva.
90. Duas abas com revisões diferentes: aviso e gravação bloqueada na antiga.
91. Fechar/reabrir: dados persistem no mesmo navegador.
92. Instalar no Android e modo avião após primeira carga: interface e operações locais disponíveis.
93. CDN indisponível: gráfico reduzido nativo, sem bloquear os dados.
94. Novo service worker: fechar abas antigas e reabrir atualiza sem apagar lançamentos.
95. Abrir em outro aparelho: não aparece sincronizado; transferência é por backup e substitui destino.

Ausência absoluta de bugs não pode ser garantida. Confira os primeiros lançamentos com contagem física e guarde uma cópia de backup fora do aparelho.
