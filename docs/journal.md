# Journal de bord

> État du projet d'une session à l'autre. Mis à jour par Claude en fin de session.
> Quand ce fichier dépasse ~200 lignes, déplacer les vieilles sessions dans `docs/journal-archive.md`.

## À faire avant la première session de travail

**Côté machine (Léonard)**
- [ ] Acheter un SSD externe : NVMe en boîtier USB4/Thunderbolt (idéal) ou SSD portable USB 3.2 10 Gb/s. Formater en **APFS non sensible à la casse**.
- [ ] Libérer de la place : `S6B2` → iCloud, `projet-jv` (nindo_arena + GameAnimationSample) → corbeille, `uv cache clean`.
- [ ] Installer **Xcode 26.1.1** (developer.apple.com/download/all) et le sélectionner : `sudo xcode-select -s /Applications/Xcode-26.1.1.app`.
- [ ] Installer **UE 5.8.3** sur le SSD : plateforme Mac uniquement, sans les symboles de débogage.
- [ ] Désinstaller UE 5.7.
- [ ] GitHub : vérifier que le budget **Git LFS est à 0 $** (Settings → Billing and licensing → Budgets and alerts), pour être bloqué plutôt que facturé en cas de dépassement.

**Côté Claude Code (Léonard)**
- [ ] Installer le plugin d'Epic : `/plugin install unreal-engine-skills-for-claude-code@claude-plugins-official`.

**Première session avec Claude = Phase 0 de `docs/feuille-de-route.md`**
- [ ] Demander à Léonard le chemin d'installation réel d'UE 5.8.3 et la version d'Xcode active ; mettre à jour les notes Unreal.
- [ ] Créer `Plugins/CharacterKit` et `Projects/CKSandbox` (voir `architecture.md`), faire compiler.
- [ ] Editor Preferences → Loading & Saving → **Auto Save toutes les 5 minutes**.
- [ ] Activer les plugins `ModelContextProtocol` et `AllToolsets` ; démarrage automatique du serveur MCP (`bAutoStartServer`).
- [ ] Console : `ModelContextProtocol.GenerateClientConfig ClaudeCode` → `.mcp.json`, puis relancer Claude Code depuis le dossier du projet.
- [ ] Valider les commandes de compilation Mac (§7 des notes Unreal) et écrire `scripts/`.

**Plus tard : quand l'ami sous Windows rejoint le projet**
- [ ] L'ajouter comme collaborateur sur `Videogame`.
- [ ] Activer le verrouillage LFS : attribut `lockable` sur `*.uasset` / `*.umap` + plugin UEGitPlugin (ProjectBorealis).
- [ ] Activer « One File Per Actor » sur les maps.
- [ ] Ajouter une section Windows aux notes Unreal.

## Règles de sauvegarde

1. Auto Save d'Unreal toutes les 5 minutes.
2. Commit juste avant chaque série d'actions MCP qui modifie des assets.
3. Commit après chaque étape validée.
4. Push en fin de session.
5. Mise à jour de ce journal en fin de session.

## Sessions

### 2026-09-28 — Mise en place de l'environnement
- **Décisions** : Unreal Engine 5.8 + MCP officiel d'Epic, sur Mac ; C++ d'abord ; aucun outil payant ; SSD externe pour le moteur ; `nindo_arena` abandonné ; tout le travail dans le dépôt `Videogame`.
- **Fait** : notes Unreal vérifiées (`docs/unreal-engine-notes.md`), `CLAUDE.md`, `.gitignore` / `.gitattributes`, premier push. Dépôt git vide du dossier personnel mis à la corbeille.
- **Prochaine étape** : définir le périmètre du module (tri de la liste des mécaniques).

### 2026-09-29 — Cadrage du module
- **Décisions** : toutes les mécaniques seront implémentées, dans l'ordre de `docs/feuille-de-route.md` (priorités fixées par Claude) ; jeux **multijoueur** ; jeux pilotes = arènes 1v1 (shinobi puis sorcier) ; univers inspirés, sans propriété intellectuelle protégée.
- **Architecture** (`docs/architecture.md`) : plugin `CharacterKit` (préfixe `CK`) partagé par plusieurs projets dans `Projects/` ; CharacterMovementComponent étendu ; GAS avec ASC sur le PlayerState ; serveur d'écoute ; ressource générique « Energy ».
- **Fait** : `CLAUDE.md` réécrit, `architecture.md`, `feuille-de-route.md`.
- **Prochaine étape** : Léonard installe UE 5.8.3, Xcode 26.1.1 et le plugin Claude d'Epic → Phase 0.
