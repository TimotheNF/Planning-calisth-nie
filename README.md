# Planning Calisthénie

Application web personnelle de suivi calisthénie. Un seul fichier HTML, aucune dépendance, aucun serveur, aucun compte à créer.

**En ligne :** `https://<utilisateur>.github.io/planning-calisthenie/`

## Ce que fait l'appli

- **Planning hebdomadaire** par créneaux : chaque jour est une pile de séances qu'on ajoute, remplace ou supprime en deux clics.
- **Street Originel** / **Street** — au poids du corps. Un objectif de reps par mouvement, 5 tours à tenir. Chaque séance validée aux 5 tours ajoute **+1 rep à chaque mouvement**, sans limite.
- **Street Originel Plus** / **Street Plus** — reps fixes, c'est la **courbe de poids** qui monte : un palier par séance réussie (35 paliers, de 1,25 à 18,5 kg, éditables un par ligne).
- **Validation détaillée** : nombre de tours complets, plus les reps du dernier tour laissé en cours si tu n'es pas allé au bout.
- **Chrono de repos** sous chaque séance : 60 s / 90 s / 2 min / 3 min, bip et vibration à la fin.
- **Test de max** toutes les 8 semaines, sur n'importe quelle séance.
- **Abdo** : circuit S1 → S7 calculé depuis un *Max réf.* qui suit la dernière perf loggée.
- **Performances** : une carte par mouvement principal avec le dernier max, l'écart et une sparkline, plus une courbe détaillée.
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
