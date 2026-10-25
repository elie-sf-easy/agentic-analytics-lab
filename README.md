# Agentic Analytics Lab

Projet personnel d'Analytics Engineering : mesurer et améliorer la justesse d'agents IA qui interrogent des données, en travaillant la modélisation dbt et la couche sémantique.

> **Fil rouge** : un agent donne de bonnes réponses quand le contexte est bon (modèles propres, grain explicite, descriptions, tests, métriques définies). Construire ce contexte, c'est le travail de l'Analytics Engineer.

## Stack

| Brique | Choix |
|---|---|
| Entrepôt | BigQuery (datasets `lab_*`, région US) |
| Données | `bigquery-public-data.thelook_ecommerce` (e-commerce fictif) |
| Transformation | dbt Core 1.12 + dbt-bigquery (versions figées dans `requirements.txt`) |
| Couche sémantique | MetricFlow (à venir) |
| Agents testés | Claude + MCP dbt / BigQuery, nao, Omni, IA native BigQuery (à venir) |
| CI | GitHub Actions (à venir) |

## Avancement

| Étape | Statut |
|---|---|
| Paysage et setup | ✅ |
| Staging | ⏳ |
| Marts documentés | ⏳ |
| Couche sémantique + 20 questions de référence | ⏳ |
| Agents via MCP | ⏳ |
| Mesure de départ | ⏳ |
| Amélioration du contexte (avant / après) | ⏳ |
| CI/CD | ⏳ |

## Lancer le projet en local

```bash
python -m venv .venv
# Windows : .\.venv\Scripts\Activate.ps1   |   macOS/Linux : source .venv/bin/activate
pip install -r requirements.txt
dbt debug
```

Le profil dbt (`agentic_analytics_lab`) reste dans `~/.dbt/profiles.yml`, hors du repo. Il fixe `location: US` et une limite `maximum_bytes_billed` par requête.

## Documentation

- [Paysage de l'agentic analytics](docs/01-paysage.md)
