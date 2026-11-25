# Leçon 03 — Voir ce qu'il y a dedans

**Par Dorian Chávez, fondateur de Hábil et architecte d'intégration.**

**Avant de lire, cinq mots.** Une ligne chacun ; les autres arrivent à l'étape où ils servent.

- **Journal :** le fichier où un programme note ce qu'il fait, une ligne par chose et avec son heure. En anglais, et sur
  les serveurs, tu le verras sous le nom de *log*.
- **Ligne :** une ligne d'un fichier. Elle se termine par un saut de ligne, l'Entrée qu'on ne voit pas. Quand une
  commande compte des lignes, elle compte ces sauts.
- **Texte et binaire :** un fichier texte contient des lettres, des chiffres et des signes que tu peux lire. Un fichier
  binaire contient des octets pensés pour un programme, pas pour tes yeux : une photo, un programme, un fichier compressé.
- **Paginateur :** un programme qui te montre un texte écran par écran et te laisse aller et venir dedans. Celui de cette
  leçon s'appelle `less`.
- **Compressé :** un fichier qu'un programme, comme `gzip`, a rendu plus petit. Pour le lire, il faut d'abord le
  décompresser.

**Ce que tu en retires :** tu affiches un fichier entier avec `cat`, aussi avec le numéro de chaque ligne ; tu sors le
début, la fin ou une ligne précise avec `head` et `tail` ; tu comptes les lignes avec `wc -l` ; tu feuillettes et tu
cherches dans `less`, et tu en sors sans te perdre ; et avant d'ouvrir un fichier que tu ne connais pas, tu demandes à
`file` ce que c'est.

**Idée nouvelle de cette leçon (la seule qui est évaluée) :** sur un serveur, presque tout est du texte, et il se lit
sans ouvrir d'éditeur. Avant de lire, **tu décides de quel morceau tu as besoin** : tout, le début, la fin, une ligne,
combien il y en a, ou feuilleter pour chercher quelque chose. La question n'est pas de savoir quelles commandes existent,
mais quel morceau demande chaque situation.

**Dans quel ordre :** tu montes une salle d'astreinte (étape 1), tu lis des fichiers entiers (étape 2), tu sors des
morceaux et tu comptes (étape 3), tu feuillettes et tu cherches (étape 4), tu demandes ce qu'est un fichier avant de
l'ouvrir (étape 5) et, à la fin, tu contrôles tout avec `verifica` (étape 6).

**Une note sur la langue de la salle.** Comme dans les leçons 00, 01 et 02, les messages du cours et les noms de la
salle sont en espagnol et font partie du code : ils ne sont donc pas traduits. Les messages apparaissent exactement comme
le terminal les affiche, avec une glose en français, en italique, sous chaque bloc, ou entre parenthèses à l'intérieur
d'un tableau. Les noms de la salle sont glosés une seule fois, à l'étape 1 : `pagos` (« paiements »), `ensayo`
(« essai »), `corto` (« court »), `cien` (« cent »), `arranque` (« démarrage »), `reportes` (« rapports »),
`sucursales` (« succursales ») et le reste. Les sept clés de la feuille de réponses sont glosées à l'étape 2 et aux
suivantes, là où tu les utilises pour la première fois : `espera` (« attente »), `folio-137`, `ultima-hora` (« dernière
heure »), `renglones` (« lignes »), `sucursales`, `primer-error` (« première erreur ») et `tipo-respaldo` (« type de
sauvegarde »). Les commandes `practicar`, `prepara`, `verifica` et `anota` gardent aussi leur nom : tu les connais déjà
par les leçons précédentes.

---

## La scène

*Scénario inventé. Les écrans de la leçon ont été copiés du terminal d'entraînement.*

Jeudi, 3 h 12. Le téléphone de Lucía vibre sur la table.

« Les succursales refusent toutes les cartes. Ça a commencé il y a un moment, on ne sait pas quand. Que dit le journal
des paiements ? »

Lucía sait déjà arriver au dossier sans le voir, et elle sait qu'on n'y supprime rien sans regarder. Ce qu'elle n'a
jamais fait, c'est **lire** un fichier sur une machine sans fenêtres. Il n'y a pas de double-clic. Il y a la même ligne
de toujours, qui attend.

Le journal compte des centaines de lignes, et sur un vrai serveur il pourrait en compter des millions. Quelque part dans
ce fichier se trouve la réponse : la ligne où tout a commencé, et la dernière, ce qui a été écrit de plus récent. À côté,
il y a un autre journal, celui d'hier, avec un nom qui finit par `.gz` et qui ne se laisse pas lire comme les autres. À
l'autre bout, quelqu'un attend une heure, un nombre et une phrase pour décider quoi faire de centaines de paiements.

Un point joue en sa faveur : les journaux sont souvent du texte. *The Twelve-Factor App*, le guide d'Adam Wiggins sur la
façon de construire des applications qui tournent sur des serveurs, le décrit ainsi : à l'état brut, beaucoup de journaux
sont du texte, dans l'ordre du temps, et ils portent souvent un événement par ligne ; une exception courante est la trace
d'une erreur, qui peut en occuper plusieurs. Si c'est du texte, il se lit avec les commandes de cette leçon.

Cette leçon est cette demi-heure, dans une salle d'astreinte conçue pour lire sans conséquences.

---

## Avant de commencer

**Ce dont tu as besoin :** la leçon 02 faite ; le matériel du cours à jour ; et du papier et un crayon pour tes
prédictions.

**Mets à jour ton terminal d'entraînement.** Comme aux leçons précédentes, depuis ton ordinateur, dans le dossier du
cours :

```bash
git pull
./practicar reconstruir
```

Cette fois, l'image ajoute trois paquets : `file`, `gzip` et `ncurses-bin`. Dans cette version, la reconstruction depuis
zéro n'a pas été mesurée : selon Docker et ce que tu as déjà sur ta machine, elle peut demander le réseau et durer plus
longtemps qu'à la leçon 02. Elle doit se terminer par `✅ Imagen lista.` (« Image prête. ») Ton dossier `~/practica` n'est
pas touché. Ensuite, entre avec `./practicar`.

**Où cette leçon a été testée :**

| Système | État |
|---|---|
| Linux (mx1) | Testé le 25 nov. 2026 sur la branche `leccion/03` (3bf1787), mesuré par master : hôte x86_64, Docker 29.10.0 ; l'image s'est construite en 9 s et pèse 232 656 309 octets ; Ubuntu 26.04, `cat` (uutils coreutils) 0.10.0, `less` 668 et `file-5.46`. Ont tourné `prepara 03`, chaque commande des étapes 1 à 6, et `verifica 03` en rouge et en vert, quand la feuille avait huit clés. `verifica 03` avec les sept clés actuelles a été mesuré de nouveau le même jour (branche en 1a3a7b5) : rouge avec 7 ❌, vert avec 7 ✅, vert aussi après `simula 03`, et `anota 03 arranque` se refuse. Non mesuré : `./practicar reconstruir` (l'image a été construite avec `docker build`) et `prepara 03` sur une salle déjà montée |
| Mac avec Apple silicon, Ubuntu, Windows avec Docker Desktop et WSL 2 | Pas encore testé avec cette leçon |

**Ce qui va coïncider avec ton écran et ce qui ne le fera pas.** Les noms, les contenus et les dates de la salle, ainsi
que les messages des commandes, coïncident. Changent la disposition en colonnes, les couleurs et la taille de ta
fenêtre, qui décide combien de lignes tiennent à l'écran.

---

## Étape 1 · Monte la salle d'astreinte

À l'intérieur de ton terminal d'entraînement :

```bash
prepara 03
```

```text
✅ Sala de guardia lista en /home/alumno/practica/03 (medí: 3 carpetas y 8 archivos, cada uno con
   su contenido; las dos bitácoras, por su huella).
   Te dejé la hoja de respuestas, con sus claves vacías: /home/alumno/practica/respuestas/03.md
   Empieza por ahí:  cd ~/practica/03
```

*Salle d'astreinte prête dans `/home/alumno/practica/03` (j'ai mesuré : 3 dossiers et 8 fichiers, chacun avec son
contenu ; les deux journaux, par leur empreinte). Je t'ai laissé la feuille de réponses, avec ses clés vides :
`/home/alumno/practica/respuestas/03.md`. Commence par là : `cd ~/practica/03`.*

`prepara 03` fonctionne comme celui de la leçon 02 : il ne laisse rien au hasard et il ne touche que `~/practica/03` et ta
feuille de réponses. Si la salle existe déjà, `prepara 03` t'expliquera avant de la remplacer ; ce cas n'a pas été mesuré
dans cette version. Les journaux ne sont pas écrits ligne par ligne : ils sortent d'une règle fixe, et `prepara` vérifie
à la fin que chacun a son *empreinte*, une sorte de signature du contenu. Si un seul caractère change, l'empreinte ne
coïncide plus.

**La salle, vue d'en haut :**

```text
~/practica/03
├── LEEME.txt
├── ensayo              ← para probar los comandos
│   ├── corto.txt       ← tres renglones
│   └── cien.txt        ← cien renglones numerados
├── pagos
│   ├── pagos.conf      ← la configuración del servicio
│   ├── pagos.log       ← la bitácora de hoy, la larga
│   ├── pagos.log.1.gz  ← la bitácora de ayer, comprimida
│   └── arranque.log    ← la del reinicio del servicio
└── reportes
    └── sucursales.csv  ← las sucursales que reportaron rechazos
```

*Les commentaires du dessin : « pour essayer les commandes », « trois lignes », « cent lignes numérotées », « la
configuration du service », « le journal d'aujourd'hui, le long », « le journal d'hier, compressé », « celui du
redémarrage du service » et « les succursales qui ont signalé des refus ». Les noms, en français : `LEEME.txt`
(« LISEZ-MOI.txt »), `ensayo` (« essai »), `corto.txt` (« court.txt »), `cien.txt` (« cent.txt »), `pagos`
(« paiements »), `pagos.conf` (« paiements.conf »), `pagos.log` (« paiements.log »), `arranque.log`
(« démarrage.log »), `reportes` (« rapports ») et `sucursales.csv` (« succursales.csv »).*

Les démonstrations se font dans `ensayo` ; les exercices, dans `pagos` et `reportes`. **Dans cette salle, rien ne s'édite
et rien ne se supprime :** `verifica 03` contrôle que tout est resté comme `prepara` l'a laissé, et si quelque chose a
changé, il te le dit.

Ta feuille de réponses contient sept clés, une pour chaque chose que tu vas lire. Elle se remplit comme aux leçons
précédentes, avec `anota`.

**Ce que cela démontre :** pas encore. Cette étape prépare le matériel.

---

## Étape 2 · Tout d'un coup : `cat`

Entre dans l'essai :

```bash
cd ~/practica/03/ensayo
```

`cat` vient de *concatenate*, « enchaîner » : son manuel dit qu'il colle un ou plusieurs fichiers et les écrit à l'écran.
Avec un seul fichier, il l'écrit en entier :

```bash
cat corto.txt
```

```text
primer renglón
segundo renglón
tercer renglón, el último
```

*première ligne, deuxième ligne, troisième ligne, la dernière.*

À la leçon 00, sans fichier, il restait à attendre. Avec un fichier, il lâche tout et te rend l'invite.

**Avec deux fichiers, il les enchaîne.** **Prédis :** que sort-il avec `cat corto.txt corto.txt` ?

```text
primer renglón
segundo renglón
tercer renglón, el último
primer renglón
segundo renglón
tercer renglón, el último
```

L'un derrière l'autre, sans séparation. C'est pour cela qu'il porte ce nom.

### `cat -n` : avec le numéro de chaque ligne

L'option `-n`, de *number*, donne son numéro à chaque ligne :

```bash
cat -n corto.txt
```

```text
     1	primer renglón
     2	segundo renglón
     3	tercer renglón, el último
```

Le numéro **ne fait pas partie du fichier** : `cat` l'ajoute en l'affichant. Il sert à répondre à « sur quelle ligne se
trouve telle chose ? », ce qu'on te demandera pendant une astreinte.

### Ce qui ne tient pas à l'écran

**Prédis avant de lancer :** `cien.txt` a cent lignes, et ta fenêtre en a moins. Que vas-tu voir avec `cat cien.txt` ?

```bash
cat cien.txt
```

```text
renglón 078 de 100
renglón 079 de 100
renglón 080 de 100
renglón 081 de 100
renglón 082 de 100
renglón 083 de 100
renglón 084 de 100
renglón 085 de 100
renglón 086 de 100
renglón 087 de 100
renglón 088 de 100
renglón 089 de 100
renglón 090 de 100
renglón 091 de 100
renglón 092 de 100
renglón 093 de 100
renglón 094 de 100
renglón 095 de 100
renglón 096 de 100
renglón 097 de 100
renglón 098 de 100
renglón 099 de 100
renglón 100 de 100
```

*ligne 078 sur 100, et ainsi de suite jusqu'à la ligne 100 sur 100.*

Tu vois la fin, jusqu'à `renglón 100 de 100`. Le début est parti de l'écran : `cat` ne sait rien des écrans, il écrit
tout et ce qui ne tient pas s'en va vers le haut. Avec cent lignes, tu peux remonter avec la molette de la souris ; avec
un million, non.

**De là sort la règle de cette leçon :** `cat` est fait pour les fichiers courts, ceux dont tu sais qu'ils tiennent.
Pour les longs, tu demandes un morceau (étape 3) ou tu les feuillettes (étape 4). Et pour lire un journal sur un serveur,
**choisis une commande de lecture** : elle ne modifie pas le fichier et démarre mieux avec les journaux longs. Un
éditeur peut changer le fichier si tu touches une touche de trop, et un fichier énorme peut mettre longtemps à s'ouvrir.

### À toi : la configuration

Retourne dans la salle avec `cd ~/practica/03`. La configuration du service de paiements est courte : `cat` est le bon
outil.

1. Affiche-la en entier, **avec les numéros de ligne**.
2. Les lignes qui commencent par `#` sont des commentaires, des notes pour les personnes : le service ne les lit pas.
   Repère lesquelles ce sont.
3. Cherche la ligne qui dit combien de secondes le service attend entre une nouvelle tentative et la suivante : son nom
   contient le mot `espera_segundos` (« secondes d'attente »). Note **le numéro de cette ligne** :

   ```bash
   anota 03 espera 1
   ```

   Là, le `1` est un exemple : à sa place va le numéro que tu as lu.

**Ce que cela démontre** (objectif 3.1) : la marque `espera` de `verifica 03`.

---

## Étape 3 · Juste un morceau : `head`, `tail` et `wc`

Retourne dans l'essai avec `cd ~/practica/03/ensayo`.

### `head` : le début

`head` veut dire « tête ». Selon son manuel, si tu ne lui dis pas combien, il écrit les dix premières lignes.
**Prédis :** quelle est la dernière que tu vas voir avec `head cien.txt` ?

```bash
head cien.txt
```

```text
renglón 001 de 100
renglón 002 de 100
renglón 003 de 100
renglón 004 de 100
renglón 005 de 100
renglón 006 de 100
renglón 007 de 100
renglón 008 de 100
renglón 009 de 100
renglón 010 de 100
```

De la `renglón 001` à la `renglón 010`. Dix, comme le dit le manuel. Avec `-n` et un nombre, tu lui dis combien :

```bash
head -n 3 cien.txt
```

```text
renglón 001 de 100
renglón 002 de 100
renglón 003 de 100
```

### `tail` : la fin

`tail` veut dire « queue ». Il fonctionne comme `head`, mais depuis la fin : dix lignes si tu ne lui dis rien d'autre, et
celles que tu demandes avec `-n`.

```bash
tail -n 2 cien.txt
```

```text
renglón 099 de 100
renglón 100 de 100
```

Dans un journal en ordre, la fin est le plus récent, et **`tail` est souvent le premier coup d'œil pendant une
astreinte** : il te dit la dernière chose qui *a été enregistrée*. Lis-la avec son heure et sers-t'en comme d'une piste ;
dans une vraie astreinte, tu confirmes ensuite l'état avec le signal qui correspond au service.

### Une ligne précise, avec ce que tu as déjà

On te demande la ligne 42. **Prédis :** quelle est la dernière ligne qu'écrit `head -n 42` ?

```bash
head -n 42 cien.txt
```

Sortie raccourcie : les trois dernières des 42 lignes.

```text
renglón 040 de 100
renglón 041 de 100
renglón 042 de 100
```

La dernière qui apparaît est `renglón 042 de 100`. `head -n 42` écrit les lignes de 1 à 42, donc celle qui reste tout en
bas, juste au-dessus de l'invite, **c'est la 42**. Tu n'as rien à compter : tu lis la dernière. À la leçon 06, tu verras
comment ne garder que celle-là, sans les 41 du dessus. Pour l'instant, cette astuce suffit.

### `wc` : compter sans lire

`wc` vient de *word count*, « compter les mots », même s'il compte trois choses. Sans options, il écrit trois nombres et
le nom du fichier :

```bash
wc cien.txt
```

```text
 100  400 2000 cien.txt
```

**Prédis avant de lire l'explication :** `cien.txt` a cent lignes, et chacune dit quelque chose comme
`renglón 001 de 100`. Combien de mots penses-tu qu'il y a en tout ?

Les trois nombres, dans l'ordre, sont : lignes, mots et octets. Pour `cien.txt`, ils doivent être 100, 400 et 2000.
Chaque ligne a quatre mots, parce que pour `wc` un mot est n'importe quel groupe de caractères sans espaces, selon son
manuel : `001` et `100` comptent comme des mots. Et les octets sont la taille, le même nombre que tu as vu dans `ls -l`
à la leçon 01.

Dans cette leçon, un seul te suffit : les lignes. On les demande avec `-l`, de *lines* :

```bash
wc -l cien.txt
```

```text
100 cien.txt
```

**Comment il compte les lignes.** Le manuel de `wc` dit que `-l` compte les **sauts de ligne**. Si la dernière ligne d'un
fichier ne se termine pas par son saut, `wc -l` ne la compte pas. Dans cette salle, tous les fichiers se terminent par
leur saut, mais sur un serveur ce n'est pas toujours le cas : si `wc -l` te donne un de moins que ce que tu vois, c'est
la cause.

**Ta commande, pas celle du manuel.** Ici, `cat`, `head`, `tail` et `wc` viennent de `rust-coreutils`, comme `ls` à la
leçon 01 ; selon la documentation d'Ubuntu, seuls `cp`, `mv` et `rm` restent ceux de GNU. On a testé les formes que tu
vois dans cette leçon ; les manuels GNU cités en expliquent l'idée. Si tu essaies une autre option, regarde d'abord
`commande --help` dans **ce** conteneur.

### À toi : le journal et le rapport

Retourne dans la salle avec `cd ~/practica/03`. Ce sont quatre questions. **Avant chacune, écris sur ton papier de quel
morceau tu as besoin et avec quelle commande tu vas le sortir** ; ensuite tu la lances et tu notes.

1. **Le folio de la ligne 137** de `pagos/pagos.log`. Chaque paiement porte un folio, un F, un tiret et six chiffres.
   Note-le avec `anota 03 folio-137` et le folio, tel quel.
2. **L'heure de la dernière ligne** de `pagos/pagos.log` : seulement l'heure, `hh:mm:ss`, sans la date. Elle se note avec
   `anota 03 ultima-hora` (« dernière heure »). Lis la ligne complète : c'est ce que tu diras à la personne au téléphone.
3. **Combien de lignes a** `pagos/pagos.log`. Seulement le nombre : `anota 03 renglones` (« lignes »).
4. **Combien de succursales** contient `reportes/sucursales.csv`. C'est un CSV : un tableau écrit comme du texte, une
   ligne par rangée et des virgules entre les colonnes. Regarde d'abord ses trois premières lignes avec `head`.
   **Prédiction D :** le nombre de succursales est-il le même que le nombre de lignes ? Note ta réponse avec
   `anota 03 sucursales`.

**Ce que cela démontre** (objectif 3.2) : les marques `folio-137`, `ultima-hora`, `renglones` et `sucursales` de
`verifica 03`, et ta prédiction D, que tu compares toi-même à la fin.

---

## Étape 4 · Feuilleter et chercher : `less`

`head` et `tail` servent quand tu sais où se trouve ce que tu cherches. Quand tu ne le sais pas, tu feuillettes avec
`less`, un *paginateur* : selon son auteur, un programme qui montre des fichiers texte et **pas un éditeur**.
`less fichier` sert à regarder et ne modifie pas ce fichier. Reste sur les touches du tableau ci-dessous ; évite `v` et
`!`, qui peuvent ouvrir d'autres programmes.

Son manuel dit autre chose qui compte sur un serveur : `less` n'a pas besoin de lire le fichier entier avant de
commencer, si bien qu'avec de gros fichiers il démarre plus vite qu'un éditeur.

### Les touches, tu les connais déjà

Retourne dans l'essai et ouvre `cien.txt` avec `-N`, en majuscule : chaque ligne apparaît avec son numéro, comme avec
`cat -n`, mais en feuilletant.

```bash
cd ~/practica/03/ensayo
less -N cien.txt
```

```text
      1 renglón 001 de 100
      2 renglón 002 de 100
      3 renglón 003 de 100
      4 renglón 004 de 100
      5 renglón 005 de 100
      6 renglón 006 de 100
      7 renglón 007 de 100
      8 renglón 008 de 100
      9 renglón 009 de 100
     10 renglón 010 de 100
     11 renglón 011 de 100
     12 renglón 012 de 100
     13 renglón 013 de 100
     14 renglón 014 de 100
     15 renglón 015 de 100
     16 renglón 016 de 100
     17 renglón 017 de 100
     18 renglón 018 de 100
     19 renglón 019 de 100
     20 renglón 020 de 100
     21 renglón 021 de 100
     22 renglón 022 de 100
     23 renglón 023 de 100
cien.txt
```

L'écran se remplit avec le début du fichier, et le numéro de chaque ligne est à gauche : tu ne comptes plus. Tu es
**à l'intérieur** de `less`, comme tu as été à l'intérieur de `man` à la leçon 00, et les touches sont les mêmes que
celles que tu y as utilisées, plus quelques-unes :

| Touche | Ce qu'elle fait |
|---|---|
| Barre d'espace | Descend d'un écran |
| `g` | Au début du fichier |
| `G` | À la fin du fichier |
| `/` et un texte, puis Entrée | Cherche ce texte vers le bas |
| `n` | La fois suivante où il apparaît |
| `h` | L'aide de `less`, avec toutes les touches |
| `q` | Sort et te ramène à l'invite |

Essaie chacune : quelques écrans avec la barre, puis `G`, puis `g`. Sors avec `q`.

**Ctrl+C ne te fait pas sortir.** Dans `less`, Ctrl+C arrête ce qu'il était en train de faire, par exemple une longue
recherche, et le laisse en attente de ta touche suivante ; il ne le ferme pas, selon la FAQ de son auteur. Comme dans
`man` : on sort de `less` avec `q`.

### La recherche distingue majuscules et minuscules

À l'intérieur de `less -N cien.txt`, tape `/042` et Entrée. Il saute à la ligne 42. Tape `n` : il n'y en a pas d'autre.

**Prédis :** que se passe-t-il si tu cherches `/RENGLÓN`, en majuscules ?

En bas de l'écran apparaît `Pattern not found  (press RETURN)` : il ne le trouve pas, alors que le mot est dans les cent
lignes. Le manuel de `less` le dit : **par défaut, la recherche distingue majuscules et minuscules**. Dans un journal,
cela compte : `ERROR` et `error` ne sont pas forcément la même chose. Appuie sur Entrée et sors avec `q`.

### À toi : le premier `ERROR`

Ce dont tu as besoin se trouve dans le tableau ci-dessus et dans l'aide de `less`, avec `h`. Essaie d'abord avec cela ;
si tu bloques, explique ce que tu as cherché et quel écran tu as vu avant de demander un indice.

1. Retourne dans la salle, `cd ~/practica/03`, et ouvre `pagos/pagos.log` dans `less`, **avec les numéros de ligne**.
2. Avant de chercher, feuillette : quelques écrans avec la barre, la fin avec `G`, le début avec `g`. Regarde à quoi
   ressemble une ligne normale et à quoi ressemble une ligne qui signale quelque chose.
3. **Prédiction E :** le journal a des lignes `INFO`, `WARN` et `ERROR`. T'attends-tu à ce que le premier `ERROR` soit
   près du début ou près de la fin ? Écris-le.
4. Depuis le début du fichier, cherche la **première ligne qui dit `ERROR`, en majuscules**. Lis cette ligne en entier
   et celles qui suivent : elle dit pourquoi les refus ont commencé.
5. Sors avec `q` et note le **numéro** de cette ligne avec `anota 03 primer-error` (« première erreur »).

**Ce que cela démontre** (objectif 3.3) : la marque `primer-error` de `verifica 03`, et ta prédiction E.

---

## Étape 5 · Avant de l'ouvrir, demande ce que c'est : `file`

Il reste le journal d'hier, `pagos/pagos.log.1.gz`. Son nom se termine par `.log`, plus un `.1` et un `.gz`. Est-ce du
texte ? Pour ne pas deviner, il y a `file` : il regarde le **contenu** d'un fichier, pas son nom, et dit de quel type de
fichier il s'agit. Selon son manuel, il cherche près du début les marques fixes de chaque format connu, et si le fichier
n'en correspond à aucun, il vérifie si c'est du texte.

```bash
cd ~/practica/03/ensayo
file ../pagos/* ../reportes/*
```

```text
../pagos/arranque.log:      Unicode text, UTF-8 text
../pagos/pagos.conf:        Unicode text, UTF-8 text
../pagos/pagos.log:         Unicode text, UTF-8 text
../pagos/pagos.log.1.gz:    gzip compressed data, max compression, from Unix, original size modulo 2^32 52807
../reportes/sucursales.csv: CSV Unicode text, UTF-8 text
```

`../pagos/*` est un motif, comme ceux de la leçon 02 : le terminal le remplace par tous les noms de `pagos`. Tous disent
*text*, sauf un. **Le nom ne décide pas :** `file` reconnaît le journal d'hier par son contenu, et dit que c'est du gzip.

`file` est un premier signal, pas une garantie : son manuel dit que ce qu'il marque comme *text* se lit
**probablement** sans problème dans le terminal. S'il dit *text*, tu peux normalement le lire ; sinon, n'utilise pas
`cat` avant de savoir de quel format il s'agit.

### Pourquoi on ne fait pas `cat` sur un binaire

`cat` ne demande pas ce qu'est un fichier : il écrit ses octets à l'écran, tels quels. Avec un binaire, ce sont des
octets qui ne sont pas des lettres, et parmi eux il peut y en avoir que le terminal comprend comme des ordres. **Ne lance
pas `cat` directement sur ce `.gz`, ni sur aucun binaire dans un terminal de production.**

Si un jour le terminal se retrouve **déréglé** (ce que tu tapes sort en symboles étranges, ou ne se voit pas, ou Entrée
ne passe plus à la ligne), le fichier ne s'est pas abîmé et la machine non plus. **Écris `reset` et appuie sur Entrée**,
même si tu ne vois pas ce que tu écris. Selon son manuel, `reset` remet le terminal dans sa forme normale après qu'un
programme l'a laissé dans un état anormal, et il prévient que parfois Entrée ne répond pas : dans ce cas, utilise
**Ctrl+J**.

Maintenant, un échantillon sans danger. La barre `|` passe ce qu'écrit une commande à la suivante ; tu la verras en
détail à la leçon 06. `head -c 300` ne sort que les 300 premiers octets du fichier et la barre les passe à `cat -v`, qui
remplace chaque octet qui n'est pas une lettre par des signes qui, eux, peuvent s'écrire. **Prédis :** que vas-tu voir ?

```bash
head -c 300 ../pagos/pagos.log.1.gz | cat -v
```

Sortie raccourcie : le début d'une seule ligne très longue.

```text
^_M-^K^H^@^@^@^@^@^B^CM-^LM-^]]M-.^^7vEM-_3M-^J;^A^WM-HM-sM-Csh
```

Rien de tout cela n'est des lettres d'un journal : ce sont les octets compressés, traduits en signes. Lance `reset` une
fois, pour le connaître :

```bash
reset
```

Tu ne verras pas de texte en réponse : `reset` envoie des ordres de contrôle au terminal et peut effacer l'écran ; puis
l'invite revient.

### À toi : ce qu'est le journal d'hier

Avec `file`, demande ce qu'est `pagos/pagos.log.1.gz` et note **le premier mot** de ce qu'il répond, après les deux
points, avec `anota 03 tipo-respaldo` (« type de sauvegarde »).

**Ce que cela démontre** (objectif 3.4) : la marque `tipo-respaldo` de `verifica 03`, et tes réponses aux questions 8 et
9 de « Pour conclure ».

---

## Étape 6 · Contrôle avec `verifica 03`

Lance `verifica 03` quand tu veux, depuis n'importe quel dossier. Il vérifie d'abord que la salle est intacte, fichier
par fichier ; si quelque chose a changé, il s'arrête et te demande de la remonter. Ensuite, il contrôle tes sept
réponses **en lisant les fichiers** : la bonne réponse n'est pas écrite dans `verifica`, elle sort du fichier lui-même.

Juste après avoir monté la salle, la première marque sort en vert et les sept réponses en rouge, parce qu'elles sont
vides. Voici comment se lit un rouge quand la réponse est fausse et non vide ; par exemple, si tu notais pour
`ultima-hora` l'heure de la **première** ligne. Sortie raccourcie : ce rouge et la ligne finale.

```text
❌ ultima-hora · 00:00:04 es la hora del PRIMER renglón (medí: la hora del renglón 1). head muestra el principio; el final lo muestra otro comando

❌ Lección 03: falta lo marcado con ❌. Arréglalo y corre  verifica 03  otra vez.
```

*❌ ultima-hora · 00:00:04 est l'heure de la PREMIÈRE ligne (j'ai mesuré : l'heure de la ligne 1). head montre le début ;
la fin, c'est une autre commande.*

*❌ Leçon 03 : il manque ce qui est marqué d'un ❌. Corrige-le et lance de nouveau `verifica 03`.*

Il dit ce que tu as noté, ce qui a été mesuré et où tu t'es trompé, mais pas quelle est la bonne réponse.

Corrige et recommence jusqu'à voir ceci à la fin :

```text
✅ Lección 03: todo en verde.
```

*Leçon 03 : tout est au vert.*

---

## Tes exercices

L'exercice 3.3 se tente d'abord avec le tableau de `less` et son aide, avec `h` ; si tu bloques, explique ce que tu as
cherché et quel écran tu as vu avant de demander un indice.

| # | Tu fais | C'est fait quand |
|---|---|---|
| 3.1 | Tu lis la configuration en entier avec les numéros de ligne et tu trouves `espera_segundos` | `verifica 03` marque `espera` en vert |
| 3.2 | Tu sors la ligne 137, la dernière ligne et le total du journal, et tu comptes les succursales du CSV, avec tes prédictions écrites d'avance | `verifica 03` marque les quatre en vert, et tu as comparé ta prédiction D |
| 3.3 | Tu feuillettes le journal dans `less` et tu trouves le premier `ERROR` | `verifica 03` marque `primer-error` en vert, et tu as comparé ta prédiction E |
| 3.4 | Tu demandes avec `file` ce qu'est le journal d'hier, tu regardes un échantillon sans danger avec `head -c 300` et `cat -v`, et tu utilises `reset` | `verifica 03` s'affiche entièrement au vert, et tu as répondu aux questions 8 et 9 de « Pour conclure » |

**Durée (estimée) :** de 60 à 90 minutes. C'est une estimation : elle sera ajustée avec le premier groupe qui suivra le
cours.

---

## Ça ne marche pas ?

| Tu vois | Cause probable | Ce que tu fais |
|---|---|---|
| `Todavía no hay prepara para la lección «03».` *(Il n'y a pas encore de prepara pour la leçon « 03 ».)* | Ton image est d'avant cette leçon | Sors avec `exit` et lance `./practicar reconstruir` |
| `Ya existe /home/alumno/practica/03.` et on te demande d'écrire `si` *(« /home/alumno/practica/03 existe déjà. »)* | Tu avais déjà monté la salle, et la monter de nouveau la supprime en entier, sans retour en arrière ; ta feuille de réponses reste | Pour repartir de zéro, écris `si` (« oui ») et Entrée. Avec toute autre chose, il répond `No se borró nada.` (« Rien n'a été supprimé. ») |
| L'écran de `less` ne part pas et l'invite ne revient pas | Tu es toujours à l'intérieur de `less` | `q`. Ctrl+C ne le ferme pas |
| Ce que tu écris sort en symboles étranges, ou ne se voit pas | Tu as fait `cat` sur un fichier qui n'est pas du texte | Écris `reset` et Entrée, même si tu ne le vois pas. Si Entrée ne répond pas, utilise Ctrl+J |
| `❌ La sala no está intacta` *(La salle n'est pas intacte)* | Un fichier de la salle a changé | `prepara 03`. Ta feuille de réponses n'est pas touchée |

---

## Pour conclure

**Réponds sans relire.**

**Quatre situations : de quel morceau as-tu besoin, et avec quelle commande le sors-tu ?** Tu réussis avec 3 sur 4.

1. On te demande ce qui se passe en ce moment dans un service, d'après son journal.
2. On te donne un fichier de configuration de quinze lignes et on te demande de le revoir en entier.
3. On te demande combien d'enregistrements contient un CSV de quarante mille lignes, avec un en-tête.
4. On te dit que le problème a commencé « à un moment de la nuit » et le journal a dix mille lignes.

**Et cinq questions :**

5. Comment sors-tu la ligne 250 d'un fichier avec ce que tu as vu dans cette leçon ?
6. Tu as cherché `/error` dans `less` et il n'a rien trouvé, mais tu sais qu'il y a des erreurs. Que peut-il se passer ?
7. `wc -l` te dit 99, et tu comptes cent lignes à l'écran. Que peut-il se passer ?
8. Tu tombes sur un fichier appelé `respaldo.txt` (« sauvegarde.txt »). Que lances-tu avant de faire `cat` dessus, et
   pourquoi son nom ne suffit-il pas ?
9. Tu as fait `cat` sur un binaire et maintenant ce que tu écris ne se voit pas. Que fais-tu ?

**Ce qui vient ensuite :** tu lis déjà un fichier par morceaux, sans ouvrir d'éditeur. À la leçon 04, tu cherches des
**fichiers** dans un arbre avec `find`, quand tu ne sais même pas dans quel dossier ils se trouvent.

**Et plus tard :** regarder grandir un journal avec `tail -f` arrive à la leçon 10, et lire des fichiers
compressés, à la 06.

---

## Compare tes réponses

**Prédiction D (les succursales).** Ce n'est pas le même nombre. La première ligne du CSV est l'**en-tête** : elle dit
comment s'appelle chaque colonne (`sucursal,ciudad,terminales,hora_del_reporte`, soit « succursale, ville, terminaux,
heure du rapport ») et ce n'est pas une succursale. `wc -l` compte toutes les lignes, en-tête compris, donc les
succursales sont une de moins. C'est pourquoi il valait mieux regarder avec `head` avant de compter : le nombre de `wc`
ne signifie quelque chose que lorsque tu sais ce qu'il y a dans chaque ligne.

**Prédiction E (le premier `ERROR`).** Les deux se valent si tu l'as écrite d'avance : ce qui compte, c'est que tu l'as
vérifiée. Si tu as feuilleté avant de chercher, tu as vu que le journal signale `WARN` plusieurs fois avant le premier
`ERROR` : les problèmes commencent presque jamais par une erreur, ils commencent par des avertissements que personne
n'a lus. Et si en chemin tu es tombé sur le mot `error` en minuscules, c'est une autre ligne, avec une autre histoire :
c'est pourquoi l'exercice demandait `ERROR` tel quel.

**Exercice 3.3.** Le contrôle, c'est `verifica 03` ; ici, pas de nombre. Pour savoir si tu es sur la bonne voie : tu l'as
ouvert avec `less -N`, tu es allé au début avec `g`, tu as cherché avec `/` et le texte exact, et le nombre, tu l'as lu à
gauche de la ligne, sans compter.

**Pour conclure.**

1. La fin : `tail` sur le journal, ou `tail -n` avec le nombre de lignes que tu veux. Il te dit la dernière chose qui a
   été enregistrée : lis-la avec son heure, sers-t'en comme d'une piste, puis confirme l'état avec le signal qui
   correspond au service.
2. Tout : `cat`, de préférence avec `-n`. Quinze lignes tiennent à l'écran.
3. Combien il y en a : `wc -l`, moins un pour l'en-tête. Avant, `head` pour confirmer que la première ligne est
   l'en-tête.
4. Feuilleter et chercher : `less`, de préférence avec `-N`, en cherchant avec `/` ce qui distingue la ligne du
   problème, comme `ERROR`.
5. `head -n 250` et la dernière ligne qu'il écrit est la 250.
6. La recherche de `less` distingue majuscules et minuscules : les erreurs peuvent être écrites `ERROR` ou `Error`.
   Essaie avec une autre forme, ou consulte l'aide de `less` avec `h`.
7. Il manque son saut de ligne à la dernière ligne. `wc -l` compte des sauts, pas des lignes visibles.
8. `file respaldo.txt`. Le nom, c'est une personne qui le choisit, et il peut ne pas dire la vérité ; `file` regarde le
   contenu. S'il ne dit pas *text*, n'utilise pas `cat` avant de savoir de quel format il s'agit.
9. Tu écris `reset` et Entrée, même si tu ne le vois pas. Si Entrée ne répond pas, Ctrl+J à la place.

---

## Sources de la leçon

Consultées le 24 nov. 2026. Toutes sont en anglais.

- man7.org, *The Linux man-pages project*. *cat(1)*, de GNU coreutils (enchaîne des fichiers ; `-n` numérote les
  lignes). https://man7.org/linux/man-pages/man1/cat.1.html
- man7.org. *head(1)*, de GNU coreutils (les dix premières lignes par défaut ; `-n`).
  https://man7.org/linux/man-pages/man1/head.1.html
- man7.org. *tail(1)*, de GNU coreutils (les dix dernières lignes par défaut ; `-n`).
  https://man7.org/linux/man-pages/man1/tail.1.html
- man7.org. *wc(1)*, de GNU coreutils (lignes, mots et octets, dans cet ordre ; `-l` compte les sauts de ligne ; ce
  qu'est un mot). https://man7.org/linux/man-pages/man1/wc.1.html
- Ubuntu Manpages. *less(1)*, Ubuntu 26.04, less 668 (il ne lit pas le fichier entier avant de commencer ; barre, `g`,
  `G`, `/`, `n`, `h`, `q`, `-N` ; la recherche distingue majuscules et minuscules par défaut).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/less.1.html
- Mark Nudelman, auteur de `less`. *Less FAQ* (`less` est un paginateur et non un éditeur ; Ctrl+C ne le ferme pas).
  https://www.greenwoodsoftware.com/less/faq.html
- Ubuntu Manpages. *file(1)*, Ubuntu 26.04, file 5.46 (comment il décide du type d'après le contenu ; ce que veut dire
  *text* : probablement sans danger à lire dans le terminal).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/file.1.html
- man7.org. *tset(1)*, de ncurses (`reset` ramène le terminal à des valeurs normales après qu'un programme l'a laissé
  dans un état anormal ; il se peut qu'on ne voie pas ce qu'on écrit et qu'il faille Ctrl+J au lieu d'Entrée).
  https://man7.org/linux/man-pages/man1/tset.1.html
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, section « rust-coreutils » (les utilitaires de base viennent de
  `rust-coreutils` ; `cp`, `mv` et `rm` restent ceux de GNU).
  https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- Adam Wiggins. *The Twelve-Factor App*, « XI. Logs » (à l'état brut, un journal est souvent du texte, dans l'ordre du
  temps, avec un événement par ligne ; une trace d'erreur peut en occuper plusieurs).
  https://12factor.net/logs
