
# Introdução 

O papel da camada de rede é transportar pacotes de um hospedeiro remetente a um hospedeiro destinatário.
## Função do Roteador
- Repasse
	Quando um pacote chega ao enlace de entrada de um roteador, este deve conduzi-lo até o enlace de saída apropriado.
- Roteamento
	A camada de rede deve determinar a rota ou o caminho tomado pelos pacotes ao fluírem de um remetente a um destinatário.
- Minha nota: é como se repasse fosse numa escala menor (de enlace para enlace), e roteamento é a visão geral de determinar o melhor caminho até uma saída por exemplo

## Modelo de serviço de rede

- O modelo de serviço de rede define as características do transporte de dados fim a fim entre uma borda da rede e a outra.
- Serviços que poderiam ser oferecidos:
	- Entrega garantida
	- Entrega garantida com atraso limitado
	- Entrega de pacotes em ordem
	- Largura de banda mínima na garantida
	- Jitter máximo garantido - intervalo entre pacotes vai respeitar um padrão de taxa do emissor dos pacotes
	- Serviços de segurança
![[Pasted image 20241213172958.png]]

## Redes de circuitos virtuais

- Um circuito virtual (CV) consiste em:
	1. Um caminho (isto é, uma série de enlaces e roteadores) entre hospedeiros de origem e de destino.
	2. Número de CVs, um número para cada enlace ao longo do caminho
	3. Registros na tabela de repasse em cada roteador ao longo do caminho![[Pasted image 20241213173856.png]]
	**ROTEADORES MANTÊM INFORMAÇÃO SOBRE O ESTADO DA CONEXÃO**   
- Passa por três fases que podem ser identificadas em um circuito virtual
	1. Estabelecimento de CV
	2. Transferência de dados
	3. Encerramento do CV
- Por ser organizado dessa maneira, esse tipo de serviço requer um certo nível de inteligência dentro da rede, nos enlaces.
- Por exemplo esses estados sendo armazenados dentro dos enlaces, sempre que se estabelece as conexões os enlaces tem de ser alocados para a tal conexão, e quando a conexão é encerrada tem de a ver uma desalocação dos circuitos, pra isso é necessário um mecanismo de garbage collection, oque pode gerar complexidade na rede e menor velocidade assim.

## Rede de datagramas

Em uma rede de datagramas, todas vez que um sistema final quer enviar um pacote, ele marca o pacote com o endereço de sistema final de destino e então o envia para a dentro da rede.

- Ao ser transmitido da origem ao destino, um pacote passa por uma série de roteadores.
- Cada um desses roteadores usa o endereço de destino do pacote para repassa-lo
	- O roteador pega o pacote, examina o endereço de destino usa a tabela de repasse gerada a partir do roteamento, escolhe o melhor caminho 
- Então o roteador retransmite o pacote para aquele interface de enlace de saída
- A tabela de repasse de um roteador em uma rede de CVs é modificada sempre que é estabelecida uma nova conexão através do roteador ou sempre que uma conexão é existente é desativada.

## Rede de datagrama ou CVs: por que?

- internet:
	- troca de dados entre computadores
		  - serviço "elástico" sem reqs. temporais estritos
	- sistemas terminais "inteligentes" (computadores)
		- podem se adaptar, exercer controle, recuperar de erros
		- núcleo da rede simples, complexidade na "borda"
	- muitos tipos de enlaces 
		- características diferentes
		- serviço uniforme difícil
- ATM:
	- evoluiu da telefonia 
	- conversação humana:
		- temporização estrita, requisitos de confiabilidade
		- requer serviço garantido
	- sistemas terminais "burros"
		- telefones
		- complexidade dentro da rede

# Sobre os Roteadores

## Arquitetura de um Roteador
![[Pasted image 20241213210755.png]]
Essa seria a estrutura genérica do roteador.

## Processamento de Entrada

![[Pasted image 20241213211017.png]]
- O objetivo é completar o processamento da porta de entrada na "velocidade da linha", ou seja, que não haja fila
- Porém acontece dos datagramas chegarem mais rapido que a taxa de reenvio para matriz de comutação 

## Elemento de comutação

É por meio deste que os pacotes são comutados para uma porta de saída

- A comutação pode ser realizada de inúmeras maneiras:
	- Comutação por memória
	- Comutação por  um barramento
	- Comutação por uma rede de interconexão

![[Pasted image 20241213211722.png]]


- Por memória
	- Como se fosse um computador com várias placas de rede
	- Feito sequencialmente
- Por barramento
	- Tem controle de alocação de barramento, oque pode atrasar a lirou bit
- Por interconexão
	- o mais rápido
	-  nem armazena , le o cabeçalho ja mandando pra porta de saída
	- caro abeça

## Processamento de saída

![[Pasted image 20241213212017.png]]
- buffers: são necessários quando o datagrama chega da matriz mais ŕapido do que a taxa de transmissão do fio
- Disciplina de escalonamento: qual politica que a fila vai ter para escolher que sai primeiro, pode ser FIFO ou alguns outros tipos

## Formação de Fila

Filas podem se formar tanto na entrada como na saída. O local e a extensão vai depender de:
- Carga de tráfego
- velocidade relativa do elemento de comutação
- taxa da linha 
![[Pasted image 20241213213215.png]]

- Pode acontecer de vários pacotes serem encaminhados para a mesma porta de saída, isso pode gerar atrasos
- o elemento de comutação pode ter uma taxa de processamento muito mais alta que o buffer de saída, isso pode também gerar perda de pacotes


## Introdução a IP e camada de rede
![[Pasted image 20250103162639.png]]

## Formato de um Datagrama IPV4

![[Pasted image 20250103172036.png]]

- Versão: IPV4 por exemplo
- Cumprimento de cabeçalho: ngm usa
- Tipo de serviço: ngm usa, distinguir fluxo por serviço, como VOIP ou download de Pagina Web
- Cumprimento do Datagrama: quantidade de dados ali embaixo
- identificador: ID?
- Flags: Tem a ver com Fragmentação
- Deslocamento de Fragmentação: Um pacote pode ser grande demais para um enlace isso pode causar fragmentação do pacote
- Tempo de vida: quantidade de saltos limite que um pacote pode dar
- Protocolo da camada superior: pra quem que eu entrego isso, tcp udp
- Checksum do cabeçalho
- IP de origem e destino: crucial
- opções : inutil
- Emfim os dados

## Fragmentação do Datagrama

![[Pasted image 20250103172941.png]]

![[Pasted image 20250103173103.png]]
Como funciona:
- Quantidade de bytes de cada fragmento é calculado
- perceba que ***a identificação*** de cada pacote é  ***a mesma***
- Existe um campo para deslocamento, significa que os dados são a partir do byte calculado
- 0 significa que é o ultimo fragmento 1 é o contrário
## Endereçamento IPV4

- Um endereçamento IP está tecnicamente associado com uma interface, ou seja, o IP mão é da máquina, mas sim da inteface de rede
- cada endereço IP tem comprimento de 32 bits (equivalente a 4 bytes), inclusive ja cabou samerda
- Portanto, há um total de 2^32  endereços de IP
- Esses endereços são escritos em notação decimal separados por pontos.

##### Endereços de Interfaces
- Endereço de Ip 
	- Parte de rede (bits de mais alta ordem), na **maioria** das vezes
	- Parte de desktops (bits de baixa ordem)
- Subrede da perspectiva do IP
- Interface de dispositivos com a mesma parte de rede nos seus endereços IP
- Podem alcançar um ao outro sem passar por um roteador
![[Pasted image 20250103180737.png]]
Subredes: 
- Subrede do roteador R1
- Subrede do roteador R2
- Subrede do roteador R3
- As conexões entre roteadores também são vistas como subredes mesmo que não aconteça nada ali
	- Subrede entre R1-R3
	- Subrede entre R1-R2
	- Subrede entre R2-R3

##### CIDR
- CIDR: Classless InterDomain Routing
- parte de rede do endereço de comprimento arbitrário
- formato de endereço: a.b.c.d/x onde x é o n° de bits na parte de rede do endereço![[Pasted image 20250103181501.png]]

## DHCP

- O DHCP permite que um hospedeiro obtenha um endereço IP de maneira automática (UDP porta 67)
- Processo de quatro etapas
1. Descoberta do servidos DHCP
2. Oferta dos servidores DHCP
3. Soliçitação DHCP
4. DHCP ACK

![[Pasted image 20250103215122.png]]

- O computador manda um broadcast para descobrir um servidor DHCP (DHCPDISCOVER)
- O servidor manda um broadcast oferecendo um IP (DHCPOFFER)
- O cliente manda uma requisição (broadcast) para IP (DHCPREQUEST)
- O servidor aceita a aquisição do IP pelo client com um ack (broadcast) (ACKDHCP)

## Tradução de endereços na rede (NAT)

Basicamente tinha pouco endereço de IP e o NAT deu uma prolongada legal na parada,
o roteador de uma rede domestica é o único que é valido para a internet, os ips de dentro da rede são alimentados por um DHCP do próprio roteadorzin,  e no caso os IPs e porta dos clientes são remapeados por á uma porta com o IP do roteador, para que a que o roteador funcione como um intermediador da conexão entre um individuo de fora da rede e um de dentro.

Essa dinâmica é além disso melhor para a segurança da rede isolando as redes e criando a oportunidade de usar firewalls por exemplo

## Protocolo de Mensagens de Controle da Internet (ICMP)
- O ICMP é usado por hospedeiros e roteadores para comunicar informações de camada de rede entre si
- A utilização mais comum  do ICMP é para comunicação de erros.
- Mensagens ICMP têm um campo de tipo e um campo de código
- O Conhecido Ping envia uma mensagem ICMP do tipo 8 código 0 para o hospedeiro especificado, basicamente manda o ping e mede o quão congestionado está aquela rota até o cliente destino

![[Pasted image 20250103225056.png]]

## IPV6

![[Pasted image 20250104110114.png]]
- Estrutura:
	-  Versão: versão do IP obvio
	- Classe de Tráfego: como se fosse uma fila prioritária para idoso, ou gestante esse tipo de coisa, tipo clientes que fazem muitas transações, tipo banco etc, distinção entre fluxos
	- Rótulo de fluxo: distinção de fluxos juntamente com a classe | apresentar os recursos da rede pra quem precisa mais
	- Endereço de origem e destino
	- E os Dados

- **Motivação inicial**: Espaço de 32-bits 
- **Motivação adicional**: 
	- formado de cabeçalho facilita acelerar processamento/reencaminhamento
	- Mudanças no cabeçalho para facilitar QoS (Quality of Service)
	- novo endereço "anycast": rota para o "melhor" de vários servidores replicados

- **Formato de datagrama IPV6**:
	- cabeçalho de tamanho fixo de 40 bytes
	- não admite fragmentação - se oo pacote chegar lá e o enlace não admitir é mandado um sinal para mandar dnv um pacote menor

- **Checksum**: removido completamente para reduzir tempo de processamento a cada roteador (existem outros métodos melhores)
- **Opções**: permitidas, porém fora do cabeçalho
- **ICMP v6**: versão nova de ICMP
	- tipos adcionais de mensagens, por ex. "Pacote Muito Grande"
	- Funções de gerenciamento de grupo multiponto

- Nem todos os roteadores podem ser atualizados simultâneamente
- "Dias de mudança geral" inviável

- Como a rede funciona misturando IPV4 e IPV6?
- **Tunelamento**!!: datagamas IPV6 carregados em datagramas IPV4 entre roteadores IPV4
### Transição IPV4 -> IPV6
- Abordagem de tunelamento
![[Pasted image 20250104113847.png]]
Acontece que quando quando o pacote ipv6 chega num roteador ipv4 esse pacote é involto de um cabeçalho ipv4 até chegar num roteador ipv6 novamente, então é como se passasse por apenas um ipv4 até o próximo ipv6.

### Uma breve Investida em segurança IP

- O IPsec foi desenvolvido para ser compatíval com o IPv4 e IPv6
- Em particular, para obter os benefícios do IPv6, não precisamos substituir as pilhas dos protocolos em todos os roteadores e hospedeiros na internet

- Os serviços oferecidos por uma sessão IPsec incluem:
	- Acordo criptogáfico
	- Codificação das cargas úteis do datagrama IP
	- integridade dos dados
	- Autenticação de Origem

Quando dois hospedeiros estabelecem uma sessão IPsec, todos os segmentos TCP e UDP enviados entre eles serão codificados e autenticados.

O IPsec oferece uma cobertura geral, protegendo toda a comunicação entre os dois hospedeiros para todas as aplicações de rede.

## Algorítimos de Roteamento

- um hospedeiro está ligado diretamente a um roteador, o **roteador default** para esse hospedeiro, normalmente é o roteador de borda de uma casa por exemplo
- **Roteador de Origem** é o roteador *default* do hospedeiro de origem e o **Roteador de Destino** é o roteador *default* do hospedeiro de destino.

- Grafos são utilizados para formular problemas de roteamento
	- Grafo G = (N,E)
	- N = conj. de roteadores = {u, v, w, x, y, z}
	- E = conj. de enlaces = {(u,v), (u,x), (v,x), (v,w), (x,w), (x,y), (w,y), (w,z), (y,z)}

- Custo do caminho (x1, x2, x3,..., xp) = c(x1,x2) + c(x2,x3) + ... + c(xp-1,xp)

O algorítimo de roteamento encontra o caminho de menor custo

c(x, x') = custo do enlace (x, x')

- custo poderia ser inversamente proporcional a banda ou inversamente relacionado ao congestionamento

### Algorítimos de roteamento

- Um **algorítimo de roteamento global** calcula o caminho de menor custo entre uma origem e um destino usando conhecimento completo e global sobre a rede. *basicamente pega todos os nós e calcula a melhor rota de todos* (dikjstra)
- Em um **algorítimo de roteamento descentralizado** o calculo do caminho de menor custo é realizado de modo iterativo e distribuído. *o nó vê com os vizinhos até achar o destino* (BGP)
- Em **algorítimos de roteamento estáticos**, as rotas mudam muito devagar ao longo do tempo, muitas vezes como  resultado de intervenção humana (usado na internet)
- **Algorítimos de roteamento dinâmicos** mudam os caminhos de roteamento à medida que mudam as cargas de tráfego ou a topologia da rede. *pode criar pendulos, é ruim*
- Em um **algorítimo sensível à carga**, custos de enlace variam dinamicamente para refletir o nível corrente de congestionamento do enlace subjacente *pode criar pendulos, é ruim*

#### Algorítimos de roetamento
- Algorítimo de Dijkstra: 
	- Topologia de rede, custos dos enlaces conhecidos por todos os nós
	- realizado atravésqq de "difusão do estado dos laces"
	- todos os nós tem mesma info.
	- calcula caminhos de menor custo de um nó (origem) para todos os demais
	- gera tabela de rotas para aquele nó
	- iterativo: depois de K interpretações, sabemos menor custo K destinos
- Notação
	- C(i,j): custo de i á j, infinito se os nós não forem visinhos
	- D(V): valor corrente do custo do caminho da origem ao destino V
	- p(V) nó antecessor no caminho da origem ao nó V
	- N' conjunto de nós cujo caminho de menor custo já foi determinado
**Fazer exercícios**

### O algorítimo de roteamento de vetor de distâncias (DV)

- **Equação de Bellman-Ford** (programação dinâmica)
- Define
- dx(y) = min {c(c,v)+ dv(y)}
- onde min é tomado entre todos os vizinhos v de x

A ideia é que a rota otima sempre passa por pelomenos um roteador vizinho da origem, então seria o custo da origem até o vizinho + distancia otima do nó vizinho até o destino, e assim vai se passando até chegar a origem, como se fosse uma recursão mas tecnicamente não é

- **Ideia basica**
	- cada nó envia periódicamente o seu próprio vetor de distâncias estimado para os vizinhos
	- quando um nó **x** recebe um novo VD (vetor distancia) de um vizinho, ele atualiza o seu VD usando a eq B-F

- **Iterativo, assíncrono** 
- **cada iteração local causada por**:
	- mudança do custo do enlace local
	- mensagem do vizinho:  mudança de caminho de menor custo para algum destino
- **Distribuído:**
	- cada nó avisa a seus vizinhos apenas quando muda seu caminho de menor custo para qualquer destino
		- os vizinhos então avisam a seus vizinhos, se for necessário
![[Pasted image 20250104235238.png]]

- **Mudança no custo dos enlaces**:
	- boas notícias chegam logo 
	- más noticias demoram para chegar - problema da "*contagem infinita*"
	- **Duvida** basicamente se tiver uma troca de custo, e o custo se tornar muito maior tipo 60, a atualização da tabela não vai ver que aumentou para 60 direto, ela vai ser atualizada de 1 em 1, tipo 6,7,8 ... 60 
	- uma "solução" seria mudar para infinito, pois comparado com infinito 60 n é nada, tem que intender sapoha aqui

Comparação EE x VD (dijkstra x vetor de distâncias)

- **complexidade de mensagens**
	- EE: com n nós, E enlaces, o(n^e) mensagens enviadas
	- VD: troca de mensagens apenas entre vizinhos
		- varia o tempo de convergência
- **Rapidez de convergência:
	- EE: algoritimo O(n²) (ja existe O(logn))
		- podem correr oscilações (entrar em loop)
	- VD: varia tempo para convergir
		- podem ocorrer rotas cíclicas
		- problema de contagem ao infiinto
- **Robustez**: oque acontece se houver falha do roteador?
	- EE:
		- nó pode anunciar valores incorretos de custo de enlace
		- cada nó calcula sua própria tabela (o erro se contem ao nó)
	- VD:
		- um nó VD pode anunciar um custo de caminho incorreto
		- a tabela de cada nó é usada pelos outros nós
		- um erro propaga pela rede
