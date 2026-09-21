# Prompt pour Claude Code

Copiez le texte ci-dessous dans Claude Code, dans le dossier contenant `index.html`.

---

J'ai une application web mono-fichier dans `index.html` : un outil de notation des oraux
de passation Chûnin pour un serveur RP Naruto. Elle fonctionne déjà. Je veux que tu la
fasses évoluer.

**Contexte technique**
- Un seul fichier HTML : CSS et JavaScript inclus dedans, pas de build, pas de framework
- Librairie SheetJS en local dans `vendor/xlsx.full.min.js` pour l'export Excel
- Données persistées en `localStorage` sous les clés `oral-chunin-v3` et `oral-chunin-meta`
- Thème clair/sombre automatique via variables CSS sur `:root`
- Les dialogues natifs `confirm()` et `prompt()` sont volontairement remplacés par un modal
  maison (`ask()` et `askText()`), ne les réintroduis pas

**Règle de notation actuelle**
- Bloc Investissement & Objectifs = moyenne de deux notes sur 10 :
  - Investissement = moyenne des notes par section (le nombre de sections ne doit jamais
    avantager ni pénaliser)
  - Projets et objectifs = note directe sur 10
- Bonus hors moyenne : +0,5 si sensei, +0,5 tous les 4 examens loupés
- Note finale = bloc + bonus, plafonnée à 10
- Seuil réglable, 5 par défaut. Au-dessus : « Potentiellement admis ». En dessous : « Refusé »
- Case « Cas particulier » : force l'admission quelle que soit la note

**Ce que je veux que tu ajoutes**

[Décris ici ta demande]

**Contraintes**
- Garde le fichier autonome et sans dépendance réseau
- Garde le style visuel existant (polices, variables CSS, arrondis, ombres)
- Assure-toi que les anciennes fiches enregistrées restent lisibles après ta modification
- Mets à jour l'export Excel si tu ajoutes des champs

---

## Idées d'évolutions à lui demander

- Impression d'une fiche individuelle en PDF pour le jury
- Import d'un Excel existant pour reprendre une session commencée ailleurs
- Tri et recherche dans le registre
- Plusieurs examinateurs sur un même poste, avec moyenne des notes par candidat
- Critères configurables depuis l'interface plutôt qu'en dur dans le code
- Minuteur par candidat pour cadencer les passages
- Sauvegarde et chargement du registre en fichier JSON
