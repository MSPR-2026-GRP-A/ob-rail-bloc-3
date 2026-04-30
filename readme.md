C'est une excellente précision. Travailler avec des **submodules** est très courant pour séparer le code du Frontend et du Backend tout en gardant une orchestration centrale.

Voici le README mis à jour avec une section dédiée à la gestion des submodules et la commande de clonage spécifique.

---

# Stack Docker-Compose (Multi-repo)

Ce projet orchestre une architecture complète en microservices. Le code source du Frontend et du Backend est géré via des **Git Submodules**.

## Architecture Réseau
Pour garantir la sécurité, les réseaux sont segmentés :
*   **`frontend_net`** : Liaison entre Traefik, le Frontend et le Backend.
*   **`backend_net`** : Isolation de la Database (accessible uniquement par le Backend).
*   **`monitor_net`** : Flux dédié au monitoring.

---

## Pré-requis
*   Docker & Docker Compose
*   Git

---

## Installation et Démarrage

### 1. Cloner le projet (avec les submodules)
Comme le Frontend et le Backend sont des dépôts séparés, utilisez cette commande pour tout récupérer d'un coup :
```bash
git clone --recursive <url-du-depot-parent>
cd <nom-du-projet>
```
*Si vous avez déjà cloné sans l'option `--recursive` :*
```bash
git submodule update --init --recursive
```

### 2. Lancer la stack
```bash
docker-compose up -d --build
```
> **Note :** L'option `--build` est importante pour s'assurer que les images sont reconstruites à partir du code actuel de vos submodules.

---

## Travailler avec les Submodules
Lorsque vous modifiez le code dans les dossiers `frontend/` ou `backend/` :

*   **Mettre à jour les sous-modules :** `git submodule update --remote`
*   **Reconstruire un service spécifique :** `docker-compose up -d --build backend`

---

## Notes sur PostgreSQL (v18+)
En cas d'erreur de compatibilité de dossier (`pg_ctlcluster`), réinitialisez le volume :
```bash
docker-compose down -v
docker-compose up -d
```

---

## Accès aux services
*   **Frontend** : /
*   **Backend API** : /
*   **Dashboard Traefik** : /
*   **Monitoring** : /

---

## Commandes utiles
| Action | Commande |
| :--- | :--- |
| Voir les logs | `docker-compose logs -f` |
| Arrêter la stack | `docker-compose stop` |
| Tout supprimer (volumes inclus) | `docker-compose down -v` |

```