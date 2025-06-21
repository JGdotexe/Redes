

# Camada de Transporte


- Ideia básica 
	- lado transmissor: quebra a mensagem das aplicações em vários segmentos e repassa para a camada de rede
	- lado receptor: remonta as mensagem a partir dos segmentos e repassa para a camada de aplicação

#### Protocolo de transporte X Protocolo de rede

Um protocolo de transporte fornece comunicação logica entre *processos* que rodam em hospedeiros um protocolo de rede fornece comunicação logica entre *hospedeiros*.

Os serviços que um protocolo de transporte oferecem são limitados pelo modelo de serviço dos protocolos da camada de rede. *basicamente os serviços fornecidos pela camada de transporte são limitados pelos serviços fornecidos na rede, já que um é implementado acima do outro*

#### Papel dos protocolos de transporte 

A responsabilidade fundamental do UDP e TCP é ampliar o serviço de entrega IP entre dois sistemas finais para um serviço de entrega entre dois processos que rodam nos sistemas finais. Essa ampliação entrega ao hospedeiro é chamada de **multiplexação e demultiplexação de camada de transporte**, além disso esses protocolos adicionam detecção de erros nos cabeçalhos dos segmentos. *segmentos  = pacotes* 

**A camada de rede faz o papel de entregar a carta ate o prédio a camada de transporte levaria a carta até o apartamento**

#### Features TCP x UDP

TCP:
- Entrega confiável, ordenada
- controle de congestionamento
- controle de fluxo
- estabelecimento de conexão

UDP:
- entrega não confiável
- extensão sem *frescuras* do IP

Serviços não disponíveis - Rede não provê
- Garantia de atraso
- Garantia de largura de banda
- intervalo entre pacotes


## Multiplexação e Demultiplexação

![[Captura de tela de 2024-11-28 21-10-36.png]]

Multiplexação: Separar os dados e criar os segmentos
Demultiplexação: Reunir os segmentos vindos da rede remontar e entregar a devida porta/processo

Um segmento básico da camada de transporte adiciona numero de porta de origem e numero de porta destino (obvio que isso encima do datagrama que já traz IP de origem e destino)

O UDP apenas checa no. da porta de destino e encaminha pro socket, detalhe, segmentos com porta de origem diferente podem ser encaminhados para o mesmo socket

#### Básico de TCP

- Socket identificado pela 4-dupla
	- IP de origem
	- porta de origem
	- IP destino
	- porta destino
	**Esses 4 valores são unicos em toda a internet**

- Um servidor pode dar suporte a muitos sockets TCP simultâneos
	- Cada socket é identificado pela sua propria 4-dupla

- Servidores Web têm sockets diferentes para cada conexão cilente
	- HTTP não persistente terá sockets diferentes para cada pedido


## Transporte não orientado a conexão: UDP

- Faz pouco além da multiplexação e demultiplexação
- No desenvolvimento de uma aplicação UDP o dev está falando quase diretamente com IP
- Elimina o estabelecimento de conexão (oque pode causar retardo)
- simples: não se mantém "estado" de conexão nem no remetente nem no receptor
- Cabeçalho pequeno
- Sem controle de congestionamento: UDP pode transmitir o mais rápido possíve (depende da rede também)

##### Exemplos do que usa UDP: 
- telefonia por internet pode usar UDP
- protocolo de roteamento
- tradução de nome (dns)
##### cabeçalho típico:
![[Pasted image 20241201145541.png]]

- Some de verificação(checksum)
	 - Os bits do segmento são somados e esse resultado é armazenado
	 - No sistema de destino essa soma é feita novamente e o resultado é comparado para saber se teve alguma perda ou mudança de dado

## Transferência confiável de dados
**Base do TCP**

- Usaremos maquina de estado finito
- A ideia é a partir de uma rede sem falhas ir incrementando as falhas e adaptar o nossos mecanismos até chegar a algo parecido com a realidade
- Teremos uma função rdt(Reliable Data Transfer) que a cada versão será incrementada novas mecânicas para uma transferência de fato confiável de dados

### Transferência confiável de dados sobre um canal perfeitamente confiável

#### RDT1.0

- Um protocolo para um canal completamente confiável
	- Sem erros de bits
	- Sem perda de pacotes
- FSMs separadas para remetente e receptor
	- remetente envia dados peplo canal subjacente
	- receptor recebe dados do canal subjacente

- **Remetente**
	- Espera os dados da camada de cima
	- Monta o pacote
	- manda o pacote
- **Receptor**
	- Espera os dados da camada de baixo
	- Extrai os dados
	- Manda para a camada de cima

### Transferência confiável sobre um canal com erro de bits

#### RDT2.0

- Canal subjacente pode inverter bits no pacote
	- Checksum pode detectar erros
- Como recuperar dos erros?
	- reconhecimentos (**ACKs**): receptor avisa ao remetente que o pacote chegou bem 
	- reconhecimentos negativos (**NACKS**): receptor avisa ao remetente que o pacote tinha erros
	- remetente retransmite o pacote ao receber um NAK
	- cenários humanos usando ACK e NACK: Oi tudo bem? (ACK) sim e você? (NACK) nem tanto.  ou o famoso Alou? Ham?
- Diff RDT2.0 x RDT1.0
	- detecção de erros
	- realimentação pelo receptor: msgs de controle (ACK e NACK)

- **Remetente**:
	- espera dados da camada de cima
	- monta o pacote e manda
	- Fica esperando ACK ou NACK do receptor
	- Recebeu NACK
		- Manda o pacote de novo
	- Recebeu ACK
		- Repete o processo
- **Receptor**: 
	- espera dados do canal
	- Pacote corrompido:
		- monta pacote NACK e manda
	- Pacote de boa: 
		- extrai os dados e manda para camada de cima
		- monta pacote ACK e manda

**Qual seria um possivel erro?**: os pacotes ack e nack também podem se corromper. Por exemplo em caso de um ack virar um nack o pacote seria remandado do lado do remetente isso causaria duplicata no lado destinatario, no caso de um nack virar um ack o remetente pode mandar o pacote seguinte e ficar um *buraco* na montagem de pacotes.

**Resolvendo o erro**: 
- remetente inclui número de sequencia para cada pacote
- remetente retransmite pacote atual se ack/nak recebido com erro
- receptor descarta pacote duplicado

#### RDT2.1

- Usando n°s de sequencia 0 e 1 somente

- **Remetente**
	- espera os dados do pacote 0(n° de sequencia)
	- monta o pacote e manda
	- Espera ack ou nack de 0 
		- se receber um pacote corrompido ou nack
			- re-manda o pacote
		- se receber um ack 
			- continua o processo para o pacote 1
- **Destinatário**
	- espera o pacote 0 da camada de baixo
		- pacote recebido está corrompido
			- manda nack
		- pacote recebido está de boa mas n° de sequencia não é o esperado
			- mada ack de 0
		- pacote recebido esá de boa e o número de sequencia é o esperado
			- manda ack
			- fica esperando o pacote 1 repete o processo

Discussão
- Remetente
	- n° de seq no pacote
	- bastam dois n°s de sequencia (0,1)
	- deve checar se ack e nack recebido tem erro (checksum)
	- duplicou o n° de estados
	- estado deve lembrar se pacote "corrente" tem n° de sequencia 0 ou 1 

- Receptor
	- deve checar se pacote recebido é duplicado
	- estado indica se n° de seq esperado é 0 ou 1
	- **Nota**: o receptor não tem como saber se ultimo ack/nack foi recebido bem pelo remetente

#### RDT2.2

- Um protocolo sem NAKs
- mesma funcionalidade que rdt2.1, só com ACKs
- ao invés de nack, o recebtor envia ack do ultimo pacote recebido nos conformes
- receptor deve incluir explicitamente n° de seq do pacote reconhecido
- ACK duplicado no remetente resulta na mesma ação que o NACK: retransmite pacote atual

 - **Remetente** 
	 - espera dados do pacote 0 da camada de cima
	- monta e manda o pacote
	- fica esperando ack de 0
		- se o pacote recebido for corrompido ou for um ack de 1
			- manda o pacote 0 dnv
		- se pacote não estiver corrompido e for ack de 0
	- espera dados do pacote 1 e repete o processo
- **Destinatário**
	- Espera os dados do pacote 0 da camada de baixo
		- pacote recebido é corrompido ou é 1 
			- manda ack de 1 (1 anterior a 0 que já foi recebido)
		- pacote recebido não está corrompido e é o 0
			- manda ack de 0
		- espera os dados do pacote 1 da camada de baixo
		- repete o processo

### Transferência confiável de dados sobre um canal com perda e com erro de bits

#### RDT3.0

- Remetente aguarda um tempo "razoável" pelo ACK
- retransmite se nenhum ack for recebido neste intervalo
- se pacote (ou ACK) apenas estiver atrasado (e não perdido)
	- retransmissão será duplicada, mas uso de n° de seq do ja cuida disso
	- receptor deve especificar n° de seq do pacote sendo reconhecido
- requer temporizador

- **Remetente** 
	- Espera os dados 0 da camada de cima
	- manda o pacote **start no timer de 0**
	- espera o ack 0
		- se receber um pacote corrompido ou ack de 1 remanda o pacote
		- se o **tempo acabar** remanda o pacote **restart no timer**
		- recebe ack de 0 - continua
	- espera ack 1 processo se repete
- **Destinatário**
	- EXERCÍCIO


### Transferência confiável de dados com pararelismo

- O protocolo RDT3.0 é um protocolo pare e espere, oque afeta o desenpenho da rede
![[Pasted image 20241201204216.png]]

- Adcionando pararelismo
	- remetente admite multiplos pacotes "em trânsito" ainda não reconhecidos
	- faixa de n° de seq deve aumentar
	- adição de buffers no remetente e/ou no receptor

![[Pasted image 20241201204531.png]]

- Tem-se duas formas de protocolos com pararelismo
	- Go-back-n
	- retransmissão seletiva

#### GO BACK N
- no GBN o remetente é autorizado a transmitir múltiplos pacotes sem esperar por um reconhecimento.
- porém fica limitado a ter não mais do que algum número maximo permitido , **N**, de pacotes não reconhecidos na "tubulação"
![[Pasted image 20241201212533.png]]
Descrição
- Remetente
	- ACK(n): reconhece todos os pacotes anteriores a n "ACK acumulativo"
	- pode receber acks duplicados
	- temporizador para a base da janela de transmissão
	- timeout(n) retransmite pacote n e todos os pacotes com n° de seq maiores na janela (isso basicamante diz que se eu perdi o pacote 2 ou o ack de 2 não chegar antes do timeout eu "perdi tudo que eu fiz" porque eu tenho remandar 2 e todos os pacotes a frente dele, mesmo que eles tenham sido mandados) na verdade isso é uma duvida
- Destinatário
	- usa apenas ACK: sempre envia ack para pacote recebido com maior n° de seq em ordem
		- pode gerar acks duplicados
		- so precisa se lembrar do expectedseqnum
	- pacote fra de ordem:
		- descarta -> receptor não usa buffer
		- manda ack de pacote com maior n° de seq em ordem


- **Remetente**: 
	- base = 1
	- nextseqnum = 1
	- para mandar um pacote
		- se nextseqnum estiver na janela
			- manda o pacote
			- se base == nextseqnum começa o timer
		- incrementa nextseqnum
	- se der timeout manda todos os pacotes até nextseqnum-1
	- se receber um pacote e for um ack
		- vê de qual pacote é esse ack
		- base é esse ack + 1
		- se base ==  nextseqnum (foram ackados todos os  anteriores a nextseqnum)
		- para o timer
		- se não, é porque é um ack de um pacote do meio da janela, então começa o timer pra ele pra não ser tão injusto 
- **Destinatário**
	- recebeu pacote não corrompido e tem o n°de sequencia esperado?
		- estrai e manda os dados para a próxima camada
		- incrementa o n° do expectedseqnumber
	- se não ack do ultimo pacote em ordem
	DUVIDA