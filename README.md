# Prisma — bac à sable de lumière

Démo 2D interactive destinée à [arthurlecomte.fr](https://arthurlecomte.fr)
(`/demos/prisma/`). On place des **lasers** (blancs ou colorés) et on sculpte
leurs rayons avec des **miroirs**, des **prismes** (vraie dispersion en
arc-en-ciel), des **lentilles** convergentes/divergentes, des **murs** et
des **filtres colorés**. Rendu « néon » sur fond sombre, canvas 2D, zéro
dépendance d'exécution.

## Utilisation

- **Choisir** un objet : clic ou toucher. **Déplacer** : glisser.
  **Pivoter** : tirer la poignée ronde, tourner à deux doigts, ou `R` /
  `Maj+R` au clavier.
- Barre d'outils pour ajouter des objets, scènes d'exemple en haut
  (Arc-en-ciel, Miroirs, Focale, Mélange), `?` pour l'aide.
- Clavier : `N` objet suivant, flèches déplacer, `Suppr` supprimer,
  `Échap` désélectionner.
- La scène en cours est sauvegardée localement dans le navigateur
  (`localStorage`, clés préfixées `prisma:`) ; « Réinitialiser » recharge
  la scène de départ.

## Développer

```sh
npm ci
npm run dev       # serveur de développement
npm run build     # fichiers finaux dans dist/
npm run preview   # sert le build
```

## La physique (et ses simplifications assumées)

- Réfraction par la **loi de Snell**, réflexion totale interne comprise ;
  les lentilles sont de vraies surfaces circulaires (l'aberration
  sphérique est donc visible, comme en vrai).
- **Dispersion** par une loi de Cauchy volontairement exagérée (~2× le
  verre réel) pour que l'éventail du prisme reste lisible à l'écran ; la
  lumière blanche est échantillonnée en 16 rayons spectraux à la première
  réfraction.
- Les couleurs sont choisies par **longueur d'onde** (400–680 nm), pas en
  RVB : un laser « rouge » est un vrai 650 nm qui traverse un prisme sans
  se décomposer.
- Simplifications : pas de réflexions partielles de Fresnel (un seul
  rayon par interaction), filtres à transmission gaussienne (±48 nm),
  un rayon teinté par un filtre reprend la longueur d'onde du filtre.
  Les objets en verre qui se chevauchent peuvent produire des trajets
  approximatifs.

## Code

`src/geometry.js` (maths pures) → `src/optics.js` (tracé spectral) →
`src/objects.js` (surfaces des objets) → `src/render.js` (canvas néon) ;
`src/interaction.js` (pointeur + clavier), `src/ui.js` (DOM),
`src/scenes.js` (presets), `src/storage.js` (sauvegarde locale).

## Licences des assets

Aucun asset externe : icônes SVG inline, favicon SVG et styles écrits à la
main pour ce projet ; police système. Tout le contenu du repo est fait
maison.
