# Leçon 01 — Se déplacer sans voir

**Par Dorian Chávez, fondateur de Hábil et architecte d'intégration.**

**Avant de lire, cinq mots.** Une ligne chacun ; les autres arrivent à l'étape où ils servent.

- **Dossier :** un endroit qui garde des fichiers et d'autres dossiers. Dans le terminal et dans les manuels, vous le
  verrez sous le nom de *directory*, « répertoire ». C'est la même chose.
- **Arbre :** la façon dont les dossiers sont rangés : les uns dans les autres, comme les branches d'un arbre.
- **Racine :** le dossier tout en haut, celui qui contient tous les autres. Elle s'écrit avec une seule barre oblique : `/`.
- **Dossier actuel :** le dossier où vous vous tenez en ce moment. C'est celui que `pwd` répond.
- **Chemin :** les indications pour arriver à un dossier ou à un fichier : des noms de dossiers séparés par des barres
  obliques. Par exemple, `pagos/bitacoras`.

**Ce que vous en retirez :** vous vous déplacez dans les dossiers sans les voir, avec `cd` ; vous savez dans lequel vous
allez vous retrouver avant d'appuyer sur Entrée ; vous voyez ce qu'il y a dans un dossier avec `ls`, y compris ce qui
est caché ; vous lisez chaque colonne de `ls -l` ; et vous écrivez le chemin vers un dossier de deux façons.

**Idée nouvelle de cette leçon (la seule qui est évaluée) :** les dossiers forment un arbre, et on arrive à un endroit
de cet arbre avec deux types de chemin : l'**absolu**, qui part de la racine, et le **relatif**, qui part de l'endroit
où vous vous tenez.

**Dans quel ordre :** vous montez un labyrinthe de dossiers (étape 1), vous le parcourez avec des chemins relatifs
(étape 2) et avec des chemins absolus (étape 3), vous regardez ce qu'il y a dans un dossier et vous lisez ses colonnes
(étape 4), et à la fin vous notez quatre réponses que `verifica` contrôle (étape 5).

**Une note sur la langue du labyrinthe.** Comme dans la leçon 00, les messages du cours et les noms du labyrinthe sont
en espagnol et font partie du code : ils ne sont donc pas traduits. Les messages apparaissent exactement comme le
terminal les affiche, avec une glose en français, en italique, sous chaque bloc, ou entre parenthèses à l'intérieur d'un
tableau. Les noms du labyrinthe sont glosés une seule fois, à l'étape 1 : `pagos` (« paiements »), `bitacoras`
(« journaux »), `guardia` (« astreinte »), `marzo.log` (« journal de mars ») et `.pendiente` (« en attente »), entre
autres. Il en va de même pour les quatre clés de la feuille de réponses, à l'étape 5 : `mas-grande` (« le plus grand »),
`oculto` (« caché »), `ruta-absoluta` (« chemin absolu ») et `ruta-relativa` (« chemin relatif »).

---

## La scène

*Scénario inventé ; les écrans qui apparaissent dans la leçon sont réels.*

Mardi, 23 h 47. Lucía est déjà entrée sur le serveur. Elle a déjà lu l'invite : elle sait qui elle est là-dedans et dans
quel dossier elle se tient. À l'autre bout du téléphone, on lui demande ceci :

« Va dans le dossier des journaux de paiements et dis-moi quel fichier pèse le plus lourd. »

« Va. » Sur son ordinateur, aller dans un dossier, c'est double-cliquer jusqu'à y arriver. Ici, il n'y a rien sur quoi
cliquer. Il y a la même ligne qu'avant, qui attend :

```text
operaciones@pagos-03:~$
```

Lucía doit arriver à un dossier qu'elle ne voit pas, par un chemin qu'elle ne voit pas, et dire quel fichier est le plus
gros sans se tromper, parce que, à l'autre bout, on va agir d'après ce qu'elle dira.

Cette leçon est ce parcours, dans un labyrinthe conçu pour qu'on s'y perde sans conséquences.

---

## Avant de commencer

**Ce dont vous avez besoin :** la leçon 00 faite, avec `verifica 00` au vert ; le matériel du cours à jour ; et du papier
et un crayon pour vos prédictions.

**Mettez à jour votre terminal d'entraînement.** L'image que vous avez construite à la leçon 00 ne contient pas ce que
demande cette leçon. Depuis votre ordinateur, dans le dossier du cours :

```bash
git pull
./practicar reconstruir
```

`git pull` rapporte du dépôt les nouveaux fichiers. `./practicar reconstruir` les met dans l'image et réutilise
normalement l'Ubuntu et les paquets déjà construits ; il n'a besoin du réseau que s'il lui en manque un. Sur le Mac du
test, avec tout le reste déjà construit, cela a pris moins de deux secondes. Cela se termine par `✅ Imagen lista.`
Si vous n'utilisez pas `git`, retéléchargez le ZIP du cours et lancez `./practicar reconstruir` dans le nouveau dossier.

Reconstruire n'efface pas votre travail : votre dossier `~/practica` vit à part, dans le volume de la leçon 00. Ensuite,
entrez comme vous savez déjà le faire, avec `./practicar`.

**Où cette leçon a été testée, et où cela manque :**

| Système | État |
|---|---|
| Mac avec Apple silicon, Docker 29.2.1 tournant avec Colima | Testé le 19 nov. 2026 : `./practicar reconstruir`, `prepara 01` deux fois, chaque commande des cinq étapes, `verifica 01` en rouge et en vert, et les écrans que vous voyez ici |
| Linux Mint 22.3 (x86_64), Docker Engine 29.10.0 | Testé le 19 nov. 2026 : `./practicar reconstruir-limpio`, `./practicar reconstruir` avec la couche de paquets réutilisée, `prepara 01` deux fois, `verifica 01` en vert, un mauvais chemin et un raccourci en rouge, et les six formes de `ls` identiques à celles de GNU |
| Ubuntu, Mac avec Docker Desktop, Windows avec Docker Desktop et WSL 2 | Pas encore testé avec cette leçon |

**Ce qui va coïncider avec votre écran et ce qui ne le fera pas.** Les noms, les tailles des fichiers, les dates et les
chemins coïncident. Peuvent varier la disposition en colonnes, les couleurs, la ligne `total` et la taille des
dossiers, qui dépendent de votre fenêtre et de l'endroit où Docker garde le volume.

---

## Étape 1 · Montez le labyrinthe et regardez-le d'en haut

À l'intérieur de votre terminal d'entraînement, lancez :

```bash
prepara 01
```

```text
✅ Laberinto listo en /home/alumno/practica/01 (medí: 10 carpetas y 12 archivos, cada archivo con su tamaño).
   Te dejé la hoja de respuestas, con sus claves vacías: /home/alumno/practica/respuestas/01.md
   Empieza por ahí:  cd ~/practica/01
```

*Labyrinthe prêt dans `/home/alumno/practica/01` (j'ai mesuré : 10 dossiers et 12 fichiers, chaque fichier avec sa
taille). Je vous ai laissé la feuille de réponses, avec ses clés vides : `/home/alumno/practica/respuestas/01.md`.
Commencez par là : `cd ~/practica/01`.*

`prepara` (« prépare ») est une commande du cours : elle monte le matériel d'entraînement d'une leçon. Trois choses à son
sujet :

- **Elle n'a aucun hasard.** Les noms, les tailles et les dates viennent d'une liste fixe. Lors des tests, elle a été
  montée plusieurs fois et le résultat était identique.
- **Elle touche deux choses et rien d'autre :** le dossier `~/practica/01`, et votre feuille de réponses, qu'elle crée
  seulement si elle n'existe pas.
- **On peut la relancer.** Si vous vous perdez ou cassez quelque chose, `prepara 01` remet le labyrinthe comme le premier
  jour. Pour cela, elle supprime celui qui est là, donc elle vous demande d'écrire `si` (« oui »). C'est pourquoi elle
  est à part de `verifica`, qui ne fait que regarder.

**Le labyrinthe, vu d'en haut.** Ce dessin n'est la sortie d'aucune commande : c'est ce que vous devrez imaginer en
marchant. Dans le terminal, vous ne verrez que le dossier où vous vous tenez.

```text
/                                  ← la raíz
└── home
    └── alumno                     ← tu carpeta personal: ~
        └── practica
            └── 01                 ← aquí empieza el laberinto
                ├── LEEME.txt
                ├── pagos
                │   ├── bitacoras
                │   │   ├── enero.log, febrero.log, marzo.log, abril.log, mayo.log
                │   │   └── viejas
                │   │       └── 2024.log
                │   ├── config
                │   │   ├── pagos.conf
                │   │   └── plantillas
                │   │       └── recibo.txt
                │   └── entregas
                │       └── guardia
                └── respaldos
                    ├── semana-1
                    │   └── copia-1.txt
                    └── semana-2
                        └── copia-2.txt
```

*Les commentaires du dessin : « la racine » ; « votre dossier personnel : `~` » ; « le labyrinthe commence ici ». Les
noms, en français : `LEEME.txt` (« LISEZ-MOI »), `pagos` (« paiements »), `bitacoras` (« journaux »), `enero.log`
(« journal de janvier »), `febrero.log` (« de février »), `marzo.log` (« de mars »), `abril.log` (« d'avril »),
`mayo.log` (« de mai »), `viejas` (« anciens »), `config` (« configuration »), `plantillas` (« modèles »),
`recibo.txt` (« reçu »), `entregas` (« livraisons »), `guardia` (« astreinte »), `respaldos` (« sauvegardes »),
`semana-1` et `semana-2` (« semaine 1 » et « semaine 2 »), `copia-1.txt` et `copia-2.txt` (« copie 1 » et « copie 2 »).*

Lisez-le de haut en bas. Tout en haut se trouve la racine, `/`. Dans la racine, il y a un dossier `home` ; dans `home`,
un dossier `alumno` ; et ainsi de suite. Chaque niveau vers la droite veut dire « à l'intérieur de ». Monter, c'est aller
vers la racine ; descendre, c'est entrer dans un dossier qui est à l'intérieur du vôtre. Les fichiers sont là pour
remplir : ce qui compte, c'est leur nom, leur taille et leur date.

Maintenant, placez-vous à l'entrée du labyrinthe. `cd` est la commande pour changer de dossier, et vous la verrez en
détail à l'étape suivante. Aujourd'hui, cette ligne est une recette à recopier :

```bash
cd ~/practica/01
```

```text
alumno@practica:~/practica/01$
```

`cd` n'a rien répondu : comme à la leçon 00, le silence veut dire que cela s'est bien passé. Ce qui a changé, c'est
l'invite : avant, elle disait `~` et maintenant elle dit `~/practica/01`. **L'invite vous dit sans cesse dans quel
dossier vous vous tenez.** Vérifiez-le avec `pwd` :

```text
/home/alumno/practica/01
```

L'invite utilise la forme courte, avec `~`. `pwd` répond avec la forme complète.

**Ce que cela démontre :** pas encore. Cette étape prépare le matériel des trois exercices.

---

## Étape 2 · Marchez : `cd` avec des chemins relatifs

`cd` vient de *change directory*, « changer de dossier ». On écrit `cd`, une espace et le chemin de l'endroit où l'on
veut aller.

Pour savoir où vous pouvez aller, il faut voir ce qu'il y a. `ls`, tout seul, liste ce qu'il y a dans le dossier actuel ;
vous le verrez à fond à l'étape 4.

```bash
ls
```

```text
LEEME.txt  pagos  respaldos
```

Cela coïncide avec le dessin : dans `01`, il y a un fichier et deux dossiers.

**Prédisez d'abord.** Vous allez lancer `cd pagos`. Écrivez sur votre feuille ce que l'invite dira ensuite.

```bash
cd pagos
```

```text
alumno@practica:~/practica/01/pagos$
```

Vous êtes descendu d'un niveau. `pagos` est un **chemin relatif** : il ne dit pas où se trouve `pagos` dans tout
l'arbre, il dit seulement « le dossier `pagos` qui est ici, là où je me tiens ». Cela a marché parce que, dans `01`, il y
a un dossier `pagos`.

Descendez d'un autre niveau et demandez où vous êtes arrivé :

```bash
cd bitacoras
pwd
```

```text
/home/alumno/practica/01/pagos/bitacoras
```

**Pour monter : deux points.** `..` veut dire « le dossier au-dessus de celui où je suis ». **Prédisez :** vous êtes
dans `bitacoras`. Où vous retrouvez-vous après `cd ..` ?

```bash
cd ..
pwd
```

```text
/home/alumno/practica/01/pagos
```

**Plusieurs niveaux dans un seul chemin.** Les dossiers d'un chemin se séparent par une barre oblique. Vous pouvez
descendre de deux niveaux d'un coup, et en monter deux avec deux fois `..` :

```bash
cd config/plantillas
```

```text
alumno@practica:~/practica/01/pagos/config/plantillas$
```

```bash
cd ../..
pwd
```

```text
/home/alumno/practica/01/pagos
```

**Une erreur exprès.** Vous êtes dans `pagos`. Demandez un dossier qui n'existe pas là :

```bash
cd nada
```

```text
bash: cd: nada: No such file or directory
```

*« Ce fichier ou ce dossier n'existe pas. » (`nada` veut dire « rien ».)* Le message vous a rendu l'invite, donc il ne vous a
pas piégé. Et l'invite dit toujours `pagos` : **quand `cd` échoue, vous restez où vous étiez.** Il en va de même avec
`cd Bitacoras` : le dossier s'appelle `bitacoras`, et le terminal distingue majuscules et minuscules dans les noms,
comme dans les commandes.

Tous les chemins de cette étape sont relatifs : **leur résultat dépend de l'endroit où vous vous tenez.** Le même
`cd pagos` qui a marché dans `01` échoue dans un dossier qui n'a pas de `pagos` à l'intérieur.

**Ce que cela démontre** (objectif 1.1, première moitié) : vos prédictions de cette étape, comparées à l'invite et à
`pwd`.

---

## Étape 3 · L'autre façon : les chemins absolus

Un chemin relatif sert tant que vous savez où vous êtes. Il y a une autre façon d'écrire un chemin, qui n'en dépend pas.

**Montez tout en haut.** La racine s'écrit avec une seule barre oblique :

```bash
cd /
ls
```

```text
bin   dev  home  media  opt   root  sbin  sys  usr
boot  etc  lib   mnt    proc  run   srv   tmp  var
```

L'invite dit maintenant `alumno@practica:/$`. Vous êtes dans la racine, et ce sont là les dossiers d'un système Linux.
Aujourd'hui, un seul vous importe, `home`, parce que votre dossier personnel est à l'intérieur.

**Prédisez :** vous êtes dans la racine. Que se passe-t-il si vous lancez `cd pagos`, la même commande qui a marché à
l'étape 2 ?

```text
bash: cd: pagos: No such file or directory
```

Cela a échoué, et pour cause : dans la racine, il n'y a aucun dossier `pagos`.

**Un chemin absolu commence par `/`.** Il dit le chemin complet depuis la racine, dossier par dossier. C'est pourquoi il
marche de la même façon, où que vous vous teniez. Celui-ci mène aux journaux :

```bash
cd /home/alumno/practica/01/pagos/bitacoras
```

```text
alumno@practica:~/practica/01/pagos/bitacoras$
```

Lisez-le avec le dessin de l'étape 1 : de la racine à `home`, de `home` à `alumno`, de là à `practica`, à `01`, à
`pagos` et à `bitacoras`. C'est ce que `pwd` répond quand vous y êtes : **`pwd` répond le chemin absolu du dossier
actuel.**

**La barre oblique du début change tout.** `cd pagos` demande le dossier `pagos` qui est à l'intérieur de là où je suis.
`cd /pagos` demande le dossier `pagos` qui est dans la racine. Vérifiez-le là où il y a bien un `pagos` :

```bash
cd ~/practica/01
cd /pagos
```

```text
bash: cd: /pagos: No such file or directory
```

Il y a un `pagos` à un pas de vous, mais vous en avez demandé un autre : un qui serait dans la racine, et celui-là
n'existe pas.

**Votre dossier personnel a une abréviation : `~`.** Au début d'un chemin, le terminal remplace `~` par `/home/alumno`
avant de lancer la commande. C'est pourquoi `cd ~/practica/01` et `cd /home/alumno/practica/01` mènent au même endroit,
et `cd` tout seul, sans chemin, vous mène à votre dossier personnel. Mais `~` est une abréviation du terminal et ne fait
pas partie de l'arbre : dans un autre compte, `~` est un autre dossier. Quand on vous demande un chemin absolu, écrivez-le
depuis la racine, avec `/`.

**Les quatre points de départ,** ensemble :

| Le chemin commence par | Il part de | Exemple | Dépend de l'endroit où vous êtes |
|---|---|---|---|
| `/` | La racine | `/home/alumno/practica/01` | Non |
| `~` | Votre dossier personnel | `~/practica/01` | Non |
| `..` | Le dossier au-dessus de l'actuel | `../config` | Oui |
| Un nom | Le dossier actuel | `pagos/bitacoras` | Oui |

**Lequel choisir ?** Le relatif est court : il convient quand l'endroit est proche. L'absolu ne laisse aucun doute : il
convient quand vous dictez un chemin à quelqu'un qui ne sait pas dans quel dossier vous vous tenez, comme à Lucía par
téléphone.

**Ce que cela démontre** (objectif 1.1 complet) : exercice 1.1, plus bas. Et c'est la moitié de l'objectif 1.3.

---

## Étape 4 · Regardez ce qu'il y a : `ls` et ses colonnes

Allez dans le dossier des journaux, avec le chemin que vous voulez, et listez :

```bash
cd ~/practica/01/pagos/bitacoras
ls
```

```text
abril.log  enero.log  febrero.log  marzo.log  mayo.log  viejas
```

`ls` vient de *list*, « lister ». Tout seul, il ne donne que les noms, par ordre alphabétique. Il ne donne ni tailles ni
dates : pour cela, il y a les options, les mots avec des tirets qui changent ce que fait une commande. Aujourd'hui, il y
en a trois.

### `-a` : aussi ce qui est caché

**Prédisez :** le `ls` ci-dessus a montré six noms. Pensez-vous que c'est tout ce qu'il y a dans le dossier ?

```bash
ls -a
```

```text
.   .pendiente  enero.log    marzo.log  viejas
..  abril.log   febrero.log  mayo.log
```

Trois noms qui commencent par un point sont apparus. Sous Linux, **un fichier dont le nom commence par un point est un
fichier caché** : `ls` tout seul ne le liste pas. Il n'est ni protégé ni secret ; il ne gêne simplement pas dans la
liste de tous les jours. `-a` vient de *all*, « tout ». `.pendiente` est un fichier caché du labyrinthe. `..`, vous le
connaissez déjà : c'est le dossier du dessus. Et `.` est le dossier actuel.

### `-l` : la liste longue

```bash
ls -l
```

```text
total 2168
-rw-r--r-- 1 alumno alumno  998000 Apr 30  2025 abril.log
-rw-r--r-- 1 alumno alumno     980 Jan 31  2025 enero.log
-rw-r--r-- 1 alumno alumno    9800 Feb 28  2025 febrero.log
-rw-r--r-- 1 alumno alumno 1200000 Mar 31  2025 marzo.log
-rw-r--r-- 1 alumno alumno       0 May 31  2025 mayo.log
drwxr-xr-x 2 alumno alumno    4096 Jan  2  2025 viejas
```

`-l` vient de *long*, « long » : une ligne par fichier ou dossier, avec ses données en colonnes. Lisez la ligne de
`marzo.log` de gauche à droite :

| Colonne | Dans `marzo.log` | Ce qu'elle dit |
|---|---|---|
| **Type** | `-` | La première lettre de la ligne. Un tiret est un fichier normal. Un `d`, pour *directory*, est un dossier : regardez la ligne de `viejas`. Il existe d'autres types ; dans ce labyrinthe, il n'y a que ces deux-là |
| **Permissions** | `rw-r--r--` | Les neuf lettres qui suivent. Elles disent qui peut lire, écrire et exécuter le fichier. Aujourd'hui, vous ne faites que leur donner un nom : elles se lisent et se changent à la leçon 09 |
| **Liens** | `1` | Combien de noms pointent vers ce même fichier. Dans ce cours, vous ne vous en servez pas |
| **Propriétaire** | `alumno` | L'utilisateur à qui appartient le fichier. Ici, c'est vous |
| **Groupe** | `alumno` | Le groupe d'utilisateurs auquel il appartient. Il ressort à la leçon 09 |
| **Taille** | `1200000` | La taille du fichier, en octets |
| **Date** | `Mar 31  2025` | Quand il a été modifié pour la dernière fois. Les mois s'écrivent en anglais : `Mar` est mars et `Apr` est avril |
| **Nom** | `marzo.log` | Comment il s'appelle |

Trois détails :

- **Le `total` du haut n'est pas la somme de la colonne des tailles.** C'est l'espace que ce qui est listé occupe sur le
  disque, en blocs de 1 024 octets. Dans ce cours, vous ne vous en servez pas.
- **La taille d'un dossier n'est pas ce qu'il contient.** `viejas` dit `4096` et, à l'intérieur, il y a un fichier de
  20 000 octets. Pour savoir combien il y a dedans, vous entrez et vous regardez.
- **La date change de forme selon l'âge.** Si le fichier a été modifié il y a plus de six mois, vous voyez l'année,
  comme ici. S'il est récent, au lieu de l'année vous voyez l'heure, en temps universel (UTC) et non celle de votre
  montre. Vous le remarquerez à l'étape 5.

### `-h` : des tailles qui se lisent

Lequel est le plus grand, `998000` ou `1200000` ? Il faut compter les chiffres. `-h` vient de *human-readable*, « lisible
par une personne » : avec `-l`, il écrit la taille abrégée, avec une lettre.

```bash
ls -lh
```

```text
total 2.2M
-rw-r--r-- 1 alumno alumno 975K Apr 30  2025 abril.log
-rw-r--r-- 1 alumno alumno  980 Jan 31  2025 enero.log
-rw-r--r-- 1 alumno alumno 9.6K Feb 28  2025 febrero.log
-rw-r--r-- 1 alumno alumno 1.2M Mar 31  2025 marzo.log
-rw-r--r-- 1 alumno alumno    0 May 31  2025 mayo.log
drwxr-xr-x 2 alumno alumno 4.0K Jan  2  2025 viejas
```

Sans lettre, ce sont des octets. `K` sont des kilo-octets : 1 024 octets. `M` sont des mégaoctets : 1 024 K. `G` sont des
gigaoctets : 1 024 M.

**Le piège de cette colonne : on lit d'abord la lettre, ensuite le nombre.** `975K` a un nombre plus grand que `1.2M`, et
pourtant c'est le plus petit des deux fichiers, parce qu'un M vaut plus de mille K. Et une limite : `-h` arrondit.
`marzo.log` fait 1 200 000 octets, soit 1,14 M, et `ls` écrit `1.2M`. Quand vous avez besoin du nombre exact, enlevez
le `h`.

### Les options se joignent, et `ls` accepte un chemin

Plusieurs options d'une lettre peuvent s'écrire collées : `ls -lah` revient à `ls -l -a -h`. Lancez-le : c'est la liste
longue, avec ce qui est caché et des tailles lisibles. Là, `.pendiente` a déjà sa ligne.

Et il n'est pas nécessaire d'aller dans un dossier pour voir ce qu'il contient : `ls` accepte un chemin, relatif ou
absolu, comme `cd`. `ls -lh ..` vous montre le dossier du dessus et vous restez dans `bitacoras`. Regarder ne vous
déplace pas.

### Avec Claude Code, dans cet ordre

Dans ce cours, un assistant d'IA intervient d'une seule façon : **vous prédisez → vous demandez → vous lisez → vous
lancez → vous expliquez.**

1. **Vous prédisez.** Avec votre `ls -lah` devant vous, notez sur votre feuille quel est le plus grand fichier de
   `bitacoras`.
2. **Vous demandez.** Sur votre ordinateur, à Claude Code : « Donne-moi une commande `ls` qui trie un dossier par taille,
   du plus grand au plus petit, et explique-moi chaque option ». Claude Code ne voit pas votre labyrinthe : il tourne
   en dehors.
3. **Vous lisez.** Avant de lancer ce qu'il vous a donné, cherchez chaque option dans `ls --help`, comme à la leçon 00.
4. **Vous lancez.** La commande, c'est vous qui l'écrivez, dans votre terminal d'entraînement.
5. **Vous expliquez.** Avec vos mots : pourquoi est-ce dans cet ordre ? Cela coïncide-t-il avec votre prédiction ?

**Ce que cela démontre** (objectif 1.2) : exercice 1.2, plus bas.

---

## Étape 5 · Notez vos réponses et vérifiez-les

Jusqu'ici, vous avez tout vérifié vous-même, avec l'invite et avec `pwd`. Maintenant, c'est au tour de `verifica`. Pour
qu'il puisse contrôler ce que vous avez trouvé, vous le notez dans votre **feuille de réponses**,
`~/practica/respuestas/01.md`, que `prepara` vous a déjà laissée. C'est du texte simple, avec quatre lignes : une clé,
deux-points et votre réponse.

| Clé | Ce qui y va |
|---|---|
| `mas-grande` (« le plus grand ») | Le nom du plus gros fichier du labyrinthe. Seulement le nom, tel que `ls` l'écrit, sans son dossier |
| `oculto` (« caché ») | Le nom du fichier caché du labyrinthe. Seulement le nom, avec son point |
| `ruta-absoluta` (« chemin absolu ») | Le chemin absolu du dossier de destination |
| `ruta-relativa` (« chemin relatif ») | Le chemin relatif vers le dossier de destination, au départ de `bitacoras` |

Lancez `verifica 01` avant de noter quoi que ce soit, pour voir d'où vous partez. Les deux premières lignes
s'affichent ainsi :

```text
✅ Tu laberinto está completo (medí: las 22 carpetas y archivos de la lista existen en ~/practica/01, ninguno es un enlace y cada archivo tiene su tamaño)
❌ Tu renglón «mas-grande:» está vacío (medí: lo que hay después de los dos puntos). Anótalo con  anota 01 mas-grande  y el nombre del archivo
```

*Votre labyrinthe est complet (j'ai mesuré : les 22 dossiers et fichiers de la liste existent dans `~/practica/01`,
aucun n'est un lien et chaque fichier a sa taille). Votre ligne « mas-grande: » est vide (j'ai mesuré : ce qu'il y a
après les deux-points). Notez-la avec `anota 01 mas-grande` et le nom du fichier.*

Trois autres rouges, pareils, suivent : un par clé manquante. C'est normal : la feuille est vide.

**Comment on note.** Vous n'avez pas encore vu d'éditeur de texte ; il arrive à la leçon 07. C'est pourquoi le cours
fournit une commande, `anota` (« note ») : vous écrivez le numéro de la leçon, la clé et votre réponse. Supposons que
vous pensiez que le plus grand est `enero.log` :

```bash
anota 01 mas-grande enero.log
```

```text
Anotado. Tu hoja quedó así:
mas-grande: enero.log
oculto:
ruta-absoluta:
ruta-relativa:
```

*Noté. Votre feuille est restée ainsi :* (suivent les quatre clés).

`anota` ne sait pas si vous avez trouvé juste : il écrit, c'est tout. Cela, c'est `verifica 01` qui le dit, et sa deuxième
ligne change maintenant :

```text
❌ «enero.log» no es el archivo más grande del laberinto (medí: comparé sus bytes contra los de los demás archivos de la lista; hay uno con más). Vuelve a leer la columna del tamaño con  ls -lh : fíjate en la letra, no solo en el número
```

*« enero.log » n'est pas le plus gros fichier du labyrinthe (j'ai mesuré : j'ai comparé ses octets à ceux des autres
fichiers de la liste ; il y en a un qui en a plus). Relisez la colonne de la taille avec `ls -lh` : regardez la lettre,
pas seulement le nombre.*

Voilà comment se lit un rouge : il dit ce que vous avez noté, ce qui a été mesuré et avec quelle commande revenir
regarder. Il ne donne pas la réponse. Pour corriger, vous notez de nouveau la même clé : `anota` change cette ligne. Et
`anota 01`, sans rien d'autre, vous montre la feuille.

**Les deux chemins de l'exercice 1.3.** Le dossier de destination est `guardia` : il est à l'intérieur de `entregas`, qui
est à l'intérieur de `pagos`. Cherchez-le dans le dessin de l'étape 1. Vous devez y arriver de deux façons : avec un
**chemin absolu**, et avec un **chemin relatif** qui part du dossier `bitacoras`.

Avant de noter chaque chemin, parcourez-le : placez-vous où il faut, entrez avec `cd` et demandez à `pwd` si vous êtes
arrivé à `guardia`. Quand un chemin vous y mène, notez-le tel que vous l'avez écrit. `verifica` fait le même test que vous.

Quand vous avez les quatre lignes, lancez `verifica 01`. Corrigez et recommencez jusqu'à voir ceci à la fin :

```text
✅ Lección 01: todo en verde.
```

*Leçon 01 : tout est au vert.*

Maintenant que la feuille contient vos réponses, regardez sa ligne avec `ls -l ~/practica/respuestas` :

```text
total 4
-rw-r--r-- 1 alumno alumno 139 Nov 19 07:35 01.md
```

C'est le détail resté en suspens à l'étape 4 : votre feuille est récente, et, au lieu de l'année, elle porte l'heure. La
taille et la date de la vôtre seront autres.

**Ce que cela démontre** (objectifs 1.2 et 1.3) : `verifica 01` entièrement au vert.

---

## Vos exercices

L'exercice 1.1 se fait **sans IA** et sans rien lancer avant d'avoir prédit : seulement votre feuille et le dessin de
l'étape 1.

| # | Vous faites | C'est fait quand |
|---|---|---|
| 1.1 | Vous prédisez par écrit dans quel dossier vous vous retrouvez après chacun des dix `cd` ci-dessous, puis vous les lancez | Vous avez vos dix prédictions et les dix sorties de `pwd`, et vous savez expliquer chacune de celles qui ne coïncidaient pas |
| 1.2 | Dans `bitacoras` : vous trouvez le plus gros fichier et le fichier caché et vous les notez sur votre feuille ; et vous remplissez sur votre papier le tableau des colonnes ci-dessous | `verifica 01` marque en vert `mas-grande` et `oculto`, et votre tableau coïncide avec « Comparez vos réponses » |
| 1.3 | Vous arrivez au dossier `guardia` avec un chemin absolu et avec un chemin relatif qui part de `bitacoras`, et vous notez les deux | `verifica 01` s'affiche entièrement au vert |

**Les dix `cd` du 1.1.** D'abord, écrivez les dix prédictions, avec le chemin complet que vous pensez que `pwd` va
répondre. Ne lancez encore rien. Si vous pensez qu'un `cd` va échouer, écrivez-le ainsi, et écrivez où vous restez. Vous
partez de `~/practica/01`, et chaque commande part de là où la précédente vous a laissé.

| # | Commande | # | Commande |
|---|---|---|---|
| 1 | `cd pagos/config` | 6 | `cd pagos` |
| 2 | `cd plantillas` | 7 | `cd home` |
| 3 | `cd ../../bitacoras/viejas` | 8 | `cd ~` |
| 4 | `cd ../..` | 9 | `cd ../..` |
| 5 | `cd /` | 10 | `cd ..` |

Quand vous avez les dix, allez à l'entrée avec `cd ~/practica/01` et lancez les commandes une par une, avec un `pwd`
après chacune. Il n'y a pas de note pour réussir : ce qui compte, c'est que, pour chacune que vous avez ratée, vous
puissiez dire ce que vous avez mal lu : si le chemin était absolu ou relatif, ou dans quel dossier vous vous teniez.

**Le tableau des colonnes du 1.2.** Lancez `ls -la` dans `bitacoras` et remplissez sur votre papier ces deux lignes. Cette
partie n'est pas contrôlée par `verifica` : c'est vous qui comparez.

| Nom | Fichier ou dossier ? | Permissions (les neuf lettres) | Propriétaire | Taille en octets | Date |
|---|---|---|---|---|---|
| `.pendiente` | | | | | |
| `viejas` | | | | | |

**Durée (estimée) :** de 60 à 90 minutes. C'est une estimation : elle sera ajustée avec le premier groupe qui suivra le
cours.

---

## Pour aller plus loin

*Facultatif. Rien dans cet encadré n'est évalué ni nécessaire pour les exercices.*

- **`cd -` vous ramène au dossier précédent.** C'est un tiret après `cd`. Il sert à aller et venir entre deux endroits,
  et il répond où il vous a ramené.
- **La touche Tab complète les noms.** Dans `pagos`, écrivez `cd bit` et appuyez sur Tab : le terminal complète
  `bitacoras/`, parce que c'est le seul dossier de là qui commence ainsi. Moins de lettres, moins de fautes de frappe.
- **`ls -lhS` trie par taille.** C'est une réponse possible à ce que vous avez demandé à Claude Code. Dans `ls --help`, le
  `S` dit `Sort by file size, largest first.` Voici ce que cela a donné dans `bitacoras` :

```text
total 2.2M
-rw-r--r-- 1 alumno alumno 1.2M Mar 31  2025 marzo.log
-rw-r--r-- 1 alumno alumno 975K Apr 30  2025 abril.log
-rw-r--r-- 1 alumno alumno 9.6K Feb 28  2025 febrero.log
drwxr-xr-x 2 alumno alumno 4.0K Jan  2  2025 viejas
-rw-r--r-- 1 alumno alumno  980 Jan 31  2025 enero.log
-rw-r--r-- 1 alumno alumno    0 May 31  2025 mayo.log
```

  `viejas` se retrouve au milieu parce que sa ligne dit `4.0K`, la taille du dossier et non de ce qu'il contient. Et le
  fichier caché n'apparaît pas, parce que la commande n'a pas `-a`.
- **Le `ls` de ce terminal et celui d'ailleurs.** Ici, `ls` vient de `rust-coreutils` et non du projet GNU, comme vous
  l'avez vu à la leçon 00. Dans les formes de cette leçon, la sortie était identique à celle du `ls` de GNU, qu'Ubuntu
  laisse installé sous le nom `gnuls` : comparez `gnuls -lah` avec votre `ls -lah`. Ce qui change, c'est le texte de
  `ls --help` et de `man ls`.

---

## Ça ne marche pas ?

| Vous voyez | Cause probable | Ce que vous faites |
|---|---|---|
| `bash: prepara: command not found`, ou `Todavía no hay verifica para la lección «01».` *(Il n'y a pas encore de verifica pour la leçon « 01 ».)* | Votre image est d'avant cette leçon | Sortez avec `exit` et lancez `./practicar reconstruir` |
| `Ya existe /home/alumno/practica/01.` et une question *(« /home/alumno/practica/01 existe déjà. »)* | Vous aviez déjà monté le labyrinthe, et le monter de nouveau l'efface | Pour repartir de zéro, écrivez `si`. Sinon, appuyez sur Entrée : rien n'est effacé |
| Après `prepara 01`, `ls` n'affiche rien, ou `error retrieving current directory` apparaît | Vous vous teniez à l'intérieur du labyrinthe quand il a été remonté : ce dossier n'existe plus | `cd ~/practica/01` |
| `❌ No preparo nada: … es un enlace…` *(Je ne prépare rien : … c'est un lien…)* | Dans votre dossier d'entraînement, il y a un lien là où devrait aller un dossier à vous | Depuis votre ordinateur, `./practicar desde-cero` |
| `bash: cd: …: No such file or directory` | Le dossier n'existe pas sous ce nom à l'endroit où vous l'avez demandé | `pwd` pour savoir où vous êtes et `ls` pour voir ce qu'il y a. Vérifiez les majuscules et si le chemin devait commencer par `/` |
| `❌ Tu ruta relativa … no existe saliendo de …` *(Votre chemin relatif … n'existe pas au départ de …)* | Vous l'avez essayé en vous tenant dans un autre dossier | Allez dans `bitacoras` et essayez-y votre chemin avec `cd` et `pwd` |
| `❌ Tu laberinto no está completo…` *(Votre labyrinthe n'est pas complet…)* | Vous avez supprimé ou changé quelque chose dans le labyrinthe | `prepara 01`. Votre feuille de réponses ne se perd pas |

---

## Pour conclure

**Répondez sans relire.**

**Cinq chemins : absolu ou relatif ?** Vous réussissez avec 4 sur 5.

1. `config/plantillas`
2. `/home/alumno/practica`
3. `../respaldos`
4. `/`
5. `..`

**Et cinq questions :**

6. Vous êtes dans `/home/alumno/practica/01/respaldos/semana-1`. Quel chemin relatif vous mène à `semana-2` ?
7. Vous lancez `ls` dans un dossier et rien ne s'affiche. Pouvez-vous déjà dire qu'il est vide ? Que lancez-vous pour en
   être sûr ?
8. Dans `ls -lh`, vous voyez `812K` sur une ligne et `3.1M` sur une autre. Quel fichier est le plus grand, et comment le
   savez-vous ?
9. Une ligne de `ls -l` commence par `d`. Que vous dit cette lettre ? Et que dit du contenu le nombre de la colonne de
   la taille ?
10. Vous allez dicter à un collègue le chemin vers un dossier, et vous ne savez pas dans quel dossier il se tient.
    Lui dictez-vous un chemin absolu ou un chemin relatif ? Pourquoi ?

**Ce qui vient ensuite :** vous savez maintenant vous déplacer dans les dossiers et voir ce qu'ils contiennent. À la
leçon 02, vous commencez à changer ce qu'il y a : vous créez, copiez, déplacez et supprimez, dans un terminal où il n'y
a pas de corbeille.

---

> **Ce que fait Hábil**
>
> Nous exploitons et modernisons l'infrastructure où tournent des systèmes qui ne peuvent pas tomber en panne :
> serveurs Linux, conteneurs, Kubernetes et livraison continue, pour la banque, l'assurance et les entreprises
> réglementées. Si vos serveurs dépendent d'une personne qui « sait où se trouve chaque chose », cela peut être mis en
> ordre sans arrêter l'exploitation.
>
> [**CI/CD, DevSecOps et cloud sans accroc**](https://www.habil.mx/fr/architecture-cloud-native/)
>
> Premier échange, sans frais.

---

## Comparez vos réponses

**Exercice 1.1.** Ce que `pwd` répond après chaque commande :

| # | Commande | Vous êtes dans | Pourquoi |
|---|---|---|---|
| 1 | `cd pagos/config` | `/home/alumno/practica/01/pagos/config` | Relatif : descend de deux niveaux depuis `01` |
| 2 | `cd plantillas` | `/home/alumno/practica/01/pagos/config/plantillas` | Relatif : descend d'un |
| 3 | `cd ../../bitacoras/viejas` | `/home/alumno/practica/01/pagos/bitacoras/viejas` | Monte de deux, jusqu'à `pagos`, et descend de deux par une autre branche |
| 4 | `cd ../..` | `/home/alumno/practica/01/pagos` | Monte de deux |
| 5 | `cd /` | `/` | Absolu : la racine, où que vous étiez |
| 6 | `cd pagos` | `/` | Échoue : `No such file or directory`. Dans la racine, il n'y a pas de `pagos`, et quand `cd` échoue vous restez où vous étiez |
| 7 | `cd home` | `/home` | Relatif : dans la racine, il y a bien un dossier `home` |
| 8 | `cd ~` | `/home/alumno` | Votre dossier personnel, où que vous étiez |
| 9 | `cd ../..` | `/` | Monte de deux : de `alumno` à `home`, et de `home` à la racine |
| 10 | `cd ..` | `/` | Au-dessus de la racine, il n'y a rien : `..` dans la racine est la racine elle-même. Cela ne donne pas d'erreur et ne vous déplace pas |

**Exercice 1.2, le plus gros et le caché.** `verifica 01` vous dit si vous avez trouvé juste. Le raisonnement qui doit
se trouver derrière :

- **Le plus gros** se lit dans la colonne de la taille de `ls -lh`, d'abord la lettre et ensuite le nombre : la seule
  ligne avec `M` l'emporte sur celles qui portent `K`. Sans `-h`, on arrive au même résultat en comptant les chiffres.
- **Le caché** n'apparaît qu'avec `-a`. C'est le seul nom qui commence par un point et qui n'est ni `.` ni `..`. Sur votre
  feuille, il va avec son point : le point fait partie du nom.

**Exercice 1.2, le tableau des colonnes.**

| Nom | Fichier ou dossier ? | Permissions | Propriétaire | Taille en octets | Date |
|---|---|---|---|---|---|
| `.pendiente` | Fichier : la ligne commence par un tiret | `rw-r--r--` | `alumno` | `48000` | `May 12  2025` : 12 mai 2025 |
| `viejas` | Dossier : la ligne commence par `d` | `rwxr-xr-x` | `alumno` | `4096` lors du test ; sur votre ordinateur, cela peut être autre chose | `Jan  2  2025` : 2 janvier 2025 |

Si, pour la taille de `.pendiente`, vous avez écrit `47K`, vous avez lancé `ls -lah` : c'est le même fichier, avec la taille
abrégée. Si, pour le propriétaire, vous avez hésité entre les deux colonnes qui disent `alumno` : la première est le
propriétaire et la seconde est le groupe.

**Exercice 1.3.** Ici, il n'y a pas de réponse écrite : le contrôle, c'est `verifica 01`. Pour savoir si vous êtes sur
la bonne voie :

- Votre chemin absolu commence par `/` et c'est, lettre pour lettre, ce que `pwd` répond quand vous vous tenez dans
  `guardia`. Après la racine, il porte sept noms.
- Votre chemin relatif ne commence ni par `/` ni par `~`. Le plus court monte d'un niveau et en descend de deux. Il y en
  a d'autres, plus longs, qui arrivent aussi, et `verifica` les accepte : ce qu'il mesure, c'est que le chemin écrit,
  dossier par dossier, mène à `guardia`.

**Pour conclure.**

1. Relatif.
2. Absolu.
3. Relatif.
4. Absolu : c'est la racine.
5. Relatif.
6. `../semana-2` : vous montez à `respaldos` et vous descendez à `semana-2`.
7. Pas encore : `ls` tout seul ne liste pas ce qui est caché. Vous lancez `ls -a`. Si seuls `.` et `..` apparaissent, alors
   oui, il est vide.
8. Celui de `3.1M`. On lit d'abord la lettre : un M vaut plus de mille K.
9. Que c'est un dossier. Du contenu, elle ne dit rien : ce nombre est la taille du dossier lui-même.
10. Un chemin absolu. Un chemin relatif ne sert que si les deux savent dans quel dossier il se tient ; l'absolu part de la
    racine.

---

## Sources de la leçon

Consultées le 19 nov. 2026. Toutes sont en anglais.

- The Open Group. *POSIX.1-2024, Base Definitions, chapitre 3* (3.2 « Absolute Pathname », 3.311 « Relative Pathname » et 3.111 « Dot-Dot »). https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap03.html
- The Open Group. *POSIX.1-2024, utilitaire cd* (`cd` sans chemin utilise le dossier personnel ; `cd -` va au dossier précédent et l'écrit). https://pubs.opengroup.org/onlinepubs/9799919799/utilities/cd.html
- The Open Group. *POSIX.1-2024, utilitaire ls* (l'ordre des colonnes de `-l` ; heure ou année selon les six derniers mois). https://pubs.opengroup.org/onlinepubs/9799919799/utilities/ls.html
- GNU. *Bash Reference Manual, « Bourne Shell Builtins »* (`cd` et `pwd` ; `pwd` écrit le chemin absolu du dossier actuel). https://www.gnu.org/software/bash/manual/html_node/Bourne-Shell-Builtins.html
- GNU. *Bash Reference Manual, « Tilde Expansion »* (`~` est remplacé par le dossier personnel). https://www.gnu.org/software/bash/manual/html_node/Tilde-Expansion.html
- GNU. *Coreutils, « What information is listed »* (les colonnes de `-l`, les lettres du type, la ligne `total` et `-h` avec des puissances de 1024). https://www.gnu.org/software/coreutils/manual/html_node/What-information-is-listed.html
- GNU. *Coreutils, « Which files are listed »* (`ls` ne liste pas les noms qui commencent par un point ; `-a` si). https://www.gnu.org/software/coreutils/manual/html_node/Which-files-are-listed.html
- Ubuntu Manpages. *ls(1)*, d'Ubuntu 26.04 (le `ls` de `rust-coreutils` 0.10.0 : `-a`, `-l`, `-h`, `-S`). https://manpages.ubuntu.com/manpages/resolute/en/man1/ls.1.html
- Ubuntu Manpages. *hier(7)*, d'Ubuntu 26.04 (les dossiers de la racine d'un système Linux ; `/home`). https://manpages.ubuntu.com/manpages/resolute/en/man7/hier.7.html
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, section « rust-coreutils » (les commandes de GNU restent installées avec le préfixe `gnu`, comme `gnuls`). https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- Nos propres mesures, 19 nov. 2026, sur un Mac avec Apple silicon (Docker 29.2.1 avec Colima, linux/arm64), avec l'image
  du cours sur Ubuntu 26.04.1 : `ls` de uutils coreutils 0.10.0, `gnuls` de GNU coreutils 9.7 et bash 5.3.9. De là
  viennent les écrans, la comparaison de `ls` avec `gnuls`, l'arrondi de `-h`, la touche Tab et le temps de
  `./practicar reconstruir`. Sous Linux, cette leçon n'est pas encore testée.
