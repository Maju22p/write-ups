# OhSINT  (Pt- Br)
Feito por: Maria Julia Souza  
Categoria: OSINT  Site: tryhackme.com   
Link: https://tryhackme.com/room/ohsint

## Introdução
O desafio apresentado é  descobrir o máximo de informações possível utilizando somente uma imagem do Windows XP.
![GoogleXp](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/googlexp-OhSINT1.png)   

Para facilitar o processo, utilizei a máquina virtual disponibilizada pelo próprio site; porém, dentro da room também é possível baixar a 
imagem para realizar a investigação localmente. A imagem do desafio na máquina do tryhack pode ser encontrada em `/Rooms/OhSint`.
![Diretorio](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT2.png)


### Inicio das Investigações

Como o desafio menciona uma imagem, o primeiro passo natural é verificar se ela contém metadados EXIF (informações que contêm data, horário, autor, GPS etc.). 
Então abri o diretório pelo terminal usando o comando  `cd Rooms/OhSINT` para encontrar onde a imagem estava localizada. 
Após isso usei da ferramenta exiftool (uma ferramenta específica para leitura, escrita e edição de metadados), o que me possibilitou ler as informações EXIF da imagem  

![exiftool](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT3.png)  

```
Copyright                     : OWoodflint  

GPS Position                    : 54 deg 17' 41.27" N, 2 deg 15' 1.33" W  

```
Dentro das linhas retornadas, encontramos informações bastante úteis, como o autor da imagem e uma possível localização de onde essa imagem foi feita. 
Para melhor compreensão, acabei cortando os outputs e deixando somente os relevantes para a investigação; o restante são metadados técnicos 
padrão que não seriam úteis nesse contexto. 

Ao encontrar o nome do autor (  OWoodflint ), decidi fazer uma simples busca no Google onde encontrei perfis com o mesmo usuário em dois 
sites: X.com   e github.com (no qual iremos nos aprofundar mais adiante).  

![busca](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT4.png) 


Ao encontrar o perfil do X do usuário, podemos responder às perguntas solicitadas pela room:  
![]()
![perfil x](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT5.png)

### What is this user's avatar of?
Tradução: _Do que é o avatar desse usuário?_
 Ao investigar o perfil do usuário no X, encontramos sua foto de perfil na qual é um gato  

 ![gato do perfil](https://github.com/Maju22p/write-ups/blob/main/Write%20up%20(POR)/imagens/OhSINT6.png)

**Resposta:** `cat`

###What is the SSID of the WAP he connected to? 
Tradução: _Qual é o SSID do ponto de acesso (WAP) ao qual ele se conectou?  _ 
Em uma de suas postagens no  X  podemos encontrar o BssiD de sua rede `(B4:5D:50:AA:86:41)` 


Inicialmente tentei o basic search do Wigle usando apenas o BSSID, mas não obtive nenhum resultado no site. Após várias tentativas, percebi então que era 
necessário usar o Advanced Search. Ao utilizar o advanced search da plataforma Wigle 
(uma plataforma que mapeia e indexa redes wi-fi, Bluetooth e estações de rádio base)  e informar o BSSID, encontrei as seguintes informações : 



Onde podemos verificar o nome do  SSID: `UnileverWiFi`.
Resposta: `UnileverWiFi`

###What is his personal email address? 
Tradução: _Qual é o endereço de e-mail pessoal dele? _

Para descobrir a resposta a essa pergunta e às perguntas a seguir, investiguei  o github indicado na busca : https://github.com/OWoodfl1nt/people_finder , onde
encontrei um repositório chamado `people_finder`. Nesse repositório público pude encontrar  um arquivo *README* que possibilitou adquirir mais informações 
sobre o usuário, como seu email, seu blog pessoal(o qual iremos explorar adiante) e de onde ele é.

Resposta: `OWoodflint@gmail.com`  

### What site did you find his email address on? 
Tradução: _Em qual site você encontrou o endereço de e-mail dele?_ 
Resposta: `Github`

### What city is this person in? 
Tradução: _Em qual cidade essa pessoa está?_ 
Resposta: `London` 

### Where has he gone on holiday? 
Tradução: _Para onde ele foi de férias?_ 

Ao acessar o site encontrado anteriormente no repositório do Github, pude acessar o blog pessoal do usuário. 
Se nota que é um blog bem simples e a única informação que podemos obter é um texto no qual o próprio informa sobre sua viagem a Nova York.
Podemos assim, assumir que suas férias foi em Nova York.

Resposta: New York

### What is the person's password? 
Tradução: Qual é a senha dessa pessoa? 

Após verificar as funcionalidades do blog, decidi inspecionar o código-fonte. Onde foi possível encontrar um parágrafo branco escrito “pennYDr0pper.! ” 
no código HTML (talvez uma tentativa de armazenar a senha no site camuflando-a com o fundo). Ao testar a resposta foi confirmada que essa era a senha do usuario


Resposta: pennYDr0pper.!

Conclusão
Esse desafio mostrou como é possível reunir um perfil praticamente completo de uma pessoa a partir de uma única imagem, sem nenhuma técnica de invasão: 
apenas informações públicas e metadados que a maioria das pessoas nem sabe que existem. A partir de um simples campo de "Copyright" numa foto, 
foi possível rastrear redes sociais, e-mail, cidade, rede Wi-Fi, senha e até destino de viagem.
Isso reforça um ponto importante sobre segurança digital: metadados de imagens (como GPS e autor) e informações compartilhadas em redes 
sociais podem parecer inofensivos isoladamente, mas juntos formam uma trilha capaz de comprometer a privacidade de alguém. Como boa prática, é recomendável 
remover metadados de fotos antes de publicá-las e evitar compartilhar detalhes sensíveis (como BSSID de redes ou senhas, ainda que "escondidas" no código-fonte de
um site) publicamente.

