```
version: "3.8"

services:
  # MCP Inspector (UI pour tester et déboguer les serveurs MCP)
  mcp-inspector:
    image: ghcr.io/modelcontextprotocol/inspector:latest
    container_name: mcp-inspector
    restart: unless-stopped
    ports:
      - "6274:6274"   # UI
      - "6277:6277"   # Proxy
    environment:
      - CLIENT_PORT=6274
      - SERVER_PORT=6277
      - DANGEROUSLY_OMIT_AUTH=true

  # MCP Filesystem (expose ton vault Obsidian via MCP)
  mcp-filesystem:
    image: ghcr.io/modelcontextprotocol/filesystem:latest
    container_name: mcp-filesystem
    restart: unless-stopped
    ports:
      - "6280:6280"   # API MCP Filesystem
    volumes:
      - /chemin/vers/ton/vault:/vault:ro   # read-only
    environment:
      - ROOT=/vault
      - PORT=6280

  # Qdrant (optionnel pour stocker embeddings)
  qdrant:
    image: qdrant/qdrant:latest
    container_name: qdrant
    restart: unless-stopped
    ports:
      - "6333:6333"
    environment:
      - QDRANT_API_KEY=localtest
    volumes:
      - qdrant_storage:/qdrant/storage

volumes:
  qdrant_storage:
```

$ docker compose pull
[+] Pulling 3/3
 ! mcp-inspector        Interrupted                                      0.5s 
 ✘ mcp-filesystem Error Head "https://ghcr.io/v...                       0.5s 
 ! qdrant               Interrupted                                      0.5s 
Error response from daemon: Head "https://ghcr.io/v2/modelcontextprotocol/filesystem/manifests/latest": denied


```
mcp-filesystem:
  image: ghcr.io/mcp-containers/filesystem:latest
```

$ docker compose pull
[+] Pulling 3/3
 ✘ mcp-inspector Error Head "https://ghcr.io/v2/...                      0.8s 
 ! mcp-filesystem      Interrupted                                       0.8s 
 ! qdrant              Interrupted                                       0.8s 
Error response from daemon: Head "https://ghcr.io/v2/mcp-containers/filesystem/manifests/latest": denied

```

services:
  mcp-inspector:
    image: ghcr.io/modelcontextprotocol/inspector:latest
    container_name: mcp-inspector
    restart: unless-stopped
    ports:
      - "6274:6274"
      - "6277:6277"
    environment:
      - CLIENT_PORT=6274
      - SERVER_PORT=6277
      - DANGEROUSLY_OMIT_AUTH=true

  mcp-filesystem:
    image: ghcr.io/mcp-containers/filesystem:latest
    container_name: mcp-filesystem
    restart: unless-stopped
    ports:
      - "6280:6280"
    volumes:
      - /chemin/vers/ton/vault:/vault:ro
    environment:
      - ROOT=/vault
      - PORT=6280

  qdrant:
    image: qdrant/qdrant:latest
    container_name: qdrant
    restart: unless-stopped
    ports:
      - "6333:6333"
    environment:
      - QDRANT_API_KEY=localtest
    volumes:
      - qdrant_storage:/qdrant/storage

volumes:
  qdrant_storage:
```

# Pull MCP Inspector

docker pull ghcr.io/modelcontextprotocol/inspector:latest

  

# Pull MCP Filesystem

docker pull mcp/filesystem:latest




```
version: "3.8"

services:
  mcp-inspector:
    image: ghcr.io/modelcontextprotocol/inspector:latest
    container_name: mcp-inspector
    restart: unless-stopped
    ports:
      - "6274:6274"
      - "6277:6277"
    environment:
      - CLIENT_PORT=6274
      - SERVER_PORT=6277
      - DANGEROUSLY_OMIT_AUTH=true

  mcp-filesystem:
    image: mcp/filesystem:latest
    container_name: mcp-filesystem
    restart: unless-stopped
    ports:
      - "6280:6280"
    volumes:
      - /chemin/vers/ton/vault:/vault:ro
    environment:
      - ROOT=/vault
      - PORT=6280

  qdrant:
    image: qdrant/qdrant:latest
    container_name: qdrant
    restart: unless-stopped
    ports:
      - "6333:6333"
    environment:
      - QDRANT_API_KEY=localtest
    volumes:
      - qdrant_storage:/qdrant/storage

volumes:
  qdrant_storage:
```

# Pull MCP Inspector

docker pull ghcr.io/modelcontextprotocol/inspector:latest

  

# Pull MCP Filesystem

docker pull mcp/filesystem:latest