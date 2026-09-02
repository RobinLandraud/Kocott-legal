# 🍲 Kocott

L'appli qu'on sort à la fin d'un dîner quand quelqu'un demande "tu me donnes ta recette ?". Une appli de recettes **100% locale** — pas de compte, pas de serveur, tes recettes restent chez toi — pensée pour se partager sur-le-champ, autour de la table. Créée avec [Expo](https://expo.dev) / React Native.

## Pourquoi Kocott ?

Il y a quelque chose de perdu dans les applis de recettes modernes : le geste convivial de partager une recette — celle qu'on tend à un ami en repartant d'un dîner, celle qu'on offre avec un plat, celle qui traîne depuis toujours sur un bout de papier dans la cuisine de maman. Kocott ramène ce geste-là au centre : chaque recette se partage instantanément, de main en main, par QR code ou code texte — comme on tendrait la fiche elle-même, sans compte à créer ni serveur entre les deux.

Le reste (stockage 100% local via SQLite, aucune donnée qui transite ailleurs que directement vers la personne en face) découle de cette idée : une recette, c'est un objet qu'on partage entre humains, pas une donnée qu'on confie à un service.

## Fonctionnalités

### ✅ Disponibles

- **Création manuelle** de recettes — ingrédients, étapes, portions, temps de prépa/cuisson, catégorie, cuisine
- **Icônes personnalisées** par recette (plat, soupe, salade, viande, poisson, pizza, pâtisserie, boisson), chacune avec sa propre couleur
- **Badges régime** — Végétarien / Pescétarien / Vegan, affichés directement sur la liste et la fiche recette
- **Ajustement des portions** en temps réel — les quantités d'ingrédients recalculent automatiquement à l'affichage (sans jamais modifier la recette enregistrée)
- **Mode cuisine** plein écran — une étape à la fois, liste des ingrédients toujours visible, écran qui ne se met pas en veille
- **Partage instantané** par QR code ou code texte — la recette est compressée (DEFLATE + Base45) et encodée entièrement dans le code, sans passer par un serveur
- **Import par QR code ou code texte** — scan caméra ou collage, avec validation des données (Zod) avant sauvegarde
- **Import automatique depuis une URL** — extrait les données schema.org (JSON-LD) directement sur la page d'une recette trouvée en ligne, sans avoir à la recopier à la main
- **Export PDF** d'une fiche recette, avec le QR de partage intégré (la fiche imprimée peut être rescannée pour réimporter la recette) et les badges régime mis en valeur
- **Recherche et filtres** — par nom, catégorie ou cuisine, avec filtres dédiés par catégorie et par régime alimentaire

## Le geste du partage, en vrai

**Exemple concret** : fin de dîner, quelqu'un a adoré le tajine et demande la recette. Pas besoin de la recopier sur un coin de nappe ni d'échanger un numéro — la personne ouvre Kocott, affiche le QR, l'autre scanne avec son téléphone. La recette complète (ingrédients, étapes, tout) est déjà dans son appli, prête pour le mode cuisine, avant même d'avoir fini le café. Rien n'a transité par un serveur entre les deux — le code contenait la recette entière.

Et parce que la recette **entière** tient dans le code (pas un lien vers un serveur qui pourrait fermer un jour), ce même QR fonctionne tout aussi bien imprimé, collé, épinglé — il devient un objet physique qu'on peut toucher, transmettre, accrocher au frigo. Ça permet de faire vivre le geste du carnet de recettes qu'on se passe en famille : imprime le QR et colle-le sur une fiche cartonnée, dans le classeur de cuisine, sur l'étiquette d'un pot de confiture fait maison, à côté d'un plat qu'on offre à un voisin.

**Deuxième exemple** : la tarte aux pommes de maman, écrite à la main depuis 20 ans sur une fiche jaunie et tachée. Un dimanche, elle la rentre dans Kocott, imprime le QR généré, et le colle sur la fiche originale, juste à côté de son écriture. Des années plus tard, à un repas de famille, n'importe qui — même sans avoir jamais parlé de l'appli à maman — sort son téléphone, scanne le QR sur la fiche jaunie qui traîne toujours dans le tiroir, et récupère la recette complète. Le lien avec l'original reste visible, tangible, mais la recette voyage désormais aussi loin que le veut la famille.

### 🚧 À venir

**Gratuit :**
- Une pub après un import depuis une URL, et une autre avant un export PDF — pas encore branchées, ces deux actions sont pour l'instant 100% gratuites et sans pub. Le partage QR/code texte entre deux Kocott, lui, restera toujours sans pub.
- Langue de l'appli adaptée automatiquement à celle de l'appareil (aujourd'hui tout est en français, quelle que soit la langue du téléphone)

**Version payante (achat unique, pas d'abonnement) :**
- Retrait des pubs
- Export PDF du livre de recettes complet (ou d'une sélection)
- Partage de menus — regrouper plusieurs recettes en un seul QR, pratique pour donner toutes les recettes d'un repas d'un coup
- Collections d'événements réutilisables (repas de Noël, anniversaire...) qui s'enrichissent d'année en année
- Liste de courses générée automatiquement à partir d'un menu ou d'un événement

## Stack technique

| | |
|---|---|
| Framework | Expo SDK 54 · React Native 0.81 · React 19 |
| Langage | TypeScript 5.9 |
| Stockage | SQLite (`expo-sqlite`) via [Drizzle ORM](https://orm.drizzle.team) |
| Navigation | React Navigation (native-stack) |
| UI | Police [Nunito](https://fonts.google.com/specimen/Nunito), icônes [Lucide](https://lucide.dev) |
| Partage | `pako` (DEFLATE) + Base45 (RFC 9285) + `zod` pour la validation, QR via `react-native-qrcode-svg` / `expo-camera` |
| Import URL | Extraction JSON-LD (schema.org) depuis le HTML brut, normalisation + validation `zod` |
| Export PDF | `expo-print` + `expo-sharing`, QR généré via `qrcode` |
| Lint & format | [Biome](https://biomejs.dev) |

## Démarrer le projet

### Prérequis

- Node.js
- L'appli [Expo Go](https://expo.dev/go) sur ton téléphone (le plus simple pour tester), ou un émulateur Android/iOS

### Installation

```bash
npm install
npm run start
```

Scanne le QR code affiché dans le terminal avec Expo Go (Android) ou l'appareil photo (iOS).

### Scripts disponibles

| Commande | Description |
|---|---|
| `npm run start` | Lance le serveur de développement Metro |
| `npm run android` | Lance sur un émulateur/appareil Android |
| `npm run typecheck` | Vérifie les types TypeScript |
| `npm run lint` | Vérifie le style de code (Biome) |
| `npm run lint:fix` | Corrige automatiquement ce qui peut l'être |
| `npm run db:generate` | Régénère les migrations SQL après un changement de schéma |

## Structure du projet

```
src/
├── data/          # Couche base de données (schéma Drizzle, repository, migrations)
├── domain/        # Logique métier pure (modèle de recette, partage, régimes, durées...)
├── navigation/     # Configuration de la navigation
├── screens/        # Écrans de l'appli
├── theme/          # Couleurs et typographie partagées
└── components/     # Composants UI réutilisables

drizzle/            # Migrations SQL générées
```

## Confidentialité

Aucune donnée ne quitte ton téléphone, sauf dans deux cas explicites :
- **Partage d'une recette** (QR code ou code texte) — directement vers l'appareil de la personne qui scanne, sans intermédiaire, sans serveur.
- **Import depuis une URL** — l'app va chercher la page du site que tu indiques, comme le ferait un navigateur classique.

Aucun compte, aucun tracking, aucune donnée envoyée ailleurs.
