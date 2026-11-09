# M4S GESTÃO 2.1 — arquitetura

Interface em HTML/CSS/JavaScript puro, sem build. As únicas dependências de execução são o navegador e Chart.js 4.4.8 por CDN, com alternativa de gráfico nativo quando indisponível.

## Camadas

- `core.js`: validação, persistência local, migração, documentos e razão de estoque. Saldos nunca são editados diretamente pelas telas.
- `payments.js`: calendário, preço vigente, confirmação com valor congelado, ciclos, previsão, ajustes, baixas, estornos, parcelas e resumos de pagamento. As telas e os relatórios leem desta camada.
- `pending.js`: coleção única de pendências. Itens têm id, módulos espelhados, título, descrição, gravidade e ação. A mesma coleção alimenta sino, selos, faixas, alertas e relatórios.
- `views.js`: design system, cards, listas, KPIs, chips e uma instância de gráfico por tela de dados. Detalhes em folhas inferiores.
- `app.js`: formulários, navegação, eventos, notificações, compartilhamento, exemplos e ligação entre UI e domínio.
- `reports.js`: texto WhatsApp, períodos, seções e divisão dos links. Não altera estoque nem calcula regras financeiras por fora de payments.
- `sw.js`: cache versionado do aplicativo e cache opcional da biblioteca de gráficos.
- `manifest.json`: instalação e ícones.

## Entidades principais

Contas: id, nome, marketplace, cor, Full, ativa, cobertura e prazo.
Produtos: id, SKU, nome, tipo, fator do kit, base, variação, custo/peça e preços por conta.
Costureiras: id, nome, bairro, ativa, histórico de preços/data, dias de pagamento e política de fim de semana.
Fornecedores: id, nome, contato e ativo.
Insumos: id, nome, categoria, unidade e mínimo.
Rolos: id, identificação, tipo, kg, valor total, fornecedor, data e mínimo opcional.
Posições: id, produto, conta, vendas 7/15/30, estoque, trânsito, dias e prazo.
Documentos: id, tipo, data, referências, quantidade, perdas, previsão, insumos, preço confirmado, ciclo e status.
Razão: id, documento, data, item, local, origem, delta, saldo após e motivo.
Ajustes de pagamento: id, costureira, ciclo, data, delta assinado, motivo e criação.
Pagamentos: id, costureira, ciclo, data, forma, ids de recebimentos/ajustes, linhas congeladas por SKU/preço, peças, valor e saldo pago.
Estornos: id, pagamento, data, motivo, delta negativo e saldo pago.
Pagamentos de fornecedor: id, compra, parcela, data, valor e saldo devedor após.
Simulações: id, produto, conta, data e conjunto de valores usados.
Relatórios: id, criação, período e texto; últimos 20.
Auditoria: data/hora, id e descrição de cada transação salva.
Configurações: taxas, custos, cobertura, prazos, margem mínima, mínimo de tecido, telefone, lembrete e data/hora do backup.

## Integridade e migração

A transação clona o estado, executa a operação, valida as referências e o razão, verifica a revisão e tenta persistir. Só depois substitui a memória. Falhas de armazenamento não deixam meia operação salva. Dados inválidos na leitura inicial são preservados para recuperação, sem limpeza silenciosa.

O armazenamento interno mantém compatibilidade com os registros da versão 1 e acrescenta appVersion 2.1.0. O envelope de exportação é versão 2; a importação aceita envelopes 1 e 2. Recebimentos antigos já pagos são convertidos em eventos de baixa; dados de pagamento já confirmados conservam valores históricos. Não há downgrade compatível para a interface antiga.

Preço e ciclo são registrados no recebimento. Baixa consome ids específicos, impedindo dupla contagem. Estornar cria evento inverso, mantendo a baixa original. Alterações de calendário podem reagendar apenas recebimentos em aberto e registram de/para.

Saldos do razão são da ordem de lançamento, incluindo lançamentos retroativos; não representam uma reconstrução cronológica por data econômica. localStorage não fornece travamento distribuído: use uma aba por vez e mantenha backups.

## Layout e acessibilidade

Grid com min-width:0, nomes curtos, valores compactos com detalhe ao toque, sem tabelas na interface. Faixas de origem separadas. Cabeçalho sem sombra/retângulo decorativo. Áreas seguras e espaço inferior para navegação.

A grade usa duas colunas em tamanho normal. Se a fonte ampliada não deixar os nomes caberem, adapta para uma coluna. Isso evita esconder o nome por falta de espaço. Elementos de orientação mantêm tamanho estável, enquanto textos de leitura acompanham a ampliação. Motion reduzido desativa o pulso e a animação dos gráficos.
