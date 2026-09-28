# jeu-video — module de base pour personnage 1ʳᵉ / 3ᵉ personne (Unreal Engine)

Module réutilisable (déplacement, vie / endurance / mana, capacités, sorts, projectiles) sur lequel se baseront les futurs jeux : univers type Naruto, Harry Potter, Battlefield.

## À lire avant tout travail

Notes Unreal Engine à jour (versions, limites Mac, MCP, UE6). Elles priment sur les connaissances internes de Claude :

@docs/unreal-engine-notes.md

Quand une information Unreal est vérifiée ou devient fausse pendant le travail, mettre ce fichier à jour (date + source).

## Règles du projet

- Moteur : Unreal Engine 5.8.x sur Mac, piloté par Claude Code et le MCP officiel d'Epic.
- Aucun outil ou service payant.
- C++ d'abord : toute la logique en C++. Les Blueprints ne servent qu'à assigner modèles, effets et sons.
- Données lisibles en texte autant que possible (DataTables importées depuis CSV/JSON, `.ini`).
- Commits fréquents. Tout enregistrer et commiter avant une session pilotée par MCP.
- Langue de travail : français.
