```
version: '3.8'
services:
  mcp-server:
    image: ghcr.io/modelcontextprotocol/server:latest
    container_name: mcp-server
    restart: unless-stopped
    ports:
      - "3000:3000"
    environment:
      - MCP_CONFIG=/app/config.json
    volumes:
      - ./mcp-config.json:/app/config.json
```

```
{
  "tools": [
    {
      "name": "obsidian-couchdb",
      "type": "http",
      "endpoint": "http://host.docker.internal:5984",
      "auth": {
        "type": "basic",
        "username": "admin",
        "password": "your_couchdb_password"
      }
    }
  ]
}
```
 docker compose up -d
WARN[0000] /home/f4blox/Desktop/MCP/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
unable to get image 'ghcr.io/modelcontextprotocol/server:latest': permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.50/images/ghcr.io/modelcontextprotocol/server:latest/json": dial unix /var/run/docker.sock: connect: permission denied


 docker compose up -d
unable to get image 'ghcr.io/modelcontextprotocol/server:latest': permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.50/images/ghcr.io/modelcontextprotocol/server:latest/json": dial unix /var/run/docker.sock: connect: permission denied

```
# 1) Vérifier que le daemon tourne
systemctl status docker --no-pager

# 2) Ajouter ton user au groupe docker
sudo usermod -aG docker "$USER"

# 3) Recharger la session pour prendre en compte le nouveau groupe
newgrp docker

# 4) Vérifier que l’accès fonctionne sans sudo
docker info
docker run --rm hello-world
```