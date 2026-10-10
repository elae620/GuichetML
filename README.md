# Guichet ML

## Objectif du projet

**Guichet ML** est une API de détection de langue et d'inférence de modèles de Machine Learning. Elle permet de soumettre un texte ou un lot de textes afin d'identifier automatiquement la langue associée avec un score de confiance et une gestion des seuils de révision humaine.

---

## Technologies utilisées

- **Python** (3.10+)
- **FastAPI** : Framework web moderne et rapide pour la création d'APIs.
- **Pydantic** : Validation des données et typage des requêtes / réponses.
- **Uvicorn** : Serveur ASGI haute performance.
- **Pytest** : Framework de tests automatisés.

---

## Installation et lancement

### 1. Cloner ou ouvrir le projet

Placez-vous dans le répertoire racine du projet :
```bash
cd "C:\DSI31\Atelier ML\guichet-ml"
```

### 2. Créer et activer un environnement virtuel

Sous Windows :
```bash
python -m venv .venv
.\.venv\Scripts\activate
```

Sous Linux / macOS :
```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Installer les dépendances

```bash
pip install -r requirements.txt
```

### 4. Configuration de l'environnement

Copiez le fichier d'exemple `.env.example` vers `.env` si ce n'est pas déjà fait :
```bash
copy .env.example .env
```

Variables disponibles :
- `INFERENCE_MODE` : Mode d'inférence (`mock` ou `hf`).
- `HF_TOKEN` : Jeton d'accès Hugging Face (optionnel si mode mock).
- `MODEL_ID` : Modèle utilisé (par exemple `papluca/xlm-roberta-base-language-detection`).
- `REVIEW_THRESHOLD` : Seuil de confiance en dessous duquel une révision est requise (ex: `0.60`).

### 5. Démarrer le serveur

Lancez l'application avec Uvicorn :
```bash
uvicorn app.main:app --reload
```

L'API est alors accessible sur :
- **API** : http://127.0.0.1:8000
- **Documentation interactive Swagger** : http://127.0.0.1:8000/docs
- **Documentation ReDoc** : http://127.0.0.1:8000/redoc

---

## Routes disponibles

| Méthode | Route | Description |
|---|---|---|
| `GET` | `/health` | Vérifie l'état de l'API, le mode actif et le fournisseur d'inférence. |
| `POST` | `/detect-language` | Détecte la langue d'un texte unique. |
| `POST` | `/detect-language/batch` | Détecte les langues d'un lot de plusieurs textes. |

---

## Exemple de requête pour le traitement Batch

Pour analyser plusieurs textes en une seule requête sur la route `POST /detect-language/batch` :

### Corps de la requête (JSON)

```json
{
  "texts": [
    {"text": "Bonjour, comment allez-vous ?"},
    {"text": "Hello, how are you?"}
  ]
}
```

### Exemple avec `curl`

```bash
curl -X POST "http://127.0.0.1:8000/detect-language/batch" \
     -H "Content-Type: application/json" \
     -d "{\"texts\": [{\"text\": \"Bonjour, comment allez-vous ?\"}, {\"text\": \"Hello, how are you?\"}]}"
```

---

## Exécution des tests

Pour lancer la suite de tests automatisés avec pytest :

```bash
python -m pytest -q
```
