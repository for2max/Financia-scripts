# Aspiro'BOT — fiche technique administrateur

> Documentation réservée à l'administration du serveur. Cette fiche décrit le fonctionnement actuellement défini dans les scripts Aspiro'BOT.

## Présentation

L'Aspiro'BOT est un hopper spécial qui attire les objets tombés à proximité et les dépose dans son inventaire. Il fonctionne à la redstone, peut être amélioré jusqu'au niveau 7 et affiche son état au-dessus du hopper.

## Permissions

| Permission | Accès |
|---|---|
| `admin.aspirobot` | Commandes d'administration et accès au menu des Aspiro'BOT appartenant à un autre joueur |
| `premium.aspirobot` | Configuration du filtre d'aspiration sur ses Aspiro'BOT |

Le propriétaire peut ouvrir le menu de son robot et le recharger sans permission administrative.

Le filtre premium est réservé aux joueurs à qui le serveur attribue `premium.aspirobot`.

## Commandes d'administration

Toutes les commandes nécessitent `admin.aspirobot`.

| Commande | Fonction |
|---|---|
| `/adminaspirobot help` | Affiche l'aide en jeu. |
| `/adminaspirobot info <ID>` | Affiche les informations d'un robot enregistré. |
| `/adminaspirobot info` | Affiche les informations du robot visé par le bloc ciblé. |
| `/adminaspirobot give <joueur> <niveau>` | Donne à un joueur connecté un robot du niveau indiqué, de 1 à 7. La limite de robots du joueur s'applique aussi à cette commande. |
| `/adminaspirobot supprimer <ID>` | Supprime les données du robot et son hopper si celui-ci est encore à l'emplacement enregistré. |
| `/adminaspirobot supprimer tout` | Supprime tous les robots enregistrés, leurs données et les hoppers présents à leurs emplacements enregistrés. |

### Attention : suppression globale

La commande `supprimer tout` est irréversible et ne demande pas de confirmation. Les inventaires des hoppers supprimés ne sont pas conservés. Vérifiez les conséquences avec le joueur et sauvegardez le serveur avant une suppression de masse.

Lorsqu'un hopper n'est plus présent à l'emplacement enregistré, la suppression retire tout de même les données du robot et indique qu'aucun hopper n'a été trouvé.

## Journal des actions

Les actions suivantes sont enregistrées avec la date et l'heure dans le fichier `LOG.log` du dossier des scripts Skript :

- pose et retrait d'un Aspiro'BOT par un joueur ;
- commande administrative `give`, suppression d'un robot et suppression totale.

Les entrées comprennent les informations disponibles sur l'acteur, le robot et le résultat de la suppression. Skript ajoute automatiquement l'extension `.log` et met le nom du fichier en minuscules.

## Fonctionnement et caractéristiques

| Niveau | Rayon d'aspiration | Délai entre les cycles | Stock maximal de redstone | Utilisations par redstone |
|---:|---:|---:|---:|---:|
| 1 | 2 blocs | 10 secondes | 64 | 10 |
| 2 | 3 blocs | 9 secondes | 128 | 13 |
| 3 | 4 blocs | 8 secondes | 192 | 16 |
| 4 | 5 blocs | 7 secondes | 256 | 19 |
| 5 | 6 blocs | 6 secondes | 320 | 22 |
| 6 | 7 blocs | 5 secondes | 384 | 25 |
| 7 | 8 blocs | 4 secondes | 448 | 28 |

- Le rayon est égal au niveau + 1.
- Un cycle réussi consomme une utilisation, quel que soit le nombre d'objets aspirés durant ce cycle.
- Aucun objet n'est aspiré et aucune utilisation n'est consommée si aucun objet n'est présent dans le rayon.
- L'espace disponible dans le hopper est vérifié lorsqu'il y a au moins un objet admissible à aspirer. Si le hopper est plein, le robot affiche l'erreur.
- Une redstone fournit `7 + (3 × niveau)` utilisations. Le robot commence avec 10 redstones en réserve et 5 utilisations déjà chargées.
- Deux robots ne peuvent pas être posés à moins de 8 blocs l'un de l'autre.
- Seul le propriétaire peut récupérer son robot en le cassant. Le hopper spécial est alors rendu sous forme d'objet avec son niveau et son identifiant.
- Des particules apparaissent à la position de chaque objet aspiré.

## Rechargement

Le propriétaire recharge le robot en tenant de la redstone et en faisant **Maj + clic droit** sur son hopper. Le stock est plafonné à `64 × niveau` redstones. La quantité qui dépasse la capacité restante n'est pas absorbée.

Le menu s'ouvre avec **Maj + clic droit** sans redstone en main. Il présente le stock, les utilisations restantes, l'autonomie estimée, le filtre et les améliorations disponibles.

## Filtre premium

Le propriétaire ayant la permission `premium.aspirobot` peut ajouter ou retirer jusqu'à deux types d'objet du filtre en les tenant et en faisant **Maj + clic gauche** sur son robot. L'objet tenu n'est pas consommé. Quand la liste contient au moins un type, seuls ces objets sont aspirés ; une liste vide désactive le filtre et restaure l'aspiration normale. Le bouton « Filtre d'aspiration » du menu permet de vider la liste. Les administrateurs ayant `admin.aspirobot` peuvent également régler le filtre.

## Améliorations

| Niveau atteint | Prix |
|---:|---:|
| 2 | 100 $ |
| 3 | 250 $ |
| 4 | 500 $ |
| 5 | 1 000 $ |
| 6 | 2 000 $ |
| 7 | 4 000 $ |

Le prix est prélevé sur le solde du joueur. Chaque niveau débloque le suivant ; le niveau 7 est le maximum.

## Display et états

Le display au-dessus du hopper montre le niveau, le temps avant le prochain cycle et le pourcentage d'énergie. Il peut aussi afficher :

- **ASPIRATION** pendant 3 secondes après un cycle réussi ;
- **L'automate est plein** pendant 3 secondes lorsque l'inventaire ne peut pas recevoir d'objet ;
- **Plus d'énergie** pendant 3 secondes lorsque la réserve est vide et qu'aucune utilisation chargée ne reste.

Après l'affichage d'une erreur ou d'une aspiration, le robot reprend le compte à rebours normal.

## Joueur hors ligne

Le display continue d'être actualisé. Tant que le propriétaire est hors ligne, le compte à rebours normal est suspendu : aucun nouveau cycle n'est lancé et les contrôles d'espace et d'énergie ne sont pas exécutés. En revanche, un message d'erreur ou d'aspiration déjà affiché termine son délai de 3 secondes.

## Diagnostic rapide

La commande `/adminaspirobot info <ID>` fournit notamment le propriétaire, le niveau, l'emplacement, la réserve, les utilisations chargées, le temps restant et l'état des index internes du propriétaire et de la liste globale.

| Symptôme | Vérifications conseillées |
|---|---|
| Le robot ne démarre pas | Vérifier que le propriétaire est connecté et que le robot figure dans la liste globale. |
| Le display indique que le robot est plein | Vérifier et vider l'inventaire du hopper. |
| Le display indique qu'il n'y a plus d'énergie | Vérifier le stock de redstone et les utilisations chargées, puis recharger le robot. |
| Le hopper est présent mais le robot ne fonctionne plus | Utiliser `info <ID>` pour contrôler les index. Si les données sont incohérentes, noter l'ID et l'emplacement avant toute suppression. |
| La récupération par un joueur est refusée | Seul le propriétaire enregistré peut casser le robot. Un administrateur peut utiliser la commande de suppression après vérification. |

## Données à connaître

Les scripts identifient chaque robot par un ID. Les données comprennent notamment son propriétaire, son niveau, son emplacement, sa réserve de redstone, ses utilisations chargées, son compte à rebours et son display. Les commandes de suppression effacent ces données et retirent le robot des index internes.

Les scripts ne prévoient pas de confirmation pour les suppressions et ne restaurent pas le contenu d'un hopper détruit. Effectuez les opérations de maintenance avec prudence.
