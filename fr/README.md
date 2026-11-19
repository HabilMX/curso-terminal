# Le terminal à partir de zéro

**Par Dorian Chávez, fondateur de Hábil et architecte d'intégration.**

**Page du cours :** [habil.mx/fr/cours/terminal/](https://www.habil.mx/fr/cours/terminal/), avec le programme et les leçons en cinq langues.

**À qui s'adresse ce cours :** à celles et ceux qui programment et n'ont jamais travaillé dans un terminal. Aucune
connaissance de Linux n'est supposée.

**Ce que vous saurez à la fin :** entrer dans un terminal Linux sans écran et y travailler sans crainte. Vous vous
déplacez dans les dossiers, vous créez, copiez et supprimez en sachant ce que vous faites, vous lisez des journaux,
vous trouvez des fichiers et du texte, vous enchaînez des commandes, vous éditez avec vi, vous changez du texte avec
sed, vous lisez et modifiez des permissions, et vous voyez et arrêtez des processus avec htop.

**Ce dont vous avez besoin avant de commencer :** un ordinateur avec Docker. La [leçon 00](00-tu-terminal-de-practica.md)
vous explique comment l'obtenir sous Linux, sur Mac et sous Windows, et vous laisse votre terminal d'entraînement prêt.

## Où vous vous entraînez

Dans un **conteneur** : un petit ordinateur Linux qui vit à l'intérieur du vôtre. C'est un Ubuntu 26.04 LTS, il se
lance avec une seule commande (`./practicar`) et il est séparé de vos fichiers : aucun dossier de votre ordinateur ne
lui est connecté et, à l'intérieur, il n'y a pas de réseau. Si vous le cassez, vous le jetez et vous recommencez. Tout
le monde s'entraîne là, y compris celles et ceux qui ont déjà Linux ou un Mac : ainsi, tout le groupe s'entraîne dans le
même environnement.

## Comment vous apprenez ici

- **Une idée nouvelle par leçon.** Le reste est donné et expliqué.
- **Vous prédisez, vous lancez, vous comparez.** Avant chaque commande, vous notez ce que vous croyez qu'il va se
  passer ; ensuite vous le vérifiez et vous l'expliquez avec vos propres mots.
- **Ça se vérifie tout seul.** Chaque leçon vient avec un `verifica` qui examine l'état de votre terminal
  d'entraînement et vous dit ce qui manque. Il n'évalue ni ce que vous prédisez ni ce que vous expliquez : cela, vous le
  comparez vous-même, à la fin de chaque leçon.
- **Avec l'IA, à sa place.** Un assistant d'IA, comme Claude Code, peut vous expliquer une commande. Mais dans chaque
  leçon il y a au moins un exercice qui se fait sans IA, et toute commande qui supprime ou modifie des permissions, vous
  la tapez vous-même, à la main.

## Les leçons

Elles se font dans l'ordre : chacune part de ce que la précédente a laissé. Commencez par la
[00](00-tu-terminal-de-practica.md), qui laisse prêt votre terminal d'entraînement.

| # | Leçon | L'idée nouvelle |
|---|---|---|
| 00 | [Votre terminal d'entraînement](00-tu-terminal-de-practica.md) | Dans ce terminal, il n'y a ni bureau ni fenêtres, seulement du texte : il vous dit qui vous êtes et où vous êtes sur chaque ligne, et vous lui parlez avec des commandes |
| 01 | [Se déplacer sans voir](01-moverte-sin-ver.md) | L'arbre des dossiers, et le chemin absolu face au chemin relatif |
| 02 | [Créer, copier, déplacer et supprimer sans crainte](02-crear-copiar-mover-borrar.md) | Dans le terminal, il n'y a pas de corbeille : vous réfléchissez avant de supprimer |
| 03 | Voir ce qu'il y a dedans | Presque tout, sur un serveur, est du texte, et vous le lisez sans ouvrir d'éditeur |
| 04 | Trouver des fichiers : find | `find` parcourt l'arbre et filtre selon ce que vous lui dites |
| 05 | Chercher à l'intérieur : grep | `grep` cherche du texte à l'intérieur des fichiers ; `find` cherche les fichiers |
| 06 | Tubes et redirection | La sortie d'une commande peut être l'entrée de la suivante |
| 07 | Éditer sur le serveur : vi | vi a des modes ; la moitié des problèmes vient de ne pas savoir dans lequel vous êtes |
| 08 | Changer du texte sans l'ouvrir : sed | Changer beaucoup de lignes d'un seul coup, en testant avant d'écrire |
| 09 | Permissions de base | Chaque fichier dit qui peut le lire, l'écrire et l'exécuter |
| 10 | Processus : ps, htop et kill | Un programme qui tourne est un processus, avec un numéro et un propriétaire, et vous pouvez lui demander de s'arrêter |
| 11 | Tout réunir : le journal du problème | Aucune idée nouvelle : vous utilisez tout ce qui précède pour trouver et réparer une panne |

Les leçons sans lien sont en préparation et seront publiées dans l'ordre.

## Ce qui vient ensuite

Ce cours est la porte d'entrée des cours [Docker](https://www.habil.mx/fr/cours/docker/) et
[Kubernetes](https://www.habil.mx/fr/cours/kubernetes/) de la maison.
