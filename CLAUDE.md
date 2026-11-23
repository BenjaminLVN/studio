# Le studio : consignes pour tout travail sur ce dépôt

À lire avant de coder, de rédiger ou de proposer un hébergement. Réponses en français, tutoiement pour tout le monde.

Attention : ce dépôt est **public** (relevé le 17/11, contraire à la règle de protection du projet, question ouverte
pour Benjamin). N'y écris jamais une clé, un lien du Studio (il contient sa clé), une adresse privée ni une donnée
personnelle.

## Le studio est déjà en ligne : ne pas proposer d'autre hébergement

Le studio existe sous deux formes, toutes deux déjà en service. Ne propose ni GitHub Pages, ni Netlify, ni Vercel, ni
un projet Cloudflare séparé, ni un serveur à monter.

| Quoi | Où |
|---|---|
| Le studio dans le QG | **https://qg-du-centre.pages.dev/le-studio/** : l'écran du QG (rail, filtres du Studio dans le panneau) qui affiche le Studio en mode intégré (`/studio/?integre=1`). `/studio/` tapé seul y renvoie |
| Plateforme du QG | Cloudflare Pages, projet `qg-du-centre` (branche de production `main` du dépôt QG, privé) ; copie dans `apps/studio/` (notice de référence : `apps/studio/LISEZMOI.md` du QG) |
| Le studio seul | le lien habituel des coureurs (clé publique dans le lien) ; les vidéos, tags et commentaires sont tenus par le script Google Apps Script de la photothèque. **On ne touche jamais à ce script ni à son déploiement** |
| Qui ouvre le studio du QG | l'encadrement (code PIN confirmé) et les coureurs du 420 (DECISIONS 5.6), en lecture ; taguer et commenter se font par le lien habituel |
| Protection | dans le QG : derrière la connexion (sans session : « Accès réservé », 403) ; la clé publique vient de `GET /api/studio` du QG, jamais d'une clé de gestion |

Vérifié le 20/11/2026 : `/le-studio/` répond 403 « Accès réservé » sans connexion. Pas vérifié : l'adresse exacte où
la page du Studio seul est servie (elle est dans le lien des coureurs, enregistré dans le QG).

## Comment une modification arrive en ligne

1. On modifie **ce dépôt** (`BenjaminLVN/studio`, `index.html`, source de référence), branche puis PR fusionnée sur `main`.
2. On la reporte dans le QG : `node scripts/studio.mjs depuis` dans le dépôt QG (refait `apps/studio/index.html` à
   partir d'ici, avec les retouches du mode intégré de `apps/studio/integre.mjs`), puis `npm run construire` et les
   essais d'écran. **Jamais de correction du Studio lui-même dans `apps/studio`.**
3. La PR du QG fusionnée sur `main`, Cloudflare Pages reconstruit tout seul. Le contrôle « Studio à jour » du QG
   (`.github/workflows/studio.yml`, chaque jour) passe au rouge tant que la copie du QG n'est pas la dernière version d'ici.

Le report est noté dans `erp/REGISTRE.md` des fichiers du projet « QG du Centre » (ligne « Studio » du tableau des
briques, et une ligne au journal).

Les PR se fusionnent seules quand tout est vert (accord de Benjamin du 15/11) : ne demande jamais à Benjamin de fusionner
ni de déployer.

## Relation avec le QG : une demande, les deux côtés

Règle commune des applis reliées (demandée par Benjamin le 20/11, DECISIONS 3.19 ; fiche de référence :
`docs/applis-reliees.md` du dépôt QG, liste contrôlée : `apps/applis-reliees.json` du QG).

- **Toute demande qui touche cette appli se fait des deux côtés, dans le même fil** : la modification ici, puis son
  report côté QG ; et inversement, tout changement du QG qui change ce que cette appli reçoit, affiche ou envoie
  (adresse, API, droits, cadre, retour au QG, données échangées) se reporte ici et dans ce fichier.
- Un fil qui laisse un côté en retard l'écrit au registre (« à reporter dans … »).
- Une adresse, une API ou un retour au QG qui change se change aussi dans `apps/applis-reliees.json` du QG : l'essai
  `essais/applis-reliees.test.ts` et le contrôle en ligne de chaque jour (`.github/workflows/applis.yml`) s'en servent.
- Une donnée, un seul propriétaire (DECISIONS 3.15) : cette appli lit les données du QG et n'écrit jamais chez lui
  autrement que par son API.

## Liens et filtres dans l'adresse (21/11)

- `?media=<id>` ouvre un média ; le bouton « Copier le lien » de la visionneuse le donne (clé publique seule, jamais
  la clé de gestion). `?selection=favoris` (★) ou `coeur` et `?allure=` filtrent au départ, comme `vent`, `theme`,
  `equipage`, `date`, `type`.
- Côté QG : la prépa de nav choisit ses vidéos de référence dans le Studio intégré (message `etat`, champ `choix`, une
  retouche du mode intégré), et le lien d'une vidéo y est `/le-studio/?media=<id>`. Détail : `apps/studio/LISEZMOI.md`
  du dépôt QG.

## Ce que le QG lit du Studio : les News du coureur (23/11)

Les News du QG (DECISIONS 5.16) disent au coureur qu'une vidéo a été publiée pour lui ici. Le studio le dit au QG par
son API habituelle, sans rien de nouveau dans le script : `GET /api/studio/nouveautes` du QG lit le catalogue avec la
clé publique, comme cette page (`POST` à l'adresse de `const API`, `{fn:'getData', args:[cle], gestion:'', p:{cle}}`),
et n'en garde que `id`, `type`, `date`, `crews`, `riders` des médias récents et « Hors dates ». Le QG décide ensuite
ce qui est pour le coureur (son équipage, ou son prénom dans « Coureurs ») et ouvre la vidéo par `/le-studio/?media=<id>`.

**À reporter côté QG dans le même fil** si l'un de ces points change ici : l'adresse `/exec` de `const API` (l'essai
`essais/studio-nouveautes.test.ts` du QG la compare à `apps/studio/index.html` et passe au rouge au report), la forme
de la réponse de getData, les noms des champs ci-dessus, le libellé « Hors dates », ou le paramètre `?media=`. Détail :
`apps/studio/LISEZMOI.md` du QG, partie « Les News du coureur ».

## Où lire le reste

- Le studio dans le QG (mode intégré, contrat, vidéo, cadre) : `apps/studio/LISEZMOI.md` du dépôt QG.
- Décisions validées : `erp/DECISIONS.md` des fichiers du projet « QG du Centre » (couleurs : 13.6, choix D du 16/11) ;
  rien n'y change sans l'accord écrit de Benjamin.
