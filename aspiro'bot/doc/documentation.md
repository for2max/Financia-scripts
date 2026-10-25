# Aspiro'BOT — guide du joueur

## À quoi sert l'Aspiro'BOT ?

L'Aspiro'BOT est une trémie spéciale qui aspire automatiquement les objets tombés à proximité et les dépose dans son inventaire. Il fonctionne avec de la redstone et peut être amélioré jusqu'au niveau 7.

## Obtenir et poser un Aspiro'BOT

Lorsque vous recevez un Aspiro'BOT, placez-le comme une trémie normale. Respectez la limite de robots autorisée par votre grade : vous pouvez en poser jusqu'à deux fois votre niveau de grade.

Les Aspiro'BOT doivent être espacés d'au moins 8 blocs. Un robot ne peut pas être posé à proximité d'un autre.

## Utiliser le menu

Faites **Maj + clic droit** sur la trémie de votre Aspiro'BOT, sans tenir de redstone, pour ouvrir son menu.

Le menu indique notamment :

- le niveau du robot ;
- son stock de redstone et sa capacité maximale ;
- les utilisations restantes sur la redstone en cours ;
- l'autonomie estimée en pourcentage ;
- les améliorations disponibles, leur prix et leurs effets.

Cliquez sur l'amélioration suivante pour l'acheter. Le prix est prélevé sur votre solde et les niveaux doivent être débloqués dans l'ordre, jusqu'au niveau 7.

## Recharger en redstone

Tenez de la redstone, puis faites **Maj + clic droit** sur la trémie pour recharger votre Aspiro'BOT. Le robot absorbe uniquement la quantité nécessaire pour remplir sa réserve. Le stock maximal dépend de son niveau : `64 × niveau` redstones.

Chaque redstone fournit `7 + (3 × niveau)` utilisations. Le robot possède aussi des utilisations chargées séparément de son stock de redstone.

## Niveaux et caractéristiques

| Niveau | Rayon d'aspiration | Délai entre les cycles | Stock maximal | Utilisations par redstone |
|---:|---:|---:|---:|---:|
| 1 | 2 blocs | 10 secondes | 64 redstones | 10 |
| 2 | 3 blocs | 9 secondes | 128 redstones | 13 |
| 3 | 4 blocs | 8 secondes | 192 redstones | 16 |
| 4 | 5 blocs | 7 secondes | 256 redstones | 19 |
| 5 | 6 blocs | 6 secondes | 320 redstones | 22 |
| 6 | 7 blocs | 5 secondes | 384 redstones | 25 |
| 7 | 8 blocs | 4 secondes | 448 redstones | 28 |

Un cycle réussi consomme une utilisation, même s'il aspire plusieurs objets. Aucun objet et aucune utilisation ne sont consommés lorsqu'il n'y a rien à aspirer.

## Lire le display

Le texte au-dessus de votre robot affiche son niveau, le temps avant le prochain cycle et son autonomie énergétique.

Il peut également afficher :

- **ASPIRATION** : le robot vient d'effectuer un cycle réussi ;
- **L'automate est plein** : videz l'inventaire de la trémie ;
- **Plus d'énergie** : rechargez le robot en redstone.

Les messages temporaires restent affichés quelques secondes, puis le display revient aux informations habituelles.

## Récupérer son robot

Seul le propriétaire peut récupérer son Aspiro'BOT. Cassez sa trémie : le robot est rendu sous forme d'objet avec son niveau et peut être replacé.

## À savoir

- Le compte à rebours et les cycles sont suspendus lorsque vous êtes hors ligne. Le robot ne lance pas de cycle en votre absence.
- Si un message temporaire est déjà affiché au moment où vous vous déconnectez, il peut rester visible jusqu'à la fin de son délai.
- Un robot plein ne peut pas déposer les objets aspirés. Pensez à vider régulièrement son inventaire.
