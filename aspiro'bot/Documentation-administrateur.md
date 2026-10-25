# Aspiro'BOT — fiche technique administrateur

> Documentation réservée à l'administration du serveur. Cette fiche décrit le fonctionnement actuellement défini dans les scripts Aspiro'BOT.

## Présentation

L'Aspiro'BOT est une trémie spéciale qui attire les objets tombés à proximité et les dépose dans son inventaire. Il fonctionne à la redstone, peut être amélioré jusqu'au niveau 7 et affiche son état au-dessus de la trémie.

## Permissions

| Permission | Accès |
|---|---|
| `admin.aspirobot` | Commandes d'administration et accès au menu des Aspiro'BOT appartenant à un autre joueur |

Le propriétaire peut ouvrir le menu de son robot et le recharger sans permission administrative.

## Commandes d'administration

Toutes les commandes nécessitent `admin.aspirobot`.

| Commande | Fonction |
|---|---|
| `/adminaspirobot help` | Affiche l'aide en jeu. |
| `/adminaspirobot info <ID>` | Affiche les informations d'un robot enregistré. |
| `/adminaspirobot info` | Affiche les informations du robot visé par le bloc ciblé. |
| `/adminaspirobot give <joueur> <niveau>` | Donne à un joueur connecté un robot du niveau indiqué, de 1 à 7. La limite de robots du joueur s'applique aussi à cette commande. |
| `/adminaspirobot supprimer <ID>` | Supprime les données du robot et sa trémie si celle-ci est encore à l'emplacement enregistré. |
| `/adminaspirobot supprimer tout` | Supprime tous les robots enregistrés, leurs données et les trémies présentes à leurs emplacements enregistrés. |

### Attention : suppression globale

La commande `supprimer tout` est irréversible et ne demande pas de confirmation. Les inventaires des trémies supprimées ne sont pas conservés. Vérifiez les conséquences avec le joueur et sauvegardez le serveur avant une suppression de masse.

Lorsqu'une trémie n'est plus présente à l'emplacement enregistré, la suppression retire tout de même les données du robot et indique qu'aucune trémie n'a été trouvée.

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
- L'espace disponible dans la trémie est vérifié à chaque cycle. Si la trémie est pleine, le robot affiche l'erreur même lorsqu'aucun objet ne se trouve à proximité.
- Une redstone fournit `7 + (3 × niveau)` utilisations. Le robot commence avec 10 redstones en réserve et 5 utilisations déjà chargées.
- La limite de robots posés par joueur est égale à `2 × grade`.
- Deux robots ne peuvent pas être posés à moins de 8 blocs l'un de l'autre.
- Seul le propriétaire peut récupérer son robot en le cassant. La trémie spéciale est alors rendue sous forme d'objet avec son niveau et son identifiant.

## Rechargement

Le propriétaire recharge le robot en tenant de la redstone et en faisant **Maj + clic droit** sur sa trémie. Le stock est plafonné à `64 × niveau` redstones. La quantité qui dépasse la capacité restante n'est pas absorbée.

Le menu s'ouvre avec **Maj + clic droit** sans redstone en main. Il présente le stock, les utilisations restantes, l'autonomie estimée et les améliorations disponibles.

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

Le display au-dessus de la trémie montre le niveau, le temps avant le prochain cycle et le pourcentage d'énergie. Il peut aussi afficher :

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
| Le display indique que le robot est plein | Vérifier et vider l'inventaire de la trémie. L'erreur est détectée au cycle, même sans objet à aspirer. |
| Le display indique qu'il n'y a plus d'énergie | Vérifier le stock de redstone et les utilisations chargées, puis recharger le robot. |
| La trémie est présente mais le robot ne fonctionne plus | Utiliser `info <ID>` pour contrôler les index. Si les données sont incohérentes, noter l'ID et l'emplacement avant toute suppression. |
| La récupération par un joueur est refusée | Seul le propriétaire enregistré peut casser le robot. Un administrateur peut utiliser la commande de suppression après vérification. |

## Données à connaître

Les scripts identifient chaque robot par un ID. Les données comprennent notamment son propriétaire, son niveau, son emplacement, sa réserve de redstone, ses utilisations chargées, son compte à rebours et son display. Les commandes de suppression effacent ces données et retirent le robot des index internes.

Les scripts ne prévoient pas de confirmation pour les suppressions et ne restaurent pas le contenu d'une trémie détruite. Effectuez les opérations de maintenance avec prudence.
