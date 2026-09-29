# Feuille de route du module CharacterKit

> Tout sera implémenté ; l'ordre ci-dessous fixe les priorités. Chaque phase se termine par une démo jouable à 2 joueurs.
> Étiquettes : **N** = arène shinobi, **HP** = arène sorcier, **BF** = tireur. Sans étiquette = commun.
> Cocher au fur et à mesure. Les phases peuvent être réordonnées après discussion, noter la raison dans le journal.

## Phase 0 — Fondations
- [ ] Structure du dépôt (voir `architecture.md`), projet `CKSandbox`, plugin `CharacterKit` vide qui compile
- [ ] Scripts `scripts/` : compiler, lancer l'éditeur, lancer les tests (valider les commandes §7 des notes Unreal)
- [ ] Réglages éditeur : sauvegarde auto 5 min, MCP (démarrage auto), `.mcp.json`
- [ ] Profil de test multijoueur : 2 joueurs, serveur d'écoute, émulation réseau (150 ms, 1 % de perte)
- [ ] Framework de tests d'automatisation + un premier test
- [ ] Salle d'essai « Gym » : pentes, escaliers, murs, rebords, plateformes, eau, cibles (via MCP)
- [ ] Outils de debug : affichage des états et attributs à l'écran, commandes de triche console

## Phase 1 — Personnage de base en réseau
- [ ] `ACKCharacter`, `ACKPlayerController`, `ACKPlayerState`, `ACKGameMode` répliqués
- [ ] Enhanced Input : actions abstraites, contextes, remappage, clavier/souris + manette
- [ ] Caméra 3ᵉ personne orbitale, vue épaule (changement d'épaule), 1ʳᵉ personne, bascule 1P/3P
- [ ] Anti-collision caméra, champ de vision configurable
- [ ] Marche, course, saut répliqués et prédits ; mannequin animé

## Phase 2 — Locomotion complète
- [ ] Sprint, accroupi, glissade ; couché **BF**
- [ ] Courbes d'accélération / décélération, rotation vers le mouvement ou strafe
- [ ] Pentes, marches, plateformes mobiles
- [ ] Saut à hauteur variable, tolérance au bord (coyote time), mémorisation de l'appui (jump buffer)
- [ ] Double saut / sauts multiples **N**, contrôle en l'air, dégâts de chute, roulade d'atterrissage
- [ ] Rebords : s'accrocher, se hisser, franchir un obstacle **BF**
- [ ] Modificateurs de vitesse (bonus, malus, poids)
- [ ] Bruits de pas selon la surface

## Phase 3 — Attributs (GAS)
- [ ] AbilitySystemComponent sur le PlayerState, attributs Vie / Endurance / Energy (+ max)
- [ ] Régénération avec délai après dégâts ; sprint consomme l'endurance
- [ ] Modificateurs (fixes / %, temporaires / permanents, règles de cumul)
- [ ] Armure / bouclier, résistances par type de dégâts (éléments **N**)
- [ ] Statuts : brûlure, poison, saignement **BF**, étourdissement, silence, ralentissement, confusion **HP**
- [ ] Mort, réapparition ; état « à terre » + réanimation **BF**
- [ ] HUD : barres (avec traînée des dégâts récents), icônes de statuts

## Phase 4 — Système de capacités
- [ ] Capacité de base `UCKGameplayAbility` : coût, recharge, charges, recharge globale
- [ ] Incantation, canalisation, interruption, possible ou non en mouvement
- [ ] Déclenchement : appui, maintien, charge-relâche, séquence de touches (mudras **N**)
- [ ] Ciblage : soi, direction, point visé, cible verrouillée, zone au sol, cône, rayon
- [ ] Emplacements et équipement des capacités, rangs / améliorations
- [ ] Passifs, capacités activables en continu, ultime avec jauge, transformations (modes **N**)
- [ ] Fenêtres d'annulation, combos
- [ ] Première capacité : dash / esquive avec invulnérabilité
- [ ] HUD : barre de capacités, recharge circulaire, barre d'incantation

## Phase 5 — Projectiles et combat
- [ ] Projectiles répliqués avec prédiction client : balistique, tête chercheuse, rebond, perforation
- [ ] Tir instantané (hitscan) + compensation de latence côté serveur **BF**
- [ ] Rayon continu, explosion avec atténuation, cône, arc électrique, zones persistantes
- [ ] Entités invoquées (clones **N**), pièges (**N**, **BF**), murs et boucliers invoqués
- [ ] Zones de frappe / zones touchables, calcul des dégâts (base → modificateurs → résistances → armure)
- [ ] Critiques, tirs à la tête, dégâts par partie du corps, tir ami configurable
- [ ] Corps à corps, combos **N**, blocage, parade, garde brisée
- [ ] Verrouillage de cible **N/HP**, réactions aux coups, arrêt sur image, projection / envoi en l'air, ragdoll
- [ ] Contre-sort, bouclier magique, duel de faisceaux **HP**
- [ ] Mannequin d'entraînement + bot StateTree qui utilise le même système de capacités
- [ ] Ressenti : tremblements, vibrations, chiffres de dégâts, indicateur de direction des coups

## Phase 6 — Mobilité avancée
- [ ] Course sur les murs, saut mural **N**
- [ ] Marche sur murs et plafonds (gravité personnalisée) **N**, marche sur l'eau **N**
- [ ] Téléportation (**N**, **HP**), permutation **N**, grappin, tyrolienne, planer
- [ ] Nager, plonger, échelles, lévitation **HP**
- [ ] Enchaînements de parkour, conservation de l'élan

## Phase 7 — Jeu pilote n°1 : arène shinobi 1v1
- [ ] Projet `ArenaShinobi` qui utilise le module
- [ ] Déroulé de match : manches, réapparition, score, fin de partie
- [ ] 2 personnages originaux, 4 à 6 capacités chacun
- [ ] Partie en ligne entre deux machines (serveur d'écoute)
- [ ] Retour d'expérience → améliorations du module

## Phase 8 — Jeu pilote n°2 : arène sorcier 1v1
- [ ] Projet `ArenaSorcier` : visée de sorts, boucliers, contre-sorts, duel de faisceaux
- [ ] Vol sur balai (mode de déplacement dédié) **HP**
- [ ] Retour d'expérience → améliorations du module

## Phase 9 — Plugin tireur `CKWeapons` **BF**
- [ ] Armes : modes de tir, cadence, chargeur, rechargement, recul, dispersion, visée, lunettes, accessoires
- [ ] Grenades avec trajectoire prévisualisée, suppression, respiration retenue
- [ ] Penchement, caméra de recul, viseur dynamique, marqueurs de touche
- [ ] Classes, escouades, tableau des scores, boussole / mini-carte

## Phase 10 — Au-delà
- [ ] Véhicules (entrée, sortie, places) **BF**, montures et invocations **N**
- [ ] Destruction de l'environnement **BF**
- [ ] Interaction : ramassage, inventaire, consommables, objets physiques, télékinésie **HP**
- [ ] Serveur dédié, Iris, services en ligne (matchmaking)
- [ ] Sauvegarde / chargement, menus complets, accessibilité, réglages joueur
