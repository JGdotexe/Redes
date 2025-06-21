
# Introdução

- Princípios básicos da camada de enlace:
	- detecção de erros
	- compartilhamento de canal de difusão (fio ou ar)
	- endereçamento da camada de enlace
	- transferência confiável de dados (em alguns casos mais importantes que outros) e controle de fluxo

- Datagrama é transmitido por diferentes protocolos de enlace em diferentes enlaces.
	ex: Celular (4g) no primeiro enlace, Ethernet em enlaces intermediários e 802.11 (wi-fi) no ultimo enlace
- Cada protocolo de enlace provê diferentes serviços
	 ex: pode ou não prover transporte confiável de dados (pro wifi pode ser importante, mas pra fibra óptica nem tanto)

- Uma boa analogia seria uma vigem longa onde o passageiro usa diferentes tipos de transportes (carro trem, avião etc).

## Destrinchando principios

- Enquadramento de dados
	- encapsula datagrama num quadro adicionando cabeçalho e cauda
	- 'endereços físicos (MAC)' são usado nos cabeçalhos dos quadros para identificar origem e destinos de quadro em enlaces multiponto
		- diferente do IP
- Acesso ao enlace
	- implementa acesso ao canal de meio for compartilhado
- Entrega confiável (entre nós adjacentes):
	- já foi visto anteriormente
	- raramente usada em canais com baixas taxas de erro (fibra óptica, alguns tipos de pares trançados)
	- Canais sem fio: altas taxas de erros
- Detecção de erros:
	- Erros são causados por atenução  do sinal e por ruído
	- receptor detecta presença de erros
	- receptor sinaliza ao remetente para retransmissão, ou simplesmente descarta o quadro em erro
	- **Correção**: Mecanismo que permite que o receptor localize e ***corrija*** os erros sem precisar de retransmissão
- Outras:
	- Controle de fluxo:
		- Compatibilizar taxas de reprodução e consumo de quadros entre remetentes e receptores
	- Half-duplex e full duplex:
		- com half duplex, os nós de cada podem transmitir mas não simultaneamente

## Técnicas de detecção e correção de erros

### Verificação de reduncancia cíclica (CRC)

- Uma técnica de detecção de erros muito usadas nas redes de computadores de hoje é baseada em códigos de verificação de redundância cíclica (CRC).
- Códigos de CRC também são conhecidos como códigos polinomiais.

![[Pasted image 20250418211716.png]]

![[Pasted image 20250418211909.png]]

## Enlaces e protocolos de acesso multiplo

- Enlace ponto a ponto: consiste em um único remetente em uma extremidade do enlace e um único receptor na outra.
- enlace de difusão: pode ter vários nós remetentes e receptores, todos conectados ao mesmo canal de transmissão, único e compatilhado
- protocolos de acesso multiplo: através dos quais os nós regulam sua transmissão pelos canais de difusão compartilhados
### Protocolos de divisão de canal
#### TDM e FDM - lá do inicio de redes

- O TDM divide o tempo em quadros temporais, os quais depois divide em N compartimentos de tempo
- O FDM divide o canal de R bits em frequências diferentes e reserva cada frequência a um dos N nós criando, desse modo, N canais menores de R/N bits a partir de um único canal maior de  R bits
- Protocolo de acesso múltiplo por divisão de código (CDMA) atribui um código diferente a cada nó
	- se os códigos forem escolhidos com cuidado, as redes CDMA terão a maravilhosa propriedade de permitir que nós diferentes transmitem simultaneamente.

#### Protocolos de acesso aleatório

##### Slotted ALOHA
- hípoteses:
	- todos os quadros têm o mesmo tamanho
	- tempo é dividido em slots de tamanho igual, tempo para transmitir 1 quadro
	- nós começam a transmitir quadros apenas no início dos slots
	- nós são sincronizados
	- se 2 ou mais nós transmitirem num slot, todos os nós detectam a colisão
- operação:
	- quando o nó obtém um novo quadro, ele transmite no próximo slot
	- sem colisões, nó pode enviar novo quadro no proximo slot
	- caso haja collisão, nó retransmite o quadro em cada slot subsequente com probabilidade p ate obnter sucesso
- vantagens:
	- único nó ativo pode transmitir continuamente na taxa máxima do canal
	- Altamente descentralizado: apenas slots nos nós precisam estar sincronizados
	- simples
- desvantagens: 
	- colisões, slots desperdiçados
	- slots ociosos
	- nós podem ser capazes de detectar colisões num tempo inferior ao da transmissão do pacote

NOTA: pelo oque eu entedi; o canal é dividido em slots de tempo, igual o TDM, e nele apenas um host pode mandar um datagrama, se ele mandar, e ele perceber que houve colisão, ele precisa mandar dnv, ent ele joga uma moeda, e ve se ele vai mandar no proximo slot ou no 3° proximo ou assim por diante (depende de como é configurado), então acontece mt de ter colisões, e esses hosts ficarem jogando moeda e tendo colisões dnv, até alguma hora só uma pessoa mandar e dar certo.

- Eficiencia: 
	- A eficiencia é um coco, no melhor dos casos chega a 37%

##### ALOHA Puro

- Os caras acharam que sem a delimitação por slot seria melhor
	- não é
		- se antes o os pacotes so podiam colidir em 1 slot de tempo certinho, agora sem slots eles podem colidir a qualquer momento, bem noggers
- Eficiencia: pior que a do ALOHA comum

### CSMA (acesso multiplo com detecção de portadora)

- Existe algumas regras em conversas educadas entre seres humanos, isso vai se aplicar pra bits no fio tbm

- *Ouça antes de falar*. Se uma pessoa falar, espera que ela termine. Em redes, isso se chama detecção de portadora, --- ***Um nó ouve o canal antes de transmitir***
- *Se alguem falar ao mesmo tempo que vc, pare de falar*. Em redes isso se chama detecção de colisão ---  ***Um nó que está transmitindo ouve o canal enquanto transmite***

Essas regras são incorporadas na família de protocolos de **acesso multiplo com detrcção de portadora** (CSMA) e CSMA **com detecção de colisão** (CSMA/CD) 

![[Pasted image 20250501213344.png]]

- **Colisões ainda podem acontecer** pois quando um nó lança o pacote, um outro nó mais longe fisicamente pode demorar pra ouvir o sinal (por uma questão fisica até) e mandar o pacote dele, isso pode causar colisão, é oque acontece no grafico 
- Essa colisão desperdiça todo o pacote
- essa distancia fisica e atraso tem papel na determinação da probabilidade de colisão

***CSMA/CD***
- As colisões são detectadas e as transmissões são abortadas reduzindo o desperdício do canal
- Detecção de colisões:
	- Fácil em LANs cabeadas: mede a potencia do sinal, compara o sinal recebido dom o transmitido
	- Difícil em LANs sem fio: o receptor é desligado durante a transmissão, alem disso nem todos os clientes se enchergam na rede, o ponto de acesso enxerga todos, mas os clientes N


![[Pasted image 20250501214243.png]]

Assim fica o grafico no caso de CSMA/CD, percebesse que mesmo detectando a colisão, o primeiro nó continua propagando por mais um tempinho pra poder chegar pros outros clientes da rede

NOTA: essas detecções são percebidas a nivel de bits, então um canal manda um bit no canal pra ver se teve colisão, se não ouve ele manda o resto, se colidir, ele continua um chorinho de tempo pra poder chegar o sinal nos outros nós. e ainda continua aquela parada de jogar uma moeda pra cima e esperar um certo tempo pra retransmitir os bits

## Protocolos de revezamento

- **Pooling**
	- nó mestre "convida" os nós escravos a transmitir em revezamento
	- Preucupações:
		- overhead com consultas
		- Latencia
		- Ponto unico de falha(mestre)
NOTA: basicamente o nó mestre fica indo de nó em nó perguntando se vai transmitir ou n, ja deu pra ver que n é tão eficiente, e se o mestre falhar fudeu

- **Passagem de permissão (token)**
	- controla a permissão passada de um nó para o próximo de forma sequencial
	- mensagem de passagem da permissão
	- Preucupações:
		- overhead com a passagem
		- latencia
		- ponto unico de falha(permissão)

NOTA: sla

### DOCSIS - protocolo da camada de enlace para acesso à internet

- **Data-over-cable Service Interface Specifications**:
	- especifica a arquitetura de rede de dados a cabo e seus protocolos.
	- utiliza FDM, TDM, pooling e acesso aleatório
	![[Pasted image 20250502201204.png]]
	- FDM: o terminal de distribuição separa grupo de casas por frequência, em cabos coaxiais por exemplo
	- TDM: as casas mandam uma solicitação para o CMTS pra mandar pacotes, e o CMTS responde mandando os slots de tempo que cada cliente vai poder usar
	- Pooling: nesse caso o "nó mestre" é o CMTS.
	- Acesso Aleatório: na rota dos clientes até o CMTS pode haver colisao, em caso de colisão, aplica-se acesso aleatório pra detecção de colisões

## Redes Locais Comutadas LAN

Em redes locais, cada dispositivo tem 1 endereço de IP e endereço MAC por interface(placa de rede) o endereço MAC é o identificador utilizado no nível da camada de enlace
- Switches não tem tem pois são apenas comutadores
- cada interface de rede mantem uma tabela de endereços MAC da rede local (apenas temporáriamente, +- uns 20 min) essa é a **tabela ARP**

Mas e se eu quero acessar um outro mac que está em outra rede LAN?
- Mando o pacote com o roteador como destino e o roteador manda

Duvida: o cliente envolve em dois pacotes? um com endereço do roteador e mais 1 com endereço do destino final?


### Ethernet

- Tomou conta do mercado de LANs
- Alta velocidade e barata

Estrutura do Quadro Ethernet
![[Pasted image 20250504150021.png]]
- Preambulo ajuda na transmissão em diferentes frequencias (dependendo das interfaces de rede da LAN)


## Comutadores da camada de enlace

- Função básica é receber quadros e repassá-los
- O comutador só chega até a camada de enlace (**não tem camada de rede**) então é transparente aos roteadores na sub-rede, ele só associa um MAC a uma porta e vai repassando
- Filtragem: capacidade determinar se um quadro deve ser repassado ou descartado
- Repasse: capacidade de determinar as interfaces para as quais um quadro deve ser dirigido

- Filtragem e repasse são feitos com uma **tabela de comutação**.
		![[Pasted image 20250504150630.png]]
- Comutadores são autodidatas (plug&play)
- Vantagens no uso de comutadores
	- Eliminação de colisões (cada interface tem um fio proprio ate o comutador)
	- Enlaces heterogêneos (conexões de multiplas taxas? 100mb 1gb) DUVIDA
	- Gerenciamento

## VLANs

- Adcionam mais uma camada de segurança
- "separa as redes locais" -> segurança -> organização
- Gerencia de usuários ou até setores inteiros
 ![[Pasted image 20250504172303.png]]

- Podem ser configuradas em switches, no caso pode-se mapear um conjunto de portas para uma vlan enquanto outro conjunto servirá para outra vlan por exemplo
![[Pasted image 20250504172436.png]]

O quadro VLAN Ethernet vai diferenciar um pouco do de ethernet comum
![[Pasted image 20250504172602.png]]
- identificador de protocolo de rotulo: só indica o switch que é uma vlan
- controle de informação de rotulo: ID da vlan


DUVIIDA: MPLS


