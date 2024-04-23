# Tutorial de Docker

## Introdução

O Docker é uma plataforma de código aberto que permite automatizar o processo de criação, implantação e execução de aplicativos em contêineres. Neste tutorial, você aprenderá os conceitos básicos do Docker e como começar a usá-lo.

## Passo 1: Instalação do Docker

Antes de começar, você precisa [instalar o Docker](https://www.docker.com) em sua máquina. Siga as instruções de instalação fornecidas no site oficial do Docker para o seu sistema operacional.
O link é: 

## Passo 2: Imagens

O docker funciona a partir de um conceito de *"imagens"*, do qual encontramos elas em um site da docker chamado [Docker Hub](https://www.docker.com). Lá você pode pesquisar qual imagem quer usar, por exemplo: MySQL, NodeJS e afins...

## Passo 3: Criação de um contêiner


Para simular a criação de um contêiner vamos usar como exemplo o MySQL

Para baixar a imagem do MySQL você pode rodar o comando:

`docker pull mysql:tag`

Para rodar a aplicação você pode usar o seguinte comando:

`docker run --name NOME_AQUI -e MYSQL_ROOT_PASSWORD=SENHA_AQUI -p PORTA_HOST:PORTA_CONTAINER -d mysql:VERSÃO_AQUI`

- Você pode definir o nome do contêiner 
- Pode definir a senha
- Escolher a versão 
- Definir a porta de "host" e "container"

**Qual a diferença entre as portas?**


Definir as portas: Aqui é onde as coisas podem ficar um pouco confusas. Existem duas portas envolvidas: a porta do contêiner e a porta do seu computador.

A porta do contêiner é a porta em que o banco de dados vai funcionar dentro do contêiner. Por exemplo, o MySQL geralmente usa a porta 3306.

A porta do seu computador é a porta que você vai usar para se conectar ao contêiner. Por exemplo, se você quiser acessar o banco de dados do seu computador, você pode abrir a porta 3306 no seu computador.

Então, quando você usa o comando `-p PORTA_HOST:PORTA_CONTAINER`, você está dizendo ao Docker para mapear a porta do seu computador (PORTA_HOST) para a porta do contêiner (PORTA_CONTAINER). Por exemplo, se você quiser abrir a porta 3306 do seu computador e mapeá-la para a porta 3306 do contêiner, você pode usar -p 3306:3306.

<br></br>

# Exemplo com NodeJS


Para criar um contêiner com NodeJS, você pode seguir os seguintes passos:

1. Baixe a imagem do NodeJS usando o comando:
    ```
    docker pull node:tag
    ```
    *Tag é a versão que será usada*
    
<br></br>

2. Crie um diretório para o seu aplicativo NodeJS e navegue até ele:
    ```
    mkdir myapp
    cd myapp
    ```
<br></br>

3. Crie um arquivo chamado `app.js` e adicione o seguinte código:
    ```javascript
    const http = require('http');

    const hostname = '0.0.0.0';
    const port = 3000;

    const server = http.createServer((req, res) => {
      res.statusCode = 200;
      res.setHeader('Content-Type', 'text/plain');
      res.end('Hello, World!\n');
    });

    server.listen(port, hostname, () => {
      console.log(`Server running at http://${hostname}:${port}/`);
    });
    ```
<br></br>
4. Crie um arquivo chamado `Dockerfile` e adicione o seguinte código:
    ```
    FROM node:tag
    WORKDIR /usr/src/app
    COPY package*.json ./
    RUN npm install
    COPY . .
    EXPOSE 3000
    CMD [ "node", "app.js" ]
    ```

<br></br>

5. Construa a imagem do Docker usando o comando:
    ```
    docker build -t mynodeapp .
    ```

<br></br>

6. Execute o contêiner usando o comando:
    ```
    docker run -p 3000:3000 -d mynodeapp
    ```

<br></br>

Agora você pode acessar o aplicativo NodeJS em http://localhost:3000.