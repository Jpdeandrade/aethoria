# Infraestrutura — Aethoria

Documentação técnica da VPS, stack de deploy e manutenção.

---

## Servidor

| Item | Valor |
|---|---|
| IP | 46.224.29.188 |
| OS | Linux |
| Acesso SSH | `ssh -i ~/.ssh/notebook root@46.224.29.188` |
| Chave SSH | `~/.ssh/notebook` (local) |

---

## Stack

```
Docker Swarm
  └── Traefik v3.5.3         (reverse proxy + SSL automático)
  └── Portainer               (painel de gerenciamento)
  └── aethoria_aethoria       (container Nginx do site)
```

---

## Estrutura de pastas na VPS

```
/root/sites/
  aethoria.online/
    html/
      index.html              ← landing page
    docker-compose.yml
```

---

## docker-compose.yml (aethoria.online)

```yaml
version: '3.8'
services:
  aethoria:
    image: nginx:alpine
    volumes:
      - ./html:/usr/share/nginx/html:ro
    networks:
      - traefik-public
    deploy:
      labels:
        - traefik.enable=true
        - traefik.http.routers.aethoria.rule=Host(`aethoria.online`) || Host(`www.aethoria.online`)
        - traefik.http.routers.aethoria.entrypoints=websecure
        - traefik.http.routers.aethoria.tls.certresolver=letsencrypt
        - traefik.http.services.aethoria.loadbalancer.server.port=80

networks:
  traefik-public:
    external: true
```

---

## Deploy de atualização (workflow atual)

```bash
# 1. Enviar arquivo atualizado
scp -i ~/.ssh/notebook aethoria-landing-page.html \
  root@46.224.29.188:/root/sites/aethoria.online/html/index.html

# 2. Reiniciar o serviço
ssh -i ~/.ssh/notebook root@46.224.29.188 \
  "docker service update --force aethoria_aethoria"
```

---

## Deploy de novo site (script)

Na VPS existe o script `/root/novo-site.sh` que cria a estrutura e o `docker-compose.yml` automaticamente.

**Atenção:** O script tem um bug de escape nas labels do Traefik. Se o novo site não responder, verificar o `docker-compose.yml` gerado e corrigir as labels manualmente com backticks nos hostnames.

---

## Comandos úteis na VPS

```bash
# Ver serviços rodando
docker service ls

# Ver logs do serviço
docker service logs aethoria_aethoria

# Forçar update (reload sem mudança de imagem)
docker service update --force aethoria_aethoria

# Ver containers ativos
docker ps
```

---

## Integrações externas

| Serviço | URL | Uso |
|---|---|---|
| Webhook (n8n) | `https://webhook.infrajp.xyz/webhook/cba6d127-...` | Recebe cadastros do formulário da LP |
| Google Fonts | CDN | Cinzel + Crimson Text |
| Let's Encrypt | Via Traefik | SSL automático |
