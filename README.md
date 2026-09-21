# Oral de passation Chûnin — Registre des examinateurs

Application web de notation des oraux de passation Chûnin (serveur RP Zenkai).
Fonctionne entièrement dans le navigateur, sans serveur ni compte. En option, le
registre peut être **partagé en direct entre tous les examinateurs** via une base
Firebase gratuite (voir `CONFIGURATION.md`).

## Utilisation immédiate

Double-cliquez sur `index.html`. C'est tout.

## Ce que fait l'application

- Fiche de notation par candidat : prénom, nom, années en Genin Confirmé, nature de chakra
- Critère **Investissement** : choix du nombre de sections (0 à 4), sélection de chaque section
  dans la liste des sections de Konoha, puis note sur 10 pour chacune. La note retenue est la
  **moyenne** des sections : une seule section bien tenue vaut autant que trois.
- Critère **Projets et objectifs** : note sur 10
- Ces deux critères forment le bloc **Investissement & Objectifs**, dont la moyenne est la note de base
- **Bonus** : +0,5 si le candidat a un sensei ; +0,5 tous les 4 examens loupés (persévérance)
- Note finale plafonnée à 10. Seuil réglable, 5 par défaut
- Verdict : **Potentiellement admis** (retenu pour la mission de passation) ou **Refusé**
- Case **Cas particulier** : force l'admission quelle que soit la note
- **Minuteur** par candidat dans l'en-tête de la fiche : passage de **2 minutes**, s’arrête et sonne
  à la fin (chrono rouge clignotant), durée réelle enregistrée dans la fiche et l'Excel
- Registre latéral avec statistiques en direct, **recherche** (nom, section, examinateur…) et
  **tri** (ordre de passage, nom, note, admis d'abord, examinateur), modification et suppression
- **Partage en direct** (optionnel) : tous les examinateurs rejoignent la même « salle » et
  voient le même registre instantanément ; chaque fiche porte le nom de son examinateur.
  Un bandeau « En ce moment » montre qui note quel candidat, avec son chrono
- **Thème** clair / sombre / automatique, bouton dans l’en-tête, mémorisé par poste
- **Export Excel** : deux onglets (Résultats détaillés + Synthèse avec la liste des admis)
- **Sauvegarde JSON** : export complet du registre et rechargement (fusion sans doublons),
  pour reprendre une session sur un autre poste ou garder une archive

Les fiches sont conservées dans le navigateur (localStorage) et survivent à la fermeture.
Les fiches enregistrées avec les versions précédentes restent lisibles.

## Structure

```
index.html                  application complète (HTML + CSS + JS)
vendor/xlsx.full.min.js     librairie SheetJS pour l'export Excel (locale, hors ligne)
CONFIGURATION.md            guide pas à pas pour activer le partage en direct (Firebase)
README.md
```

## Partage entre examinateurs

Sans configuration, chaque poste a son propre registre (mode local). Pour un registre
commun en temps réel, suivez `CONFIGURATION.md` (10 minutes, gratuit, sans carte bancaire),
puis collez la configuration dans `FIREBASE_CONFIG` en tête du script de `index.html`.
Le SDK Firebase est chargé depuis internet uniquement dans ce cas ; hors ligne,
l'application repasse automatiquement en mode local.

## Mise en ligne (optionnel)

Le dossier est un site statique. Il se déploie tel quel sur GitHub Pages, Netlify ou Vercel :
déposez le dossier, aucune configuration nécessaire. Avec le partage activé, le bouton
« Copier l'invitation » fournit alors un lien qui ouvre directement la bonne salle.
