# Planning Calisthénie

Application web personnelle de suivi calisthénie. Un seul fichier HTML, aucune dépendance, aucun serveur, aucun compte à créer.

**En ligne :** `https://<utilisateur>.github.io/planning-calisthenie/`

## Ce que fait l'appli

- **Planning hebdomadaire** par créneaux : chaque jour est une pile de séances qu'on ajoute, remplace ou supprime en deux clics.
- **Séances** Street Original, Street Plus, Abdo : tableau de progression S1 → S7 calculé à partir d'un *Max réf.*, plus **S8 = semaine test** (2 séries au max, 4 min de repos) qui remet les références à jour et relance le cycle.
- **Max réf. synchronisé** : la référence de chaque exercice suit automatiquement la dernière performance loggée pour le même nom d'exercice. Un cadenas permet de figer une valeur à la main.
- **Front Lever** : échauffement dédié, 4 séances, paliers de progression.
- **Full Planche**, **Défi Pompes 30 jours**, **Performances** avec courbes dessinées en canvas natif.
- **Vue mobile** : sur écran étroit, les séances n'affichent que la semaine en cours et la semaine test au lieu des huit colonnes.

## Installation sur téléphone

Ouvrir l'adresse ci-dessus dans le navigateur, puis :

- **Android / Chrome** : menu ⋮ → *Installer l'application* (ou *Ajouter à l'écran d'accueil*).
- **iOS / Safari** : bouton Partager → *Sur l'écran d'accueil*.

L'appli s'ouvre ensuite en plein écran, avec son icône, et **fonctionne sans réseau** grâce au service worker.

## Données

Tout est stocké dans le navigateur de l'appareil (`localStorage`), sauvegardé automatiquement à chaque modification. Rien n'est envoyé nulle part par défaut.

**Synchro optionnelle** (onglet ☁ Synchro) : l'appli lit et écrit un Gist secret de ton compte GitHub, ce qui synchronise téléphone et ordinateur. Il faut un *personal access token* avec la seule portée `gist`. Le token est stocké dans le navigateur de l'appareil et **n'est jamais écrit dans le Gist ni dans l'export JSON**.

Sauvegarde froide : bouton **💾 Export** → fichier JSON à ranger où tu veux. **📂 Import** pour le relire.

## Structure

```
index.html              toute l'application (interface, logique, graphiques)
sw.js                   service worker — cache de l'app shell, mode hors-ligne
manifest.webmanifest    métadonnées d'installation (nom, icônes, couleurs)
icons/                  icônes 192 / 512 / maskable / apple-touch
```

## Modifier l'appli

Éditer `index.html`, commiter, pousser. GitHub Pages redéploie en une minute environ.

Après un changement, penser à incrémenter `VERSION` dans `sw.js` — sinon les appareils déjà installés continuent de servir l'ancienne version depuis leur cache.

Les données de départ (séances, exercices, paliers) sont dans la constante `SEED` en haut de `index.html`. Elles ne servent qu'au premier lancement et au bouton *Reset* : les modifications faites dans l'interface vivent dans le stockage du navigateur, pas dans le code.
