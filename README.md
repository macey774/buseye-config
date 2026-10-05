# buseye-config

Contenu distant de l'application BusEye : modifier `config.json` met à jour tous les téléphones
(au prochain lancement, environ 5 minutes après l'enregistrement), sans nouvel APK.

## Modifier
1. Ouvre `config.json` sur GitHub, puis le crayon (Edit).
2. Change ce qu'il faut, puis **Commit changes**.
3. Si tu fais une erreur : onglet **History** du fichier, puis restaure la version précédente.
   L'application ignore une section invalide et garde l'ancien contenu.
4. Une section **supprimée** du fichier revient à la valeur intégrée dans l'application.

## Arrêts et lignes
- **stops** : `id` (jamais modifié, c'est lui qui retient les favoris), `name`, `lat`, `lng`.
  Déplacer ou renommer : change `name`, `lat` ou `lng`, jamais `id`.
  Nouvel arrêt : ajoute un bloc avec un `id` unique (minuscules, chiffres, tirets).
- **lines** : `id`, `name`, `color` (`#RRGGBB`) et `stops` (liste d'`id`, dans l'ordre).
  Un arrêt appartient à une seule ligne ; les autres vont dans « Autres arrêts ».

## Annonce (message en haut de l'accueil)
```json
"announcement": {
  "id": "maintenance-2026-10-10",
  "message": "Maintenance du suivi ce soir de 22h à 23h.",
  "level": "warning",
  "expiresAt": "2026-10-10T23:00:00+01:00"
}
```
`level` : `info`, `warning`, `error`. Change l'`id` à chaque nouvelle annonce (sinon ceux qui
l'ont fermée ne la reverront pas). Options : `startsAt`, `url` (https), `dismissible` (false).
`"announcement": null` = aucune annonce.

## Publicités
Chaque publicité de la liste **ads** est affichée avec l'étiquette « SPONSORISÉ ».
```json
{
  "id": "resto-chez-mado-oct",
  "placement": "home",
  "advertiser": "Chez Mado",
  "title": "Menu étudiant à 1 000 FCFA",
  "text": "Du lundi au vendredi, face à l'IUG.",
  "imageUrl": "https://raw.githubusercontent.com/macey774/buseye-config/main/ads/chez-mado-1.jpg",
  "url": "https://wa.me/237600000000",
  "cta": "Commander",
  "startsAt": "2026-10-10T00:00:00+01:00",
  "expiresAt": "2026-11-10T23:59:00+01:00"
}
```
- `placement` : `home` (grande carte sur l'accueil) ou `list` (petite bannière dans Mes bus, Arrêts, Lignes).
- Obligatoires : `id` (unique), `advertiser`, `text` (220 caractères max), `url` (https).
  Facultatifs : `title`, `imageUrl` (https), `cta` (texte du bouton), `startsAt`, `expiresAt`.
- La publicité s'arrête toute seule à `expiresAt`. Plusieurs publicités sur le même emplacement
  tournent à chaque lancement de l'application.
- **Image** : JPG, PNG ou WebP, 800 Ko maximum, format conseillé 16/9 (ex. 1280 x 720).
  Mets-la dans un dossier `ads/` du dépôt. Pour changer l'image d'une pub en cours, change son
  nom (ou ajoute `?v=2` à l'adresse) : les téléphones gardent l'ancienne en cache.
- **Vidéo** : mets le lien YouTube ou Facebook dans `url` et `"cta": "Voir la vidéo"`.
- L'exemple fourni (`exemple-desactive`) est expiré : il n'apparaît pas. Copie-le pour en créer un.
- Toucher la pub demande confirmation, puis ouvre le lien dans le navigateur.

## Raccourcis de l'accueil
`shortcuts` : 1 à 8 boutons (4 par ligne). Chacun a `label` (16 caractères max), `icon` et `action`.
- `action` : `my_buses`, `stops`, `lines`, `schedules`, `favorites`, `map`, `notifications`,
  `profile`, `help`, `community`, ou `url` (ajoute alors `"url": "https://..."`).
- `icon` : `bus`, `place`, `route`, `schedule`, `star`, `map`, `notifications`, `person`, `groups`,
  `link`, `campaign`, `info`, `help`, `school`, `restaurant`, `phone`, `wifi`, `shield`,
  `calendar`, `gift`, `cart`, `book`, `event`, `apps`.

## Aide
`help` : liste de `id` (unique), `title` (140 max), `body` (1 500 max). Affichée dans Profil, Aide.

## Liens
`links.community` : lien du groupe WhatsApp (https).

## Politique de confidentialité et conditions
`policy` : `version`, `date`, `highlights` (encadré « À retenir ») et `sections`
(`title` + `paragraphs`). **Change `version` pour redemander l'acceptation à tous les
utilisateurs** (obligatoire si tu modifies le fond du texte). Si tu ne changes que des fautes de
frappe, garde la même version.

## Versions
- **minVersion** : en dessous, écran bloquant « Mise à jour requise ». Il faut aussi `updateUrl`.
  À utiliser avec précaution : une version trop haute bloque tout le monde.
- **recommendedVersion** : simple bannière « nouvelle version disponible » (fermable).
- **updateUrl** : lien de téléchargement, par exemple
  `https://github.com/macey774/buseye-config/releases/latest/download/BusEye.apk`.

## Réglages
`settings` : `passageRadiusMeters` (20 à 200) et `offlineAfterMinutes` (1 à 30).

## Règles de sécurité
- Dépôt public : n'y mets jamais de mot de passe ni de donnée privée.
- Ne change pas `"schema": 1`.
- Tous les liens doivent commencer par `https://`.
- Coordonnée hors du Cameroun, `id` en double ou couleur mal écrite : la section arrêts/lignes
  est ignorée et l'ancienne version reste affichée.
