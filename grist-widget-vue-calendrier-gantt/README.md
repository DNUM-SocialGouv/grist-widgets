# grist-calendrier-gantt-dsfr

Widget personnalisé Grist qui affiche une table sous forme de **calendrier** ou de **diagramme de Gantt**, dans l'esprit des interfaces Airtable, avec le **Système de design de l'État (DSFR)**.

Un seul fichier (`index.html`), sans build ni dépendance à installer. Les cartes, la fenêtre de détail, les droits et les formats sont ceux du widget [grist-kanban-dsfr](../grist-kanban-dsfr).

## Fonctionnalités

### Choix de l'affichage

Dans **Configurer → Affichage** : *Calendrier* ou *Gantt*.

### Calendrier

- **Mois** ou **semaine** (choix par défaut dans la configuration, modifiable à tout moment dans la barre d'outils).
- Les éléments qui durent plusieurs jours s'étirent sur toute leur durée, y compris d'une semaine à l'autre.
- **Glisser-déposer** d'un élément sur un autre jour : les dates de début et de fin sont décalées dans Grist.
- Vue **semaine** : cartes complètes (titre + champs choisis). Vue **mois** : barres compactes ; au **survol**, une bulle affiche le titre, les dates et les champs configurés de la carte. L'option « Afficher les champs dans la vue mois » permet aussi d'afficher des cartes complètes en permanence.

### Gantt

Deux modes (**Configurer → Gantt → Mode de défilement**, modifiable dans la barre d'outils) :

- **Défilement continu** : une seule frise avec les **mois**, les **semaines** (numéro ISO et date) et, à l'échelle « Jours », les jours, alignés sur la même grille. On défile vers la droite **ou vers la gauche** sans limite : la période s'agrandit automatiquement en approchant d'un bord. Quatre échelles : *Jours*, *Semaines*, *Mois* et *Année* (12 mois et plus d'un coup d'œil). Les flèches ‹ › font défiler, « Aujourd'hui » recentre.
- **Calendrier** : une période à la fois (**mois** ou **semaine**), avec les flèches ‹ › pour changer de période.

Dans les deux modes :

- **Lignes du Gantt** (**Configurer → Gantt → Lignes du Gantt**) : *une ligne par élément* (titre et champs choisis sous le titre) ou *une ligne par intervenant* (voir ci-dessous) ;
- **barres déplaçables** (glisser) et **étirables** (poignées aux deux extrémités) : les dates sont mises à jour dans Grist ;
- **regroupement des lignes** par une colonne Choice / Ref (facultatif, vue « une ligne par élément »), groupes repliables d'un clic ;
- week-ends estompés et ligne rouge pour aujourd'hui.

### Gantt par intervenant : la charge de chacun

Avec **Lignes du Gantt → Une ligne par intervenant (charge)**, le Gantt devient un planning de charge : **personnes en lignes, mois en colonnes**.

- **Une ligne par personne**, choisie dans une colonne *Référence* (ou Choix) : **Configurer → Gantt → Intervenant**. Toutes les personnes de la table liée apparaissent, **y compris celles sans étude** (on voit qui est libre ; une case permet de les masquer). Les études sans intervenant sont regroupées dans une ligne « Sans intervenant ».
- **Études empilées** : quand les études d'une même personne se chevauchent, elles sont placées sur plusieurs lignes superposées. Les barres se déplacent et s'étirent comme dans l'autre vue, et un clic ouvre la fenêtre de détail.
- **Couleur par intervenant** : chaque personne a sa couleur, reprise pour toutes ses barres et pour la pastille devant son nom. Le réglage « Couleur selon » ne s'applique pas à cette vue.
- **Charge = nombre d'études en parallèle**, calculé jour par jour :
  - une **bande de charge** sous chaque personne montre les périodes d'occupation : **verte** sous le maximum conseillé, **orange** à ce maximum, **rouge** au-dessus. Les trous sont les périodes libres. Le survol d'un tronçon indique le nombre d'études et leurs titres ;
  - une **pastille** devant le nom donne le **pic** de la personne (le plus grand nombre d'études en parallèle) ;
  - le **maximum conseillé** (2 par défaut) se règle dans la configuration.
- **Vue condensée** : trois **densités** (*Aérée*, *Compacte*, *Très compacte*), réglables dans la configuration et dans la barre d'outils, et l'échelle *Année* pour voir plus d'une année sur l'écran. Aux échelles *Mois* et *Année*, l'en-tête n'affiche que les mois.
- Fonctionne avec le défilement continu comme avec le mode calendrier (mois / semaine) du Gantt. Les études sans date n'ont pas de barre : elles restent accessibles par le bouton « N sans date ».
- Une étude confiée à plusieurs personnes (colonne *Liste de références*) apparaît sur la ligne de chacune.

### Cartes et fenêtre de détail

Identiques au kanban :

- le **titre** et les **champs sous le titre** se règlent dans la configuration ; leur forme suit le type du champ (pastille, date, texte…) ;
- un clic sur une carte, une barre ou une ligne du Gantt ouvre la **fenêtre de détail** : titre et champs modifiables, **mise en page** libre (champs, titres, sections repliables, séparateurs, largeurs ⅓ / ½ / ⅔ / pleine, options par champ), champs calculés en lecture seule, pièces jointes.

### Autres

- **Couleur selon** : une colonne Choice / Ref donne sa couleur aux éléments (couleurs des choix définies dans Grist, sinon palette DSFR).
- **Sans date** : les éléments sans date n'apparaissent pas dans le calendrier ; un bouton « N sans date » les liste (clic = ouverture). Dans le Gantt « une ligne par élément » ils restent en liste, avec la mention « Sans date » ; dans la vue par intervenant, ils sont accessibles par le bouton.
- **Recherche** : la **loupe** de la barre d'outils s'élargit en champ au clic (et se referme d'elle-même si on la quitte sans rien taper ; Échap la ferme). En tapant (titres et champs des cartes, sans tenir compte des accents ni des majuscules), le widget propose une liste de cartes. Un clic sur une proposition (ou Entrée pour la première) **déplace le calendrier ou le Gantt jusqu'à la carte**, si elle n'est pas dans la période affichée, et la fait clignoter. Si la carte n'a pas de date, le clic **ouvre sa fenêtre de détail**. La recherche ne masque aucune carte.

## Version

La version du widget s'affiche **en bas à droite**. Elle est définie par la constante `WIDGET_VERSION` en tête du script de `index.html` : à augmenter à chaque modification.

| Version | Contenu |
| --- | --- |
| 1.1.1 | Barre d'outils plus compacte : configuration réduite à une roue dentée, recherche réduite à une loupe qui s'élargit au clic, titre masqué si la place manque |
| 1.1.0 | Gantt par intervenant (charge, couleur par personne), densités, échelle « Année », version affichée |
| 1.0.0 | Calendrier (mois / semaine), Gantt (défilement / calendrier), recherche avec propositions, bulle au survol |

## Installation

1. Poussez ce dépôt sur GitHub, puis **Settings → Pages**, branche `main`, dossier `/ (root)`. Le widget est alors disponible sur `https://<utilisateur>.github.io/grist-calendrier-gantt-dsfr/`.
2. Dans Grist, ajoutez un widget **Custom** sur la table voulue, choisissez **Custom URL**, collez l'adresse, et réglez l'accès sur **Accès complet au document**.
3. Cliquez sur **Configurer** :
   1. **Affichage** : Calendrier ou Gantt ;
   2. **Dates** : colonne de début (Date ou Date et heure) et, facultativement, colonne de fin. Sans date de fin, chaque élément occupe un seul jour ;
   3. **Carte** : titre, couleur selon (facultatif), champs sous le titre ;
   4. réglages propres au calendrier ou au Gantt (pour le planning de charge : lignes par intervenant, colonne intervenant, maximum conseillé, densité).

À l'ouverture, le widget propose par défaut la première colonne de date comme début, la deuxième comme fin et la première colonne Choice comme couleur.

La configuration est enregistrée avec le widget dans le document Grist.

## Qui peut configurer ?

Comme pour le kanban : le bouton **Configurer** et le mode **Mise en page** de la fenêtre de détail sont réservés aux **propriétaires du document** (détection par l'API Grist, puis par la table `OwnerAccess` avec la règle `user.Access != OWNER → refuser la lecture`). `?forceOwner=1` dans l'URL désactive la détection pour dépanner. Les lecteurs du document peuvent naviguer mais pas déplacer les éléments ni modifier les cartes.

## Tester hors de Grist

Ouvrez `index.html` dans un navigateur : le widget passe en **mode démo** avec des données fictives centrées sur la date du jour (badge « Mode démo »). `?role=owners|editors|viewers` simule un rôle.

## Limites connues

- Les dates sont décalées d'un nombre entier de jours : une date-heure garde son heure, au changement d'heure d'été près.
- Un élément dont la date de fin précède la date de début est affiché sur un seul jour.
- La première semaine est le lundi ; la langue d'affichage est celle du document (« en-US » est remplacé par fr-FR).
- Dans la vue par intervenant, la charge compte les études, pas les jours de travail : deux études de durées très différentes pèsent autant. Déplacer une barre ne change pas son intervenant (à modifier dans la fenêtre de détail).
- Dans le Gantt, une barre ne s'étire que si une colonne de fin est choisie et si les colonnes de dates ne sont pas des formules (les éléments sont alors en lecture seule).
- La structure (colonnes, choix, tables référencées) est lue au chargement : utilisez **Recharger** après l'avoir modifiée dans Grist.
- Les styles DSFR sont chargés depuis le CDN jsDelivr (`@gouvfr/dsfr@1.12.1`) : une connexion internet est nécessaire.

## Note sur le DSFR

Le Système de design de l'État est destiné aux sites et services publics de l'administration. Vérifiez les [conditions d'utilisation](https://www.systeme-de-design.gouv.fr/) avant tout usage en dehors de ce cadre.

## Structure du dépôt

```
grist-calendrier-gantt-dsfr/
├── index.html   # le widget (HTML, CSS et JS)
└── README.md
```
