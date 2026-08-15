# Planning Calisthénie

Application web personnelle de suivi calisthénie. Un seul fichier HTML, aucune dépendance, aucun serveur, aucun compte à créer.

**En ligne :** `https://<utilisateur>.github.io/planning-calisthenie/`

## Ce que fait l'appli

- **Planning hebdomadaire** par créneaux : chaque jour est une pile de séances qu'on ajoute, remplace ou supprime en deux clics.
- **Street Original** / **Street** — progression par réussite au poids du corps. Un objectif de reps par mouvement et un nombre de tours cible (5). Chaque séance validée aux tours cible ajoute **+1 rep à chaque mouvement**. Au plafond, l'appli propose de passer à la version lestée.
- **Street Original Plus** / **Street Plus** — mêmes mouvements, reps figées au plafond, c'est la **courbe de poids** qui monte : un palier par séance réussie (35 paliers, de 1,25 à 18,5 kg).
- **Test de max** toutes les 8 semaines : 2 séries au maximum, 4 min de repos, meilleur des deux retenu.
- **Abdo** : circuit S1 → S7 calculé depuis un *Max réf.* qui suit la dernière perf loggée.
- **Performances** : une carte par mouvement principal avec le dernier max, l'écart et une sparkline, plus une courbe détaillée avec lecture au survol.
- **Front Lever**, **Full Planche**, **Défi Pompes 30 jours**.

## Installation sur téléphone

Ouvrir l'adresse ci-dessus dans le navigateur, puis :

- **Android / Chrome** : menu ⋮ → *Installer l'application* (ou *Ajouter à l'écran d'accueil*).
- **iOS / Safari** : bouton Partager → *Sur l'écran d'accueil*.

L'appli s'ouvre ensuite en plein écran, avec son icône, et **fonctionne sans réseau** grâce au service worker.

## Premier lancement

L'appli est livrée **vierge** : les exercices, les séances et les paliers sont là, mais aucun *Max réf.* ni aucun historique de performance. Les colonnes S1-S7 affichent « — » tant qu'une référence n'est pas renseignée. Trois façons de démarrer :

- taper un nombre dans la colonne **Max réf.** de chaque exercice ;
- logger une perf dans la carte **Courbe de progression** (le Max réf. se remplit tout seul) ;
- charger une sauvegarde existante avec **📂 Import**, ou activer l'onglet **☁ Synchro**.

Aucune donnée personnelle n'est présente dans ce dépôt. Les sauvegardes JSON sont exclues par `.gitignore` : ne les commite pas si le dépôt est public.

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
