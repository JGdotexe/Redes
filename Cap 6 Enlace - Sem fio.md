

# Enlaces e redes sem fio

Elementos em uma rede sem fio:
- Hospedeiro sem fio (celular pc os karlh a 4)
- Enlaces sem fio(mobilidade, alncance, handoff...)
- Estação base(pontos de acesso 802.11, torres de acesso)
- infra de rede


Diferenças entre enlace com fio e sem fio
- Redução da força do final (refração, obstaculos no caminho)
- Iferencia de outras fontes (muitos pontos de acesso em que as frequencias se fodem)
- Propagação multivias (possivel reflexão ou refração do sinal)

Problemas Adcionais
- Atenuação de sinal (classico)
- Terminal oculto 
	- numa rede sem fio nem todos os clientes se enxergam, então tecnicas como o CRC classico que detecta colisões por pulsos no fio (verificando se o vizinho está mandando algo) ,não funcionam tão bem, por exemplo, uma pessoa pode estar conectada a uma rede em um lugar e outra pode se conectar em outro extremo do local.
	![[Pasted image 20250504223449.png]]

### CDMA
É uma técnica que possibilita torres mandarem sinais em que as frequências mesmo com colisão (se misturando) conseguem ser separados e lidos normalmente
- Legado

## WIFI

Canais e associação:
- Cada estação precisa se associar a um ponto de acesso (obvio)
- Ao instalar um AP o Admin pode setar um SSID (identificador de conjunto de serviços)
- E também é setado um numero de canal de 1 a 11 pra frequência operar
- Em geral o hospedeiro escolhe o AP cuja intensidade de sinal é mais alta
- Varredura passiva:
	- O AP manda sinais na rede falando que ele ta lá
- Varredura Ativa
	- O hospedeiro manda mensagens na rede para descobrir qual o ponto de acesso mais proximo

### MAC 802.11

- Com inspiração do sucesso da Ethernet e o protocolo de acesso aleatório, o wifi teve algo parecido
- CSMA é o protocolo de acesso aleatório com **previsão de colisão** ou **CSMA/CA (colision avoidense)
- Em vez de detecção **de colisão**, o wifi usa **prevenção de colisão** 
- Usa um esquema de **reconhecimento/retransmissão (ARQ)** de camada de enlace


### Uso Simples:
![[Pasted image 20250504223021.png]]
- primeiramente existe alguns delay's pra prevenção de colisões os DIFS e SIFS
- A origem manda o pacote, se a chegada do datagrama for sucessiva, o destino manda um ack
	- DUVIDA: Quando o ack não volta, existe um timer, é algo parecido com o TCP no sentido de fragmentação de pacotes?
### Rede lotada

em uma rede mais cheia apenas um ack não é o suficiente pra prevenção de colisões então pra isso tem o RTS e CTS que são **quadros de controle**

- RTS (request-to-send)
- CTS (clear-to-send)

Funciona assim, quando um hospedeiro quer mandar um pacote, ele manda um RTS, o AP manda um broadcast de CTS para os outros hospedeiros ficarem em stand-by, o hospedeiro manda o pacote e depois recebe o ack do AP (um broadcast, agr todo mundo sabe que acabou aquela comunicação)(dessa vez sem colisões), e assim continuamente
- usado mais pra redes com muitos hospedeiros
![[Pasted image 20250504224330.png]]


### O quadro IEEE 802.11

![[Pasted image 20250504224628.png]]

### Mobilidade na mesma sub-rede
![[Pasted image 20250504225012.png]]

Pra um hospedeiro se mover pela rede, ele precisa primeiro mover de AP e depois ele mesmo manda uma frame pro comutador (uma requisição básica tipo qual é o próprio IP), que percebe que ele trocou de sub-rede e passa o MAC do hospedeiro pra sub-rede correta.

DUVIDA: entendi a parte de acesso celular a internet