# Videogame — module CharacterKit pour jeux multijoueur en 1ʳᵉ / 3ᵉ personne

Module Unreal réutilisable (déplacement, vie / endurance / énergie, capacités, sorts, projectiles, combat) sur lequel se bâtiront des jeux **multijoueur** : d'abord deux arènes 1v1 pilotes (univers shinobi, univers sorcier), puis un tireur.
Univers **inspirés de** grandes œuvres, jamais copiés : aucun nom, personnage, logo ou asset protégé.

## À lire avant tout travail (chargé automatiquement)

Notes Unreal à jour — elles priment sur les connaissances internes de Claude :
@docs/unreal-engine-notes.md

Décisions d'architecture — à respecter, ou à rediscuter explicitement :
@docs/architecture.md

État du projet, tâches en cours, règles de sauvegarde :
@docs/journal.md

La feuille de route complète est dans `docs/feuille-de-route.md` : la lire en début de phase ou pour choisir la prochaine tâche.

## Règles du projet

- Unreal Engine 5.8.x sur Mac, piloté par Claude Code et le MCP officiel d'Epic. Aucun outil ou service payant.
- **Multijoueur toujours** : serveur-autoritaire, prédiction client ; chaque fonctionnalité testée à 2 joueurs avec latence simulée.
- **C++ d'abord** : toute la logique en C++. Blueprints = coquilles fines (modèles, effets, sons, valeurs).
- Données en texte autant que possible (GameplayTags en `.ini`, DataTables depuis CSV/JSON).
- Le module (`Plugins/CharacterKit`) ne contient rien de propre à un univers.
- Avancer par petites étapes, une couche à la fois, dans l'ordre de la feuille de route. Léonard valide le ressenti de jeu.

## Méthode de travail de Claude

- **Compilation C++** : pas de Live Coding sur Mac. Regrouper les modifications, fermer l'éditeur, compiler avec `scripts/`, relancer. Claude s'en charge.
- **MCP** : commit juste avant toute série d'actions qui modifie des assets ; après une modification de Blueprint, le compiler et vérifier les erreurs ; relire tout script Python qui touche beaucoup d'assets.
- **Ne jamais modifier un `.uasset` / `.umap` à la main.** Passer par le MCP ou par l'éditeur.
- **Commits** : Claude commite et pousse lui-même. Le code, aussi souvent qu'utile. Les fichiers lourds (LFS), seulement quand une étape est validée : chaque version commitée compte pour toujours dans le quota. Jamais de pack d'assets tiers non modifié dans git.
- **Fin de session** : mettre à jour `docs/journal.md`, cocher la feuille de route, pousser.
- **Notes Unreal** : quand une info est vérifiée ou devient fausse, mettre à jour `docs/unreal-engine-notes.md` (date + source).
- Langue de travail : français. Identifiants de code en anglais, commentaires en français.
