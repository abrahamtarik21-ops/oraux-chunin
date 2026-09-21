# Configuration du partage en direct (Firebase, gratuit)

Sans configuration, l'application fonctionne en **mode local** : chaque examinateur
garde ses fiches dans son propre navigateur. Pour que **tous les examinateurs voient
le même registre en direct**, il faut une base Firebase Realtime Database.

Le plan gratuit de Firebase (« Spark ») suffit largement : pas de carte bancaire,
1 Go de stockage, 100 connexions simultanées. Ne choisis **jamais** le plan « Blaze »
(payant) — il n'est pas nécessaire.

Compte à faire : 10 minutes, une seule fois, par une seule personne.

---

## 1. Créer le projet Firebase

1. Va sur <https://console.firebase.google.com> et connecte-toi avec un compte Google.
2. **Créer un projet** → nom au choix (ex. `oral-chunin`) → Continuer.
3. Quand on te propose Google Analytics, **désactive-le** (inutile) → **Créer le projet**.

## 2. Créer la base de données

1. Dans le menu de gauche : **Créer** (ou « Build ») → **Realtime Database**.
2. **Créer une base de données**.
3. Emplacement : **Belgique (europe-west1)** ou le plus proche → Suivant.
4. Règles de sécurité : choisis **Mode verrouillé** → **Activer**.
5. En haut de l'onglet **Données**, tu vois l'adresse de ta base, du type
   `https://oral-chunin-xxxxx-default-rtdb.europe-west1.firebasedatabase.app`.
   Garde-la sous la main : c'est le `databaseURL`.

## 3. Régler les règles d'accès

1. Onglet **Règles** de la Realtime Database.
2. Remplace tout le contenu par :

```json
{
  "rules": {
    "salles": {
      "$code": {
        ".read": true,
        ".write": true
      }
    }
  }
}
```

3. **Publier**.

Ce que ça fait : toute personne qui connaît un **code de salle** peut lire et écrire
le registre de cette salle, mais personne ne peut lister les salles existantes ni lire
autre chose. Choisis donc des codes de salle difficiles à deviner
(ex. `konoha-lune21-7f3k` plutôt que `test`), et ne les diffuse qu'aux examinateurs.

## 4. Récupérer la configuration

1. Roue dentée en haut à gauche → **Paramètres du projet** → onglet **Général**.
2. Tout en bas, section **Vos applications** → clique sur l'icône **Web** `</>`.
3. Surnom au choix (ex. `oral-chunin-web`), **ne coche pas** « Firebase Hosting »
   → **Enregistrer l'application**.
4. Firebase affiche un bloc `const firebaseConfig = { ... }`. Copie les valeurs.

Ces valeurs (y compris `apiKey`) ne sont **pas secrètes** : elles servent uniquement
à identifier ton projet. Ce sont les règles de l'étape 3 qui protègent les données.

## 5. Coller la configuration dans l'application

Ouvre `index.html` avec un éditeur de texte (Bloc-notes suffit), cherche
`FIREBASE_CONFIG` tout au début du `<script>` et remplis les champs :

```js
const FIREBASE_CONFIG = {
  apiKey: "AIza...",
  authDomain: "oral-chunin-xxxxx.firebaseapp.com",
  databaseURL: "https://oral-chunin-xxxxx-default-rtdb.europe-west1.firebasedatabase.app",
  projectId: "oral-chunin-xxxxx",
  storageBucket: "oral-chunin-xxxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

Si le bloc affiché par Firebase ne contient pas `databaseURL`, prends l'adresse
notée à l'étape 2.5. Enregistre le fichier. C'est terminé côté configuration.

## 6. Distribuer l'application aux examinateurs

Deux possibilités, au choix :

**A. Chacun ouvre le fichier chez lui (le plus simple).**
Envoie le dossier complet (`index.html` + `vendor/`) à chaque examinateur, par
exemple en zip sur Discord. Chacun double-clique sur `index.html`. La synchronisation
passe par Firebase, il n'y a rien à héberger.

**B. Mettre l'application en ligne (un seul lien pour tous).**
Le dossier est un site statique. Dépose-le sur un hébergeur gratuit :
- **Netlify Drop** (<https://app.netlify.com/drop>) : glisse le dossier, tu obtiens
  un lien immédiatement.
- **GitHub Pages** : dépôt public, dossier à la racine, activer Pages.
Avec un lien, le bouton **Copier l'invitation** produit une adresse qui ouvre
directement la bonne salle (`…/index.html#salle=code`).

## 7. Utilisation au quotidien

1. Le responsable de la session choisit un **code de salle** (ex. `konoha-lune21-7f3k`),
   l'entre dans la barre « Salle » et clique **Rejoindre**. Le point devient vert.
2. Il clique **Copier l'invitation** et envoie le code (ou le lien) aux autres examinateurs.
3. Chaque examinateur entre **son nom RP** dans « Examinateur », rejoint la même salle,
   et note ses candidats. Chaque fiche enregistrée apparaît chez tout le monde en
   moins d'une seconde, avec le nom de l'examinateur qui l'a notée. Le bandeau
   « En ce moment » du registre montre qui est connecté et qui note quel candidat,
   avec le chrono du passage.
4. Le nom de session et le seuil d'admission sont partagés dans la salle ; le nom
   d'examinateur reste propre à chaque poste.
5. **Réinitialiser la session** efface le registre **pour tout le monde** dans la
   salle : exporte d'abord (Excel ou JSON).
6. Pour une nouvelle session, prends simplement un nouveau code de salle.

Si le réseau coupe, le point passe au rouge : tu peux continuer à noter, les fiches
sont envoyées automatiquement au retour de la connexion. Si tu ouvres l'application
sans être dans une salle, elle reste en mode local ; en rejoignant une salle, elle
propose d'y envoyer les fiches locales qui n'y sont pas encore.

## Dépannage

| Symptôme | Cause probable | Solution |
|---|---|---|
| « Mode local — partage désactivé » | `databaseURL` vide dans `index.html` | Étape 5 |
| « Accès refusé à la salle » | Règles non publiées ou différentes de l'étape 3 | Étape 3, puis **Publier** |
| Le point reste orange (« Connexion à la salle… ») | Mauvaise `databaseURL`, ou pas d'internet | Vérifie l'adresse (étape 2.5), et que les autres sites se chargent |
| Les autres ne voient pas mes fiches | Ils ne sont pas dans la même salle | Comparer le code de salle, respecter l'orthographe |
| Firebase demande un paiement | Tu as cliqué sur « Passer au plan Blaze » | Refuse : le plan Spark gratuit suffit |
