# Segurança em redes

## Introdução

Propriedades desejáveis numa comunicação segura

- Confidencialidade:
	- Apenas o transmissor e o receptor desejado devem entender o conteúdo da mensagem
- integridade de mensagem:
	- transmissor e receptor querem garantir que a mensagem não seja alterada (em transito ou após) sem que isso seja detectado
- Autenticação do Ponto Final:
	- transmissor e receptor querem confirmar a identidade um do outro
- Segurança operacional:
	- os serviços devem estar acessíveis e disponíveis para os usuários)(detecção de invasão, worms, firewalls, internet publica)

*Oque um vilão pode fazer?*

- grampo: interceptação de mensagens
- inserir ativamente mensagens na conexão
- falsidade ideológica: pode imitar/falsificar endereço de origem de um pacote(ou qualquer campo do pacote)
- sequestro: assumir conexão em andamento removendo o transmissor ou o receptor colocando-se no lugar
- Negação de serviço: impede que o serviço seja usado por outros
	*etc*

## Princípios da Criptografia

- Técnicas criptográficas permitem que um remetente disfarce os dados de modo que um intruso não consiga obter nenhuma informação dos dados interceptados
- **criptografia de chave simétrica**: as chaves do transmissor e do receptor são idênticas
- **criptografia chave publica**: cifra com cave pública, decifra com ceve secreta (privada)

### Criptografia de chaves Simétricas

Cenários de uma possível interceptação

- Ataque exclusivo a texto cifrado: se tenho apenas o texto cifrado -> analise estatística
- Ataque com texto aberto conhecido: tenho conhecimento que "Alice e Bob" estão no texto, algumas letras conhecidas facilitarão a quebra do código
- Ataque com texto aberto escolhido: forçar a entrega de um texto conhecido, assim vc faz uma engenharia reversa a partir do hash gerado

DUVIDA: ainda não entendi oque é cifras de bloco

- As cifras de bloco em geral usam uma técnica chamada **Encadeamento de Bloco de Cifra** (CBC - cypher block chaining)
- enviar somente um valor aleatório junto com a primeira mensagem e, então, fazer o emissor e o receptor usarem blocos codificados em vez do número aleatório subsequente
- Um XOR entre a mensagem e o número aleatório é feito antes de transmitir
- **Consequencia**: é preciso fornecer um mecanismo dentro do protocolopara distribuir o vetor de inicialização do emissor ao receptor
- OBS: oque eu entendi foi: vc pega uma chave/numero aleatório, e faz o XOR com o primeiro byte, depois pega o resultado e faz XOR com o segundo, e assim por diante
- **Problema**: ainda tem a dificuldade da troca das chaves

**Principal problema das chaves simétricas é combinar as chaves**

### Criptografia de Chave Pública

- Cada cliente tem 2 chaves uma pública e outra privada
	- apenas a chave pública pode descriptografar a chave privada
	- assim como apenas a chave privada pode descriptografar a chave publica
- dada a chave pública deve ser impossível calcular a chave privada 
- **RSA**: algorítimo de Rivest Shamir e Adelson

#### RSA

O RSA faz uso extensivo das operações aritméticas usando a aritmética de modulo-n

Existem dois componentes inter-relacionados do RSA:

- A escola da chave pública e da chave privada
- O algorítimo para cifrar e decifrar
A segurança do RSA reside no fato de que não se conhecem algorítimos para fatorar rapidamente um número, nesse caso. o calor público **n**, em números primos **p** e **q**.
![[Pasted image 20250622194741.png]]

- O RSA é múito utilizado para troca da chave simétrica, pois tem um custo computacional importante
- Usado também para assinatura digital e na garantia da integridade da mensagem

## Integridade de Mensagens e Assinaturas Digitais


- Para autenticar a mensagem, BOB precisa verificar se:
 1. A mensagem foi realmente enviada por Alice
 2. A mensagem não foi alterada em seu caminho para BOB
- Um ataque relativamente fácil no algoritmo de roteamento é Trudy distribuir mensagens de estado de enlace falsas
- é um exemplo da necessidade de verificar a integridade da mensagem, ou seja, ter certeza de que o roteador A criou a mensagem e que ninguém alterou em trânsito

### Funções HASH criptográficas

- Uma função hash criptográfica deve apresentar a seguinte propriedade:
	- Em termos de processamento, seja impraticável encontrar duas mensagens, diferentes x e y tais que H(x) = H(y)
- **Problema**: alguém poderia interceptar a mensagem, mudar o conteúdo, passar a função hash no conteúdo modificado e repassar a mensagem, isso configuraria como um pacote integro
#### Código de Autenticação

- Para realizar a integridade da mensagem, além de usar as funções de hash, Alice e Bob precisarão de um segredo compartilhado **s**, que não é nada mais do que uma cadeia de bits denominada **chave de autenticação**.
![[Pasted image 20250622221448.png]]
Veja que s é concatenado a mensagem assim a função hash é passada por cima, assim, trudy não teria esse segredo para poder falsificar algo, além disso a chave serve de identidade de clientes já que cada um vai ter sua chave de autenticação única

Exemplos de Funções HASH:
- MD5
- SHA-1

DUVIDA: Pedir pare explicar a parte de assinatura digital 
