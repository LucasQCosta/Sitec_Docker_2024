# Jupyter un Docker

# Executando Jupyter Notebook com Docker

Este guia passo a passo ajuda a configurar e rodar um contêiner com o Jupyter Notebook usando Docker. O Jupyter é uma ferramenta muito utilizada para desenvolvimento e análise de dados em Python, e o Docker facilita a criação de um ambiente isolado para rodá-lo.

## Passo 1: Baixar a Imagem do Jupyter

Vamos usar uma imagem oficial do Jupyter Notebook disponível no Docker Hub, chamada `jupyter/base-notebook`. Esta imagem contém o ambiente Jupyter configurado para ser executado em um contêiner Docker.

### Comando:

```bash
docker pull jupyter/base-notebook
```

### Explicação:

- `docker pull`: Comando para baixar uma imagem do Docker Hub.
- `jupyter/base-notebook`: Nome da imagem que contém o Jupyter Notebook. Ao rodar este comando, você baixa a versão mais recente da imagem base do Jupyter Notebook.

## Passo 2: Rodar o Contêiner do Jupyter Notebook

Agora que temos a imagem, vamos iniciar um contêiner que executa o Jupyter Notebook. Usaremos uma série de parâmetros para especificar o comportamento do contêiner.

### Comando:

```bash
docker run -p 8888:8888 jupyter/base-notebook
```

### Explicação:

- `docker run`: Comando para criar e iniciar um novo contêiner.
- `-p 8888:8888`: Define o mapeamento de portas, onde `8888` na máquina local é mapeado para `8888` no contêiner. O Jupyter Notebook roda na porta 8888 por padrão, e esse mapeamento permite acessá-lo localmente.
- `jupyter/base-notebook`: Especifica a imagem que será usada para criar o contêiner.

> **Observação**: Quando o contêiner for iniciado, você verá uma mensagem com uma URL contendo um "token" de autenticação. Copie e cole esta URL no navegador para acessar o Jupyter Notebook.

## Passo 3: Montar um Volume para Persistência de Dados (Opcional)

Se quiser salvar os notebooks que criar ou editar no Jupyter, você pode montar um volume que permita que os arquivos sejam salvos no seu sistema de arquivos local.

### Comando:

```bash
docker run -p 8888:8888 -v volume/sua/máquina:seu/volume/container jupyter/base-notebook
```

### Explicação:

- `-v volume/sua/máquina:seu/volume/container: Monta um volume no contêiner`, onde `volume/sua/máquina` representa o diretório atual em seu sistema e `"seu/volume/container"` é o diretório de trabalho padrão no contêiner para o usuário "jovyan" (o usuário padrão na imagem do Jupyter).
- Com essa opção, qualquer notebook ou arquivo criado ou modificado no Jupyter Notebook dentro do contêiner será salvo no diretório atual do sistema local, permitindo persistência dos dados.

## Passo 4: Acessar o Jupyter Notebook

Após iniciar o contêiner, acesse o Jupyter Notebook no navegador:

- URL de acesso: `http://localhost:8888`
- Se solicitado, use o token gerado para autenticar.

Pronto! Agora você está rodando o Jupyter Notebook em um contêiner Docker.

## Parar o Contêiner

Para interromper o contêiner, basta pressionar `Ctrl + C` no terminal onde ele está rodando. Se o contêiner estiver rodando em segundo plano, você pode pará-lo com o seguinte comando:

```bash
docker ps  # Lista os contêineres em execução
docker stop <container_id>  # Substitua <container_id> pelo ID do contêiner do Jupyter
```

> Use o comando `docker ps` para encontrar o `container_id` do contêiner Jupyter em execução.

Para criar um ambiente completo com as dependências e configurações desejadas, você pode usar um `Dockerfile` personalizado. Esse Dockerfile permitirá que você adicione bibliotecas específicas e personalize o ambiente do Jupyter Notebook.

## Passo Extra: Usando um Dockerfile Personalizado

Vamos criar um `Dockerfile` para instalar pacotes Python adicionais, como `pandas` e `matplotlib`, que são úteis para análise de dados. Esse Dockerfile também configura um volume padrão para salvar os notebooks e dados.

### Dockerfile

Crie um arquivo chamado `Dockerfile` com o seguinte conteúdo:

```Dockerfile
# Use uma imagem base do Jupyter Notebook com Python
FROM jupyter/base-notebook

# Define o diretório de trabalho padrão no contêiner
WORKDIR /home/jovyan/work

# Copia o arquivo de dados CSV para o contêiner (caso necessário)
# COPY data.csv .

# Instala pacotes adicionais
RUN pip install pandas matplotlib

# Expõe a porta padrão do Jupyter Notebook
EXPOSE 8888
```

### Explicação do Dockerfile

1. **Imagem Base**: Usamos `jupyter/base-notebook`, uma imagem oficial com o Jupyter Notebook e Python.
2. **Diretório de Trabalho**: Definimos o diretório padrão como `/home/jovyan/work`, que é o diretório onde o Jupyter Notebook armazena arquivos.
3. **Instalação de Pacotes**: Instalamos `pandas` e `matplotlib`, pacotes muito usados em ciência de dados e análise de dados.
4. **Exposição de Porta**: Exponha a porta `8888`, que é onde o Jupyter Notebook roda por padrão.

### Construindo e Executando o Contêiner com o Dockerfile

#### 1. Construir a Imagem

No terminal, dentro do diretório onde está o `Dockerfile`, execute o seguinte comando para construir a imagem Docker personalizada:

```bash
docker build -t jupyter-custom .
```

Esse comando cria uma imagem chamada `jupyter-custom` com base no Dockerfile.

#### 2. Executar o Contêiner

Para iniciar o contêiner com a imagem personalizada, execute o seguinte comando na pasta ´example/jupyter´:

```bash
docker run -p 8888:8888 -v ./code:/home/code jupyter-custom
```

### Explicação dos Parâmetros

- **-p 8888:8888**: Mapeia a porta `8888` no contêiner para a porta `8888` na sua máquina local.
- **-v "$PWD":/home/jovyan/work**: Monta o diretório atual (`$PWD`) no sistema local para o diretório de trabalho do contêiner (`/home/jovyan/work`), permitindo persistência dos arquivos.
- **jupyter-custom**: Nome da imagem Docker personalizada que criamos.

#### 3. Acessar o Jupyter Notebook

Após iniciar o contêiner, você verá uma mensagem com uma URL e um token para acessar o Jupyter Notebook. Copie e cole essa URL no navegador:

- Acesse o Jupyter em: `http://localhost:8888`
- Insira o token exibido no terminal, se solicitado.

#### 4. Parar o Contêiner

Para interromper o contêiner, basta pressionar `Ctrl + C` no terminal onde ele está rodando. Se estiver rodando em segundo plano, pare-o com o seguinte comando:

```bash
docker ps  # Lista os contêineres em execução
docker stop <container_id>  # Substitua <container_id> pelo ID do contêiner do Jupyter
```

Com esses passos, você terá um ambiente completo e configurado com o Jupyter Notebook, rodando no Docker e com os pacotes e configurações desejadas!

---

Este exemplo ajuda a configurar rapidamente o Jupyter Notebook com Docker, garantindo um ambiente isolado e fácil de gerenciar. Para mais detalhes e opções, consulte a [documentação oficial do Docker para Jupyter Notebook](https://docs.docker.com/guides/jupyter/).