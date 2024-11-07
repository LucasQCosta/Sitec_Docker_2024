# Configuração de Traefik com Docker

Este repositório contém as instruções e arquivos necessários para configurar o Traefik como um proxy reverso com Docker e Docker Compose. Traefik é uma ferramenta que ajuda a gerenciar o tráfego HTTP e HTTPS para serviços em contêineres, facilitando a configuração de balanceamento de carga, roteamento e outros recursos avançados de rede.

## Requisitos

Antes de começar, certifique-se de ter as seguintes ferramentas instaladas:

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Configuração

Este repositório inclui:

- **docker-compose.yml**: arquivo principal para definir o Traefik e os serviços que ele gerenciará.
- **traefik.toml** ou **traefik.yml**: arquivo de configuração do Traefik para especificar roteamento, certificados TLS, e outras configurações.

### Estrutura de Arquivos

```plaintext
.
├── docker-compose.yml
└── traefik
    └── traefik.yml  (ou traefik.toml)
```

### 1. Configuração do Traefik

O arquivo `traefik.yml` (ou `traefik.toml`) deve conter as definições básicas do Traefik, como:

- Configurações de entrada e saída de rede
- Regras de roteamento para os contêineres
- Certificado SSL/TLS (opcional)

**Exemplo básico de configuração (`traefik.yml`):**

```yaml
entryPoints:
  web:
    address: ":80"
  websecure:
    address: ":443"

providers:
  docker:
    endpoint: "unix:///var/run/docker.sock"
    exposedByDefault: false

certificatesResolvers:
  myresolver:
    acme:
      email: seu-email@exemplo.com
      storage: acme.json
      httpChallenge:
        entryPoint: web
```

### 2. Arquivo `docker-compose.yml`

O `docker-compose.yml` cria o contêiner do Traefik e configura os serviços que ele gerenciará. Um exemplo de configuração básica:

```yaml

services:
  traefik:
    image: traefik:v2.5
    command:
      - "--api.insecure=true"
      - "--providers.docker=true"
      - "--entrypoints.web.address=:80"
      - "--entrypoints.websecure.address=:443"
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock:ro"
      - "./traefik/traefik.yml:/etc/traefik/traefik.yml"
      - "./acme.json:/acme.json"
    networks:
      - web

  app:
    image: nginx
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.app.rule=Host(`app.localhost`)"
      - "traefik.http.services.app.loadbalancer.server.port=80"
    networks:
      - web

networks:
  web:
    external: true
```

### 3. Permissões do arquivo `acme.json`

O Traefik armazena certificados SSL neste arquivo, que deve ter permissões específicas para funcionar corretamente:

```bash
touch acme.json
chmod 600 acme.json
```

### 4. Executando o ambiente

Execute o comando a seguir para iniciar o ambiente com Traefik e o contêiner do serviço:

```bash
docker-compose up -d
```

Este comando iniciará o Traefik como proxy reverso e o serviço configurado. O Traefik estará disponível em `http://localhost` e `https://localhost`, dependendo da configuração.

### 5. Testando a Configuração

- Acesse `http://app.localhost` para verificar o serviço roteado pelo Traefik.
- O painel de administração do Traefik pode estar acessível em `http://localhost:8080` se a opção `--api.insecure=true` estiver ativada (atenção para uso apenas em ambientes de desenvolvimento).

## Recursos Adicionais

Para mais informações e configurações avançadas, consulte a [documentação oficial do Traefik](https://docs.traefik.io/).
