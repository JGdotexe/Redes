
# Redes Multimídia

## Introdução

- A internet NÃO oferece:
	- garantia de entrega
	- tempo fim a fim
	- ordem de pacotes
	- intervalo entre as entregas
	- banda mínima, máxima ou média

- A rede apenas oferece o desempenho suficiente para as aplicações de multimídia

**Classe de aplicações Multimídia**

- Fluxo contínuo (streams) de áudio e vídeo armazenados
- Fluxo contínuo (streams) de áudio e vídeo ao vivo
- nídeo interativo de tempo real

- **Jitter**: é uma variabilidade dos atrasos dos pacotes dentro de um mesmo fluxo de pacotes
	- hora vc pega hora melhor caminho hora outro, isso varia

## **Características fundamentais**: 
- tipicamente são sensiveis a atrasos fim a fim, variação do atraso(jitter), mas são tolerantes a perdas: perdas não muito frequentes causam apenas pequenos distúrbios
- antítese da transferencia de dados que é tolerante a perdas mas tolerante a atrasos

### Propriedades de Vídeo

- Taxa alta de bits
- Pode ser compactado, compensando assim a qualidade com taxa de bits
	- pode-se usar a compactação para criar multiplas versões do mesmo vídeo

### Propriedades de Audio

- Exige largura de banda bem menor
- Usuários são mais sensíveis a pequenas falhas de audios do que pequenas falhas de vídeo

## Audio e vídeo de fluxos contínuo de armazenados

- Netflix, Youtube etc.
- Pode contribuir com mais de 50% do trafego nas redes de internet
### Características importantes 

- Fluxo contínuo
- Interatividade
- Reprodução Contínua
### Voz sobre IP interativos
-  é camada de telefonia da internet
- Aplicações multimídia são toleráveis a perda
- Restrições de tempo fim a fim -> experiencia desagradável quando o retardo é longo
### Áudio e vídeo de fluxo continuo ao vivo
- Rede precisa oferecer a cada fluxo de multimídia ao vivo uma vazão media que seja maior que a taxa de consumo de vídeo
- Atraso pode ser um problema
- Grupos IP, mais comumente por aplicações em camadas (p2p, ou CDN) ou múltiplos fluxos individuais separados

## Vídeo de fluxo continuo armazenado

- UDP de fluxo contínuo
- HTTP de fluxo contínuo
- HTTP de fluxo adaptatívo

- Característica comum entre todos:
	Usam buffer no lado do cliente pra tentar suavizar a variação de atraso da rede

![[Pasted image 20250621213857.png]]
- A taxa em que o servidor manda os dados na rede é constante
- Mas a recepção de dados é variável(por isso o uso de buffers) 
- Então existe um atraso (controlado pela aplicação) para a exibição de conteúdo proveniente do buffer para o usuário

### UDP Fluxo Contínuo

- O servidor transmite vídeo a uma taxa que corresponde à taxa de consumo de vídeo do cliente
- Normalmente usa um pequeno buffer no lado do cliente
- Funciona Muito bem numa rede com baixo congestionamento e que não "ohle fluxos"
- Pode deixar de oferecer reprodução contínua
- Exige um servidor de controle de mídia(RTSP - Real Time Streaming Protocol).
- Muitos firewalls são configurados para bloquear o tráfego UDP
- Algumas aplicações trabalham com fluxo adaptativo mas o TCP predomina hj em dia
### HTTP de Fuxo Contínuo

- O vídeo é apenas armazenados em um servidor HTTP como um arquivo comum com uma URL específica
- O uso do HTTP sobre TCP tembém permite ao vídeo atravessar firewalls e NATs facilmente
- Também usa servidores RTSP

OBS: o yt usa metodos para diminuir o uso geral de banda por exemplo, armazenando metadata(previews) de partes dos vídeos, assim usando o recurso de requirir sessões de um arquivo do HTTP ele não baixa todo o vídeo para o cliente apenas as partes em que o cliente pedir
![[Pasted image 20250621220249.png]]

OBS: perceba que não se mexe no buffer tcp, se usa o tcp como o sistema fornece, o buffer da aplicação só da mais um pouco de gerencia sobre o que sai do buffer, além de dar mais uma gordurinha caso tenha perda de pacotes

### Fluxo contínuo adaptativo e DASH
- Pelo DASH (fluxo contínuo adaptativo dinamicamente sobre HTTP), o vídeo ´q codificado em muitas versões diferentes, cada qual uma taxa de bits e u diferente nível de qualidade. -> diferentes versões do vídeo para o usuário escolher ou ser usado de acordo com a rede
- O servidor contém um arquivo  de manifesto com a localização (URL), taxas, codificação
- O cliente pode baixar alguns segundos nessas várias codificações e selecionar alguma
- Durante a apresentação as codificações/taxas podem ser alteradas para se adaptar à banda disponível no momento
- Permite aos clientes com diferentes taxas de acesso á internet fluis em um vídeo por diferentes taxas codificadas
- Cada versão do vídeo é armazenada em um servidor HTTP, cada um com uma diferente URL
### Redes de distribuição de conteúdo
Uma CDN (Rede de Distribuição de Conteúdo)

- Armazena cópias dos vídeos em seus servidores
- Tenta redirecionar cada requisição do usuário para uma localidade CDN que proporcionará a melhor experiência para o usuário
	- Enter-Deep: servidores os mais próximo possível dos ISPs, dentro deles (Akamai -> 1700 locais) OBS: 
		- Vantagens: se um vídeo for requisitado ele provavelmente vai ser requisitados por mais gente nessas redondezas, assim acessando um CDN na região a qualidade de entrega é boa
		- Desvantagens: Manutenção desses milares de CDNs obviamente tem que ser terceirizadas
	- Bring-Home: Grandes datacenters/clusters, rede de interconexão pesadas POPs e ISPs nivel 1 (Limeligt, Google, ...)
	- Manutenção, escala.
	- Desvantagens: se esgotar a capacidade da rede que se conecta ali, é preciso fazer um upgrade de toda uma infra, isso pode ser um tico mais demorado

DUVIDA: Como funciona imagem das paradas de DNS no CDN

#### Estratégia de seleção de clusters

- é um mecanismo para direcionamento dinâmico de clientes para um *cluster* de servidor ou uma central de dados dentro da CDN
- Uma estratégia simples é associar o cliente ao cluster que está geograficamente mais próximo
- As CDNs podem realizar medições  em tempo real do atraso e problemas de baixo desempenho entre os clusters e clientes, para determinar o melhor cluster para um cliente baseado nas atais condições de tráfego
- Uma alternativa para medir as propriedades dos caminhos é usar as características do trafego mais recente entre os clientes e os servidores da CDN.
- Outra abordagem é utilizar o IP para qualquer membro do grupo ???
	- A ideia por trás do IP para qualquer membro do grupo é colocar os roteadores na rota da internet dos pacotes do cliente para o cluster "mais próximo" como determinado pelo BGP
OBS: se e tenho o IP de origem e IP destino eu posso usar os dados de saltos do BGP para achar a rota com menores saltos para alcançar uma região

![[Pasted image 20250621232448.png]]
DUVIDA: pedir pro leandro explicar essa imagem e como funciona esse algorítimo

#### Aplicações

- Netflix: CDNs (notadamente Akamai) e links de terceiros, HTTP de fluxo contínuo adaptativo. Servidores próprios de registros e pagamentos. Converte os vídeos em várias resoluções  e disponibiliza (nuvem Amazon).
- Youtube: Data centers da Google, direciona via DNS para os datacenters. Desde seu modelo de negócios Google usa o HTTP de fluxo.
- Kankan: maior da China e usa tecnologia proprietária P2P com hash distribuido (tipo torrent).UDP sempre que possível

## Voz sobre IP (VOIP)

### As limitações de um serviço IP de melhor esforço

- **Perda de pacotes**: A perda de pacotes pode ser eliminada enviando pacotes por TCP, em vez de por UDP(facilita no NAT). Se tivermos um enlace altamente congestionado ou um wifi ruidoso pode não dar certo(pq o tcp tem aquela parada de despencar a taxa por causa da perda de pacotes).
- **Atraso fim a fim**: acumulo de atraso de processamento, de transmissão e de formação de filas nos roteadores; atraso de propagaçãonos enlaces e atrasp de processamento em sistemas finais
- **Variação de atraso do pacote**: tempo decorrido entre o momento em que um pacote é gerado na fonte e o momento em que é recebido no destinatário, pode variar de pacote para pacote

### Eliminação da variação de atraso no receptor 

 Atraso por reprodução adaptativa
- **Objetivo**: minimizar o atraso de reprodução mantendo baixa taxa de perdas
- **Abordagem** : auste adaptativo do atraso de reprodução
	- estima o atraso da rede e ajusta o atraso de reprodução no início de cada surto de voz.
	- período de silencio são comprimidos e alongados
	- pedaços ainda são reproduzidos a cada 20 mseg durante um surto de voz
DUVIDA: vai cair aquela formulinha do atraso por reprodução adaptativa, explica td pq eu to meio em duvida slides 52 ao 54

E os pacotes que foram perdidos?

### Recuperação de perda de pacotes

##### FEC (Foward Error Correction)

- A ideia da FEC é adicionar informações redundantes ao fluxo de pacotes original 
- Dando carona á informação redundante de qualidade mais baixa
![[Pasted image 20250622001525.png]]

##### Intercalação
- Pacote carrega informação de alguns outros pacotes que vem intercalados, assim se houver a perda de 1 os outros pacotes podem recompor (pelo menos boa parte do que foi perdido)
![[Pasted image 20250622001934.png]]
percebe-se que teve alguma perda, mas é imperceptível para uma pessoa perceber uma perda dessas em áudio

## RTP Protocolo de tempo real 

- Pode ser usado para transportar formatos comuns como PCM, ACC e MP3 para som e MPEG h.263 para vídeo.
- Também pode ser sado para transportar formatos proprietários de som e de vídeo
- Hoje, o RTP é amplamente implementado em muitos produtos e protótipos de pesquisa.

- O RTP especifica uma estrutura de pacote para pacotes que transportam dados de áudio e de vídeo
	- RFC 3550
- pacote RTP provê:
	- identificação do tipo de carga
	- numeração da sequência de pacotes
	- carimbo de tempo
- Comumente roda sobre UDP

- RTP tem ampla implementação por muitas aplicações
- Não fornece nenhum mecanismo que assegure a entre a de dados a tempo e nem fornece garantias de qualidade de serviço
- Permite que seja atribuído a cada origem seu próprio fluxo de pacotes RTP
- Não são limitados às aplicações individuais

**Exemplo**

- Considere o envio de voz codificada em PCM de 64kbps sobre RTP
- Aplicação coleta os dados codificados em pedaços, ex.,  a cada 20ms = 160bytes num pedaço
- o pedaço de áudio junto com o cabeçalho RTP formam um pacote RTP, que é encapsulado  num segmento UDP
	- o cabeçalho RTP indica o tipo da codificação durante a conferência
- contém números de sequencia e carimbos de tempo

## Suporte de rede para multimídia

- **Até o momento**: extraímos o máximo do melhor esforço
	- Um único tamanho veste todos os modelos de serviço

- **Alternativa**: múltiplas classes de serviço
	- Dividir o tráfego em classes
	- A rede trata diferentes classes de tráfego de modo diferente (analogia: serviço VIP)
- **Granularidade**: serviço diferenciado entre múltiplas classes, não entre conexões individuais
- **história**: bits ToS

**Dimensionado Redes de melhor esforço**

- A questão de como projetar uma topologia de rede para alcançar determinado nível de desempenho de fim a fim é um problema de projeto de redes que muitas vezes é chamado de dimensionamento de redes.
- Sabendo que a internet de melhor esforço de hoje poderia dar suporte para o tráfego de multimídia em um nível de desempenho apropriado, se fosse dimensionada para fazer isso, a questão é  porque a internet de hoje não o faz.
- As respostas são principalmente econômicas e organizacionais
