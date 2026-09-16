# Dataset Paint Plan Play

Le Dataset que lisent les applications Paint Plan Play : Units, Detachments,
Enhancements et Stratagems de Warhammer 40,000, **sans aucun texte de règles**.
Ce que fait une règle est porté par son *Effect* (format de
[40kdc-data](https://github.com/wn-mitch/40kdc-data)), par un résumé d'une ligne
écrit par le projet, ou par son seul nom — gardez votre codex à portée de main.

## Ce que contient ce dépôt

| | |
|---|---|
| `<gameSystem>/` | le Dataset construit : `index.json`, `core.json`, `armies/<army>.json` |
| `corrections/` | les Corrections, un fichier par correctif, avec la valeur amont et la raison |
| `authored/` | ce que le projet écrit lui-même : Battle Sizes, cibles de référence, List d'exemple, Effects et résumés |
| `registry/` | les identifiants stables du Dataset |
| `manifest.json` | les Dataslates et, pour chacune, la Dataset Release à lire |

Chaque **Dataset Release** est un tag immuable, rattaché à une **Dataslate**.
Le manifeste désigne la Dataslate courante et les deux précédentes ; revenir en
arrière, c'est le repointer.

## Attribution

Construit à partir de trois sources, que nous remercions :

- [BSData/wh40k-11e](https://github.com/BSData/wh40k-11e) — profils, armes, options, mots-clés ;
- [BSData/wh40k-11e-mfm](https://github.com/BSData/wh40k-11e-mfm) — le Munitorum Field Manual : points, DP, rattachements ;
- [wn-mitch/40kdc-data](https://github.com/wn-mitch/40kdc-data) — règles de Detachment, Stratagems, Effects et leur format.

## Licence

L'apport de ce projet — modèle, identifiants, Corrections, Effects et résumés
écrits par nous — est publié sous
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) (voir [LICENSE](LICENSE)).
Les contributions sont acceptées sous cette même licence.

**Warhammer 40,000**, les noms, marques et logos qui en relèvent appartiennent à
**Games Workshop Limited**. Ce dépôt n'est ni affilié à Games Workshop ni
approuvé par lui, et le contenu du jeu n'est pas licencié par nous. Aucun texte
de règles n'est publié ici, et une vérification automatique le bloque à chaque PR.

**Ayants droit** : pour toute demande, ouvrez une issue sur ce dépôt (contact
dédié à venir).

## Contribuer

Une Correction, un Effect ou un résumé passe par une PR : voir le
[modèle de PR](.github/pull_request_template.md). La CI vérifie le schéma et
l'absence de texte de règles. Le Dataset est construit par le
[Dataset Tool](https://github.com/PaintPlanPlay/dataset-tool), publié sous MIT.
