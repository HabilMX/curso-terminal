# Leçon 02 — Créer, copier, déplacer et supprimer sans crainte

**Par Dorian Chávez, fondateur de Hábil et architecte d'intégration.**

**Avant de lire, quatre mots.** Une ligne chacun ; les autres arrivent à l'étape où ils servent.

- **Source et destination :** quand tu copies ou déplaces, la source est ce que tu as déjà et la destination est
  l'endroit où cela va. Dans une commande, la source s'écrit en premier et la destination à la fin.
- **Écraser :** écrire par-dessus un fichier qui existait déjà. Ce qu'il contenait avant est perdu.
- **Corbeille :** l'endroit où ton ordinateur garde ce que tu as supprimé, au cas où tu changerais d'avis. Dans le
  terminal, il n'y en a pas.
- **Motif :** un nom avec des jokers, comme `*.log`, qui attrape plusieurs noms d'un seul coup. Le `*` veut dire
  « n'importe quoi ».

**Ce que tu en retires :** tu crées des dossiers et des fichiers avec `mkdir` et `touch` ; tu copies, déplaces et
renommes avec `cp` et `mv`, et tu sais ce qui se passe quand la destination existe déjà ; tu supprimes avec `rm` et
`rmdir`, en voyant d'abord ce qui va partir ; et tu sais expliquer pourquoi une variable vide transforme une petite
suppression en une suppression énorme.

**Idée nouvelle de cette leçon (la seule qui est évaluée) :** dans le terminal, il n'y a pas de corbeille. Ce que `rm`
supprime ne se récupère pas, et `cp` et `mv` écrasent sans demander. C'est pourquoi **tu réfléchis avant de supprimer** :
d'abord tu regardes ce qui va partir, et ensuite tu supprimes.

**Dans quel ordre :** tu montes un atelier d'entraînement (étape 1), tu crées (étape 2), tu copies, déplaces et
renommes (étape 3), tu supprimes avec soin (étape 4), tu vois le danger de la variable vide sans le lancer
(étape 5) et, à la fin, tu contrôles tout avec `verifica` (étape 6).

**Une note sur la langue de l'atelier.** Comme dans les leçons 00 et 01, les messages du cours et les noms de l'atelier
sont en espagnol et font partie du code : ils ne sont donc pas traduits. Les messages apparaissent exactement comme le
terminal les affiche, avec une glose en français, en italique, sous chaque bloc, ou entre parenthèses à l'intérieur d'un
tableau. Les noms de l'atelier sont glosés une seule fois, à l'étape 1 : `taller` (« atelier »), `config`
(« configuration »), `plantillas` (« modèles »), `respaldo` (« sauvegarde »), `bitacoras` (« journaux »), `entrada`
(« entrée »), `reportes` (« rapports »), `limpieza` (« nettoyage »), `ensayo` (« essai ») et le reste. La clé de la
feuille de réponses est glosée à l'étape 4 : `patron` (« motif »). Les commandes `practicar`, `prepara`, `verifica` et
`anota` gardent aussi leur nom : tu les connais déjà par les leçons 00 et 01.

---

## La scène

*Scénario inventé ; les écrans qui apparaissent dans la leçon sont réels.*

Mercredi, 0 h 07. Lucía a trouvé le journal des paiements. La dernière ligne dit :

```text
2025-06-02 00:09 pagos: disco lleno, no puedo escribir
```

*2025-06-02 00:09 paiements : disque plein, je ne peux pas écrire.*

À l'autre bout du téléphone, on lui donne trois instructions :

« Avant de toucher à quoi que ce soit, sauvegarde la configuration. Ensuite, mets le rapport de juin dans son dossier.
Et supprime les fichiers temporaires du cache, ce sont eux qui ont rempli le disque. »

Dans le cache, il y a des fichiers qui se terminent par `.tmp`, et aussi un, `cache-indice.txt` (« cache-index.txt »),
sans lequel le service ne redémarre pas. Lucía sait une chose de la commande pour supprimer : elle ne demande rien, et il
n'y a pas de corbeille où récupérer ce qu'elle efface.

Cette leçon est cette demi-heure, dans un atelier où se tromper n'a pas de conséquences.

---

## Avant de commencer

**Ce dont tu as besoin :** la leçon 01 faite ; le matériel du cours à jour ; et du papier et un crayon pour tes
prédictions.

**Mets à jour ton terminal d'entraînement.** Comme à la leçon 01, depuis ton ordinateur, dans le dossier du cours :

```bash
git pull
./practicar reconstruir
```

Cela suffit pour recevoir cette leçon : `./practicar reconstruir` (« reconstruire ») réutilise l'Ubuntu et les paquets déjà
construits et ne met dedans que les nouveaux fichiers. Sur le Mac du test, cela a pris moins de deux secondes et s'est
terminé par `✅ Imagen lista.` (« Image prête. ») Ton dossier `~/practica` n'est pas touché. Ensuite, entre avec
`./practicar`.

**Où cette leçon a été testée :**

| Système | État |
|---|---|
| Mac avec Apple silicon, Docker 29.2.1 tournant avec Colima | Testé le 19 nov. 2026 : `./practicar reconstruir`, `prepara 02` deux fois avec le même résultat, chaque commande des étapes 1 à 6, et `verifica 02` en rouge et en vert |
| Linux Mint 22.3 (x86_64), Docker Engine 29.10.0 | Testé le 19 nov. 2026 : `./practicar reconstruir`, `prepara 02` deux fois avec le même résultat que sur le Mac, les messages de `mkdir`, `touch` et `rmdir`, les questions de `cp -i`, `mv -i`, `rm -i` et `rm -I`, la leçon complète en tant qu'étudiant, et `verifica 02` en vert et en rouge avec un lien physique |
| Ubuntu, Mac avec Docker Desktop, Windows avec Docker Desktop et WSL 2 | Pas encore testé avec cette leçon |

**Ce qui va coïncider avec ton écran et ce qui ne le fera pas.** Les noms, les contenus, les dates de l'atelier et les
messages des commandes coïncident. Changent l'heure de ce que tu crées ou touches, la disposition en colonnes et les
couleurs.

---

## Étape 1 · Monte l'atelier

À l'intérieur de ton terminal d'entraînement :

```bash
prepara 02
```

```text
✅ Taller listo en /home/alumno/practica/02 (medí: 12 carpetas y 22 archivos, cada archivo con su contenido).
   Te dejé la hoja de respuestas, con su clave vacía: /home/alumno/practica/respuestas/02.md
   Empieza por ahí:  cd ~/practica/02
```

*Atelier prêt dans `/home/alumno/practica/02` (j'ai mesuré : 12 dossiers et 22 fichiers, chaque fichier avec son
contenu). Je t'ai laissé la feuille de réponses, avec sa clé vide : `/home/alumno/practica/respuestas/02.md`.
Commence par là : `cd ~/practica/02`.*

`prepara 02` fonctionne comme celui de la leçon 01 : il ne laisse rien au hasard, il ne touche que `~/practica/02` et
ta feuille de réponses, et pour remonter l'atelier il le supprime, donc il te demande d'écrire `si` (« oui »).
Chaque fichier contient une ligne de texte différente : grâce au contenu, tu vois si une copie est la bonne ou si
quelque chose a été écrasé.

**L'atelier, vu d'en haut :**

```text
~/practica/02
├── LEEME.txt
├── config            ← lo que se respalda
│   ├── pagos.conf
│   └── plantillas
│       ├── aviso de corte.txt
│       ├── aviso.txt
│       └── recibo.txt
├── respaldo          ← el respaldo, con una versión vieja de pagos.conf
│   └── pagos.conf
├── bitacoras
│   ├── ayer.log
│   └── hoy.log
├── entrada           ← el reporte nuevo
│   └── reporte-junio.txt
├── reportes          ← ya hay un reporte con el mismo nombre: el borrador viejo
│   └── reporte-junio.txt
├── limpieza          ← lo del ejercicio de borrar
│   ├── cache-01.tmp, cache-02.tmp, cache-03.tmp
│   ├── cache-indice.txt, tmp-notas.txt
│   ├── sesiones-activas/
│   ├── sesiones-viejas/
│   └── vacia/
└── ensayo            ← para probar los comandos antes de usarlos en serio
    ├── uno.txt, dos.txt, tres.log
    └── caja/
        └── adentro.txt
```

*Les commentaires du dessin : « ce qu'on sauvegarde », « la sauvegarde, avec une ancienne version de pagos.conf », « le
nouveau rapport », « il y a déjà un rapport du même nom : l'ancien brouillon », « ce qui sert à l'exercice de
suppression » et « pour essayer les commandes avant de s'en servir pour de bon ». Les noms, en français : `LEEME.txt`
(« LISEZ-MOI.txt »), `config` (« configuration »), `pagos.conf` (« paiements.conf »), `plantillas` (« modèles »),
`aviso de corte.txt` (« avis de coupure.txt »), `aviso.txt` (« avis.txt »), `recibo.txt` (« reçu.txt »), `respaldo`
(« sauvegarde »), `bitacoras` (« journaux »), `ayer` (« hier »), `hoy` (« aujourd'hui »), `entrada` (« entrée »),
`reporte-junio.txt` (« rapport-juin.txt »), `reportes` (« rapports »), `limpieza` (« nettoyage »), `cache-indice.txt`
(« cache-index.txt »), `tmp-notas.txt` (« tmp-notes.txt »), `sesiones-activas` (« sessions actives »),
`sesiones-viejas` (« anciennes sessions »), `vacia` (« vide »), `ensayo` (« essai »), `uno`, `dos` et `tres` (« un »,
« deux » et « trois ») et `caja` (« boîte »). `adentro.txt` est « dedans.txt ».*

Les démonstrations se font dans `ensayo` ; les exercices, dans le reste.

**Ce que cela démontre :** pas encore. Cette étape prépare le matériel.

---

## Étape 2 · Créer : `mkdir` et `touch`

Entre dans le dossier d'essai :

```bash
cd ~/practica/02/ensayo
ls
```

```text
caja  dos.txt  tres.log  uno.txt
```

### `mkdir` : un nouveau dossier

`mkdir` vient de *make directory*, « créer un dossier ». Tu écris `mkdir` et le nom :

```bash
mkdir nueva
```

Silence : cela s'est bien passé. (`nueva` veut dire « nouvelle ».) **Prédis :** que se passe-t-il si tu lances la
même ligne une seconde fois ?

```text
mkdir: nueva: File exists
```

« Il existe déjà. » `mkdir` n'écrase pas un dossier : il refuse, et rien n'est perdu.

**Plusieurs dossiers, l'un dans l'autre.** **Prédis :** `uno` n'existe pas. Que se passe-t-il avec `mkdir uno/dos/tres` ?

```text
mkdir: No such file or directory
```

Cela a échoué, parce que `mkdir` crée le dernier dossier du chemin et attend que les précédents existent déjà. Pour
créer tout le chemin d'un coup, il y a l'option `-p`, de *parents*, « ceux du dessus aussi » :

```bash
mkdir -p uno/dos/tres
ls uno/dos
```

```text
tres
```

Avec `-p`, si le dossier existait déjà, il ne se plaint pas.

### `touch` : un fichier vide

`touch` veut dire « toucher ». Si le fichier n'existe pas, il le crée vide :

```bash
touch nota.txt
ls -l nota.txt
```

```text
-rw-r--r-- 1 alumno alumno 0 Nov 19 09:28 nota.txt
```

Zéro octet, et l'heure de maintenant. (`nota.txt` veut dire « note.txt ».) **Prédis :** `uno.txt` existe déjà et dit `uno`.
Si tu le touches, perd-il son contenu ?

```bash
ls -l uno.txt
touch uno.txt
ls -l uno.txt
cat uno.txt
```

```text
-rw-r--r-- 1 alumno alumno 4 Jun  2  2025 uno.txt
-rw-r--r-- 1 alumno alumno 4 Nov 19 09:28 uno.txt
uno
```

Il ne le perd pas : sur un fichier qui existe, `touch` change seulement sa date pour celle de maintenant. `cat` montre ce
qu'un fichier contient ; tu le verras à fond à la leçon 03.

### Ce que tu verrais de différent sur un autre Linux

Dans cette image, `mkdir`, `touch` et `rmdir` viennent de `rust-coreutils`, comme le `ls` de la leçon 01 ; `cp`, `mv` et
`rm` restent ceux de GNU. C'est ce que disent `--version` et la documentation d'Ubuntu. Ce que tu remarques, c'est le
texte de deux erreurs de `mkdir`. Celui de GNU reste installé sous le nom `gnumkdir`, et voici comment il a répondu :

| Commande | Le `mkdir` de cette image | `gnumkdir`, celui de GNU |
|---|---|---|
| Créer `nueva`, qui existe déjà | `mkdir: nueva: File exists` | `gnumkdir: cannot create directory ‘nueva’: File exists` |
| Créer `uno/dos/tres`, sans `-p` | `mkdir: No such file or directory` | `gnumkdir: cannot create directory ‘uno/dos/tres’: No such file or directory` |

Sur un Linux avec GNU, tu verrais le second texte, en commençant par `mkdir:`. Ce qui compte : ici, l'erreur **ne dit
pas quel chemin a échoué**. Si cela t'arrive avec plusieurs dossiers sur la même ligne, vérifie chacun avec `ls`. Les
erreurs de `rmdir` et de `touch` sont sorties identiques à celles de GNU.

### À toi : l'arbre de l'astreinte

Retourne à l'atelier avec `cd ~/practica/02` et crée, tel quel, le dossier d'astreinte de Lucía (`guardia`,
« astreinte ») :

```text
~/practica/02
└── guardia
    └── 2025-06-02
        ├── bitacoras/
        ├── reportes/
        │   └── resumen.txt
        └── pendientes.txt
```

Ceux qui se terminent par `/` sont des dossiers ; les deux autres sont des fichiers vides. (`resumen.txt` veut dire
« résumé.txt » et `pendientes.txt`, « en-attente.txt ».) Indice : `mkdir -p` et `touch` acceptent plusieurs chemins sur
la même ligne. À la fin, `ls guardia/2025-06-02` doit afficher `bitacoras  pendientes.txt  reportes`.

**Ce que cela démontre** (objectif 2.1) : la première marque de `verifica 02`, qui contrôle l'arbre **tel quel** : chaque
dossier et chaque fichier, de son type, et rien en trop. Une faute de frappe comme `reprotes` sort en rouge.

---

## Étape 3 · Copier, déplacer et renommer : `cp` et `mv`

Retourne dans le dossier d'essai avec `cd ~/practica/02/ensayo`.

### `cp` : copier

`cp` vient de *copy*, « copier ». Tu écris `cp`, la source et la destination. Regarde d'abord ce que dit `dos.txt` :

```bash
cat dos.txt
```

```text
dos
```

**Prédis avant de lancer la ligne suivante.** `dos.txt` existe déjà. Si tu copies `uno.txt` par-dessus, te
demande-t-il quelque chose ? Que dit `dos.txt` ensuite ?

```bash
cp uno.txt dos.txt
cat dos.txt
```

```text
uno
```

Il n'a rien demandé. Il a **écrasé**, en silence. Le `dos` qui était dedans n'existe plus nulle part.

**Pour qu'il demande : `-i`,** de *interactive*. Il demande avant d'écraser, et si tu réponds `n`, il ne copie pas :

```bash
cp -i tres.log uno.txt
```

```text
cp: overwrite 'uno.txt'? n
```

*« J'écrase `uno.txt` ? »* Avec `n`, `uno.txt` dit toujours `uno`. Avec `y`, de *yes*, « oui », il copie.

**Copier dans un dossier.** Si la destination est un dossier qui existe déjà, `cp` laisse la copie à l'intérieur, avec le
même nom :

```bash
cp caja/adentro.txt nueva
ls nueva
```

```text
adentro.txt
```

### `cp -r` : copier un dossier entier

**Prédis :** `cp` copie-t-il un dossier comme un fichier ?

```bash
cp caja otra
```

```text
cp: -r not specified; omitting directory 'caja'
```

*« Tu n'as pas mis `-r` ; je saute `caja`. »* (`otra` veut dire « autre ».) Pour copier un dossier avec tout ce qu'il
contient, il faut `-r`, de *recursive*, « jusqu'au fond » :

```bash
cp -r caja otra
ls otra
```

```text
adentro.txt
```

**Le piège de `cp -r`.** **Prédis :** tu lances exactement la même ligne une seconde fois. `otra` existe déjà. Que
reste-t-il ?

```bash
cp -r caja otra
ls otra
```

```text
adentro.txt  caja
```

La seconde fois, la destination était déjà un dossier, donc `cp` a mis la copie **dedans** : il y a une `caja` à
l'intérieur de `otra`. La même ligne fait deux choses selon que la destination existe ou non. Avant de copier un dossier,
regarde avec `ls` si la destination est déjà là.

### `mv` : déplacer et renommer

`mv` vient de *move*, « déplacer ». Il fonctionne comme `cp`, avec une différence : **la source disparaît**.

**Renommer, c'est déplacer au même endroit :**

```bash
mv uno.txt primero.txt
ls
```

```text
caja  dos.txt  nota.txt  nueva  otra  primero.txt  tres.log  uno
```

Il n'y a plus de `uno.txt`. C'est le même fichier, avec un autre nom. (`primero` veut dire « premier ».)

**Prédis :** `dos.txt` existe. Que se passe-t-il si tu déplaces `tres.log` par-dessus ?

```bash
mv tres.log dos.txt
cat dos.txt
```

```text
tres
```

Comme `cp` : il a **écrasé sans demander**, et `tres.log` n'existe plus. `mv -i` demande d'abord :

```bash
mv -i primero.txt dos.txt
```

```text
mv: overwrite 'dos.txt'? n
```

*« J'écrase `dos.txt` ? »*

Et si la destination est un dossier, `mv` met le fichier dedans, comme `cp` :

```bash
mv nota.txt nueva
ls nueva
```

```text
adentro.txt  nota.txt
```

**En résumé :**

| Commande | La source | Si la destination est un fichier qui existe | Si la destination est un dossier |
|---|---|---|---|
| `cp` | Reste | L'écrase sans demander | Copie dedans |
| `cp -r` | Reste | Comme `cp` | Copie dedans, la seconde fois aussi |
| `mv` | Disparaît | L'écrase sans demander | Déplace dedans |
| `cp -i`, `mv -i` | Pareil | Demande d'abord | Pareil |

### À toi : la sauvegarde et le rapport

Retourne dans `~/practica/02`. Il y a quatre tâches. **Avant chacune, écris sur ton papier ce que tu penses qu'il
va se passer** ; les trois prédictions marquées se comparent à la fin, dans « Compare tes réponses ».

1. **Copie** `config/pagos.conf` par-dessus `respaldo/pagos.conf`. Regarde d'abord ce que dit la sauvegarde avec `cat`.
   *Prédiction A :* va-t-il te demander quelque chose ? Que dira `respaldo/pagos.conf` à la fin ?
2. **Copie** le dossier `config/plantillas` en entier vers `respaldo/plantillas`, **une seule fois**.
   *Prédiction B :* si tu la copies deux fois avec la même ligne, que reste-t-il dans `respaldo/plantillas` ?
3. **Renomme** `bitacoras/hoy.log` en `bitacoras/2025-06-02.log`.
4. **Déplace** `entrada/reporte-junio.txt` vers `reportes`, où il y a déjà un `reporte-junio.txt`.
   *Prédiction C :* qu'arrive-t-il à l'ancien brouillon de `reportes` ? Et au dossier `entrada` ?

Les commandes, tu les composes toi-même, avec ce que tu as vu dans le dossier d'essai.

**Avec Claude Code.** Dans ce cours, un assistant d'IA intervient d'une seule façon : tu prédis, tu demandes, tu
lis, tu lances et tu expliques. Pour supprimer, il y a une règle de plus : tu ne lui demandes jamais la commande ;
tu lui demandes de t'expliquer la tienne. L'exercice avec Claude Code de cette leçon, sur `mv -n`, se trouve dans
« Pour aller plus loin ».

**Ce que cela démontre** (objectif 2.2) : les marques deux à cinq de `verifica 02`, et tes prédictions A, B et C, que
tu compares toi-même. `verifica` compare les **contenus**, pas seulement les noms : il sait si la sauvegarde contient la
bonne version ou l'ancienne, et si la copie de `plantillas` contient la même chose que l'original.

---

## Étape 4 · Supprimer : `rm` et `rmdir`

Avant d'écrire un seul `rm`, ce qui rend cette partie différente :

- **Il n'y a pas de corbeille.** `rm` n'envoie rien nulle part : il supprime.
- **Dans cet atelier, `rm` ne demande normalement rien :** les fichiers sont à toi et tu peux y écrire. Dans d'autres
  cas, le `rm` de GNU peut poser la question, mais ne compte pas sur cette question : regarde d'abord avec `ls`, et
  utilise `rm -i` quand tu veux qu'il confirme.
- **Toute commande qui supprime, tu l'écris à la main**, y compris les exemples de cette étape : ni recopiée de la
  leçon, ni demandée à une IA.

Reste dans le dossier d'essai, `cd ~/practica/02/ensayo`.

### `rm` : supprimer un fichier

`rm` vient de *remove*, « retirer » :

```bash
rm primero.txt
ls
```

```text
caja  dos.txt  nueva  otra  uno
```

Sans demander et sans message. `primero.txt` n'existe plus.

**Prédis :** `rm` supprime-t-il un dossier ?

```bash
rm caja
```

```text
rm: cannot remove 'caja': Is a directory
```

*« Je ne peux pas : c'est un dossier. »* C'est ainsi par défaut : `rm` ne supprime que des fichiers.

### `rmdir` et `rm -r` : supprimer des dossiers

`rmdir` vient de *remove directory*, « retirer un dossier ». **Prédis :** `caja` contient un fichier. Le supprime-t-il ?

```bash
rmdir caja
```

```text
rmdir: failed to remove 'caja': Directory not empty
```

Non. `rmdir` **ne supprime que des dossiers vides**, et c'est ce qui le rend sûr : s'il y avait quelque chose dedans, il
refuse. Avec un dossier vide, il fonctionne : `rmdir uno/dos/tres` supprime le `tres` que tu as créé à l'étape 2, en
silence.

Pour supprimer un dossier **avec tout ce qu'il contient**, il y a `rm -r`, le même `-r` que dans `cp` :

```bash
rm -r caja
```

Sans demander, `caja` et ce qu'elle contenait sont partis. `rm -r` est la commande la plus dangereuse de cette leçon : il
ne regarde pas combien il y a dedans.

### `rm -i` : qu'il demande

```bash
rm -i dos.txt
```

```text
rm: remove regular file 'dos.txt'? n
```

*« Je supprime le fichier ordinaire `dos.txt` ? »* Avec `n`, il reste. Avec un fichier vide, il dit `regular empty file`.

### Les motifs : voir d'abord ce qui va partir

Crée trois fichiers de test :

```bash
touch a.bak b.bak c.bak.txt
```

**Prédis :** lesquels le motif `*.bak` attrape-t-il ? Le `*` veut dire « n'importe quoi », donc `*.bak` est « n'importe
quel nom qui se termine par `.bak` ».

```bash
ls *.bak
```

```text
a.bak  b.bak
```

`c.bak.txt` n'est pas tombé dedans : il se termine par `.txt`. **Un motif est exact**, et c'est pourquoi il faut le
regarder avant de s'en servir.

**Le motif n'est pas lu par `ls` : il est lu par le terminal.** Avant de lancer la commande, bash remplace `*.bak` par la
liste des noms qu'il attrape, et `ls` reçoit cette liste. `echo`, qui répète ce que tu écris, te le fait voir :

```bash
echo *.bak
```

```text
a.bak b.bak
```

C'est exactement ce que recevrait `rm *.bak` : `rm a.bak b.bak`. **De là vient l'habitude de cette leçon :**

1. Écris le motif avec `ls` et lis la liste.
2. Si c'est exactement ce que tu veux supprimer, écris `rm` avec **le même motif**, lettre pour lettre.
3. Si la liste contient quelque chose en trop, change le motif et retourne au 1.

Et quand le motif n'attrape rien, bash le laisse tel quel :

```bash
ls *.zip
```

```text
ls: cannot access '*.zip': No such file or directory
```

*« Je ne peux pas accéder à `*.zip` : ce fichier ou ce dossier n'existe pas. »* `echo *.zip` répond `*.zip`. Il n'y a pas
de fichiers `.zip` : `ls` en a cherché un qui s'appellerait, littéralement, `*.zip`.

**Avec `-i`, le motif demande un par un.** Écris-le toi-même : `rm -i *.bak`. Réponds `y` au premier et `n` au
second :

```text
rm: remove regular empty file 'a.bak'? y
rm: remove regular empty file 'b.bak'? n
```

*« Je supprime le fichier ordinaire vide `a.bak` ? » et « … `b.bak` ? »*

Avec trois cents fichiers, l'habitude ci-dessus vaut plus que `-i`.

### À toi, sans IA : le nettoyage

Cet exercice se fait **sans IA et sans recopier de commandes** : ni de cette leçon ni d'ailleurs. Tu vas dans
`~/practica/02/limpieza` et tu fais ce qu'on a demandé à Lucía :

1. **Supprime les fichiers dont le nom se termine par `.tmp`, et rien d'autre.** D'abord, écris un motif avec `ls` et
   lis ce qu'il attrape. Attention aux voisins : `cache-indice.txt` et `tmp-notas.txt` restent.
2. **Note ton motif** avec `anota`, **entre apostrophes simples**. Pour un motif `ejemplo*` (« exemple* »), ce serait
   `anota 02 patron 'ejemplo*'`. (`patron` est la clé de ta feuille de réponses : « motif ».) Sans apostrophes, le
   terminal remplacerait ton motif par les noms qu'il attrape avant que `anota` ne le voie.
3. **Supprime les `.tmp`** avec un `rm` que tu écris toi-même, avec le même motif.
4. **Supprime le dossier `sesiones-viejas`**, avec tout ce qu'il contient, **et le dossier `vacia`**. Commence par
   `rmdir` sur les deux et lis ce que répond chacun : il te dit lequel demande autre chose. `sesiones-activas`
   reste.
5. Lance `verifica 02`.

Si tu supprimes quelque chose en trop **dans cet atelier**, il ne se passe rien de grave : `prepara 02` le restaure.
Sers-toi-en pour pratiquer la pause de regarder avant de supprimer.

**Ce que cela démontre** (objectif 2.3, première partie) : les marques six à huit de `verifica 02`. La six teste ton
motif contre les huit noms que `prepara` a mis dans `limpieza`. La sept vérifie que ce qui était demandé n'existe plus.
La huit, que **tout le reste est toujours là, avec son contenu**. Ces deux-là comptent autant : supprimer trop peu se
corrige ; supprimer trop, non.

---

## Étape 5 · La variable vide, vue seulement avec `echo`

⛔ **À cette étape, aucun `rm` ne se lance.** Tout se voit avec `echo` devant, qui écrit seulement la commande à l'écran.
Si tu tapes une de ces lignes sans le `echo`, tu ne fais pas l'exercice.

### La suppression qui grandit toute seule

Les scripts de maintenance gardent le dossier qu'ils nettoient dans une **variable**, `CARPETA` (« dossier » ; le strict
minimum, dans l'encadré à la fin de cette étape), et ils suppriment avec une ligne comme celle-ci : `rm -rf "$CARPETA/"`.
`-f`, de *force*, dit à `rm` de ne rien demander et de ne pas se plaindre si quelque chose n'existe pas.

**Prédis :** si `CARPETA` est vide, que demande de supprimer cette ligne ? Écris ceci, avec le `echo` :

```bash
CARPETA=
echo rm -rf "$CARPETA/"
```

```text
rm -rf /
```

La variable vide a disparu, et la barre oblique qui venait après est restée seule. **`/` est la racine : tout le
système.** Un script qui devait supprimer un dossier d'entraînement demande de tout supprimer. S'il y a un `*` au bout de
la ligne, c'est pire :

```bash
echo rm -rf "$CARPETA"/*
```

```text
rm -rf /bin /boot /dev /etc /home /lib /media /mnt /opt /proc /root /run /sbin /srv /sys /tmp /usr /var
```

Chaque dossier de la racine. Selon son manuel, le `rm` de GNU refuse de supprimer `/` tel quel (`--preserve-root`) ; avec
le `*`, il ne reçoit plus `/`, mais cette liste. Nous ne l'avons pas testé, et tu ne vas pas le tester non plus.

**Un cas signalé.** En janvier 2015, le ticket public n° 3671 de Steam pour Linux a décrit une perte de fichiers après
l'exécution de `steam.sh`. Des commentaires du fil ont signalé `rm -rf "$STEAMROOT/"*` et le risque si `STEAMROOT`
restait vide. C'est une raison de valider les variables ; tu n'as pas besoin de tester l'incident pour apprendre la
règle.

### Pourquoi tu ne le testes pas, pas même dans le conteneur

**Le conteneur est jetable. Ton dossier `~/practica`, non :** il vit dans un volume qui survit au conteneur. Regarde à
qui appartient chaque chose :

```bash
ls -ld / /usr /etc ~/practica
```

```text
drwxr-xr-x 1 root   root   4096 Nov 19 09:28 /
drwxr-xr-x 1 root   root   4096 Nov 19 09:28 /etc
drwxr-xr-x 4 alumno alumno 4096 Nov 19 09:28 /home/alumno/practica
drwxr-xr-x 1 root   root   4096 Nov  6 10:27 /usr
```

Les dossiers du système sont à `root`, l'administrateur : `touch /usr/prueba` répond `Permission denied` (« permission
refusée »). Mais `~/practica` est à toi, et ce qui est à toi, tu peux le supprimer. Tes réponses ne reviennent pas
avec `./practicar`.

### Le frein : `${CARPETA:?}`

Les scripts se protègent avec un frein qui a un nom : `${CARPETA:?}`. Si la variable est vide, le terminal refuse de
lancer la ligne. Avec `echo` devant, comme toujours à cette étape :

```bash
CARPETA=
echo rm -rf "${CARPETA:?}/"
```

```text
bash: CARPETA: parameter null or not set
```

*« La variable est vide ou n'existe pas. »* Et le `echo` **ne s'est pas exécuté** : selon le manuel de bash, le terminal
annule la commande, et avec un `rm`, elle ne s'exécuterait pas non plus. Pour toi, cela suffit : **si un `rm` porte une
variable, la variable s'écrit `${NOM:?}`**.

> **Le strict minimum sur une variable.** `CARPETA=ensayo` garde `ensayo` sous le nom `CARPETA` : tout collé, sans
> espaces. Avec un `$` devant, `$CARPETA`, le terminal remplace le nom par sa valeur avant de lancer la commande, comme
> il remplace un motif par la liste. Une variable vide, ou qui n'a jamais rien gardé, vaut rien, **sans donner d'erreur**.
> Ce cours ne va pas plus loin que cela.

**Ce que cela démontre** (objectif 2.3, seconde partie) : ton explication des questions 9 et 10 de « Pour conclure »,
comparée à « Compare tes réponses ».

---

## Étape 6 · Contrôle avec `verifica 02`

Lance `verifica 02` quand tu veux, depuis n'importe quel dossier. Juste après avoir monté l'atelier, sept marques
sortent en rouge et une en vert : la dernière, parce que tu n'as encore rien supprimé de ce qui devait rester.

Voici comment se lit un rouge de la marque du motif, avec le `ejemplo*` utilisé plus haut comme exemple :

```text
❌ 2.3 · Tu patrón «ejemplo*» deja fuera «cache-01.tmp», que sí se borra (medí: probé tu patrón contra los 8 nombres que prepara puso en limpieza)
```

*Ton motif « ejemplo* » laisse de côté « cache-01.tmp », qui, lui, doit être supprimé (j'ai mesuré : j'ai testé ton
motif contre les 8 noms que prepara a mis dans limpieza).*

Il dit ce que tu as noté, ce qui a été mesuré et ce qui a échoué, mais pas quel est le bon motif.

**Ce qui ne le trompe pas :** un lien, le raccourci de la leçon 01. Un lien vers `config/pagos.conf` « dit » la même
chose qu'une copie, mais ce n'est pas une sauvegarde : si l'original se perd, le lien n'a rien. `verifica 02` vérifie le
vrai chemin de chaque chose, et un lien à la place d'un fichier ou d'un dossier sort en rouge. Un **lien physique**
(`ln`) ne le trompe pas non plus : un second nom pour le même fichier, qui ressemble à un fichier normal. Si tu changes
l'un, l'autre change. `verifica` le remarque parce que les deux noms partagent l'*inode*, le numéro par lequel le disque
connaît le fichier.

Corrige et recommence jusqu'à voir ceci à la fin :

```text
✅ Lección 02: todo en verde.
```

*Leçon 02 : tout est au vert.*

---

## Tes exercices

L'exercice 2.3 se fait **sans IA**, et toutes ses commandes de suppression, c'est toi qui les écris.

| # | Tu fais | C'est fait quand |
|---|---|---|
| 2.1 | Tu crées l'arbre de l'astreinte de l'étape 2 | `verifica 02` marque l'arbre en vert |
| 2.2 | Les quatre tâches de l'étape 3, avec tes prédictions A, B et C écrites d'avance | `verifica 02` marque les quatre en vert, et tu as comparé tes prédictions avec « Compare tes réponses » |
| 2.3 | Le nettoyage de l'étape 4 : tu regardes avec ton motif, tu le notes et tu supprimes à la main | `verifica 02` s'affiche entièrement au vert, et tu as répondu aux questions 9 et 10 de « Pour conclure » |

**Durée (estimée) :** de 60 à 90 minutes. C'est une estimation : elle sera ajustée avec le premier groupe qui suivra le
cours.

---

## Pour aller plus loin

*Facultatif. Rien dans cet encadré n'est évalué ni nécessaire pour les exercices.*

- **`-v` raconte ce qu'il a fait.** Avec `cp -v`, `mv -v`, `rm -v` et `mkdir -v`, chaque commande écrit une ligne par
  chose touchée. `rm -v` a répondu, par exemple, `removed 'viaje/vuelta/dos.txt'` (« supprimé `viaje/vuelta/dos.txt` »).
- **Des accolades pour plusieurs dossiers.** `mkdir -p viaje/{ida,vuelta}` crée `viaje/ida` et `viaje/vuelta`. (`viaje`
  veut dire « voyage », `ida` « aller » et `vuelta` « retour ».) Les accolades sont développées par bash, comme le `*` :
  vérifie-le avec `echo viaje/{ida,vuelta}`.
- **Avec Claude Code : `mv -n`.** Tu prédis sur ton papier ce que fait `mv -n`. Tu demandes à Claude Code, sur
  ton ordinateur : « Que fait `mv -n` et en quoi se distingue-t-il de `mv -i` ? ». Tu lis `-n` dans `mv --help` et
  tu compares. Tu lances, dans le dossier d'essai : `touch a.txt b.txt`, `mv -n a.txt b.txt` et `ls`. Tu expliques
  avec tes mots si cela a coïncidé. Ce qui s'est passé lors du test se trouve dans « Compare tes réponses ».
- **`rm -I` ne demande qu'une fois.** Avec plus de trois fichiers, ou avec `-r`, il demande avant de commencer : `rm -I x*`
  sur quatre fichiers a répondu `rm: remove 4 arguments?` (« je supprime 4 arguments ? »). Avec `n`, il n'en a supprimé
  aucun.

---

## Ça ne marche pas ?

| Tu vois | Cause probable | Ce que tu fais |
|---|---|---|
| `bash: prepara: …` ou `Todavía no hay prepara para la lección «02».` *(Il n'y a pas encore de prepara pour la leçon « 02 ».)* | Ton image est d'avant cette leçon | Sors avec `exit` et lance `./practicar reconstruir` |
| `Ya existe /home/alumno/practica/02.` et une question *(« /home/alumno/practica/02 existe déjà. »)* | Tu avais déjà monté l'atelier, et le monter de nouveau le supprime | Pour repartir de zéro, écris `si` (« oui »). Sinon, appuie sur Entrée : rien n'est supprimé |
| `mkdir: No such file or directory` | Il manque un dossier au milieu, et ce `mkdir` ne dit pas lequel | Utilise `mkdir -p`, ou vérifie chaque tronçon du chemin avec `ls` |
| `rmdir: failed to remove …: Directory not empty` | Le dossier contient quelque chose | Regarde ce qu'il contient avec `ls -a` avant de décider |
| `❌ … son nombres, no un patrón` *(… ce sont des noms, pas un motif)* | Tu as noté le motif sans apostrophes, et le terminal l'a remplacé par les noms | `anota 02 patron` et ton motif entre apostrophes simples |
| `❌ … Copiaste dos veces` *(… Tu as copié deux fois)* | Tu as lancé deux fois le même `cp -r` | Supprime `respaldo/plantillas` avec soin et copie-la une fois, ou `prepara 02` |
| `❌ … ya no está, y no se pedía borrarlo` *(… n'est plus là, et il n'était pas demandé de le supprimer)* | Ton motif ou ton `rm -r` a emporté trop | Dans cet atelier, il ne se passe rien de grave : `prepara 02` le restaure, et tu recommences |
| `❌ … es un enlace…` *(… c'est un lien…)* | Il y a un raccourci là où devait aller un vrai fichier ou un vrai dossier | Fais-le avec `cp` ou `mv`, ou `prepara 02` |
| `❌ … es un enlace duro…` *(… c'est un lien physique…)* | Ce fichier est le même que l'original, avec un autre nom | Supprime-le avec soin et copie-le avec `cp` |

---

## Pour conclure

**Réponds sans relire.**

**Cinq situations : que se passe-t-il ?** Tu réussis avec 4 sur 5.

1. `cp a.txt b.txt`, et `b.txt` existait déjà avec un autre texte.
2. `mv a.txt docs`, et `docs` est un dossier qui existe.
3. `mkdir x/y`, et `x` n'existe pas.
4. `rmdir fotos`, et `fotos` contient douze photos.
5. `touch notas.txt`, et `notas.txt` contenait déjà trois pages de notes.

**Et cinq questions :**

6. Quelle différence y a-t-il entre `cp` et `mv` dans ce qui arrive à la source ?
7. Tu vas supprimer avec `rm *.log`. Que lances-tu d'abord, et que cherches-tu dans ce qu'il répond ?
8. Tu as écrit `anota 02 patron *.tmp`, sans apostrophes, en te tenant dans `limpieza`. Qu'est-ce qui a été noté, et
   pourquoi ?
9. Explique avec tes mots, en trois lignes, pourquoi `rm -rf "$CARPETA/"` est dangereux si `CARPETA` est vide.
10. Comment vois-tu, sans rien lancer de dangereux, ce que cette ligne supprimerait ? Et comment la protèges-tu ?

**Ce qui vient ensuite :** tu sais maintenant créer, copier, déplacer et supprimer sans perdre ce que tu ne voulais
pas perdre. À la leçon 03, tu ouvres ce qu'il y a à l'intérieur des fichiers : tu lis de longs journaux sans ouvrir
d'éditeur.

---

> **Ce que fait Hábil**
>
> Nous exploitons et modernisons l'infrastructure où tournent des systèmes qui ne peuvent pas tomber en panne :
> serveurs Linux, conteneurs, Kubernetes et livraison continue, pour la banque, l'assurance et les entreprises
> réglementées. Si tes serveurs dépendent d'une personne qui « sait où se trouve chaque chose », cela peut être mis en
> ordre sans arrêter l'exploitation.
>
> [**CI/CD, DevSecOps et cloud sans accroc**](https://www.habil.mx/fr/architecture-cloud-native/)
>
> Premier échange, sans frais.

---

## Compare tes réponses

**Prédiction A (copier par-dessus la sauvegarde).** Il ne demande rien. À la fin, `respaldo/pagos.conf` dit
`puerto=8443 version=2025-06-02 (la buena, la de hoy)` (« la bonne, celle d'aujourd'hui »), comme `config/pagos.conf`. La
version du port 8080 s'est perdue : ici, c'était ce que tu voulais, mais avec un autre fichier, ce pourrait être la
seule que tu ne voulais pas perdre.

**Prédiction B (`cp -r` deux fois).** La première fois, il crée `respaldo/plantillas` avec ses trois fichiers. La
seconde, comme `respaldo/plantillas` existe déjà, il met une autre copie dedans : il reste `respaldo/plantillas/plantillas`.
Voici ce que `ls` a répondu lors du test :

```text
'aviso de corte.txt'   aviso.txt   plantillas   recibo.txt
```

Les apostrophes ne font pas partie du nom : `ls` les met parce que le nom contient des espaces.

**Avec Claude Code : `mv -n`.** Lors du test, `mv -n a.txt b.txt` n'a rien déplacé et n'a rien dit : `ls` a continué à
afficher `a.txt  b.txt`. `-n` veut dire « n'écrase jamais », en silence ; `-i` demande. Si Claude Code t'a dit autre
chose, ce que tu as vu dans ton terminal l'emporte.

**Prédiction C (déplacer par-dessus le brouillon).** `mv` ne demande rien : l'ancien brouillon de `reportes` est écrasé
et perdu, et dans `reportes/reporte-junio.txt` il reste la nouvelle version. Le fichier disparaît de `entrada`, mais le
dossier `entrada` reste, vide : `mv` déplace ce que tu lui demandes et rien de plus.

**Exercice 2.3.** Le contrôle, c'est `verifica 02` ; la commande n'est pas donnée ici. Pour savoir si tu es sur la
bonne voie :

- Ton motif attrape les trois `.tmp` et aucun voisin. `cache*` emporte `cache-indice.txt` ; `*tmp*` emporte
  `tmp-notas.txt`. Le bon regarde **comment le nom se termine**.
- `rmdir` a supprimé `vacia` et a refusé avec `sesiones-viejas`. Celui-là demandait `rm -r`, et tu l'as regardé avec
  `ls` avant.

**Pour conclure.**

1. `b.txt` se retrouve avec le texte de `a.txt`, sans demander. Ce qu'il contenait avant est perdu.
2. `a.txt` se retrouve dans le dossier : `docs/a.txt`. Il n'est plus là où il était.
3. Cela échoue avec `No such file or directory`, et dans cette image sans dire quel chemin. Avec `mkdir -p x/y`, les deux
   sont créés.
4. Il refuse : `Directory not empty`. `rmdir` ne supprime que des dossiers vides, et rien n'est perdu.
5. Les notes sont toujours là. `touch` change seulement sa date pour celle de maintenant.
6. Avec `cp`, la source reste et une copie apparaît. Avec `mv`, la source disparaît : le fichier change d'endroit ou de
   nom.
7. `ls *.log`, avec le même motif. Tu cherches à ce qu'il n'apporte que ce que tu veux supprimer ; s'il en apporte
   trop, tu changes le motif.
8. Les noms des trois `.tmp`. Bash a remplacé `*.tmp` par la liste avant de lancer `anota`, comme avec `rm` et avec `ls`.
   `verifica` le détecte et te demande les apostrophes.
9. Quelque chose comme ceci : si `CARPETA` est vide, le terminal la remplace par rien et la ligne devient `rm -rf /`, qui
   demande de supprimer la racine, tout le système. `-r` entre dans tous les dossiers et `-f` ne demande rien. Avec un `*`
   à la fin, il reçoit un à un les dossiers de la racine, et `--preserve-root` ne l'arrête plus.
10. Tu le vois avec `echo` devant : `echo rm -rf "$CARPETA/"` écrit la commande sans la lancer. Tu la protèges avec
    `"${CARPETA:?}/"` : si la variable est vide, bash annule la commande avant de la lancer.

---

## Sources de la leçon

Consultées le 19 nov. 2026. Toutes sont en anglais.

- Ubuntu Manpages. *rm(1)*, Ubuntu 26.04, GNU coreutils 9.7 (sans `-r`, il ne supprime pas de dossiers ; `-i`, `-I`, `-r`,
  `-f` ; `--preserve-root` par défaut ; la question devant des fichiers où l'on ne peut pas écrire).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/rm.1.html
- Ubuntu Manpages. *cp(1)*, Ubuntu 26.04, GNU coreutils 9.7 (`-i`, `-r`).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/cp.1.html
- Ubuntu Manpages. *mv(1)*, Ubuntu 26.04, GNU coreutils 9.7 (renomme ou déplace dans un dossier ; `-i`, `-n`).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/mv.1.html
- Ubuntu Manpages. *mkdir(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (`-p`).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/mkdir.1.html
- Ubuntu Manpages. *touch(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (change la date ; crée le fichier s'il n'existe pas).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/touch.1.html
- Ubuntu Manpages. *rmdir(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (seulement les dossiers vides).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/rmdir.1.html
- Ubuntu Manpages. *bash(1)*, Ubuntu 26.04, bash 5.3, le manuel de bash : `${parameter:?word}` (avec la variable vide, un
  terminal interactif ne lance pas la commande), *Pathname Expansion* (bash remplace le motif par la liste des noms, ou
  le laisse tel quel s'il n'attrape rien). https://manpages.ubuntu.com/manpages/resolute/en/man1/bash.1.html
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, section « rust-coreutils » (`cp`, `mv` et `rm` restent ceux de GNU).
  https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- GitHub, dépôt ValveSoftware/steam-for-linux, ticket 3671, ouvert le 14 janvier 2015 : le signalement d'un utilisateur et
  les commentaires du fil, qui signalent `rm -rf "$STEAMROOT/"*`. https://github.com/ValveSoftware/steam-for-linux/issues/3671
- Ubuntu Manpages. *ln(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (crée des liens physiques par défaut ; avec `-s`,
  symboliques). https://manpages.ubuntu.com/manpages/resolute/man1/ln.1.html
- Nos propres mesures, 19 nov. 2026, sur un Mac avec Apple silicon (Docker 29.2.1 avec Colima, linux/arm64), avec l'image
  du cours sur Ubuntu 26.04.1 : `mkdir`, `touch` et `rmdir` de uutils coreutils 0.10.0, `cp`, `mv` et `rm` de GNU
  coreutils 9.7, et bash 5.3.9. De là viennent les écrans, le tableau de `mkdir` et le temps de `./practicar reconstruir`.
