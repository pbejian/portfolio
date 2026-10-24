# Portfolio — infrastructure

Configuration du serveur qui héberge mes applications :
Docker Compose et reverse proxy Caddy (HTTPS automatique via Let's Encrypt).

Chaque application vit dans son propre dépôt, cloné à côté de celui-ci :

    portfolio/
    ├── 00_portfolio/   ← ce dépôt
    └── 01_addition/    ← github.com/pbejian/addition

## Applications en ligne

- Addition : https://addition.bejian.fr