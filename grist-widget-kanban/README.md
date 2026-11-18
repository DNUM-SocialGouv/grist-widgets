# grist-kanban-dsfr

Widget personnalisé Grist qui affiche une table sous forme de **kanban**, dans l'esprit des interfaces Airtable, avec le **Système de design de l'État (DSFR)**.

Un seul fichier (`index.html`), sans build ni dépendance à installer.

## Fonctionnalités

- **Colonnes du kanban** : alimentées par une colonne de type *Choice*, *ChoiceList*, *Ref* ou *RefList*.
- **Glisser-déposer** des cartes d'une colonne à l'autre : la valeur est mise à jour dans Grist.
  - Pour une *RefList* ou une *ChoiceList*, la carte quitte la colonne d'origine et rejoint la nouvelle, les autres valeurs sont conservées.
- **Couleurs des colonnes** : reprises des couleurs des choix définies dans Grist, sinon une palette DSFR par défaut.
- **Cartes configurables** :
  - le titre est choisi parmi les champs de la table ;
  - des champs complémentaires s'affichent sous le titre, ajoutés un à un et réordonnables par **glisser-déposer** (poignée ⠿) ;
  - chaque champ s'affiche avec son **titre** (libellé de la colonne) au-dessus de sa valeur ;
  - la forme de la valeur suit le type du champ (aucun réglage) : *pastille* colorée pour les choix et références, *date* avec icône pour les dates, *texte* sinon.
- **Fenêtre de détail** au clic sur une carte :
  - le titre est modifiable en haut de la fenêtre, ainsi que tous les champs (sauf les colonnes formules) ;
  - la **mise en page se règle directement dans la fenêtre** (bouton « Mise en page »), dans l'esprit des pages de détail d'Airtable : champs, **titres**, **sections** repliables et **séparateurs**, placés par glisser-déposer, avec une largeur au choix (demi-largeur ou pleine largeur) par champ.
- **Recherche** dans les titres et champs des cartes.

## Types de colonnes pris en charge dans la fenêtre de détail

| Type Grist | Éditeur |
| --- | --- |
| Text | zone de texte |
| Numeric, Int | champ numérique |
| Bool | case à cocher |
| Date, DateTime | sélecteur de date / date et heure |
| Choice | liste déroulante |
| ChoiceList | cases à cocher |
| Ref | liste déroulante (colonne affichée de la table référencée) |
| RefList | cases à cocher |
| Autres (pièces jointes…) | lecture seule |

## Installation

### 1. Héberger le widget (GitHub Pages)

1. Poussez ce dépôt sur GitHub.
2. Dans le dépôt : **Settings → Pages → Build and deployment**, source *Deploy from a branch*, branche `main`, dossier `/ (root)`.
3. Le widget est alors disponible à l'adresse :

   ```
   https://<utilisateur-ou-organisation>.github.io/grist-kanban-dsfr/
   ```

### 2. L'ajouter dans Grist

1. Dans votre document, ajoutez un widget de type **Custom** sur la table voulue.
2. Choisissez **Custom URL** et collez l'adresse GitHub Pages ci-dessus.
3. Réglez l'accès sur **Accès complet au document** (*Full document access*). Il est nécessaire pour lire les choix des colonnes et le contenu des tables référencées.

### 3. Configurer

Cliquez sur **Configurer** en haut du widget (ou sur l'option « Open configuration » du panneau Grist) :

1. **Colonnes du kanban** : choisissez la colonne Choice / Ref qui définit les colonnes.
2. **Titre de la carte** : choisissez le champ utilisé comme titre.
3. **Champs sous le titre** : ajoutez des champs un à un et ordonnez-les par glisser-déposer.

La configuration est enregistrée avec le widget dans le document Grist.

### 4. Mettre en page la fenêtre de détail

1. Cliquez sur une carte, puis sur **Mise en page** en haut de la fenêtre.
2. Une barre d'éléments d'une seule ligne apparaît (le corps de la fenêtre garde presque toute la hauteur) :
   - les boutons **Titre**, **Section**, **Séparateur** et **Table liée** ;
   - un champ **Rechercher un champ à ajouter…** : au clic ou à la saisie, une courte liste déroulante propose les champs de la table qui ne sont pas encore placés (filtre sans accent ni casse ; Échap ferme la liste).

   Glissez un élément (bouton ou résultat de recherche) à l'endroit voulu dans la fenêtre, ou cliquez dessus pour l'ajouter à la fin.
3. Chaque bloc a une poignée ⠿ pour le déplacer, un bouton pour sa largeur (champs) et un bouton **Retirer**. Le texte des titres et des sections se modifie directement.
4. Cliquez sur **Terminer la mise en page**. En lecture, les sections se replient d'un clic.

Quand un bloc est trop étroit, les boutons de sa barre passent en icônes seules (⚙ options, ✕ retirer…) ; le libellé s'affiche en info-bulle au survol. La largeur de chaque champ se choisit dans la liste de sa barre : **⅓, ½, ⅔ ou pleine largeur** (grille de 6 colonnes, en dehors comme à l'intérieur des sections). Un bloc se saisit par sa poignée ⠿ ou le libellé de sa barre, et peut être lâché sur un bloc, entre deux blocs ou sur le corps d'une section (le bloc le plus proche est alors visé). Pendant la mise en page, les valeurs déjà saisies mais pas encore enregistrées sont conservées. Une **section** peut être rendue **pliable** (bouton « Pliable » dans sa barre), éventuellement **repliée au départ** ; ses champs sont affichés sur un léger fond bleuté. Une section regroupe tous les blocs qui la suivent jusqu'à la section suivante : pour sortir un bloc d'une section, déplacez-le avant celle-ci ; déplacer une section déplace aussi ses blocs.

## Modifier les enregistrements d'une table liée

Cas d'usage : une table *Étude* avec une table *Étape* qui la référence (5 étapes par étude, distinguées par un champ *Type*), et un *Statut* à modifier pour chaque étape sans quitter la fenêtre de l'étude.

1. **Mise en page → Table liée.** Le widget détecte les colonnes des autres tables qui référencent la table du kanban (*Ref* ou *RefList*) ; s'il y en a plusieurs, choisissez la relation dans la liste « Table liée ».
2. Réglez le bloc : **titre du bloc**, **titre de chaque ligne** (par exemple *Type*), **tri** (par défaut sur le champ du titre ; un champ *Choice* suit l'ordre de ses valeurs dans Grist), puis **ajoutez les champs du modèle** avec « Ajouter un champ… » (recherche dans les champs de la table liée).
3. Ce **modèle est répété pour chaque enregistrement lié** : une étude à 5 étapes affiche 5 lignes, une étude à 3 étapes en affiche 3, une étude sans étape affiche « Aucun enregistrement lié ». Rien n'est créé automatiquement.
4. Les champs du modèle se règlent comme ceux de la carte (largeur ⅓ ½ ⅔ plein, nom affiché, icône, texte si vide, lecture seule, couleur des pastilles) et se réordonnent par glisser-déposer. Le bloc peut être rendu pliable et se déplace comme les autres blocs, y compris dans une section.
5. **Enregistrement** : mêmes règles que les champs de la carte (bouton « Modifier » puis Enregistrer, ou modification directe). Les modifications de la carte **et** des enregistrements liés partent **en une seule opération Grist** : tout est enregistré, ou rien (en cas de refus, par exemple par une règle d'accès, l'affichage revient à l'état précédent). Seuls les champs réellement modifiés sont envoyés. Les colonnes formules ne sont pas modifiables.
6. Les données de la table liée sont relues à chaque ouverture de la fenêtre. Pour la structure (nouveau choix, colonne renommée…), utilisez **Recharger**.

Les droits d'accès Grist sur la table liée s'appliquent comme pour n'importe quelle modification. Le widget doit avoir l'accès complet au document.

## Fenêtre de détail : même fonctionnement que la fiche

Chaque champ s'affiche selon son type, comme dans le widget « Fiche Produit » :

| Type | Lecture | Modification |
|---|---|---|
| Texte | texte (style de colonne Grist : couleur, gras…) ; lien cliquable si format « lien » | zone de texte qui s'agrandit |
| Nombre | format de la colonne (%, devise, séparateurs, décimales, négatifs entre parenthèses) | champ numérique ; en pourcentage on saisit `35` pour 35 %, avec l'unité affichée |
| Date / date-heure | format de la colonne (JJ/MM/AAAA, HH:mm par défaut), fuseau de la colonne | sélecteur de date / date-heure |
| Case à cocher | case désactivée | case |
| Choix | pastille aux couleurs de Grist | liste déroulante de pastilles colorées |
| Liste de choix | pastilles | pastilles avec ×, bouton + |
| Référence / liste | pastilles | pastilles avec ×, recherche dans la table liée |
| Pièces jointes | noms de fichiers à télécharger | retrait (×) et ajout de fichiers |

- **Modification** : bouton « Modifier » puis Enregistrer / Annuler (par défaut), ou directement dans les champs (Configurer → « Modification des données »). Un lecteur du document ne voit pas le bouton « Modifier ».
- **Champs calculés** (formules) : jamais modifiables, badge « Formule ». Le ⓘ affiche la description de la colonne définie dans Grist.
- **Options par champ** (Mise en page → bouton « Options » de chaque champ) : nom affiché, icône, texte si vide, lecture seule, et pour les références le champ affiché et la couleur des pastilles. Ces réglages restent dans le widget : ils ne modifient pas Grist.
- **Langue** : affichage à la française (la langue « en-US », réglage par défaut de Grist, est remplacée par fr-FR ; euro par défaut). Un format de date ou d'heure choisi dans Grist est respecté.

## Qui peut configurer ?

Le bouton **Configurer**, le panneau de configuration et le mode **Mise en page** de la fenêtre de détail ne sont visibles que par les **propriétaires du document**. Les autres utilisateurs voient le kanban et la fenêtre de détail, sans ces réglages.

- Le rôle est détecté au chargement, en essayant dans l'ordre : `grist.getUserSession()` (si disponible), l'API REST de Grist (`access`), un jeton d'accès Grist, puis une **sonde ACL** (lecture de la table `OwnerAccess`). Tant que le rôle n'est pas connu, ou si aucune méthode ne répond, les réglages restent **masqués**.
- **Sonde ACL (recommandée, la plus fiable)** : créez dans le document une table `OwnerAccess` d'une ligne, puis ajoutez une règle d'accès sur cette table : condition `user.Access != OWNER` → **refuser la lecture**. Seuls les propriétaires peuvent alors la lire, et le widget en déduit leur rôle. Si votre document contient déjà cette table (par exemple pour le widget « Fiche Produit »), il n'y a rien à faire.
- Ce masquage ne concerne que l'interface du widget : il ne remplace pas les droits définis dans Grist. La modification des données reste régie par les droits d'accès du document.
- **Dépannage** : si vous êtes propriétaire mais que les boutons n'apparaissent pas, ajoutez `?forceOwner=1` à l'URL du widget (par exemple `https://<compte>.github.io/grist-kanban-dsfr/?forceOwner=1`). Cela désactive la détection : les réglages sont alors visibles pour tous ceux qui ouvrent le document, donc à réserver aux documents dont tous les utilisateurs sont de confiance. Merci de signaler le cas, la détection doit fonctionner sans cela.

## Tester hors de Grist

Ouvrez `index.html` directement dans un navigateur : le widget passe en **mode démo** avec des données fictives (le badge « Mode démo » s'affiche en bas à gauche). Pratique pour travailler le design sans document Grist.

Le mode démo inclut une table liée « Etapes » (5 étapes pour le premier ticket) pour tester le bloc « Table liée ».

Pour simuler un autre rôle, ajoutez un paramètre à l'URL : `?role=owners`, `?role=editors`, `?role=viewers`, `?role=error` (échec de la détection), `?role=probe-owner` ou `?role=probe-other` (réponse sans champ `access`, détection par sonde de secours).

## Limites connues

- L'ordre des cartes à l'intérieur d'une colonne n'est pas géré.
- Le bloc « Table liée » ne crée ni ne supprime d'enregistrements : il affiche et modifie ceux qui existent. Les champs d'une table liée ne sont pas affichés sur les cartes du kanban.
- Les valeurs *DateTime* sont lues et écrites sans tenir compte du fuseau horaire de la colonne.
- Les styles DSFR et les icônes sont chargés depuis le CDN jsDelivr (`@gouvfr/dsfr@1.12.1`) : une connexion internet est nécessaire. Pour une utilisation hors ligne ou sans CDN, hébergez les fichiers DSFR avec le widget et modifiez les deux balises `<link>` en tête de `index.html`.
- Les cartes se mettent à jour en direct, mais la structure (liste des colonnes, choix d'une liste de choix, contenu des tables référencées) est lue au chargement : utilisez le bouton **Recharger** après l'avoir modifiée dans Grist (nouveau choix, nouvelle personne dans une table référencée, colonne renommée…).

## Note sur le DSFR

Le Système de design de l'État est destiné aux sites et services publics de l'administration. Vérifiez les [conditions d'utilisation](https://www.systeme-de-design.gouv.fr/) avant tout usage en dehors de ce cadre.

## Structure du dépôt

```
grist-kanban-dsfr/
├── index.html   # le widget (HTML, CSS et JS)
└── README.md
```
