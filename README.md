# Portfolio — infrastructure

Configuration du serveur qui héberge mes applications :
Docker Compose, et bientôt le reverse proxy Caddy (HTTPS).

Chaque application vit dans son propre dépôt, cloné à côté de celui-ci :

    portfolio/
    ├── 00_portfolio/   ← ce dépôt
    └── 01_addition/    ← github.com/pbejian/addition