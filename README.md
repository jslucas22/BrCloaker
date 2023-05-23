# BrCloaker - Build Brasileira por @jslucas22 - & *i get it from ->  [Yellow Web](https://yellowweb.top)*
**Use a versão PHP 7.2 ou superior e crie certificados HTTPS para todos os seus domínios!!!**

# Descrição
Um script de cloaking modificado para arbitragem de tráfego, originalmente encontrado em [Black Hat World](http://blackhatworld.com).

# Instalação
Baixe a versão mais recente de todos os arquivos deste repositório e faça o upload deles para o seu servidor de hospedagem. O servidor de hospedagem deve ter o PHP versão 7.2 ou superior e você deve criar um certificado HTTPS para o seu domínio. Sem HTTPS, o cloaker não funcionará corretamente e simplesmente habilitar o HTTPS no CloudFlare não funcionará! Sim, se você estiver usando o CloudFlare, depois de obter um certificado adequado, ative o HTTPS no modo "Full"! Posso recomendar a hospedagem Beget para cloaking, é simples e conveniente e você pode obter um certificado HTTPS em apenas alguns cliques.

Se você tiver pagina de vendas e cloaks locais, crie uma pasta para cada um deles na pasta raiz do cloaker e copie os arquivos correspondentes para cada pasta.
Por exemplo:
Se você tiver 2 cloaks e 2 landings, crie 2 pastas para os cloaks: p1 e p2. E duas pastas para as landings: land1, land2.

# Configuração
Para configurar o cloaker, foi criada uma interface do usuário que pode ser acessada em: https://seu.domínio/admin?password=12345 Não se esqueça de alterar a senha de acesso!

## Configuração do White
White é a página exibida ao visitante que não passou pelos filtros do cloaker. Eles são visitantes indesejados.

Primeiro, você precisa decidir que tipo de pagina white deseja usar. O cloaker pode:

- Mostrar paginas whites
- Redirecionar para outro site
- Carregar conteúdo de qualquer outro site por meio de CURL
- Retornar qualquer código HTTP (por exemplo, erro 404 ou simplesmente 200)

Quando decidir, altere o valor para um dos seguintes:

### Rota do diretório da página white
Isso é para às páginas whites. Você deve criar uma pasta na raiz do cloaker, por exemplo, *white*, e copiar todos os arquivos do white para essa pasta. Em seguida, insira o nome da pasta no campo correspondente.

### Redrecionamento
Reciona todo o tráfego do white para outro site. Insira o URL do site e selecione o tipo de redirecionamento. Pode ser: 301, 302, 303 ou 307. Pesquise a diferença, se for importante para você.

### CURL
Carrega o conteúdo de qualquer outro site. Insira o URL do site no campo correspondente.

### Retornar código HTTP
Você pode retornar qualquer erro HTTP para o tráfego do white. Por exemplo: 404. Ou o código 200 para exibir uma página em branco.

## Whites individuais para diferentes domínios
Se você tiver vários domínios (ou subdomínios) vinculados à sua hospedagem e estiver enviando tráfego para eles, pode configurar diferentes paginas whites para diferentes domínios, alterando a configuração correspondente.

Em seguida, preencha os campos. O formato é o seguinte:
`seu.domínio => whiteaction:valor`

Por exemplo:
`https://meudominio.com => curl:https://ya.ru`
Todos os valores possíveis para whiteaction são: *folder, curl, redirect, error*

## Configuração do funil
O cloaker pode trabalhar com os seguintes funis:
- landing local (ou várias landings locais)
- cloak local -> landings locais
- cloaks locais + redirecionamento para landing em outro site
- redirecionamento imediato para outro site
Vamos analisar todas essas configurações.

### Pagina de vendas locais
Você pode usar uma ou várias landings. O tráfego será dividido igualmente entre elas. Por exemplo, para duas landings, seria 50/50. Cada landing deve estar em sua própria pasta. Marque a opção "Não usar prelanding" e selecione o método de carregamento das landings como "Local landings from folder". Se houver várias landings, use vírgula como separador. Por exemplo:
`land1,land2`

### Cloaks locais - Paginas de vendas locais
Faça exatamente o mesmo que na seção sobre Landings locais, mas também preencha o campo "Folders where prelands are located". Por exemplo, para dois cloaks:
`p1,p2`

### Cloaks locais + redirecionamento
Preencha os nomes das pastas dos cloaks. Por exemplo, para dois cloaks:
`p1,p2`
Em seguida, altere o método de carregamento das landings para Redirecionamento. Último passo: insira o URL de redirecionamento.

### Redirecionamento imediato
Se você deseja simplesmente redirecionar todo o tráfego que passa pelos filtros do cloaker, use **$black_action = *'redirect'** e preencha o URL de redirecionamento em **$black_redirect_url.** Também selecione o tipo de redirecionamento: 301, 302, 303 ou 307. Pesquise a diferença, se for importante para você. Insira o tipo de redirecionamento em **$black_redirect_type.**

### Configuração do script de conversão da pagina de vendas local
Cada landing tem a capacidade de enviar leads para um processador de pagamento. E cada processador de pagamento tem sua própria lógica de envio de leads.

Por padrão, o cloaker procura pelo arquivo order.php na pasta da landing. Se o script do seu processador de pagamento tiver um nome diferente, renomeie o valor da variável $black_land_conversion_script. Para descobrir o nome do script de envio, abra o arquivo de índice da landing e procure por qualquer formulário - <form. Verifique o atributo action do formulário, pois é onde o script está definido. Se não houver atributo action, significa que o lead é enviado pelo arquivo de índice!
Se o script estiver em uma pasta, insira o caminho relativo para o script, por exemplo:
**$black_land_conversion_script=*'pasta/conversion.php'*;**
Depois de configurar tudo isso, envie um lead de teste. Se o lead não aparecer nas estatísticas do processador de pagamento, abra o script de envio de leads e verifique se não há uma linha
`exit();`
Se houver, remova ou comente essas linhas (observando a sintaxe da linguagem!!!).

### Configuração da página de Agradecimento.
O visitante é redirecionado para a página de Agradecimento depois de enviar seus dados no formulário preto *ou branco*! O conteúdo da página é carregado a partir da pasta *thankyou*. Se você olhar dentro dela, encontrará vários arquivos HTML com códigos de idiomas de duas letras como nome. Digite o idioma desejado para a página de Agradecimento em **$thankyou_page_language**.

Se não houver uma página de Agradecimento no seu idioma, crie uma. É simples: abra a versão em inglês da página de Agradecimento no navegador Chrome e use o tradutor embutido para traduzi-la para o idioma desejado. Em seguida, salve a tradução com o nome apropriado, por exemplo, *IT.html*.
**Atenção**: abra a página traduzida em um editor de texto e verifique se as duas macros *{NAME}* e *{PHONE}* NÃO foram traduzidas. Se tiverem sido, restaure-as para o original!

Se você deseja usar sua própria página de Agradecimento, renomeie-a com o código de idioma de duas letras e coloque todos os arquivos necessários na pasta *thankyou*.
#### Coleta de e-mails na página de Agradecimento
Por padrão, a página de Agradecimento contém um formulário para coletar endereços de e-mail. Se você não precisa dele, simplesmente remova-o do código da página. Mas se você precisa, é necessário criar outra página: aquela em que o usuário será redirecionado APÓS fornecer o seu e-mail. Ela deve ser nomeada usando o código de duas letras do idioma + email.html. Por exemplo: *SKemail.html*. Um exemplo dessa página está na pasta *thankyou*.

## Configuração de pixels
Você pode adicionar vários pixels aos seus prelanders e landings. Aqui está a lista completa:
- Yandex.Metrica
- Google Tag Manager
- Facebook Pixel

### Yandex.Metrica
Para adicionar o script do Yandex.Metrica aos seus prelanders e landings, basta preencher o ID da métrica em **$ya_id**.
### Google Tag Manager
Para adicionar o script do Google Tag Manager aos seus prelanders e landings, basta preencher o ID do GTM em **$gtm_id**.
### Facebook Pixel
O ID do pixel do Facebook é extraído do link. Ele deve estar no formato: *px=1234567890*. Por exemplo:
`https://your.domain?px=5499284990`
Se o parâmetro *px* estiver presente no URL, o script completo do pixel do Facebook será adicionado à página de Agradecimento. Você pode definir o evento do pixel desejado na variável **$fb_thankyou_event**. Por padrão, é *Lead*, mas você pode alterá-lo para *Purchase* ou qualquer outro evento.
Você também pode usar o evento *PageView*. Para ativá-lo, altere **$fb_use_pageview** para *true*. Após isso, o código do pixel será adicionado a todas as páginas principais de prelanders e landings, e essas páginas enviarão o evento *PageView* para o Facebook para cada visitante.
**Observação:** Use a extensão *Facebook Pixel Helper* no Google Chrome para verificar se os eventos estão sendo enviados corretamente!
## Configuração dos filtros do cloaking
O cloaking pode filtrar o tráfego com base nos seguintes critérios:
- Base de IP embutida
- Sistema operacional do visitante
- País do visitante
- User Agent do visitante (navegador)
- Provedor de Internet (ISP) do visitante
- Presença de referenciador
- Qualquer parte do URL pelo qual o visitante foi direcionado

*Observação:* sempre que você desejar usar vários parâmetros, use uma vírgula como separador!
Para começar, adicione todos os sistemas operacionais permitidos em **$os_white**. Aqui está a lista de opções disponíveis:
- Android
- iOS
- Windows
- Linux
- OS X
- e outros menos populares...

Selecione os que você precisa.
Em seguida, preencha os códigos de duas letras dos países permitidos em **$country_white**. Por exemplo: *RU,RS,IT,ES*.

Agora, remova todos os provedores de internet indesejados. Adicione-os a **$isp_black**. Por exemplo: *google,facebook,yandex*. Se você quiser proteger sua oferta de serviços de análise de tráfego, adicione todos os provedores em nuvem aqui, como: *amazon,azure*, etc.

Adicione palavras à lista de User Agents proibidos em **$ua_black** para filtragem. Por exemplo: *facebook,Facebot,curl,gce-spider*.

Adicione palavras à lista de tokens em **$tokens_black** que podem estar presentes no URL pelo qual o visitante foi direcionado e sinalizam que um redirecionamento para uma página branca é necessário. Ou deixe essa variável vazia - ''.

Se você possui uma lista adicional de endereços IP que deseja bloquear, adicione-os a **$ip_black**.
E finalmente: se você deseja bloquear visitantes *diretos*, altere **$block_without_referer** para *true*. **Atenção**: alguns sistemas operacionais e navegadores podem não transmitir corretamente ou não transmitir o referenciador. Portanto, se você deseja usar essa função, teste-a primeiro com um pequeno volume de tráfego, caso contrário, você pode perder dinheiro.

## Configuração de distribuição de tráfego
Você pode temporariamente desativar todos os filtros de cloaking e enviar todo o tráfego para o white. Por exemplo, durante a moderação. Para fazer isso, altere **$full_cloak_on** para *true*.
Você também pode desativar todos os filtros de cloaking e enviar todo o tráfego para o black. Por exemplo, para testar o black. Para fazer isso, altere **$disable_tds** para *true*.
Você pode manter o "caminho" do usuário (ou seja, os prelanders e landings que ele visita na sequência). Assim, ele sempre verá as mesmas páginas, não importa quantas vezes ele entre. Para fazer isso, altere **$save_user_flow** para *true*.
## Configuração de estatísticas e postback
A visualização das estatísticas é protegida por senha. Defina a senha na variável **$log_password**.
Se você sempre nomeia suas criativas da mesma forma e transmite os nomes delas para o cloaking do seu provedor de tráfego, você poderá ver quantos cliques cada criativa recebeu na página de estatísticas. Para fazer isso, defina o nome do parâmetro que contém os nomes das criativas na variável **$creative_sub_name**. Por exemplo, se o link do provedor de tráfego for assim:
`https://seu.domínio?mycreoname=ótima_criativa`
então você precisa alterar a variável da seguinte maneira:
`$creative_sub_name = 'mycreoname';`
e na estatística você verá:
*ótima_criativa - 154 cliques*
### Configuração de postback
O cloaking pode receber postbacks dos provedores de pagamento (PP) e exibir o status dos leads na estatística. Primeiro, você precisa transmitir um ID exclusivo do visitante para o PP - subid. O subid é gerado automaticamente para cada visitante e é armazenado em um cookie. Você precisa perguntar ao seu gerente como transmitir o subid para o PP (eles geralmente conhecem esse parâmetro como clickid). Peça a eles que informem em qual subtag você deve transmiti-lo, pois diferentes PPs têm subtags diferentes. Como exemplo, vamos supor que a subtag seja chamada *sub1*. O array **$sub_ids** é responsável por transmitir os parâmetros para o PP. Vamos alterar o nome à direita do *subid* para *sub1*:
`$sub_ids = array("subid"=> "sub1", .....);`
Dessa forma, configuramos o cloaking para pegar o valor do cookie *subid* e transmiti-lo para a subtag *sub1*. Se, por exemplo, o subid fosse *12uion34i2*, o resultado seria:
- se for um prelander local, um campo de entrada oculto será adicionado a todos os formulários do prelander
`<input type="hidden" name="sub1" value="12uion34i2"`
- se for um redirecionamento, será: `http://redirecionamento.link?sub1=12uion34i2`

Em seguida, precisamos indicar ao PP para onde enviar o postback. O arquivo *postback.php* é responsável pelo processamento dos postbacks no cloaking. Precisamos obter dois parâmetros do PP: *subid* e o status do lead. Usando essas informações, o cloaking atualiza o status do lead nos logs e exibe as alterações na estatística.
Consulte a documentação do PP ou pergunte ao seu gerente qual macro o PP usa para transmitir o status do lead. Geralmente é chamado *{status}*. Voltando ao nosso exemplo: como enviamos o *subid* na subtag *sub1*, a macro para obter o *subid* do PP será *{sub1}*. Vamos criar a URL completa do postback. Você deve inseri-lo no campo URL de postback no seu PP. Por exemplo:
`https://seu.domínio/postback.php?subid={sub1}&status={status}`
E finalmente, entenda ou pergunte ao seu gerente quais são os status que o PP envia no postback. Geralmente são:
- Lead
- Purchase
- Reject
- Trash

Se o seu PP envia status de maneira diferente, ajuste os valores das seguintes variáveis de acordo com as configurações do PP:
- **$lead_status_name**
- **$purchase_status_name**
- **$reject_status_name**
- **$trash_status_name**

Após configurar, envie um lead de teste e observe como o status do lead é atualizado para Trash na página de Leads na estatística.

## Configuração de scripts adicionais
### Desativar o botão "Voltar"
Você pode desativar o botão "Voltar" no navegador do visitante para que ele não possa sair da sua página. Para fazer isso, altere **$$disable_back_button** para *true*.
### Substituir o botão "Voltar"
Você pode alterar o endereço para o qual o visitante será redirecionado ao clicar no botão "Voltar". Essa função pode ser usada para monetização ou para enviar o visitante para outra oferta. Altere **$replace_back_button** para *true* e insira o endereço em **$replace_back_address**.
**Atenção:** Não use esse script junto com o script **Desativar o botão Voltar**!!!
### Desabilitar o menu de contexto, seleção de texto e salvar usando Ctrl+S
Você pode desativar a capacidade de selecionar texto em seus prelanders e landings, desativar a capacidade de salvar a página usando as teclas Ctrl+S e também desativar o menu de contexto do navegador. Para fazer isso, altere **$disable_text_copy** para *true*.
### Substituir o prelander por outro site
Você pode habilitar essa opção para que o landing seja aberto em uma nova guia do navegador e o prelander seja substituído por outro site. Isso pode ser usado para monetizar o tráfego. Para habilitar, altere **$replace_prelanding** para *true* e insira o endereço em **$replace_prelanding_address**.
### Máscaras de telefone
Você pode configurar o cloaking para aplicar máscaras a campos de entrada de número de telefone. Quando você habilita essa função, o visitante não pode inserir letras no número e não pode inserir mais ou menos dígitos do que o necessário. A máscara define os prefixos de telefone, a quantidade de dígitos e os separadores. Para habilitar as máscaras, altere **$black_land_use_phone_mask** para *true* e edite a máscara em **$black_land_phone_mask**.
# Verificação
Adicione o código do seu país à lista de permitidos para poder acessar o black. Passe por todos os elementos do funil. Verifique o pixel e os postbacks no PP.
# Visualização de tráfego e estatísticas
Depois de começar a enviar tráfego, você pode ver as estatísticas de tráfego na página de Estatísticas:
`https://seu.domínio/logs?password=suasenha`
onde *suasenha* é o valor da variável **$log_password** no arquivo *settings.php*.
# Integração de JS do cloaking com construtores de página
`<script src='https://seu.domínio/js/index.php'></script>`
# Contatos
Para todas as perguntas, envie issues no GitHub ou no grupo http://vk.com/yellowweb.

