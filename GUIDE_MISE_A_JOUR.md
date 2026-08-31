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

### En un clic avec changer_version.py

Plutôt que de modifier les fichiers un par un, utilise
`changer_version.py` (paquet dev) : il met à jour les trois fichiers
d'un coup, et peut même publier directement `version.json` sur GitHub
sans que tu aies besoin d'ouvrir le site.

Pour activer cette dernière option, il faut un **jeton d'accès
personnel** GitHub, à créer une seule fois :

1. Sur github.com : photo de profil → *Settings* → tout en bas,
   *Developer settings* → *Personal access tokens* → *Fine-grained
   tokens* → *Generate new token*.
2. Donne-lui un nom (ex. "changer_version"), une expiration (ex. 1 an).
3. Dans *Repository access*, choisis *Only select repositories* et
   sélectionne uniquement `Tanducteur_JDR`.
4. Dans *Permissions* → *Repository permissions* → *Contents*, mets
   **Read and write**. Laisse tout le reste sans accès.
5. Génère le jeton, copie-le (il ne sera plus jamais réaffiché).

Colle-le dans `changer_version.py`, coche *Publier version.json
directement sur GitHub*, et au besoin *Retenir le jeton sur cet
ordinateur* pour ne pas avoir à le recoller à chaque fois. Le jeton
reste uniquement en local (dans `.changer_version_config.json`, à côté
du script) — ne partage jamais ce fichier.

### L'installeur (.exe) se publie tout seul aussi

Une fois "Retenir le jeton" coché ci-dessus, `build_exe.bat` publie
automatiquement `TraducteurJDR_Setup.exe` dans `downloads/` du dépôt à
chaque fois qu'il termine de le fabriquer (via
`publish_setup_to_github.py`, appelé tout seul en fin de script). Plus
besoin de le copier à la main sur le site : lance `build_exe.bat`,
attends la fin, et le nouveau setup est déjà en ligne.

Si le jeton n'est pas configuré, cette étape est simplement ignorée
(avec un message dans la console) — rien ne bloque le build, il suffit
de copier le fichier à la main comme avant.

## Important : ce que ça fait, et ce que ça ne fait pas

Ce système **prévient** l'utilisateur qu'une nouvelle version existe et
lui montre un lien — il **ne télécharge ni n'installe rien tout seul**
chez l'utilisateur. C'est à lui de cliquer et de relancer l'installeur
manuellement. Aucune vraie mise à jour automatique n'est en place côté
utilisateur, volontairement : c'est plus simple, plus sûr, et suffisant
pour un projet de cette taille.

Côté publication en revanche (toi, le développeur), `version.json` ET
`TraducteurJDR_Setup.exe` partent directement sur GitHub dès que tu
lances `changer_version.py` puis `build_exe.bat` — plus besoin de
manipuler le site à la main pour une mise à jour normale.
