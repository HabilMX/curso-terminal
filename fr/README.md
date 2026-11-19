# Le terminal à partir de zéro

**Par Dorian Chávez, fondateur de Hábil et architecte d'intégration.**

**Page du cours :** [habil.mx/fr/cours/terminal/](https://www.habil.mx/fr/cours/terminal/), avec le programme et les leçons en cinq langues.

**À qui s'adresse ce cours :** à celles et ceux qui programment et n'ont jamais travaillé dans un terminal. Aucune
connaissance de Linux n'est supposée.

**Ce que tu sauras à la fin :** entrer dans un terminal Linux sans écran et y travailler sans crainte. Tu te
déplaces dans les dossiers, tu crées, copies et supprimes en sachant ce que tu fais, tu lis des journaux,
tu trouves des fichiers et du texte, tu enchaînes des commandes, tu édites avec vi, tu changes du texte avec
sed, tu lis et modifies des permissions, et tu vois et arrêtes des processus avec htop.

**Ce dont tu as besoin avant de commencer :** un ordinateur avec Docker. La [leçon 00](00-tu-terminal-de-practica.md)
t'explique comment l'obtenir sous Linux, sur Mac et sous Windows, et te laisse ton terminal d'entraînement prêt.

## Où tu t'entraînes

Dans un **conteneur** : un petit ordinateur Linux qui vit à l'intérieur du tien. C'est un Ubuntu 26.04 LTS, il se
lance avec une seule commande (`./practicar`) et il est séparé de tes fichiers : aucun dossier de ton ordinateur ne
lui est connecté et, à l'intérieur, il n'y a pas de réseau. Si tu le casses, tu le jettes et tu recommences. Tout
le monde s'entraîne là, y compris celles et ceux qui ont déjà Linux ou un Mac : ainsi, tout le groupe s'entraîne dans le
même environnement.

## Comment tu apprends ici

- **Une idée nouvelle par leçon.** Le reste est donné et expliqué.
- **Tu prédis, tu lances, tu compares.** Avant chaque commande, tu notes ce que tu crois qu'il va se
  passer ; ensuite tu le vérifies et tu l'expliques avec tes propres mots.
- **Ça se vérifie tout seul.** Chaque leçon vient avec un `verifica` qui examine l'état de ton terminal
  d'entraînement et te dit ce qui manque. Il n'évalue ni ce que tu prédis ni ce que tu expliques : cela, tu le
  compares toi-même, à la fin de chaque leçon.
- **Avec l'IA, à sa place.** Un assistant d'IA, comme Claude Code, peut t'expliquer une commande. Mais dans chaque
  leçon il y a au moins un exercice qui se fait sans IA, et toute commande qui supprime ou modifie des permissions, tu
  la tapes toi-même, à la main.

## Les leçons

Elles se font dans l'ordre : chacune part de ce que la précédente a laissé. Commence par la
[00](00-tu-terminal-de-practica.md), qui laisse prêt ton terminal d'entraînement.

| # | Leçon | L'idée nouvelle |
|---|---|---|
| 00 | [Ton terminal d'entraînement](00-tu-terminal-de-practica.md) | Dans ce terminal, il n'y a ni bureau ni fenêtres, seulement du texte : il te dit qui tu es et où tu es sur chaque ligne, et tu lui parles avec des commandes |
| 01 | [Se déplacer sans voir](01-moverte-sin-ver.md) | L'arbre des dossiers, et le chemin absolu face au chemin relatif |
| 02 | [Créer, copier, déplacer et supprimer sans crainte](02-crear-copiar-mover-borrar.md) | Dans le terminal, il n'y a pas de corbeille : tu réfléchis avant de supprimer |
| 03 | Voir ce qu'il y a dedans | Presque tout, sur un serveur, est du texte, et tu le lis sans ouvrir d'éditeur |
| 04 | Trouver des fichiers : find | `find` parcourt l'arbre et filtre selon ce que tu lui dis |
| 05 | Chercher à l'intérieur : grep | `grep` cherche du texte à l'intérieur des fichiers ; `find` cherche les fichiers |
| 06 | Tubes et redirection | La sortie d'une commande peut être l'entrée de la suivante |
| 07 | Éditer sur le serveur : vi | vi a des modes ; la moitié des problèmes vient de ne pas savoir dans lequel tu es |
| 08 | Changer du texte sans l'ouvrir : sed | Changer beaucoup de lignes d'un seul coup, en testant avant d'écrire |
| 09 | Permissions de base | Chaque fichier dit qui peut le lire, l'écrire et l'exécuter |
| 10 | Processus : ps, htop et kill | Un programme qui tourne est un processus, avec un numéro et un propriétaire, et tu peux lui demander de s'arrêter |
| 11 | Tout réunir : le journal du problème | Aucune idée nouvelle : tu utilises tout ce qui précède pour trouver et réparer une panne |

Les leçons sans lien sont en préparation et seront publiées dans l'ordre.

## Ce qui vient ensuite

Ce cours est la porte d'entrée des cours [Docker](https://www.habil.mx/fr/cours/docker/) et
[Kubernetes](https://www.habil.mx/fr/cours/kubernetes/) de la maison.
