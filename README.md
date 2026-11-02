# Portfolio — Naya Fajloun

Site statique d'une seule page, bilingue anglais / arabe (bascule RTL incluse).
Aucune dépendance, aucun build : ce sont des fichiers servis tels quels.

## Déploiement sur Vercel

### Option 1 — par le tableau de bord

1. Pousser ce dossier sur un dépôt GitHub / GitLab / Bitbucket.
2. Sur vercel.com : **Add New → Project**, importer le dépôt.
3. Framework Preset : **Other**. Laisser Build Command et Output Directory vides.
4. **Deploy**.

### Option 2 — en ligne de commande

```bash
npm i -g vercel
cd <ce-dossier>
vercel          # déploiement de préversion
vercel --prod   # mise en production
```

Répondre « Other » si Vercel demande un framework, et laisser la commande de build vide.

## Après la mise en ligne du domaine

Deux balises se complètent une fois l'adresse définitive connue. Les ajouter dans
`<head>` de `index.html`, en remplaçant le domaine :

```html
<link rel="canonical" href="https://exemple.com/">
<meta property="og:url" content="https://exemple.com/">
```

L'image de partage (`og.jpg`) est déjà référencée en chemin absolu `/og.jpg`,
elle fonctionne sans modification.

## Contenu du dossier

| Fichier | Rôle |
|---|---|
| `index.html` | la page entière : structure, styles et script inclus |
| `media/reel-01…05.mp4` | les cinq vidéos, lues directement dans la page |
| `media/poster-01…05.jpg` | vignettes affichées avant lecture |
| `naya-fajloun-cv.pdf` | CV ouvert depuis la section « Full CV » |
| `og.jpg` | aperçu affiché par WhatsApp, LinkedIn, Instagram, Twitter |
| `favicon.svg`, `apple-touch-icon.png`, `icon-192.png`, `icon-512.png` | icônes |
| `site.webmanifest` | manifeste pour l'ajout à l'écran d'accueil |
| `vercel.json` | URLs propres, en-têtes de sécurité, cache long sur les médias |
| `robots.txt` | indexation ouverte |

## Modifications courantes

**Ajouter une vidéo.** Compresser d'abord, sinon la page s'alourdit :

```bash
ffmpeg -i source.mp4 -vf scale=576:1024 -c:v libx264 -crf 26 -preset slow \
  -profile:v main -pix_fmt yuv420p -c:a aac -b:a 64k -movflags +faststart \
  media/reel-06.mp4
ffmpeg -ss 10 -i media/reel-06.mp4 -frames:v 1 -q:v 4 media/poster-06.jpg
```

Puis dupliquer un bloc `<div class="frame">` dans le groupe client voulu de la
section `#work`, et changer l'`id` de la balise `<video>` ainsi que l'attribut
`data-for` du bouton — les deux doivent correspondre, sinon le bouton de lecture
ne déclenche rien.

Les vidéos portent `preload="none"` : rien ne se télécharge avant un clic, donc
le poids total du dossier `media/` n'affecte pas la vitesse de chargement.
Au-delà de 8 Mo pour un seul fichier, mieux vaut quand même passer par Vimeo.

**Ajouter un client.** Dupliquer un bloc `<div class="group">` avec son
`<h3 class="grp">` : nom du client dans `.who`, type de contenu dans `.what`.

**Changer un texte.** Chaque élément bilingue porte `data-en` et `data-ar`. Le
contenu visible dans le HTML est la version anglaise ; l'arabe vient de
`data-ar`. Modifier les deux.

**Changer les couleurs.** Tout part des variables du bloc `:root` en haut de la
feuille de style. Le thème sombre reprend les mêmes noms dans les deux blocs
qui suivent — modifier les trois pour rester cohérent.
