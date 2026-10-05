# API Jeux

API REST du catalogue de jeux, développée en Python avec FastAPI et SQLAlchemy.

## À propos

Cette application expose un catalogue de jeux avec:

- la liste, la recherche et la pagination des jeux,
- les filtres par genre, note et année,
- la gestion des éditeurs et de leurs jeux,
- l'authentification, les rôles et les droits d'accès,
- l'historique des modifications.

## Prérequis

- Python 3.12+
- pip
- PostgreSQL ou SQLite pour les tests (le projet est compatible SQLite pour les tests automatisés et PostgreSQL pour le fonctionnement standard)

## Installation

1. Cloner le dépôt.
2. Créer un environnement virtuel.
3. Installer les dépendances.

```bash
python -m venv .venv
. .venv\Scripts\activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m pip install -r requirements-dev.txt
```

## Configuration

Copier le fichier d'exemple et compléter la configuration locale :

```bash
copy .env.example .env
```

Variables attendues :

- `DATABASE_URL` : chaîne de connexion SQLAlchemy
- `CLE_SECRETE` : clé secrète utilisée pour signer les jetons JWT
- `ENVIRONNEMENT` : `developpement`, `test` ou `production`
- `ORIGINES_AUTORISEES` : origins CORS autorisées
- `DUREE_JETON_MINUTES`, `NIVEAU_JOURNAL`, etc.

Exemple d'environnement de développement :

```env
DATABASE_URL=sqlite:///jeux.db
CLE_SECRETE=remplacez_par_une_cle_generique
ENVIRONNEMENT=developpement
ORIGINES_AUTORISEES=http://localhost:5173
```

## Lancer l'API

### Avec Uvicorn directement

```bash
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

### Avec Docker Compose

```bash
docker compose up --build
```

L'API est ensuite disponible sur :

- http://localhost:8000
- documentation Swagger : http://localhost:8000/docs
- documentation Redoc : http://localhost:8000/redoc

## Utilisation

### Endpoints principaux

- `GET /api/v1/jeux` : lister les jeux avec filtres, tri et pagination
- `GET /api/v1/jeux/{id}` : lire un jeu
- `POST /api/v1/jeux` : créer un jeu
- `PATCH /api/v1/jeux/{id}` : modifier partiellement un jeu
- `DELETE /api/v1/jeux/{id}` : supprimer un jeu
- `GET /api/v1/jeux/statistiques` : statistiques du catalogue
- `GET /api/v1/editeurs` : lister les éditeurs
- `POST /api/v1/connexion` : connexion utilisateur

Exemple de requête d'authentification :

```bash
curl -X POST http://localhost:8000/api/v1/connexion \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "username=alice@example.com&password=motdepasse123"
```

## Tests

```bash
python -m pytest -q
```

Pour le linting :

```bash
python -m ruff check .
```

## Architecture

```text
app/
├── base_donnees.py       # moteur SQLAlchemy et session
├── config.py             # configuration via pydantic-settings
├── dependances.py        # dépendances FastAPI (session, utilisateur, pagination)
├── exceptions.py         # exceptions métier
├── journalisation.py     # configuration logging
├── main.py               # assemblage de l'application, middlewares et gestionnaires
├── modeles/              # schémas Pydantic de validation et sérialisation
├── routeurs/             # endpoints FastAPI
├── securite.py           # JWT, hachage et sécurité
├── services/             # logique métier
├── tables/               # modèles SQLAlchemy
└── __init__.py
```

## Sécurité

- Les mots de passe sont hachés avant stockage.
- Les jetons JWT sont vérifiés selon les règles configurées.
- Les accès sont contrôlés par rôle et par propriété du jeu.
- Les codes d'erreur HTTP sont normalisés pour fournir une API cohérente.

## Contribution

1. Créer une branche dédiée pour la fonctionnalité ou la correction.
2. Appliquer les changements avec des commits lisibles.
3. Ouvrir une pull request avec contexte, impact et procédure de test.
4. Valider la revue avant fusion.
