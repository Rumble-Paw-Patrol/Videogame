# Architecture du module — décisions techniques

> Référence technique du projet. Toute décision qui la contredit doit d'abord être discutée avec Léonard, puis notée ici avec sa date.
> Nom de travail du module : **CharacterKit** (préfixe C++ `CK`). Renommable avant la phase 0 seulement.

## 1. Principes

1. **Multijoueur dès le départ.** Tout système est pensé serveur-autoritaire et testé à 2 joueurs avec latence simulée. On n'ajoute jamais le réseau « après ».
2. **Le module ne connaît aucun univers.** Il manipule « une ressource », « une capacité », « un projectile ». Les univers (shinobi, sorcier, militaire) sont des **données** et du contenu, dans les projets de jeu.
3. **C++ d'abord.** Toute la logique en C++. Les Blueprints sont des coquilles fines : modèle 3D, effets, sons, valeurs par défaut.
4. **Données en texte** autant que possible : GameplayTags dans des `.ini`, stats dans des DataTables importées depuis CSV/JSON, profils dans des DataAssets.
5. **Composition plutôt qu'héritage.** Des composants indépendants qu'un personnage assemble.
6. **Logique pure séparée du moteur.** Les calculs (dégâts, coûts, recharges, modificateurs) vivent dans des fonctions/structs C++ testables sans Actor. Facilite les tests et une future migration UE6.
7. **Propriété intellectuelle.** Univers « inspirés de », jamais copiés : aucun nom, personnage, logo, son ou asset protégé dans le dépôt, ni dans le code, ni dans les données.

## 2. Structure du dépôt

```
Videogame/
├── CLAUDE.md
├── docs/                         architecture, feuille de route, notes Unreal, journal
├── scripts/                      compilation, lancement, tests (bash)
├── Plugins/
│   └── CharacterKit/             LE MODULE réutilisable (partagé par tous les jeux)
│       ├── Source/
│       │   ├── CKCore/           types communs, GameplayTags, logs, calculs purs, debug
│       │   ├── CKMovement/       CharacterMovementComponent étendu, modes de déplacement
│       │   ├── CKCamera/         modes de caméra 1P / 3P / épaule / verrouillage
│       │   ├── CKAbilities/      GAS : AbilitySystemComponent, attributs, capacités, effets
│       │   ├── CKCombat/         projectiles, détection des coups, calcul des dégâts
│       │   ├── CKUI/             HUD (barres, capacités, viseur) branché sur événements
│       │   └── CKTests/          tests d'automatisation
│       ├── Content/              Blueprints fins et assets de test du module
│       └── Config/Tags/          GameplayTags en .ini
└── Projects/
    ├── CKSandbox/                projet de test : salles d'essai (« gyms »), débogage
    ├── (plus tard) ArenaShinobi/ jeu pilote 1v1 n°1
    └── (plus tard) ArenaSorcier/ jeu pilote 1v1 n°2
```

Chaque `.uproject` référence le module via `"AdditionalPluginDirectories": ["../../Plugins"]`. [à valider en phase 0]

Plus tard, le tireur (armes, véhicules) viendra dans un second plugin `CKWeapons`, pour que les jeux sans armes à feu ne l'embarquent pas.

## 3. Choix techniques

| Sujet | Choix | Pourquoi |
|---|---|---|
| Déplacement | **Character Movement Component** étendu (`UCKCharacterMovementComponent`), modes custom (`MOVE_Custom`), prédiction client via `FSavedMove` | Mature, répliqué, très documenté. **Mover** n'est pas encore « production-ready » en 5.8 : on le réévaluera plus tard |
| Attributs, capacités, effets | **Gameplay Ability System (GAS)** | Coûts, recharges, effets, tags et prédiction réseau déjà gérés |
| Emplacement de l'AbilitySystemComponent | Sur le **PlayerState** pour les joueurs (survit à la mort), sur le Character pour les PNJ | Pratique standard, respawn simple |
| Mode de réplication GAS | `Mixed` pour les joueurs, `Minimal` pour les PNJ | Standard multijoueur |
| Réplication réseau | Système par défaut au début ; **Iris** évalué en phase réseau avancée | Iris est prêt en 5.8 mais on garde la voie la plus documentée pour démarrer |
| Topologie | **Serveur d'écoute** (un joueur héberge) pour les arènes 1v1 ; serveur dédié plus tard | Suffisant pour du 1v1, gratuit |
| Services en ligne (matchmaking, NAT) | Epic Online Services, gratuit [à valider au moment venu] | Pas avant les jeux pilotes |
| Entrées | **Enhanced Input**, actions abstraites (`IA_Move`, `IA_Ability1`…) | Remappage, manette, contextes |
| Caméra | Composant caméra maison + `SpringArm`, piloté par profils | Contrôle total 1P/3P/verrouillage |
| Interface | **CommonUI** + widgets C++ ; l'UI écoute des événements, ne lit jamais le gameplay en boucle | Unifié avec Enhanced Input en 5.8 |
| IA de test | **StateTree** | Moderne, suffisant pour mannequins et bots |
| Animation | Mannequin gratuit d'Epic, Animation Blueprint fin alimenté par des variables C++ | Placeholder, remplaçable |

## 4. Conventions de code

- Standard de code Unreal. Classes préfixées `CK` : `ACKCharacter`, `UCKAttributeSet_Vitals`, `UCKGameplayAbility`, `FCKDamageSpec`.
- Identifiants en **anglais** (convention UE). Commentaires courts en **français**.
- GameplayTags : racine `CK.` dans le module (`CK.State.Stunned`, `CK.Ability.Dash`), racine `Game.` dans les jeux.
- Ressource générique appelée **Energy** dans le code (chakra, magie… selon le jeu, seulement dans l'UI et les données).
- Catégorie de log : `LogCK` (et `LogCKMovement`, `LogCKAbilities`…).
- Toute valeur de réglage passe par une donnée (`UPROPERTY(EditDefaultsOnly)`, DataAsset, courbe), jamais en dur. Évite de recompiler (pas de Live Coding sur Mac).

## 5. Définition de « terminé » pour chaque étape

Une fonctionnalité est terminée quand :
1. elle compile sans avertissement ;
2. elle fonctionne en éditeur avec **2 joueurs, serveur d'écoute, latence simulée ~150 ms et 1 % de perte** ;
3. la logique pure a des tests d'automatisation qui passent ;
4. elle est réglable par données ;
5. elle est testable dans une salle d'essai du projet `CKSandbox` ;
6. Léonard l'a essayée et validée (le ressenti de jeu, c'est lui) ;
7. elle est commitée, et le journal est à jour.
