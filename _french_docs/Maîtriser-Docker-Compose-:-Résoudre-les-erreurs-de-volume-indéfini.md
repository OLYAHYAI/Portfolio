~/Desktop/MCP$ docker ps
CONTAINER ID   IMAGE                                           COMMAND                  CREATED         STATUS                          PORTS                                                                                      NAMES
4fc78fc513c7   qdrant/qdrant:latest                            "./entrypoint.sh"        4 minutes ago   Up 4 minutes                    [REDACTED_IP]:6333->6333/tcp, [REDACTED_IP]:6333->6333/tcp, 6334/tcp                                      qdrant
50705ccfc8ec   mcp/filesystem:latest                           "node /app/dist/inde…"   4 minutes ago   Restarting (1) 25 seconds ago                                                                                              mcp-filesystem
02bb602d4ded   ghcr.io/modelcontextprotocol/inspector:latest   "npm start"              4 minutes ago   Up 4 minutes                    [REDACTED_IP]:6274->6274/tcp, [REDACTED_IP]:6274->6274/tcp, [REDACTED_IP]:6277->6277/tcp, [REDACTED_IP]:6277->6277/tcp   mcp-inspector
883abdbb0ddd   jc21/nginx-proxy-manager:latest                 "/init"                  18 hours ago    Up 2 hours                      [REDACTED_IP]:80-81->80-81/tcp, [REDACTED_IP]:80-81->80-81/tcp, [REDACTED_IP]:443->443/tcp, [REDACTED_IP]:443->443/tcp   desktop-app-1
cfb9c5791ca9   n8nio/n8n:latest                                "tini -- /docker-ent…"   18 hours ago    Up 2 hours                      [REDACTED_IP]:5678->5678/tcp, [REDACTED_IP]:5678->5678/tcp                                                n8n-n8n-1
5e298fcb1438   couchdb:latest                                  "tini -- /docker-ent…"   40 hours ago    Up 2 hours                      4369/tcp, 9100/tcp, [REDACTED_IP]:5984->5984/tcp, [REDACTED_IP]:5984->5984/tcp

---
**Avertissement :** Ce document fait partie d'un ensemble de documentation professionnelle. Toutes les informations sensibles, y compris les adresses IP, ont été masquées pour des raisons de sécurité et de confidentialité.