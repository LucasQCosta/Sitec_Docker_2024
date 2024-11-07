# Configuração e Execução do Ambiente Docker para Llama-2

Este guia fornece um passo a passo para configurar o Docker e executar um contêiner com a imagem `harsh-manvar-llama-2-7b-chat-test:latest` do Hugging Face, que hospeda o modelo Llama-2. A execução do contêiner expõe o modelo via uma interface web na porta 7860, acessível a partir do host.

## Pré-requisitos

- **Docker**: Certifique-se de ter o Docker instalado em seu sistema. Siga as instruções de instalação disponíveis no [site oficial do Docker](https://docs.docker.com/get-docker/).

Após a instalação, verifique se o Docker está instalado corretamente executando o seguinte comando no terminal:

```bash
docker --version
```

Esse comando deve exibir a versão do Docker instalada em sua máquina.

## Execução do Contêiner

O comando a seguir executa o contêiner com a imagem `harsh-manvar-llama-2-7b-chat-test:latest` e configura o ambiente necessário para acessar o modelo Llama-2.

### Comando para executar o contêiner:

```bash
docker run -it -p 7860:7860 --platform=linux/amd64 \
    -e HUGGING_FACE_HUB_TOKEN="YOUR_VALUE_HERE" \
    registry.hf.space/harsh-manvar-llama-2-7b-chat-test:latest python app.py
```

### Explicação dos parâmetros do comando:

- **`-it`**: Executa o contêiner em modo interativo e anexa um terminal a ele, permitindo que você interaja com o contêiner e seus processos.
- **`-p 7860:7860`**: Expõe a porta 7860 do contêiner para a porta 7860 do host, possibilitando o acesso ao servidor web do contêiner a partir do host.
- **`--platform=linux/amd64`**: Especifica que o contêiner deve ser executado em uma máquina Linux com arquitetura AMD64, garantindo compatibilidade.
- **`-e HUGGING_FACE_HUB_TOKEN="YOUR_VALUE_HERE"`**: Define a variável de ambiente `HUGGING_FACE_HUB_TOKEN` para o token que você possui. Esse token é necessário para autenticação e acesso ao modelo na Hugging Face.
- **`registry.hf.space/harsh-manvar-llama-2-7b-chat-test:latest`**: Especifica a imagem do contêiner armazenada no registro do Hugging Face.
- **`python app.py`**: Executa o script `app.py` no contêiner, que inicia o servidor e disponibiliza a interface do modelo para interações.

> **Nota:** Substitua `"YOUR_VALUE_HERE"` pelo seu token da Hugging Face para acessar o modelo.

### Acessando o Modelo

Após a execução bem-sucedida, o contêiner estará rodando o servidor web na porta 7860. Para acessar a interface do modelo Llama-2, abra o navegador e vá para:

```
http://localhost:7860
```

### Finalizando o Contêiner

Para parar a execução do contêiner, pressione `Ctrl + C` no terminal onde o contêiner está sendo executado. Isso encerrará o processo e parará o servidor web.

## Recursos Adicionais

Para mais detalhes sobre a utilização do Hugging Face, consulte a [documentação oficial do Hugging Face](https://huggingface.co/docs/).

# Outra possibilidade

Para isso você vai gerar o modelo na sua máquina. Então, você precisa ter `git` instalado na sua máquina para copiar esse repositório:

```
docker run -it -p 7860:7860 --platform=linux/amd
```

Então, acesse a pasta clona e rode esse comando:
```
docker buildx  build --platform=linux/amd64  -t lo
```

E então, execute o container usando esse comando:

```
docker run -it -p 7860:7860 --platform=linux/
```
---