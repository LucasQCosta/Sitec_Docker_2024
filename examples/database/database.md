# Pré-carregamento de Imagens Docker em um Dispositivo (Pre-seeding)

Este guia ensina a configurar e executar o pré-carregamento (pre-seeding) de imagens Docker em um dispositivo, como um servidor ou dispositivo IoT, antes de ele ser enviado ao destino final. Essa prática permite que os dispositivos iniciem rapidamente com as imagens Docker já armazenadas em cache, economizando largura de banda e tempo ao evitar downloads iniciais.

## Visão Geral

O pré-seeding de imagens Docker pode ser útil para cenários onde um dispositivo será enviado para locais com conexão de rede limitada ou quando o tempo de inicialização precisa ser reduzido.

### Objetivo

Neste tutorial, vamos:
1. Baixar as imagens necessárias usando `docker pull`.
2. Exportar as imagens em um arquivo `.tar` usando `docker save`.
3. Carregar as imagens no dispositivo de destino com `docker load`.

## Pré-requisitos

Certifique-se de ter o Docker instalado em seu ambiente local e no dispositivo de destino. Para instalar o Docker, consulte o [Guia de Instalação do Docker](https://docs.docker.com/get-docker/).

## Passo a Passo

### Passo 1: Baixar as Imagens Necessárias

Para pré-carregar as imagens, você precisa primeiro baixá-las no seu ambiente local ou no ambiente de preparação.

Use o comando `docker pull` para baixar cada imagem necessária. Por exemplo:

```bash
docker pull nginx:latest
docker pull redis:alpine
docker pull postgres:latest
```

### Passo 2: Exportar as Imagens para um Arquivo `.tar`

Após o download, exporte as imagens para um arquivo `.tar` usando o comando `docker save`. Isso permitirá que você copie todas as imagens como um único arquivo para o dispositivo de destino.

```bash
docker save -o images.tar nginx:latest redis:alpine postgres:latest
```

Este comando cria um arquivo chamado `images.tar` contendo as três imagens.

### Passo 3: Transferir o Arquivo `.tar` para o Dispositivo de Destino

Copie o arquivo `images.tar` para o dispositivo onde deseja pré-carregar as imagens. A transferência pode ser feita via SCP, USB, ou qualquer método de sua preferência.

Exemplo de comando SCP para transferência para um dispositivo remoto:

```bash
scp images.tar usuario@dispositivo:/caminho/destino/
```

### Passo 4: Carregar as Imagens no Dispositivo de Destino

Após a transferência, faça login no dispositivo de destino e execute o comando `docker load` para importar as imagens:

```bash
docker load -i /caminho/destino/images.tar
```

Este comando carrega todas as imagens contidas no arquivo `images.tar` para o cache do Docker do dispositivo de destino. Assim, quando um contêiner for iniciado a partir dessas imagens, ele não precisará fazer download.

### Passo 5: Verificar as Imagens Carregadas

Para garantir que as imagens foram carregadas corretamente, você pode listar as imagens no dispositivo de destino com o comando:

```bash
docker images
```

O comando exibirá uma lista das imagens disponíveis localmente no dispositivo. As imagens `nginx:latest`, `redis:alpine`, e `postgres:latest` devem estar listadas.

### Exemplo Completo de Script para Pré-seeding

Você também pode automatizar o processo de pré-seeding usando um script Bash. Este script cobre o processo de download, exportação e carregamento das imagens:

```bash
#!/bin/bash

# Define as imagens
IMAGES=("nginx:latest" "redis:alpine" "postgres:latest")

# Baixa as imagens
for IMAGE in "${IMAGES[@]}"; do
    docker pull "$IMAGE"
done

# Salva as imagens em um arquivo .tar
docker save -o images.tar "${IMAGES[@]}"

# Carrega as imagens no dispositivo de destino
# docker load -i /caminho/destino/images.tar  # Comente ou ajuste para uso no dispositivo de destino

echo "Processo de pré-seeding concluído com sucesso."
```

> **Nota:** Execute o comando `docker load` no dispositivo de destino após transferir o arquivo `images.tar`.

## Considerações Adicionais

- **Espaço em Disco:** Certifique-se de que o dispositivo de destino possui espaço suficiente para armazenar as imagens.
- **Automatização:** Este processo pode ser adicionado a pipelines de CI/CD para preparar dispositivos antes de enviá-los.
- **Rede Limitada:** Em ambientes de rede limitada, o pré-seeding pode reduzir significativamente o tempo de implantação.

## Recursos

Para mais informações e detalhes avançados sobre o pré-seeding de imagens Docker, consulte a [documentação oficial da Docker](https://docs.docker.com/guides/pre-seeding/).
