# Unreal Engine — notes à jour (mémoire projet)

> **À lire au début de chaque session de travail sur un projet Unreal.**
> Ce fichier complète et corrige les connaissances de Claude, arrêtées vers juin 2026.
> En cas de conflit entre ce fichier et la mémoire de Claude, **ce fichier fait foi**.
>
> Dernière vérification complète : **2026-09-28**

## Règles de tenue du fichier

- Chaque information porte sa date de vérification et, si possible, sa source (liste en bas).
- `[vérifié]` = lu dans une source officielle ou fiable. `[à valider]` = déduit ou non testé sur cette machine.
- Quand une info devient fausse : la corriger et le noter dans le journal en bas (ne pas la laisser traîner).
- Revérifier sur le web quand : un nouveau hotfix sort, une erreur de compilation est inexpliquée, un outil MCP échoue ou a changé de nom.

---

## 1. Versions et calendrier

| Élément | Info | Statut |
|---|---|---|
| UE 5.8.0 | Sortie le 17 juin 2026 | [vérifié] |
| Hotfix 5.8.1 | 28 juillet 2026 (~260 correctifs) | [vérifié] |
| Hotfix 5.8.2 | 25 août 2026 | [vérifié] |
| Hotfix 5.8.3 | 22 septembre 2026 — **version cible du projet** | [vérifié] |
| UE 5.9 | Pas planifiée ; Epic se réserve le droit d'en sortir une « si nécessaire ». 5.8 = dernière version UE5 prévue, maintenue en correctifs | [vérifié] |
| UE 6 Early Access | Fin 2027 | [vérifié] |
| UE 6 version complète | 12 à 18 mois après l'Early Access (2028–2029) | [vérifié] |

## 2. Machine de développement (au 2026-09-28)

- Mac puce **M4**, **16 Go** de RAM (minimum requis par UE 5.8 ; 32 Go recommandés).
- macOS **26.6.2** (Tahoe). Xcode installé : **26.4.1** ⚠️ (voir §3).
- Disque 228 Go, **~34 Go libres**. Réglage iCloud « Optimiser le stockage du Mac » **activé**.
- Moteur installé : **UE 5.7.4** dans `/Applications/UE_5.7` (37 Go). **UE 5.8 pas encore installé** (mise à jour prévue par l'utilisateur).
- Ancien projet `~/vs-code/projet-jv/nindo_arena` : **abandonné, aucun lien avec ce projet**, voué à être supprimé. Ne rien en reprendre.
- Dépôt du projet : `~/vs-code/jeu-video` → GitHub `git@github.com:Rumble-Paw-Patrol/Videogame.git` (branche `main`, Git LFS pour les binaires).
- Quota Git LFS gratuit GitHub : 10 Go de stockage + 10 Go de bande passante par mois ; au-delà, avec un budget à 0 $, LFS est bloqué jusqu'au mois suivant (pas de facturation). [vérifié 2026-09-28]
- L'utilisateur prévoit de changer de machine dans les mois qui viennent → mettre à jour cette section.

## 3. macOS : exigences et limitations (UE 5.8)

**Exigences** [vérifié] :
- macOS minimum Sonoma 14.5, recommandé Sequoia 15 (Tahoe 26 n'est pas cité comme recommandé → risque de soucis).
- Xcode minimum 26.0, **recommandé 26.1.1**. La doc indique : **Xcode 26.4 n'est pas compatible**.
- Problème connu **UE-377426** (listé dans les notes 5.8.2) : erreur de compilation Mac avec Xcode ≥ 26.4. Contournement : Xcode 26.1 ou antérieur. Pas signalé comme corrigé dans 5.8.3.
  → **Action : installer Xcode 26.1.1 à côté (ou à la place) de 26.4.1 et le sélectionner avec `sudo xcode-select -s <chemin>`.**

**Rendu sur Apple Silicon** [vérifié] :
- Lumen (ray tracing logiciel) et TSR : OK dès M1.
- Nanite et Virtual Shadow Maps : **bêta** (M2+).
- Lumen ray tracing matériel et MegaLights : **expérimental** (M2+).

**Réservé à Windows ou absent sur Mac** :
- **Live Coding** (recompiler le C++ sans fermer l'éditeur) : absent en pratique sur Mac (rapports forum, la doc ne cite pas macOS). [vérifié partiellement]
  → Workflow : fermer l'éditeur → compiler en ligne de commande → relancer. Regrouper les modifications C++. Mettre les valeurs de réglage dans des données (DataAssets, courbes, `.ini`) pour éviter de recompiler.
- **Packager un jeu Windows depuis un Mac** : impossible, il faut une machine Windows. [à valider pour 5.8, vrai historiquement]
- MetaHuman Animator — animation du corps : Windows uniquement (le facial fonctionne sur Mac depuis 5.8). [vérifié]
- DLSS / Reflex (NVIDIA) : Windows uniquement. Sans objet pour nous.
- Certains plugins Fab sont livrés compilés pour Windows seulement → vérifier « Supported Platforms » avant d'en adopter un.
- Pas de Visual Studio : utiliser VS Code (déjà en place), Rider ou Xcode.

## 4. Unreal MCP (officiel, **expérimental**)

**Activation dans l'éditeur** [vérifié] :
- Plugins à activer : `ModelContextProtocol` et `AllToolsets`.
- Toolset GAS : plugin `GASToolsets`, **désactivé par défaut**, déclenche un avertissement « expérimental » à l'activation.
- Démarrer le serveur : console → `ModelContextProtocol.StartServer` (port 8000, `http://127.0.0.1:8000/mcp`). Option de démarrage automatique : `bAutoStartServer`.
- Générer la config Claude : console → `ModelContextProtocol.GenerateClientConfig ClaudeCode` → écrit `.mcp.json` à la racine du projet. Lancer Claude Code depuis cette racine. Regénérer si le port change.

**Plugin Claude Code d'Epic** [vérifié] :
- Installation : `/plugin install unreal-engine-skills-for-claude-code@claude-plugins-official`
- Contient la skill `unreal-mcp` + un hook de démarrage de session qui détecte un projet UE.
- Fonctionne sur macOS sans configuration particulière.

**Caractéristiques techniques** [vérifié] :
- Transport HTTP + SSE seulement (pas stdio, pas WebSocket). Écoute en local uniquement, **aucune authentification**.
- 30+ toolsets : acteurs/scène, Blueprints, assets, matériaux, meshes, animation, Sequencer, Niagara, UI, GAS, StateTree, Control Rig, tests d'automatisation, scripting.
- Toolsets par défaut : ActorTools, SceneTools, MaterialInstanceTools, ObjectTools (majoritairement en Python).
- `ProgrammaticToolset.execute_tool_script` exécute du **Python arbitraire** avec accès complet au projet.
- La découverte automatique des toolsets ne fonctionne que dans l'éditeur.

**Correctifs MCP notables** [vérifié] :
- 5.8.1 : crash fatal corrigé lors de l'ajout d'un composant à un Blueprint sans SimpleConstructionScript ; correction du format des réponses `tools/call` ; **transactions désactivées pendant l'exécution des scripts d'outils** → considérer que Ctrl+Z ne défait pas les actions MCP. Git est le seul vrai retour arrière.
- 5.8.2 et 5.8.3 : rien de spécifique au MCP.

**Règles d'usage pour ce projet** :
1. Tout enregistrer et faire un commit avant une session pilotée par MCP.
2. Ne jamais lancer Claude Code avec `--dangerously-skip-permissions` quand le plugin est chargé.
3. Relire tout script Python qui touche beaucoup d'assets avant de l'autoriser.
4. Après une modification de Blueprint via MCP : compiler le Blueprint et vérifier les erreurs.
5. Ne jamais exposer le port 8000 hors de la machine.

## 5. Nouveautés 5.8 utiles pour le module personnage

- **Mover** (nouveau système de déplacement) : se rapproche du statut « production » ; amélioration de la prédiction et du rollback réseau, support Iris optionnel, prédiction de trajectoire pour le motion matching (ChaosMover). Pas encore annoncé « production-ready ». [vérifié]
- **Character Movement Component** : toujours la valeur sûre pour un personnage. [à valider : aucun changement relevé dans les notes]
- **Iris** (réplication réseau) : **production-ready** en 5.8. [vérifié]
- **Enhanced Input** unifié avec Common UI / Common Input ; nouveau débogueur d'entrées. [vérifié]
- **StateTree** : état de départ configurable, gestionnaire de compilation. [vérifié]
- Animation Mixing dans Sequencer (expérimental), nouveau Unreal Animation Framework. [vérifié]
- **GAS** : aucun changement notable relevé dans les notes 5.8 ni dans les hotfixes. [vérifié]

## 6. Unreal Engine 6 — ce qui compte pour nous

- Nouveau langage de gameplay **Verse** et nouveau framework **Scene Graph**, qui remplaceront à terme Blueprints et Actors. [vérifié]
- Blueprints et Actors **restent présents dans les premières versions d'UE6** et ne seront dépréciés qu'une fois le nouveau framework mûr. Des outils de conversion sont prévus. [vérifié]
- Le MCP devient central dans UE6. [vérifié]
- Décision projet : développer sur **UE 5.8.x**. Garder la logique métier et les données (stats, capacités, profils) aussi indépendantes que possible du framework Actor pour faciliter une future migration.

## 7. Commandes de référence sur Mac [à valider à l'installation de 5.8]

```bash
# Compiler la cible éditeur d'un projet (éditeur fermé)
"/Applications/UE_5.8/Engine/Build/BatchFiles/Mac/Build.sh" <NomProjet>Editor Mac Development -Project="<chemin>/<NomProjet>.uproject" -WaitMutex

# Ouvrir le projet dans l'éditeur
open -a "/Applications/UE_5.8/Engine/Binaries/Mac/UnrealEditor.app" --args "<chemin>/<NomProjet>.uproject"

# Changer de version d'Xcode active
sudo xcode-select -s /Applications/Xcode-26.1.1.app
```

---

## Journal

- **2026-09-28** — Création du fichier. Recherche : notes de version 5.8, hotfixes 5.8.1 à 5.8.3, exigences macOS, doc Unreal MCP, plugin Claude d'Epic, feuille de route UE6. Inventaire de la machine.
- **2026-09-28** — `nindo_arena` abandonné (aucun lien). Dépôt `Videogame` initialisé. Quota LFS GitHub ajouté.

## Sources

- Notes de version UE 5.8 : https://dev.epicgames.com/documentation/unreal-engine/unreal-engine-5-8-release-notes
- Exigences macOS UE 5.8 : https://dev.epicgames.com/documentation/unreal-engine/macos-development-requirements-for-unreal-engine
- Unreal MCP : https://dev.epicgames.com/documentation/unreal-engine/unreal-mcp-in-unreal-editor
- Plugin Claude Code d'Epic : https://github.com/EpicGames/unreal-engine-skills-for-claude-code-plugin
- Hotfix 5.8.1 : https://forums.unrealengine.com/t/5-8-1-hotfix-released/2738864
- Hotfix 5.8.2 : https://forums.unrealengine.com/t/5-8-2-hotfix-released/2746335
- Hotfix 5.8.3 : https://forums.unrealengine.com/t/5-8-3-hotfix-released/2833315
- Live Coding (doc) : https://dev.epicgames.com/documentation/unreal-engine/using-live-coding-to-recompile-unreal-engine-applications-at-runtime
- Live Coding absent sur Mac (forum) : https://forums.unrealengine.com/t/in-mac-where-is-the-livecoding-settings-the-settings-are-missing-the-livecoding-console-does-not-appear/586868
- Feuille de route UE6 : https://www.unrealengine.com/news/the-road-to-ue-6
- Résumé State of Unreal 2026 : https://tech-insider.org/unreal-engine-6-state-of-unreal-2026/
- Facturation Git LFS GitHub : https://docs.github.com/billing/managing-billing-for-git-large-file-storage/about-billing-for-git-large-file-storage
