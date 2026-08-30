# Site de téléchargement — mode d'emploi

Ce dossier est un site statique (une seule page). Aucun serveur
particulier n'est nécessaire : n'importe quel hébergement de fichiers
statiques fonctionne.

## Ce qui est déjà prêt

- `index.html` — la page, avec toutes les images qu'il lui faut dans
  `assets/`.
- `downloads/traducteur_jdr_user_linux.zip` — la version Linux, prête
  à télécharger telle quelle.
- `downloads/LICENCE.txt` — la licence.
- `version.json` — pour le système de mise à jour de l'application, voir
  `GUIDE_MISE_A_JOUR.md` pour l'activer (deux-trois valeurs à remplacer).

Le zip des sources (code, utile pour toi pour recompiler) n'est
**volontairement pas** dans ce dossier ni lié sur le site : il ne doit
pas être accessible publiquement. Garde-le de ton côté.

## Ce qu'il te reste à faire : le fichier Windows

Le bouton principal du site pointe vers
`downloads/TraducteurJDR_Setup.exe`, qui **n'existe pas encore dans ce
dossier** : je ne peux pas fabriquer ce fichier depuis mon
environnement (voir README.md du paquet dev, section installeur). Pour
le compléter :

1. Sur une machine Windows, décompresse le paquet dev (fourni à part,
   ne pas mettre en ligne).
2. Installe NSIS si besoin : <https://nsis.sourceforge.io/Download>
3. Lance `build_exe.bat`. Il produit automatiquement
   `TraducteurJDR_Setup.exe` (grâce à NSIS détecté).
4. Copie ce fichier ici, dans `downloads/TraducteurJDR_Setup.exe`.

Une fois ce fichier ajouté, le bouton « Télécharger pour Windows »
fonctionne immédiatement — rien d'autre à changer sur le site.

## Héberger le site

N'importe laquelle de ces options gratuites convient :

- **GitHub Pages** : crée un dépôt, mets-y tout le contenu de ce
  dossier (`index.html`, `assets/`, `downloads/`), active Pages dans les
  paramètres du dépôt.
- **Netlify / Vercel** : glisse-dépose ce dossier sur leur interface
  (« déploiement manuel » / drag-and-drop), aucune configuration requise.
- **Ton propre hébergement web** : dépose ce dossier tel quel à la
  racine (ou dans un sous-dossier) de ton espace web par FTP.

Dans tous les cas, garde la même structure de dossiers
(`index.html` + `assets/` + `downloads/` côte à côte) : les chemins dans
la page sont relatifs, donc rien à reconfigurer.

## Mettre à jour le site plus tard

Si tu ajoutes une langue, améliores l'appli, etc. : reconstruis les
paquets (dev/Linux/installeur), remplace les fichiers dans
`downloads/`, et republie le dossier. Le contenu de la page
(`index.html`) n'a pas besoin de changer sauf si tu veux mettre à jour
les captures d'écran ou le texte.
