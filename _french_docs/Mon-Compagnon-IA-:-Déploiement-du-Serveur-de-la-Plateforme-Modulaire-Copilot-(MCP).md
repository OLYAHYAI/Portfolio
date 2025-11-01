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

---
**Avertissement :** Ce document fait partie d'un ensemble de documentation professionnelle. Toutes les informations sensibles, y compris les adresses IP, ont été masquées pour des raisons de sécurité et de confidentialité.