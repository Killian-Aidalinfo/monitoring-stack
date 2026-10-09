# Stack de monitoring : Docker Compose + Caddy

Stack auto-hébergée avec Caddy en reverse proxy et HTTPS automatique (Let's Encrypt) :

| Service | URL | Rôle |
|---|---|---|
| [Uptime Kuma](https://github.com/louislam/uptime-kuma) | `https://kuma.<DOMAIN>` | Supervision de la disponibilité |
| [ntfy](https://ntfy.sh) | `https://ntfy.<DOMAIN>` | Notifications push |
| [whoami](https://github.com/traefik/whoami) | `https://hello.<DOMAIN>` | Service de test |

## Prérequis

- Un serveur avec Docker et le plugin Compose
- Un nom de domaine avec des enregistrements DNS `kuma`, `ntfy` et `hello` (ou un wildcard `*`) qui pointent vers l'IP du serveur
- Les ports 80 et 443 ouverts

## Installation

```bash
git clone https://github.com/Killian-Aidalinfo/monitoring-stack.git
cd monitoring-stack
cp .env.example .env        # puis remplacer example.com par votre domaine
```

Dans `caddy/Caddyfile`, remplacez `admin@example.com` par votre adresse e-mail (Let's Encrypt l'utilise pour ses notifications).

```bash
docker compose up -d
docker compose logs -f caddy   # suivre l'obtention des certificats
```

## ntfy : créer un utilisateur

L'accès est fermé par défaut (`deny-all`). Pour créer un compte administrateur :

```bash
docker exec -it ntfy ntfy user add --role=admin admin
```
