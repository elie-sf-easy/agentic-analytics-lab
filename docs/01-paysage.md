# Paysage de l'agentic analytics

Notes de cadrage (octobre 2026) : les notions clés et les familles d'outils.

## 1. Agent IA

Un **agent** est un LLM qui dispose d'outils et qui tourne en boucle : il réfléchit, appelle un outil, lit le résultat, corrige si besoin, et recommence jusqu'à avoir une réponse.

| Étape | Exemple : « CA du mois dernier par pays » |
|---|---|
| Réfléchir | Il me faut les ventes et le pays du client |
| Appeler un outil | Liste les tables, lit le schéma de `order_items` et `users` |
| Agir | Écrit et lance le SQL |
| Lire le résultat | Erreur « colonne inconnue » |
| Recommencer | Corrige, relance, répond |

## 2. Text-to-SQL et ses échecs classiques

Le **text-to-SQL** traduit une question en langage naturel en requête SQL. Les échecs typiques :

| Échec | Exemple sur thelook |
|---|---|
| **Mauvaise jointure** | Mauvaise clé entre `orders` et `users`, produit cartésien |
| **Grain raté** | Joindre `orders` à `order_items` puis sommer un montant de niveau commande : CA multiplié |
| **Mauvaise définition de métrique** | CA qui inclut les commandes `Cancelled` et `Returned` |
| **Filtre oublié** | Mauvaise date de référence, statut non filtré, fuseau horaire |

**Le danger principal : la requête tourne sans erreur et renvoie un chiffre plausible mais faux.** D'où la nécessité de mesurer la justesse sur des questions de référence.

## 3. Couche sémantique

Les **définitions métier écrites une seule fois**, dans du code versionné : métriques, dimensions, jointures autorisées. L'outil génère le SQL à partir de ces définitions.

- Exemples : MetricFlow (dbt Semantic Layer), LookML (Looker), modèle Omni, Cube.
- Pour un agent : il demande la métrique `revenue` par `country` au lieu d'inventer le SQL. Moins de liberté, donc moins d'erreurs.

## 4. MCP (Model Context Protocol)

Standard ouvert (Anthropic, fin 2024) pour **brancher un LLM à des outils** via un « serveur MCP ».

- **dbt-mcp** : modèles, documentation, lineage, commandes dbt, métriques.
- **MCP Toolbox for Databases** (Google) : lister les tables et exécuter du SQL sur BigQuery.
- L'agent hérite des droits du compte utilisé : **lecture seule et limite de coût** se règlent à ce niveau.

## 5. Contexte

Tout ce que l'agent voit : schémas, noms de colonnes, descriptions, tests, métriques, fichier de consignes, exemples de requêtes. **Construire ce contexte est le travail de l'Analytics Engineer**, et c'est ce que ce labo mesure.

## 6. Les trois familles d'outils

| Famille | Pour qui | Ce que ça fait | Exemples |
|---|---|---|---|
| **BI avec IA** | Le métier | Chat sur des données gouvernées par une couche sémantique | Omni, Looker conversational analytics, Copilot Power BI, Hex |
| **IDE et agents de code** | L'AE | Écrire, tester et documenter des modèles plus vite | nao, Claude Code + dbt-mcp, Cursor |
| **IA native de l'entrepôt** | Les deux | Agents directement dans l'entrepôt | Gemini et data agents BigQuery, Snowflake Cortex Analyst, Databricks Genie |

**Comment choisir** : qui pose la question (métier ou équipe data) et où vivent les définitions métier. Pour une organisation qui part de zéro, la priorité reste la même quel que soit l'outil : des modèles propres et documentés.

## Sources

- Omni, levée de série C (avril 2026) : https://omni.co/blog/press-release-omni-series-c-funding
- nao Labs (YC X25) : https://linkedin.com/company/getnao
- Google Cloud, data agents et Conversational Analytics API : https://cloud.google.com/blog/products/data-analytics/build-data-agents-with-conversational-analytics-api
