# buseye-config

Contenu distant de l'application BusEye : modifier `config.json` met à jour tous les téléphones
(au prochain lancement, quelques minutes après l'enregistrement), sans nouvel APK.

## Modifier
1. Ouvre `config.json` sur GitHub, puis le crayon (Edit).
2. Change ce qu'il faut, puis **Commit changes**.
3. Si tu fais une erreur : onglet **History** du fichier, puis restaure la version précédente.
   L'application ignore un fichier invalide et garde l'ancien contenu.

## Contenu
- **stops** : `id` (jamais modifié, c'est lui qui retient les favoris), `name`, `lat`, `lng`.
  Pour déplacer ou renommer un arrêt, change `name`, `lat` ou `lng`, jamais `id`.
  Nouvel arrêt : ajoute un bloc avec un `id` unique (lettres minuscules, chiffres, tirets).
- **lines** : `id`, `name`, `color` (`#RRGGBB`) et `stops` (liste d'`id`, dans l'ordre).
  Un arrêt appartient à une seule ligne ; les arrêts sans ligne vont dans « Autres arrêts ».
- **announcement** : message sur l'accueil. `null` = aucune annonce.
  ```json
  "announcement": {
    "id": "maintenance-2026-10-10",
    "message": "Maintenance du suivi ce soir de 22h à 23h.",
    "level": "warning",
    "expiresAt": "2026-10-10T23:00:00+01:00"
  }
  ```
  `level` : `info`, `warning` ou `error`. `id` doit changer à chaque nouvelle annonce
  (sinon ceux qui l'ont fermée ne la reverront pas). Options : `startsAt`, `url` (https),
  `dismissible` (false = non fermable).
- **minVersion** : en dessous, l'application affiche un écran bloquant « Mise à jour requise ».
  Il faut aussi renseigner `updateUrl` (https), sinon rien n'est bloqué.
  Utilise-le avec précaution : une version trop haute bloque tout le monde.
- **recommendedVersion** : simple bannière « nouvelle version disponible » (fermable).
- **updateUrl** : lien de téléchargement de la nouvelle version.
- **settings** : `passageRadiusMeters` (20 à 200) et `offlineAfterMinutes` (1 à 30).

## Règles de sécurité
- Dépôt public : n'y mets jamais de mot de passe ni de donnée privée.
- Ne change pas `"schema": 1`.
- Une coordonnée hors du Cameroun, un `id` en double ou une couleur mal écrite rendent la
  section « arrêts et lignes » invalide : l'ancienne version reste affichée.
