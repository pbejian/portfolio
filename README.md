# Portfolio — infrastructure

Configuration du serveur qui héberge les applications de mon portfolio : un VPS OVH sous Ubuntu, avec Docker Compose et le reverse proxy Caddy (HTTPS automatique via Let's Encrypt).

- **Portfolio :** https://www.bejian.fr

## Architecture

```
Internet ──► Caddy (ports 80 et 443, HTTPS)
               ├──► conteneur app A (réseau Docker interne)
               ├──► conteneur app B
               └──► …
```

- Chaque application tourne dans son propre conteneur Docker, construit à partir de son dépôt GitHub.
- Seul Caddy est exposé sur Internet ; les applications ne sont joignables que par le réseau Docker interne.
- Chaque application a son sous-domaine de `bejian.fr` ; Caddy obtient et renouvelle les certificats tout seul.
- Les conteneurs redémarrent automatiquement (`restart: unless-stopped`).

## Organisation sur le serveur

Chaque application vit dans son propre dépôt, cloné à côté de celui-ci :

```
portfolio/
├── 00_portfolio/   ← ce dépôt (docker-compose.yml, Caddyfile)
├── XY_app_a/       ← dépôt d'une application
└── XY_app_b/
```

## Fichiers

| Fichier | Rôle |
|---|---|
| `docker-compose.yml` | Déclare Caddy et un service par application (build depuis le dossier voisin). |
| `Caddyfile` | Associe chaque sous-domaine au conteneur de l'application. |

## Ajouter une application

1. Cloner le dépôt de l'application à côté de `00_portfolio/`.
2. Ajouter un service dans `docker-compose.yml` (`build: ../XY_nom`).
3. Ajouter un bloc dans le `Caddyfile` : `nom.bejian.fr { reverse_proxy nom:8501 }`.
4. Créer l'enregistrement DNS du sous-domaine vers l'IP du VPS.
5. Lancer `docker compose up -d --build`.

## Mettre à jour une application

```bash
cd ../XY_nom && git pull
cd ../00_portfolio && docker compose up -d --build nom
```

## Stack

Ubuntu · Docker · Docker Compose · Caddy · Let's Encrypt · VPS OVH

---

Pierre BÉJIAN · [www.bejian.fr](https://www.bejian.fr)
