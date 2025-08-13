# Kingshot Redeem Code – Outil d’envoi de codes cadeau par lots ✨

> Langue: Français (auto) · English version: [README.en.md](./README.en.md)

> Site en ligne: https://spaced-one.github.io/Kingshot-Redeem-Code-All-Alliance-Site/

Ce dépôt contient une application web (une seule page `index.html`) qui permet d’appliquer un code cadeau à de nombreux IDs de joueurs (p. ex. une alliance) pour le jeu Kingshot. L’outil se charge de se « connecter » pour chaque ID, puis d’exécuter le redeem un par un, en respectant les limites du serveur.

> [!NOTE]
> Cette application est 100% côté navigateur (pas de backend). Elle signe les requêtes comme le client officiel et envoie en `application/x-www-form-urlencoded`.

Si vous tombez sur ce repo sans contexte: cet outil sert à automatiser, côté navigateur, l’application d’un même code cadeau à une liste d’IDs numériques du jeu Kingshot, sans serveur ni installation.

## Pour qui ? 👥
- Responsables d’alliance ou joueurs souhaitant appliquer un code à plusieurs comptes.
- Utilisateurs qui ont une liste d’IDs et un code cadeau à distribuer.

## Ce que fait l’outil 🔧
- Colle et nettoie des listes d’IDs (lignes/virgules/points‑virgules, déduplication).
- Se connecte à chaque ID (endpoint `/player`) puis appelle le redeem (`/gift_code`).
- Traite les IDs en séquence, avec des pauses et des retries pour éviter les limites.
- Affiche un résumé clair: succès, déjà reçu, codes invalides, échecs.
- Gère des listes locales (créer/renommer/supprimer/enregistrer/charger), avec récupération d’infos joueur.
- Inclus par défaut 2 listes d’exemple: « Alliance FARM » et « APX Alliance » (stockées localement si aucune liste n’existe encore).

## Prérequis ✅
- Un navigateur moderne (Chrome, Edge, Firefox, Safari).
- Un code cadeau valide (texte).
- Une liste d’IDs numériques (copiée depuis vos sources habituelles).

## Démarrage rapide 🚀
1) Téléchargez/clonez le dépôt, puis ouvrez `index.html` dans votre navigateur.
2) Collez vos IDs dans le champ (un par ligne ou séparés par des virgules/;).
3) Saisissez le code cadeau.
4) Cliquez « Envoyer la requête ». Le script va:
   - Attendre 1 seconde, se connecter à l’ID (`/player`) avec signature.
   - Attendre à nouveau 1 seconde, tenter le redeem (`/gift_code`).
  - En cas de 429, attendre 11 secondes puis réessayer (nombre de tentatives limité).
   - S’arrêter si le code est invalidé (USED/CDK NOT FOUND) pour ne pas continuer inutilement.

> [!TIP]
> Vous pouvez activer la capture du presse‑papiers ou le mode « collage » pour ajouter automatiquement des IDs que vous copiez.

## Fonctionnement (vue d’ensemble) 🧠
- Signature: concaténation triée des paires `key=value`, MD5 avec sel `mN4!pQs6JrYwV9` via CryptoJS; envoi en `application/x-www-form-urlencoded`.
- Flux submit: séquentiel strict par ID; pauses de 1s avant login et 1s avant redeem; retry 429 avec attente fixe de 11s.
- États redeem détectés:
  - Succès: `code = 0` ou `err_code = 20000` (`SUCCESS`).
  - Déjà reçu: `err_code = 40008` ou `msg` contient `RECEIVED`.
  - Invalide/erroné: `err_code ∈ {40005 (USED.), 40014 (CDK NOT FOUND.)}` (arrêt immédiat du traitement).
  - Échec: autres erreurs/réseau.
- Récupération d’infos: bouton dédié, séquentiel avec pause de 1s et retry 429; affichage avatar/pseudo/serveur/niveau/recharges.

> [!IMPORTANT]
> En cas de code invalide/erroné, le traitement s’arrête tout de suite pour éviter de lancer des requêtes inutiles sur les IDs suivants.

## Dépannage rapide 🧰

> [!WARNING]
> 429 Too Many Requests: l’outil attend 11s puis retente automatiquement (quelques essais). Le traitement est plus lent mais évite les blocages.

> [!NOTE]
> Onglet en arrière‑plan: le navigateur peut ralentir les timers. L’outil adapte la fréquence et effectue un boost à votre retour.

> [!TIP]
> Presse‑papiers: si l’accès est refusé, utilisez le mode « collage » (sans permission).

> [!NOTE]
> CORS/réseau: l’outil appelle un domaine tiers; son fonctionnement dépend des en‑têtes du serveur et de votre réseau.

## Exécution locale (optionnel) 🖥️
Il n’y a pas de build. Ouvrez `index.html` directement ou servez‑le via un mini‑serveur pour éviter certains blocages.

```bash
# Python 3
python3 -m http.server 8000

# Node (via npx) — optionnel
# npx serve .
```

Puis visitez http://localhost:8000 et ouvrez `index.html`.

## Confidentialité et limites 🔐
- Aucune donnée n’est envoyée à un serveur de ce dépôt. Les listes/infos restent dans votre navigateur (localStorage).
- Les requêtes partent de votre navigateur vers `kingshot-giftcode.centurygame.com`.
> [!CAUTION]
> Respectez les conditions d’utilisation du jeu. Utilisez ce script à vos risques.

## FAQ ❓
• Où trouver les IDs ?
> Selon votre organisation (alliances, exports, captures…). L’outil accepte chiffres sur 1 ligne chacun ou séparés par virgules/;.

• Puis‑je modifier les listes par défaut ?
> Oui. Créez/renommez/supprimez vos listes. Elles sont stockées en local.

• Pourquoi c’est lent ?
> Le traitement est volontairement séquentiel avec pauses et retry (11s sur 429) pour éviter les blocages serveurs.

## Langues 🌐
L’interface bascule automatiquement en français ou en anglais selon la langue de votre navigateur (FR/EN). Les messages réseau restent renvoyés par le serveur.

## Licence
Pas de licence fournie pour l’instant.
- Aucun build requis. Ouvrez simplement `index.html`.
