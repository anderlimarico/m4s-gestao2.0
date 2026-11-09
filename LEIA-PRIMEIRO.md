# M4S GESTÃO · versão 2.1

Aplicativo de gestão da confecção, com visual pastel, cards curtos, gráficos, estoque por origem, pendências unificadas e pagamentos por ciclo. Os arquivos estão prontos para o GitHub Pages: não precisam de build nem instalação de pacotes.

## Atualizar a versão 1 sem perder os dados

1. Abra a versão antiga no mesmo aparelho e navegador em que você já lança os dados.
2. Exporte um backup JSON e confira se ele está em Downloads. Guarde outra cópia fora do aparelho.
3. Extraia o novo ZIP. Envie os arquivos extraídos ao MESMO repositório, na MESMA pasta do `index.html` antigo, substituindo os arquivos de mesmo nome. Inclua os novos arquivos `payments.js`, `pending.js` e `views.js`.
4. Não apague dados do navegador. Não exclua o repositório nem troque seu endereço como procedimento de atualização.
5. Aguarde a publicação do GitHub. Feche TODAS as abas e janelas da versão antiga, inclusive o app instalado. Abra o endereço com internet. A nova versão do service worker pode precisar dessa abertura e de um segundo fechamento/reabertura para assumir controle.
6. Confirme a identificação `VERSÃO 2.1` no início e a grade pastel com todos os módulos.
7. Na mesma origem, a chave local dos dados é preservada. A versão 2.1 normaliza os novos campos; mantém saldo, histórico e preços já registrados. Baixas antigas de costureiras são convertidas em eventos de pagamento e não duplicadas.
8. Confira o estoque e os pagamentos. Se abriu em outro navegador/aparelho ou mudou de endereço, importe o backup antigo pelo módulo Backup, com as confirmações. Isso SUBSTITUI a base do destino.
9. Exporte um novo backup na 2.1. O novo arquivo tem formato externo de versão 2; a 2.1 também importa o formato anterior de versão 1. Não use o backup novo para voltar ao aplicativo antigo.

Nunca deixe as versões antiga e nova abertas simultaneamente para lançar dados. O aplicativo bloqueia revisões divergentes, mas localStorage não oferece uma transação distribuída entre abas. Use uma aba por vez.

## Publicar do zero no GitHub Pages

1. Extraia o ZIP no notebook.
2. Acesse https://github.com e faça login.
3. Crie um repositório chamado `m4s-gestao`. A opção gratuita comum usa repositório público.
4. Em Add file → Upload files, envie os arquivos extraídos. Não envie apenas o ZIP. `index.html`, os arquivos JS/CSS, o manifesto e os ícones devem ficar diretamente na raiz.
5. Confirme em Commit changes.
6. Abra Settings → Pages.
7. Em Source, escolha Deploy from a branch.
8. Escolha branch main e pasta /(root). Clique em Save.
9. Aguarde a conclusão e abra o endereço que o próprio GitHub apresentar.

Referência oficial: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site

## Instalar no Android

1. Abra o endereço publicado no Google Chrome, com internet.
2. Aguarde o carregamento dos arquivos e visite os módulos.
3. No menu de três pontos, procure Adicionar à tela inicial, Instalar app ou Instalar e criar atalho. A redação varia por versão.
4. Escolha a instalação e confirme.
5. Abra pelo ícone M4S GESTÃO.
6. No módulo Backup, use Persistência. Mesmo com persistência concedida, continue exportando backups.
7. Depois da primeira carga online completa, feche e teste em modo avião. Cadastros, cálculos e relatórios locais funcionam sem servidor. WhatsApp e compartilhamentos externos dependem do aparelho e da conexão.

Referência oficial: https://support.google.com/chrome/answer/9658361?co=GENIE.Platform%3DAndroid

Não abra o HTML diretamente pelo gerenciador de arquivos: módulos JavaScript e service worker precisam de uma origem HTTP segura como o HTTPS do GitHub Pages.

## Configuração inicial

- Ajustes → Geral: cobertura, prazo, margem mínima, limite de tecido, lembrete de backup e número opcional da esposa.
- Ajustes → Taxas ML / Taxas Shopee: configure taxas REAIS. Os valores iniciais são zero e não correspondem às regras atuais dos marketplaces.
- Produtos: cadastre primeiro o unitário; depois kits com seu SKU base. Preços são por anúncio/conta; custo é por peça base.
- Estoque: informe saldos iniciais por origem com Ajustar. Não registre como nova compra o mesmo estoque já incluído no saldo inicial.
- Tecidos e Compras: cadastre rolos e insumos antes de usar em corte/envio.
- Costureiras: configure preço e dias de pagamento antes de confirmar entregas.

## Como funcionam os pagamentos na 2.1

Aba Costureiras: cadastro, valor/peça e um ou mais dias de pagamento (1 a 31 ou último dia). Em fevereiro, dias inexistentes caem no último dia. A regra de fim de semana ajusta a data para sexta anterior, segunda seguinte ou mantém. Datas que coincidem após ajuste formam um único ciclo.

Aba Envios: o envio de cortadas cria um Futuro. Se faltar corte, fica como plano e gera pendência, sem baixar insumos ou criar estoque negativo.

Ao conferir em Futuros, informe a data real da confirmação. Essa data define o preço vigente e o primeiro pagamento programado que a cobre. O preço fica congelado no recebimento: alterar o cadastro não recalcula peças já confirmadas. Não existe pagamento por peças apenas enviadas ou previstas.

Aba Pagamentos: cada ciclo vai da data de pagamento anterior (exclusive) até a atual (inclusive). Uma confirmação feita depois que um ciclo foi fechado entra no próximo ciclo, inclusive em caso de fechamento antecipado. O valor confirmado e o previsto ficam separados.

Toque no card do ciclo para ver SKU, peças, preço, subtotal, ajustes e previsão. Ajustes usam valor positivo e seleção de acréscimo ou desconto, sempre com motivo. Desconto não pode deixar o ciclo negativo.

Marcar pago pede data real e forma opcional. É um registro de controle: o aplicativo NÃO faz Pix nem movimenta sua conta bancária. O pagamento congela a lista de recebimentos e ajustes daquele ciclo. Desfazer pede motivo e confirmação, cria um estorno e volta a deixar as mesmas peças a pagar. O registro original permanece no histórico.

Ao editar os dias, a opção Aplicar calendário aos ciclos abertos reagenda recebimentos ainda não pagos, registrando a alteração. Baixas já feitas permanecem nas datas originais.

A data de confirmação pode ser retroativa. Depois de um fechamento, confirmações lançadas posteriormente são direcionadas ao próximo ciclo aberto para não entrar em uma baixa que já ocorreu. Confira a data informada; não use uma data futura para recebimento físico ou pagamento.

Os resumos de pagamento podem ser copiados, compartilhados quando suportado e baixados em TXT. São preparados para WhatsApp; o envio final é sempre feito por você.

## Pendências e selos

O sino conta itens únicos. A mesma entrega pode aparecer no card Costureiras e no card Futuros, sem ser contada duas vezes no sino. Por isso somar visualmente todos os selos dos módulos pode dar mais que o total do sino; Alertas espelha o total e não cria novos alertas de si próprio.

Vermelho significa urgente, vencido ou hoje no pagamento; laranja é atenção. Contagem zero não tem selo; mais de 99 aparece 99+. O Backup vencido usa !. O pulso é discreto e respeita a preferência de reduzir movimento.

A faixa de pendências do módulo expande ao tocar. Cada item leva à ação correspondente. Ocultar um alerta oculta o mesmo item nos selos e na central; resolver a operação remove a condição. O Backup vencido não pode ser ocultado permanentemente: Lembrar depois apenas recolhe o aviso grande naquela sessão.

## Estoque, kits e planejamento

Peças prontas ficam em três origens: Interna, Costureiras e Fornecedor. A coluna TOTAL é somente a soma; não é um quarto estoque. Cortadas e etapas em processo ficam separadas. Kits consomem o SKU base: 100 kits de 3 descontam 300 peças.

Na produção interna, cada etapa consome a anterior; perdas são informadas à parte. Só a etapa 4 entra em pronta Interna. A etapa final atende automaticamente futuros internos existentes, sem receber duas vezes. Correções geram estorno + novo registro, mantendo os originais; se houver dependência posterior ou peças já consumidas, a correção é bloqueada.

Costureiras só recebe peças cortadas. Entregas parciais entram pelo valor real e mantêm saldo reprogramado. Excedentes externos exigem motivo e ficam identificados no histórico.

No Full, a média é (7 dias ÷ 7 × 50%) + (15 dias ÷ 15 × 30%) + (30 dias ÷ 30 × 20%). A sugestão desconta estoque e trânsito e arredonda para cima. Previsão não é garantia de vendas. A data limite pressupõe trânsito chegando no prazo; atualize regularmente os números do canal.

Contas sem Full usam estoque já separado em casa para aquela conta. Transfira por envio e confirme chegada para manter essa reserva fora do estoque central. Uma fotografia manual de estoque no canal não faz uma segunda baixa física.

Corte soma metas internas com planos externos. Compara o resultado com a demanda comercial ainda descoberta e cobre o maior compromisso, evitando duplicar uma mesma venda. Desconta cortadas, etapas em processo e entregas externas abertas da parcela correspondente. Isso não faz otimização de encaixe nem agenda capacidade diária.

## Relatórios

Escolha período em chips e conteúdo no painel recolhido. Movimentos são filtrados; saldos/pendências mostram a fotografia atual. Pagamentos mostram próximos 30 dias e atrasados, com baixas filtradas pelo período e total pago no mês identificado.

Copiar usa a área de transferência; se bloqueada, oferece texto selecionável. Compartilhar aparece apenas quando o navegador tem suporte. WhatsApp prepara o texto, sem enviar automaticamente. Textos longos são divididos em partes numeradas. Baixar gera TXT; PDF usa a impressão do navegador, escolhendo Salvar como PDF. São preservados os últimos 20 relatórios.

## Offline e gráficos

Chart.js 4.4.8 é carregado por CDN e armazenado pelo service worker após uma carga online bem-sucedida. O cache de gráficos é opcional para não bloquear a instalação quando a CDN falha. Se a biblioteca não estiver disponível, a tela mostra barras nativas reduzidas com os mesmos números; as operações e os dados continuam acessíveis.

O novo gráfico destrói a instância anterior antes de redesenhar. Com dados zerados, aparece Sem dados ainda. Cada tela de dados tem até um gráfico; os menus de navegação e a grade inicial não acrescentam gráficos desnecessários.

## Privacidade e limites

Sem login, backend ou sincronização. Cada navegador/aparelho tem sua base. Para transferir dados, exporte em um e importe no outro; a importação substitui e não mescla alterações concorrentes.

O código e o endereço do GitHub Pages podem ser públicos. Os lançamentos ficam no navegador e nos JSON que você exportar. Não coloque backups nem dados pessoais no repositório. Qualquer pessoa com acesso ao navegador desbloqueado pode ver seus registros.

O navegador pode apagar armazenamento; persistência reduz risco, sem substituição do backup. Exporte e confira Downloads. O lembrete registra a solicitação de download — não consegue verificar se você guardou o arquivo. Compartilhar um backup cancelado não atualiza a data.

Notificações são verificadas ao abrir o app. Com ele fechado, não há servidor para dispará-las. API de compartilhamento, impressão em PDF, instalação e permissões dependem do aparelho.

Foram executados 92 testes automatizados e verificações no Chrome em contêineres de 320, 360 e 412 px. O roteiro completo e o alcance estão em TESTES.md. A instalação nativa Android, modo avião real e o envio efetivo pelo WhatsApp precisam ser conferidos no seu aparelho. Não é possível garantir ausência absoluta de bugs.
