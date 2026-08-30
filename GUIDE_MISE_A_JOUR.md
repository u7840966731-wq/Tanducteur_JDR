# Activer les mises à jour (GitHub Pages)

Le système est déjà tout prêt dans l'application (menu *Aide → Vérifier
les mises à jour*), il ne manque que deux choses : mettre `version.json`
en ligne, et lui dire où il se trouve.

## 1. Crée ton dépôt GitHub Pages

Si ce n'est pas déjà fait : crée un dépôt GitHub, mets-y le contenu de ce
dossier `site/` (`index.html`, `assets/`, `downloads/`, `version.json`),
puis active GitHub Pages dans les paramètres du dépôt (Settings →
Pages). Ton site sera accessible à une adresse du type :

```
https://TON-PSEUDO.github.io/TON-DEPOT/
```

## 2. Adapte version.json

Ouvre `version.json` à la racine du site et remplace `TON-PSEUDO` et
`TON-DEPOT` par tes vraies valeurs :

```json
{
  "version": "1.1.0",
  "url": "https://TON-PSEUDO.github.io/TON-DEPOT/#telecharger"
}
```

Ce fichier sera donc accessible à :
`https://TON-PSEUDO.github.io/TON-DEPOT/version.json`

## 3. Branche l'application dessus

Dans `titan_app.py` (paquet dev), cherche `UPDATE_CHECK_URL` tout en
haut du fichier, et remplace la ligne par :

```python
UPDATE_CHECK_URL = "https://TON-PSEUDO.github.io/TON-DEPOT/version.json"
```

Recompile (`build_exe.bat`) : l'application vérifiera désormais cette
adresse à chaque lancement, et affichera un message si une version plus
récente que celle installée est disponible.

## 4. À chaque nouvelle version

Deux choses à faire, à chaque fois que tu publies une mise à jour :

1. Change le numéro dans `version.json` (ex. `"1.2.0"`), republie le
   site.
2. Change aussi `APP_VERSION` dans `titan_app.py` (paquet dev) pour
   qu'il corresponde, avant de recompiler la nouvelle version à
   distribuer.

L'application compare les deux numéros : si celui du site est plus
récent que celui installé chez l'utilisateur, elle le prévient.

## Important : ce que ça fait, et ce que ça ne fait pas

Ce système **prévient** l'utilisateur qu'une nouvelle version existe et
lui montre un lien — il **ne télécharge ni n'installe rien tout seul**.
C'est à l'utilisateur de cliquer et de relancer l'installeur manuellement.
Aucune vraie mise à jour automatique n'est en place, volontairement :
c'est plus simple, plus sûr, et suffisant pour un projet de cette
taille.
