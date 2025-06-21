### Arquitetura de aplicação de rede

- Arquitetura cilente-servidor
	- web, e-mail, ftp, telnet, maioria das grandes aplicações
	- A comunicação não ocorre entre os hospedeiros, sempre passa por um servidor
- Arquitetura p2p
	- comunicação direta entre duplas de hospedeiros (denominados **pares**)
	- bitTorrent ,iptv
	- Dificuldades da arquitetura
		- ISP amigável: Atualmente oque predomina nos ISPs é a banda assimetrica, oque dificulta o p2p pois tambpem depende em banda de upload
		- Segurança!!
		- Incentivo: bla bla bla

### Mini visão
Os processos no nosso pc/threads tem acesso a camada de transporte (TCP, portas e etc) por meio dos *sockets* que são basicamente APIs. Quem fornece os sockets são os SOs

Pra conexão acontecer entre os processos precisamos de:
1. o endereço do hospedeiro
2. um indentificador que especifica o processo receptor no hospedeiro destino (***Socket***)
### Serviços de transporte disponiveis para as aplicações

- Transferência confiável de dados: a aplicação tolera perdas?
- Vazão:  restrição de banda afeta a aplicação?
- Temporização: o tempo fim a fim é relevante?
- Segurança: o transporte tem restrição de segurança?

#### TCP
- orientado a conexão - transporte confiável 
- controle de fluxo - remetente não vai "afogar" o receptor?
- Controle de congestionamento - perda de pacote? descongestiona a rede
- **Não provê:** garantias temporais ou de banda minima

#### UDP 
- Transferência de dados não confiável 
- **Não provê:** estabelecimento da conexão, confiabilidade, controle de fluxo, controle de congestionamento, garantias temporais ou banda minima


### HTTP

- Mais usado na Web
- Uma pagina web é composta por objetos que são arquivos, então o protocolo HTTP é um protocolo de compartilhamento de arquivos
- Usa o TCP como protocolo de transporte (porta 80)
- **Duvida:** oque é stateless


- Conexões não persistentes **(HTTP 1.0)**: eu acesso um site requisito um objeto, esse objeto foi entregue a conexão é encerrada então para requisitar vários objetos seriam feitas várias conexões 

- Conexões persistentes **(HTTP 1.1)** : consegue requisitar vários objetos por (com ou sem paralelismo).

#### Coockies

- Permitem que sites monitorem seus usuários
- Um número é gerado baseado na sua interação com o serviço, os cookies são armazenados no computador do client/hospedeiro que depois vai ser mandado para o servidor como um feedback de interações frequentes do usuários.

#### Cache Web

- É uma entidade de rede que atende requisições HTTP e armazena os objetos requisitados, assim é checado com o servidor que foi requisitado o objeto a data de ultima alteração daquele arquivo assim se ele não foi alterado, o Cache retorna o arquivo ou objeto requisitado poupando uma nova conexão e economizando banda.
- Get condicional - principio explicado cima

#### FTP

- **LEGADO**
- Transporta arquivos entre sistemas de arquivo local e remoto
- Parecido com o HTTP ja que também se trata de transferência de arquivos
- Protocolo fora de banda - utiliza mais de uma conexão pra realizar suas atividades, ele usa a porta 21 para mandar dados de controle (comandinhos) e na porta 20, paralelamente, é aberta uma conexão para transferência de dados

**Alguns comandos**: 
- USER username: usado para enviar identificaçao do usuário ao servidor (detalhe, em texto aberto)
- PASS password: usado para enviar a senha do usuário ao servidor (detalhe, em texto aberto)
- LIST: usado para pedir ao servidor que envie uma lista com todos os arquivos existentes no diretório remoto
- RETR filename: usado para extrair um arquivo do diretório atual do hospedeiro remoto



## Duvidas

- Oque são realmente os RRs
- 