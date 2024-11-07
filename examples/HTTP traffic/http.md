# Guia Rápido de HTTP Routing com Traefik e Docker

#### Introdução
O Traefik é um proxy reverso e balanceador de carga que facilita o roteamento entre múltiplos serviços HTTP em desenvolvimento, permitindo que diferentes contêineres ou workloads não-containerizados sejam acessíveis a partir de URLs específicas.

#### Pré-requisitos
1. Docker
2. Node.js e Yarn
3. Conhecimento básico de Docker

#### Configurando o Traefik com Docker
1. **Crie uma rede para os contêineres se comunicarem:**
   ```bash
   docker network create traefik-demo
   ```

2. **Inicie o Traefik:**
   ```bash
   docker run -d --network=traefik-demo -p 80:80 -v /var/run/docker.sock:/var/run/docker.sock traefik:v3.1.2 --providers.docker
   ```

3. **Inicie um contêiner Nginx com labels para o Traefik:**
   ```bash
   docker run -d --network=traefik-demo --label 'traefik.http.routers.nginx.rule=Host(`nginx.localhost`)' nginx
   ```

4. **Inicie outro contêiner com hostname diferente:**
   ```bash
   docker run -d --network=traefik-demo --label 'traefik.http.routers.welcome.rule=Host(`welcome.localhost`)' docker/welcome-to-docker
   ```
   
#### Recapitulando
Este guia proporciona um ambiente de desenvolvimento eficiente, roteando diferentes serviços usando o Traefik e o Docker, facilitando o acesso a múltiplos serviços em URLs específicas.