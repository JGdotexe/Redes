
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