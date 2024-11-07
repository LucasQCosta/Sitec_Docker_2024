# Aplicação Web "Bem-vindo à SITEC 2024"

Este repositório contém uma aplicação web simples que exibe uma página com um fundo azul e bolinhas, além de uma mensagem de boas-vindas ao evento "SITEC 2024". 

## Estrutura do Projeto

- **Dockerfile**: define o ambiente para execução da aplicação no Docker.
- **index.html**: contém o conteúdo da página web com HTML e CSS inline para o estilo.

## Pré-requisitos

- [Docker](https://docs.docker.com/get-docker/) instalado em seu sistema.

## Configuração do Projeto

O projeto possui os seguintes arquivos:

### 1. `Dockerfile`

Este arquivo cria uma imagem do Docker baseada no servidor NGINX, copiando o arquivo `index.html` para o diretório padrão do servidor.

```dockerfile
# Use uma imagem base do Nginx
FROM nginx:alpine

# Copia o arquivo HTML para o diretório padrão do Nginx
COPY index.html /usr/share/nginx/html/index.html

# Expõe a porta padrão do Nginx
EXPOSE 80
```

### 2. `index.html`

Este arquivo contém o código HTML e CSS para exibir a página com o estilo desejado.

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Bem-vindo à SITEC 2024</title>
  <style>
    body {
      margin: 0;
      height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      background-color: #1E90FF;
      background-image: radial-gradient(circle, #FFFFFF 1px, #1E90FF 1px);
      background-size: 30px 30px;
      color: #FFFFFF;
      font-family: Arial, sans-serif;
      font-size: 2rem;
    }
  </style>
</head>
<body>
  <h1>Bem-vindo à SITEC 2024</h1>
</body>
</html>
```

## Passo a Passo para Executar a Aplicação

### Passo 1: Construir a Imagem Docker

No diretório onde estão o `Dockerfile` e o `index.html`, execute o comando abaixo para criar a imagem Docker:

```bash
docker build -t sitec-2024 .
```

### Passo 2: Executar o Contêiner

Após a construção da imagem, execute o contêiner com o seguinte comando:

```bash
docker run -d -p 8080:80 sitec-2024
```

Esse comando inicia o contêiner e mapeia a porta 80 do NGINX para a porta 8080 da sua máquina local.

### Passo 3: Acessar a Aplicação

Abra o navegador e acesse `http://localhost:8080`. Você verá uma página com fundo azul, bolinhas decorativas e a mensagem "Bem-vindo à SITEC 2024".

## Personalizações

- **Cores e Tamanho das Bolinhas**: O CSS inline no `index.html` permite ajustar o tamanho das bolinhas (`background-size`) e as cores.
- **Mensagem**: Altere o conteúdo do `<h1>` para modificar o texto exibido na página.

## Recursos Adicionais

Para mais informações sobre como criar e executar contêineres Docker com NGINX, consulte a [documentação oficial do Docker](https://docs.docker.com/).

---
