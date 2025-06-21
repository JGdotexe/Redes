
## Antes da leitura

É interessante ter um panorama geral do que será visto
![[Pasted image 20241101222709.png]]
![[Pasted image 20241101222731.png]]
![[Pasted image 20241101222955.png]]

#### Bibliografia : [Redes de Computaroes e a Internet - Kurose](blob:https://github.com/2d1db8ec-3df0-45ae-a6b6-03479d952698)

## 1.1 Uma Descrição dos Componentes da rede

- Pode-se descrever a internet de duas maneiras
  - Componentes de Hardware

![[Pasted image 20241101215614.png]]

- Hospedeiros ou Sistemas Finais
  - Celulares, tv, desktop, tudão
  - Servidores também se incluem

### Panorama 1° cap

Os sistemas finais são conectados entre si por **enlaces (*links*) de comunicação e comutadores (*switches*) de pacotes**. na seção 1.2 veremos que há muitos tipos de enlaces de comunicação, que são constituídos de diferentes tipos de meios físicos, entre eles **cabos coaxiais, fios de cobre , fibras óticas e ondas de rádio.** Enlaces diferentes podem transmitir dados em taxas diferentes, sendo a **taxa de transmissão** de  um enlace medida em bits por segundo. Quando um sistema final possui dados para enviar a outro sistema final, o sistema emissor segmenta esses dados e adiciona bytes de cabeçalho a cada segmento. Os **pacotes** de informações resultantes são enviados através da rede ao sistema final de destino, onde são remontados para os dados originais 

Um comutador de pacotes encaminha o pacote que está chegando em um de seus enlaces de comunicação de entrada para um de seus enlaces de comunicação de saída. Há comutadores de pacotes de todos os tipos e formas, mas os dois mais proeminentes na Internet de hoje são **roteadores**e **comutadores de camada de enlace**. Esses dois tipos de comutadores encaminham pacotes a seus destinos finais. Os comutadores de camada de
enlace geralmente são utilizados em redes de acesso, enquanto os roteadores são utilizados principalmente no núcleo da rede. A sequência de enlaces de comunicação e comutadores de pacotes que um pacote percorre desde o sistema final remetente até o sistema final receptor é conhecida como **rota ou caminho** através da rede.

Sistemas finais acessam a Internet por meio de **Provedores de Serviços de Internet** (Internet Service Providers — ISPs), entre eles ISPs residenciais como empresas de TV a cabo ou empresas de telefonia; corporativos, de universidades e ISPs que fornecem acesso sem fio em aeroportos, hotéis, cafés e outros locais públicos. Cada ISP é uma rede de comutadores de pacotes e enlaces de comunicação. ISPs oferecem aos sistemas finais uma variedade de tipos de acesso à rede, incluindo acesso residencial de banda larga como modem a cabo ou DSL (linha digital de assinante), acesso por LAN de alta velocidade, acesso sem fio e acesso por modem discado de 56 kbits/s. ISPs também fornecem acesso a provedores de conteúdo, conectando sites diretamente à Internet. Esta se interessa pela conexão entre os sistemas finais, portanto os ISPs que fornecem acesso a esses sistemas também devem se interconectar. Esses ISPs de nível mais baixo são interconectados por meio de ISPs de nível mais alto, nacionais e internacionais, como Level 3 Communications, AT&T, Sprint e NTT. Um ISP de nível mais alto consiste em roteadores de alta velocidade interconectados com enlaces de fibra ótica de alta velocidade. Cada rede ISP, seja de nível mais alto ou mais baixo, é gerenciada de forma independente, executa o protocolo IP (ver adiante) e obedece a certas convenções de nomeação e endereço. Examinaremos ISPs e sua interconexão mais em detalhes na Seção 1.3.
Os sistemas finais, os comutadores de pacotes e outras peças da Internet executam **protocolos** que controlam o envio e o recebimento de informações. O **TCP** (**Transmission Control Protocol** — Protocolo de Controle de Transmissão) e o **IP** (**Internet Protocol** — Protocolo da Internet) são dois dos mais importantes da Internet. O protocolo IP especifica o formato dos pacotes que são enviados e recebidos entre roteadores e sistemas finais. Os principais protocolos da Internet são conhecidos como **TCP/IP**. Começaremos a examinar protocolos neste capítulo de introdução. Mas isso é só um começo — grande parte deste livro trata de protocolos de redes de computadores! 
Dada a importância de protocolos para a Internet, é adequado que todos concordem sobre o que cada um deles faz, de modo que as pessoas possam criar sistemas e produtos que operem entre si. É aqui que os padrões entram em ação. **Padrões da Internet** são desenvolvidos pela IETF (Internet Engineering Task Force — Força de Trabalho de Engenharia da Internet) [IETF, 2012]. Os documentos padronizados da IETF são denominados **RFCs**(**Request For Comments**— pedido de comentários). Os RFCs começaram como solicitações gerais de comentários (daí o nome) para resolver problemas de arquitetura que a precursora da Internet enfrentava [Allman,2011]. Os RFCs costumam ser bastante técnicos e detalhados. Definem protocolos como TCP, IP, HTTP (para a
Web) e SMTP (para e-mail). Hoje, existem mais de 6.000 RFCs. Outros órgãos também especificam padrões para componentes de rede, principalmente para enlaces. O IEEE 802 LAN/MAN Standards Committee [IEEE 802,2009], por exemplo, especifica os padrões Ethernet e Wi-Fi sem fio.


#### Aplicações Distribuídas

São aplicações que envolvem diversos sistemas fiais e trocam informações mutualmente, como por exemplo: navegação Web, redes sociais, mensagen instantanea, Voz sobre IP(VoIP), video em tempo real, login remoto e muitos mais. Deforma significativa, as aplicações da internet são executadas em sistemas finais - e não em comutadores de pacote no núcleo da rede.

### Protocolos
![[Pasted image 20241102154157.png]]

Um protocolo de rede é semelhante a um protocolo humano a diferença é que as entidades que trocam mensagens são hardware e software. Todas as atividades na internet que envolvem duas ou mais entidades remotas são governadas por um protocolo, como o controle de fluxo de bits no "cabo" entre duas placas de interface de rede, controle de congestionamento em sistemas finais controlam, a taxa com que os pacotes são transmitidos entre a origem e destino e etc.

****Um protocolo define o formato e a ordem das mensagens trocadas entre duas ou mais entidades comunicantes, bem como as ações realizadas na transmissão e/ou no recebimento de uma mensagem ou outro evento.***

## 1.2 A periferia da internet

- Sistemas finais podem ser divididos em:
	- Cliente 
		- Celulares, computadores desktop e etc
	- Servidor
		- servidores mesmo
		- em datacenters

Para esses sistemas finais se conectarem eles precisam de acesso a rede por meio de redes de acesso.
### 1.2.1 Redes de Acesso

#### Acesso Domestico: DSL, cabo, FTTH, Discado e satélite
##### DSL

Os dois tipos de acesso residencial banda larga predominantes são ***Linha digital de assinante(DSL)*** ou a Cabo. Normalmente uma residência obtém acesso DSL da mesma impresa que fornece telefonia. Quando a DSL é utilizada, uma operadora do cliente é tambem seu ***provedor de serviços de internet (ISP)***. 

![[Pasted image 20241102161022.png]]

Nesse caso os dados passam por um só fio, porém eles são codificados em frequências diferentes:

- Um canal de *downstream*
- Um de *upstream* 
- Um canal de telefone bidirecional comum.

A linha telefônica conduz simultaneamente os dados, sendo eles separados por um ***DSLAM (multiplexador digital de acesso à linha do assinante)*** em geral localizado na CT da operadora, o DSLAM faz o separamento de sinais e os manda para a internet.

No lado do consumidor para sinais que chegam até sua casa, um distribuídor separa os dados e os sinais telefônicos e conduz o sinal com os dados para o modem DSL.

Em razão de as taxas de transmissão e recebimento serem diferentes, o acesso é conhecido como **asimétrico** 
![[Pasted image 20241102164307.png]]

##### Cabo

Embora o DSL utilize a infraestrutura de telefone local da operadora, o **acesso à Internret a cabo** utiliza a infraestrutura de TV a cabo da operadora de televisão. 

Nesse cabo uma fibra ótica se divide em cabo coaxial que é utilizado para chegar ate as casas de maneira individual. Em razão de a fibra e o cabo coaxial fazerem parte desse sistema, a rede é denominada **híbrida fibra-coaxial (HFC)**

No lado do consumidor temos um modem a cabo que em geral é um aparelho externo que se conecta ao computador residencial pela porta Ethernet.

No lado do ISP, temos o **sistema de término do modem a cabo (CMTS)** que tem uma função parecida ao DSLAM (no sistema DSL) eles também coletam os dados de forma analógica e passam para digital e mandam para a internet.

![[Pasted image 20241102165237.png]]

A característica importante do acesso a cabo é o fato de ser um meio de transmissão compartilhado, então se muitos usuários estiverem usando o canal de downstream a taxa de transmissão diminui drasticamente.

##### Fiber to the home (FTTH)

Outra tecnologia que fornece velocidades ainda mais altas é a **fiber to the home(FTTH)**, ela oferece um caminho de fibra ótica do CT diretamente até a residência. Normalmente varios cabos maiores saem dos CTs  e vão se dividindo quando chegam perto das casas. 

Duas arquiteturas concorrentes de rede de distribuição ótica apresentam essa divisão: **redes óticas ativas (AONs) e redes óticas passivas (PONs)**. A AON é na essência a Ethernet comutada, entraremos em mais detalhes na PON

No PON cada residência possui um **terminal de rede ótica (ONT)**, que é conectado por uma fibra ótica dedicada a um distribuidor da região, essa fibra se conecta ao OLT na CT, que fornece conversão entre sinais ópticos e elétricos, se conecta à Internet por meio de um roteador da operadora.

Na residência, o usuário conecta ao ONT um roteador residencial pelo qual acessa a internet.

##### Outros tipos de acesso

Em locais onde DSL, cabo e FTTH não estão disponíveis (por exemplo, em algumas propriedades rurais), um enlace de satélite pode ser empregado para conexão em velocidades não maiores do que 1 Mbit/s; StarBand e HughesNet são dois desses provedores de acesso por satélite. O acesso discado por linhas telefônicas tradicionais é baseado no mesmo modelo do DSL — um modem doméstico se conecta por uma linha telefônica a um modem no ISP. Em comparação com DSL e outras redes de acesso de banda larga, o acesso discado é terrivelmente lento.

![[Pasted image 20241102172541.png]]


## 1.3 O núcleo da rede

### 1.3.1 Comutação de pacotes

Como vimos a rede reduz as nossas informações em **pacotes** de dados entre a origem e destino, cada um deles percorre enlaces de comunicação e **comutadores de pacotes (roteadores e comutadores da camada de enlace)**.

#### Transmissão armazena-e-reenvia
A maioria dos comutadores utiliza a **transmissão armazena e reenvia(store-and-foward)** nas entradas dos enlaces, isso significa que na passagem de pacotes de comutadores para comutadores, o aparelho vai esperar a chegada de todos os bits do pacote, reconstruir ele, processar ele, veremos que existe uma fila de pacotes que armazena eles, essa fila tem tamanho finito isso pode sim gerar atrasos.

#### Atrasos de fila e perda de pacote

a cada comutador de pacotes estão ligados vários enlaces. Para cada um destes, o comutador de pacotes tem um **buffer de saída (tambem chamado de saída)** que armazena pacotes prestes a serem enviados. Desse modo, além dos atrasos de armazenagem o e reenvio, os pacotes sofrem **atraso de fila** no buffer de saída. Esses atrasos são variáveis e dependem do grau de congestionamento da rede. Como o espaço do buffer é finito, um pacote pode encontrar a fila lotada. Nesse caso, ocorrer'uma **perda de pacote**, o pacote simplesmente será jogado fora.

![[Pasted image 20241102183502.png]]

#### Tabelas de repasse e protocolos de roteamento

Um pacote que chega a um de seus enlaces e é encaminhado a outros enlace até chegar ao destino, isso é feito com ajuda de **tabelas de encaminhamento e protocolos de roteamento**.

Quando um pacote chega a um roteador, este examina uma parte do endereço de destino e conduz a um roteador adjacente. Mais especificamente, cada roteador possui uma **tabela de encaminhamento** que mapeia os endereços de destino (ou partes dele) para enlaces de saída desse roteador.

A Internet possui uma série de **protocolos de roteamento** especiais que são utilizados para configurar as tabelas de encaminhamento nos roteadores.

### 1.3.2 Comutação de Circuitos

Há duas abordagens para a locomoção de dados através de uma rede de enlaces: **comutação de circuitos e comutação de pacotes** 

Na comutação de circuitos, quando uma conexão se estabelece, a rede estabelece uma conexão fim a fim, isso quer dizer que um caminho foi alocado por um dispositivo, e assim é até o fim da conexão, um ponto positivo é que uma certa banda larga é garantida até o fim da conexão, porém pode acontecer de todas as bandas estarem sendo utilizadas naquele momento, isso gera linhas ocupadas, como oque acontece com linhas telefonicas mesmo.

#### Multiplexação de redes de comutação de circuitos

Um circuito é implementado em um enlace por **multiplexação por divisão de frequência(*frequency-division multiplexing - FDM*)** ou por **multiplexação por divisão de tempo(*time-division-multiplexing - TDM*)**.

Com FDM o enlace reserva uma banda de frequência para cada conexão durante o período da ligação. Um exemplo seria estações de rádio FM que compartilham o spectro de frequencia entre 88Mhz e 108Mhz.


Em um enlace TDM, o tempo é dividido em quadros de duração fixa, e cada quadro é dividido em um número fixo de compartimentos(*slots*). A rede dedica à conexão um comprimento de tampo em cada quadro.
![[Pasted image 20241102195116.png]]

Os defensores da comutação de pacotes sempre argumentaram que comutação de circuitos é desperdício, já que os circuitos dedicados ficam ociosos durante **períodos de silêncio**. Além disso diz-se que reservar larguras de banda fim a fim é complicado e exige software complexo de sinalização para coordenar a operação dos comutadores ao longo do caminho.

### Concluindo

A comutação de circuitos pode trazes benefícios como a garantia de banda, que pode ser adequado para alguns tipos de aplicações como videoconferências, que pode garantir boa qualidade de transmissão e etc, porém o custo seria grande além de problemas de ociosidade
A comutação por pacotes é simples e ágil, porém gera problemas de congestionamento e atrasos. 
No final a comutação por pacotes é 99,9% usada e a rede é lotada de mecanismos para contornar isso.

### 1.3.3 Uma rede de redes

![[Pasted image 20241102202100.png]]

Vemos na imagem que não existem níveis na rede, os ISPs de acesso, que são os que o consumidor comum acessa são só o primeiro nivel de ISPs.

#### ISP nivel 1

São os backbones da internet, são as empresas que mantem conexão internacional entre a rede, empresas como a Embratel.

#### ISP regional

São as empresas que mantem conexão regional, OI, Claro, TIM, VIVO

#### ISP de acesso

Como ja dito anteriormente, são as ISPs que conectam o usuário final á grande rede.

No final, todas essas ISPs de diferentes niveis tem que se conectar e para isso, empresas compram serviços de outras, ou regiões de serviço, links e assim vai

#### IPX (internet exchange point)

São pontos, que podem ser criados por empresas terceiras, de acesso de multiplas ISPs, onde por exemplo duas ISPs podem conectar suas fibras para cobrir uma nova área.

#### POPs

São um grupo ou mais roteadores de rede do provedor onde os ISPs clientes podem alugar esses mesmos para um acesso direto a rede, a Petrobras tem desses com a Embratel, e o google tem vários espalhados pelo mundo ligados diretamente aos backbones da internet


## 1.4 Atraso, perda e vazão em redes de comutação de pacotes






























