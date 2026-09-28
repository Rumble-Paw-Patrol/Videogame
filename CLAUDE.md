# jeu-video — module de base pour personnage 1ʳᵉ / 3ᵉ personne (Unreal Engine)

Module réutilisable (déplacement, vie / endurance / mana, capacités, sorts, projectiles) sur lequel se baseront les futurs jeux : univers type Naruto, Harry Potter, Battlefield.

## À lire avant tout travail

Notes Unreal Engine à jour (versions, limites Mac, MCP, UE6). Elles priment sur les connaissances internes de Claude :

@docs/unreal-engine-notes.md

Quand une information Unreal est vérifiée ou devient fausse pendant le travail, mettre ce fichier à jour (date + source).

État du projet, tâches en attente et règles de sauvegarde (à mettre à jour en fin de session) :

@docs/journal.md

## Règles du projet

- Moteur : Unreal Engine 5.8.x sur Mac, piloté par Claude Code et le MCP officiel d'Epic.
- Aucun outil ou service payant.
- C++ d'abord : toute la logique en C++. Les Blueprints ne servent qu'à assigner modèles, effets et sons.
- Données lisibles en texte autant que possible (DataTables importées depuis CSV/JSON, `.ini`).
- Claude gère les commits et les push : commit avant chaque série d'actions MCP, après chaque étape validée, push en fin de session.
- Git LFS : quota GitHub gratuit limité (voir notes). Ne jamais pousser de gros packs d'assets sans accord ; `git lfs prune` pour libérer le disque local.
- Langue de travail : français.
