# Leçon 00 — Ton terminal d'entraînement

**Par Dorian Chávez, fondateur de Hábil et architecte d'intégration.**

**Avant de lire, six mots.** Une ligne chacun ; les autres arrivent dans la leçon où ils servent.

- **Terminal :** la fenêtre où tu écris des instructions au clavier et où l'ordinateur te répond par du texte.
- **Commande :** une instruction écrite. Par exemple, `whoami` (« qui suis-je ? »). Tu l'écris et tu appuies sur Entrée.
- **Invite :** le texte que le terminal place au début de la ligne quand il attend ta prochaine commande. En
  anglais, *prompt*.
- **Docker :** le programme qui crée et fait tourner des conteneurs.
- **Conteneur :** un petit ordinateur Linux qui vit à l'intérieur du tien, séparé de tes fichiers.
- **Image :** le moule dont sort un conteneur. Elle se construit une fois et s'utilise de nombreuses fois.

**Ce que tu en retires :** ton terminal d'entraînement qui fonctionne, c'est-à-dire un Ubuntu 26.04 dans un
conteneur ; tu sais entrer, sortir et revenir sans perdre ton travail ; tu lis l'invite ; tu demandes de
l'aide à la machine elle-même ; et tu sors de n'importe quel programme qui te piège.

**Idée nouvelle de cette leçon (la seule qui est évaluée) :** dans ce terminal, il n'y a ni bureau ni fenêtres,
seulement du texte : il te dit qui tu es et où tu es sur chaque ligne, et tu lui parles avec des commandes.

**Dans quel ordre :** d'abord tu lances le terminal (étapes 1 et 2), ensuite tu lis ce qu'il te dit (étape 3),
puis tu apprends à lui demander de l'aide (étape 4) et, pour finir, à sortir de l'endroit où tu es coincé
(étape 5).

**Une note sur la langue du script.** Le script du cours (`./practicar`) et son vérificateur (`verifica`) parlent
espagnol, et les messages qu'ils affichent aussi. Dans cette leçon, ces messages apparaissent exactement comme le
terminal les affiche, et sous chaque bloc il y a une courte glose en français, en italique. Des noms comme `practicar`,
`verifica`, `alumno` (« étudiant »), `practica` (« entraînement ») et `aqui-estuve.txt` (« j'étais ici ») font partie du
code et ne sont pas traduits.

---

## La scène

*Scénario inventé ; les écrans qui apparaissent dans la leçon sont réels.*

Mardi, 23 h 40. Lucía est dans son premier emploi depuis trois semaines et ce soir, elle est d'astreinte. Le téléphone sonne : le
service de paiements a cessé de répondre. On lui donne une adresse et une phrase : « connecte-toi au serveur et dis-moi
ce que tu vois ».

Elle entre. Il n'y a pas de bureau, ni d'icônes, ni de souris. Il y a un écran noir et une seule ligne :

```text
operaciones@pagos-03:~$
```

*(En espagnol, « operaciones » veut dire « opérations » et « pagos », « paiements ».)*

Le curseur clignote. À l'autre bout du téléphone, on attend.

Lucía ne le sait pas encore, mais cette ligne a déjà répondu à deux des trois questions qu'on va lui poser ce soir :
qui elle est là-dedans et dans quel dossier elle se trouve. La troisième, quoi faire, se demande à la machine
elle-même. Et si quelque chose reste bloqué, il y a trois touches pour sortir.

Cette leçon te donne ces quatre choses sur une machine où l'on a le droit de se tromper : la tienne, d'entraînement.

---

## Avant de commencer

**Ce dont tu as besoin :**

| | Quoi | Remarques |
|---|---|---|
| Système | Linux, Mac ou Windows | Plus bas, il est dit sur lesquels cela a déjà été testé et sur lesquels il reste à le faire |
| Docker | Installé et qui répond | Plus bas, il est dit comment, selon le système |
| Disque | Moins de 300 Mo pour l'image du cours | C'est un plafond et non un chiffre, parce que chaque outil Docker mesure différemment (plus bas) |
| Réseau | Pour télécharger Ubuntu et quelques paquets la première fois | Seul ton ordinateur l'utilise, pendant la construction ; à l'intérieur du terminal d'entraînement, il n'y a pas de réseau |
| Savoir | Ouvrir le terminal de ton ordinateur et coller une ligne | Rien n'est supposé de Linux ni de Docker |

**Où cela a été testé, et où cela manque :**

| Système | État | Ce qui a été lancé |
|---|---|---|
| Linux Mint 22.3 (x86_64), Docker Engine 29.10.0 | Testé le 19 nov. 2026 | `./practicar` a construit l'image en environ 90 secondes, `verifica 00` a donné sa cinquième marque en rouge avant la sortie et les cinq en vert au retour ; à l'intérieur, il n'existe que le réseau interne `lo` ; `./practicar desde-cero` a refusé avec la session ouverte et, une fois celle-ci fermée, a supprimé le dossier. Attention : Docker ne prend pas officiellement en charge Linux Mint ; son guide dit que cela « peut fonctionner » |
| Mac avec Apple silicon, Docker 29.2.1 tournant avec Colima | Testé les 18 et 19 nov. 2026 | Tout ce qui figure dans cette leçon : entrer, sortir, revenir, `verifica 00`, `desde-cero` et les écrans que tu vois ici |
| Ubuntu | Pas encore testé | Docker le prend en charge officiellement ; l'installation est celle de son guide |
| Mac avec Docker Desktop | Pas encore testé | L'installation est celle du guide officiel de Docker Desktop |
| Windows avec Docker Desktop et WSL 2 | Pas encore testé | L'installation est celle des guides officiels de Microsoft et de Docker |

Si tu le testes sur l'un de ceux-ci, dis-nous comment cela s'est passé.

**Les chiffres de cette leçon, et comment ils ont été mesurés :**

| Chiffre | Comment il a été mesuré |
|---|---|
| Construire l'image : 42 secondes | Mac avec Apple silicon, 18 nov. 2026, avec `time ./practicar reconstruir`, qui ce jour-là construisait sans cache (aujourd'hui, c'est `./practicar reconstruir-limpio` qui le fait), et avec l'image d'Ubuntu déjà téléchargée |
| Construire l'image : 1 minute 38 secondes | Le même Mac, 18 nov. 2026, avec `docker build --no-cache`, sur une version antérieure du Dockerfile |
| Construire l'image : environ 90 secondes | Linux Mint 22.3 (x86_64), 19 nov. 2026, avec `./practicar` |
| Taille : 221 Mo | Linux Mint, 18 nov. 2026, avec `docker image inspect` |
| Taille : 277 Mo | Mac, 18 nov. 2026, avec `docker image ls` |

**Installe Docker, selon ton système.** C'est la seule chose qui s'installe sur ton ordinateur.

- **Linux Mint ou Ubuntu :** Docker Engine, depuis le dépôt officiel de Docker. La
  [leçon 1 du cours Docker](https://www.habil.mx/fr/cours/docker/01-instalacion/) de la maison le fait pas à pas sous
  Linux Mint et explique chaque erreur. Docker ne prend pas officiellement en charge Mint, et cette leçon dit quoi faire
  à ce sujet. Sur l'ordinateur du test, l'utilisateur était dans le groupe `docker` et utilisait Docker sans `sudo` ;
  cette même leçon explique ce que cela implique d'entrer dans ce groupe.
- **Mac :** Docker Desktop, avec l'installateur de ton processeur (Apple silicon ou Intel), depuis le
  [guide officiel](https://docs.docker.com/desktop/setup/install/mac-install/). Ouvre-le et attends qu'il indique
  qu'il tourne. Sur le Mac du test, Docker tournait avec un autre outil, Colima, et non avec Docker Desktop : c'est
  pourquoi cette voie figure dans le tableau comme « pas encore testé ».
- **Windows :** la voie documentée est Docker Desktop avec WSL 2. WSL est la pièce de Windows qui fait tourner un Linux
  à l'intérieur. Selon Microsoft, sous Windows 10 (version 2004 ou ultérieure) et Windows 11, il s'installe avec
  `wsl --install` dans un PowerShell ouvert en tant qu'administrateur, et il apporte Ubuntu. Selon Docker, il faut installer
  ensuite Docker Desktop et activer l'intégration dans *Settings › Resources › WSL Integration*. Tout ce qui suit,
  tu le lances dans le terminal de cet Ubuntu, pas dans PowerShell. Cela figure aussi dans le tableau comme « pas
  encore testé ».

**Vérifie que Docker répond.** Ouvre le terminal de ton ordinateur et lance :

```bash
docker version
```

Tout va bien si tu vois deux blocs, `Client` et `Server`, chacun avec sa version. Lors des tests, c'étaient Docker
29.10.0 sous Linux et 29.2.1 sur le Mac. Si `Client` apparaît et que, à la place de `Server`, il y a une erreur, le
programme `docker` n'a pas pu parler à son moteur. Lance `docker info` et suis l'erreur exacte qu'il te donne ; les
causes les plus courantes sont dans « Ça ne marche pas ? ».

**Récupère le matériel du cours.** Il se trouve dans un **dépôt** : un dossier avec tous les fichiers du cours,
publié sur GitHub. Le programme `git` le télécharge avec la première ligne ; la seconde te fait entrer dans ce
dossier :

```bash
git clone https://github.com/HabilMX/curso-terminal.git
cd curso-terminal
```

**Sans `git` :** ouvre cette adresse dans ton navigateur, choisis *Code › Download ZIP*, décompresse le fichier
et ouvre ton terminal dans le dossier qui a été créé. Ne copie pas le `cd curso-terminal` ci-dessus : ce dossier
peut porter un autre nom.

---

## Étape 1 · Lance ton terminal et entre

**Prédis d'abord.** Écris sur une feuille : quand tu entreras, que crois-tu voir ? Une nouvelle fenêtre ? Un
bureau ? Autre chose ?

Depuis le dossier du cours :

```bash
./practicar
```

`practicar` est un script du cours. La première fois, il construit l'image : il télécharge Ubuntu 26.04 et y met ce que
le cours utilise. Lors des tests, cela a pris entre 42 secondes et 1 minute 38 secondes. Les fois suivantes, il ne
télécharge ni ne reconstruit plus : sur le Mac du test, il est entré en moins de deux secondes. Ton temps dépend de
Docker et de ta machine.

Tu verras défiler beaucoup de texte et, à la fin, ceci :

```text
✅ Imagen lista.
Entrando a tu terminal de práctica. Para salir, escribe  exit  (o Ctrl+D).
alumno@practica:~$
```

*Image prête. Entrée dans ton terminal d'entraînement. Pour sortir, tape `exit` (ou Ctrl+D).*

Aucune fenêtre ne s'est ouverte. **Tu es dans la même fenêtre, mais tu es maintenant sur une autre machine.** La
dernière ligne est l'invite de ton terminal d'entraînement, et tu la liras en détail à l'étape 3. Compare avec ce
que tu avais prédit.

**À quel point il est séparé de ton ordinateur.** Pour ces exercices, aucun dossier de ton ordinateur n'est
connecté, le terminal tourne en tant que `alumno`, sans privilèges d'administrateur, et à l'intérieur il n'y a pas de
réseau. Ton travail est enregistré uniquement dans le dossier `~/practica`. Ce que le conteneur utilise bel et bien
de ton ordinateur, c'est le processeur, la mémoire et le disque.

Lance ta première commande à l'intérieur :

```bash
verifica 00
```

`verifica` examine l'état de ton terminal d'entraînement et te dit ce qui manque. Chaque marque dit ce qu'il a
mesuré. Cela doit maintenant s'afficher ainsi :

```text
✅ Es Ubuntu 26.04 (medí: ID y VERSION_ID en /etc/os-release)
✅ Eres el usuario alumno (medí: id -un)
✅ /home/alumno/practica existe y es una carpeta (medí: que exista como carpeta)
✅ Puedes escribir en /home/alumno/practica (medí: creé un archivo vacío ahí y lo borré)
❌ Falta /home/alumno/practica/00/aqui-estuve.txt (medí: que el archivo exista). Créalo con el paso 2 de la lección, corre  verifica 00, sal con exit y vuelve a entrar

❌ Lección 00: falta lo marcado con ❌. Arréglalo y corre  verifica 00  otra vez.
```

*C'est Ubuntu 26.04 (j'ai mesuré : ID et VERSION_ID dans `/etc/os-release`). Tu es l'utilisateur alumno (j'ai
mesuré : `id -un`). `/home/alumno/practica` existe et c'est un dossier (j'ai mesuré : qu'il existe en tant que dossier).
Tu peux écrire dans `/home/alumno/practica` (j'ai mesuré : j'y ai créé un fichier vide et je l'ai supprimé). Il
manque `/home/alumno/practica/00/aqui-estuve.txt` (j'ai mesuré : que le fichier existe). Crée-le avec l'étape 2 de la
leçon, lance `verifica 00`, sors avec `exit` et rentre. Leçon 00 : il manque ce qui est marqué d'un ❌. Corrige et
relance `verifica 00`.*

Quatre verts et un rouge. Le rouge est normal : c'est ce que tu fais à l'étape 2.

**Ce que cela démontre** (objectif 0.1, première moitié) : les quatre marques vertes.

---

## Étape 2 · Laisse une marque, sors et reviens

Un conteneur est jeté quand tu en sors. Alors, où reste ton travail ? Dans un seul dossier : `practica`. Docker le
garde à part, dans ce qu'il appelle un **volume**, et te le reconnecte chaque fois que tu entres. Tout le reste
renaît propre.

**Prédis d'abord.** Tu vas créer un fichier dans `practica`, sortir et rentrer. Sera-t-il encore là ? Et si tu
l'avais créé en dehors de `practica` ?

Aujourd'hui, ces deux lignes sont une recette à recopier. Écris-les telles quelles :

```bash
mkdir ~/practica/00
date > ~/practica/00/aqui-estuve.txt
```

- `mkdir` crée un dossier : ici, le dossier `00` dans `practica`.
- `date` répond avec la date et l'heure. Le signe `>` envoie cette réponse dans un fichier au lieu de l'écran.
- `~` est l'abréviation de ton dossier personnel. Tu le verras en détail à l'étape 3.

Dans les leçons 02 et 06, tu comprendras chaque pièce en détail. Aucune des deux lignes ne répond quoi que ce soit :
dans le terminal, **le silence signifie le plus souvent que cela s'est bien passé**.

Lance `verifica 00` de nouveau, **avant de sortir**. La dernière ligne a changé :

```text
❌ Tu marca aqui-estuve.txt ya existe, y es la primera vez que la veo: fue en ESTA sesión (medí: no había apunte de una sesión anterior; lo acabo de dejar en 00/.sesion-verifica). Te falta salir con exit, volver a entrar con ./practicar y correr  verifica 00  otra vez
```

*Ta marque aqui-estuve.txt existe déjà, et c'est la première fois que je la vois : c'était dans CETTE session (j'ai
mesuré : il n'y avait pas de note d'une session précédente ; je viens de la laisser dans `00/.sesion-verifica`). Il te
reste à sortir avec `exit`, à rentrer avec `./practicar` et à relancer `verifica 00`.*

`verifica` a déjà vu ta marque et a noté dans quelle session il l'a vue. Une **session**, c'est chaque fois que tu
entres. La note reste dans ton dossier d'entraînement, dans un fichier qui s'appelle `.sesion-verifica`. Maintenant,
fais ce qu'il dit. Sors :

```bash
exit
```

```text
logout
Saliste de la terminal de práctica.
Lo que dejaste en ~/practica sigue guardado. Para volver a entrar:  ./practicar
```

*Tu es sorti du terminal d'entraînement. Ce que tu as laissé dans `~/practica` reste enregistré. Pour revenir :
`./practicar`.*

Remarque l'invite : elle a changé. Elle ne dit plus `alumno@practica`. Tu es de retour sur ton ordinateur.
Rentre avec `./practicar` et lance `verifica 00` :

```text
✅ Saliste y volviste sin perder tu carpeta (medí: tu marca ya existía en una sesión anterior a esta, según el apunte de 00/.sesion-verifica)

✅ Lección 00: todo en verde.
```

*Tu es sorti et revenu sans perdre ton dossier (j'ai mesuré : ta marque existait déjà dans une session
antérieure à celle-ci, d'après la note de `00/.sesion-verifica`). Leçon 00 : tout est au vert.*

`verifica` n'utilise pas l'horloge. Il compare deux choses : la session dans laquelle il a vu ta marque pour la
première fois et la session actuelle. Elles sont différentes et la marque est toujours là : tu es sorti, tu es
revenu et ton dossier a survécu.

**Repartir de zéro.** Si un jour tu veux que ton dossier d'entraînement soit de nouveau vide, depuis ton
ordinateur (pas depuis l'intérieur) :

```bash
./practicar desde-cero
```

Il demande que tu écrives `si` (« oui ») avant de supprimer, parce que cela ne peut pas être annulé. Il supprime
seulement ton dossier d'entraînement. Si tu as le terminal d'entraînement ouvert dans une autre fenêtre, il ne
supprime rien et te demande de sortir d'abord. C'est une commande qui supprime : écris-la toi-même, à la main. Si
tu la lances maintenant, tu devras refaire cette étape.

**Ce que cela démontre** (objectif 0.1 complet) : `verifica 00` entièrement au vert.

---

## Étape 3 · Lis l'invite

Voici la ligne que tu as devant toi :

```text
alumno@practica:~$
```

Elle comporte quatre pièces :

| Pièce | Ce qu'elle dit |
|---|---|
| `alumno` | Qui tu es : ton utilisateur sur cette machine |
| `practica` | Sur quelle machine tu es. Elle vient après le `@`, comme dans un e-mail : « alumno, sur practica » |
| `~` | Dans quel dossier tu es. `~` est l'abréviation de « ton dossier personnel » |
| `$` | Fin de l'invite : à partir d'ici, c'est toi qui écris. `$` veut dire utilisateur normal |

**Prédis d'abord.** Il y a une commande pour demander chaque chose : `whoami` (qui suis-je ?), `hostname` (comment
s'appelle cette machine ?) et `pwd` (dans quel dossier suis-je ?). Avant de les lancer, écris sur ta feuille ce que
chacune va répondre. Pour deux d'entre elles, il suffit de lire l'invite. La troisième a un piège.

Maintenant lance-les, une par une :

```bash
whoami
hostname
pwd
```

Compare avec ta feuille. Les réponses sont à la fin, dans « Compare tes réponses » ; ne les regarde pas avant de
prédire.

**Pourquoi c'est important.** Dans un vrai travail, tu auras trois terminaux ouverts qui se ressemblent : ton
ordinateur, un serveur de test et un serveur de production. L'invite est la seule chose qui te dit dans lequel tu
es sur le point d'écrire. **Lis-la avant chaque commande qui change quelque chose.**

**Ce que cela démontre** (objectif 0.2) : ta prédiction écrite, comparée à la sortie.

---

## Étape 4 · Demande de l'aide à la machine

Personne ne connaît les commandes par cœur. Ce qui compte, c'est de savoir où demander, et la réponse se trouve dans la
machine elle-même. Il y a deux façons.

**La courte : `--help`.** Presque toutes les commandes acceptent `--help` et répondent par un résumé :

```bash
whoami --help
```

```text
Print the current username.

Usage: whoami

Options:
  -h, --help     Print help
  -V, --version  Print version
```

Dans ce terminal, l'aide s'affiche en anglais. Elle a trois parties : ce que fait la commande, comment elle s'utilise
(*Usage*) et quelles options elle accepte (*Options*). Une **option** est un mot avec des tirets qui change ce que fait
la commande.

**La longue : `man`.** C'est le manuel (`man`, pour *manual*). Chaque commande a sa page :

```bash
man whoami
```

Maintenant l'écran est différent. Tu ne vois plus l'invite, et en bas il est écrit :

```text
 Manual page whoami(1) line 1 (press h for help or q to quit)
```

Tu es **dans** le manuel. Ce que tu tapes n'est plus des commandes : ce sont des touches du manuel. Ces cinq
suffisent :

| Touche | Ce qu'elle fait |
|---|---|
| Flèche vers le bas, ou barre d'espace | Descend : la flèche, d'une ligne ; la barre, d'un écran |
| Flèche vers le haut | Remonte d'une ligne |
| `/` et un mot, puis Entrée | Cherche ce mot vers le bas |
| `n` | Va à l'occurrence suivante |
| `q` | Quitte le manuel et te ramène à l'invite |

Sors avec `q`. L'invite est revenue.

**Avec Claude Code, dans cet ordre.** Dans ce cours, un assistant d'IA intervient toujours de la même façon : **tu
prédis → tu lances → tu lis ce qui est sorti → tu expliques avec tes mots.** Ici, tu peux demander à
Claude Code de t'expliquer ce que dit une page de `man` que tu n'as pas comprise. Mais la page, c'est toi qui
l'ouvre, et la réponse, c'est toi qui la vérifie, dans ton terminal. L'exercice 0.3 se fait sans IA : l'important
est que tu saches interroger la machine quand il n'y a personne d'autre.

**Ce que cela démontre** (objectif 0.3) : exercice 0.3, plus bas.

---

## Étape 5 · Sors de ce qui t'a piégé

Il y a un instant, tu étais « piégé » dans `man` et tu en es sorti avec `q`. Cela arrive à tout le monde : un
programme prend l'écran et l'invite ne revient pas. Il y a trois touches, et chacune sert à quelque chose de différent.

**Une erreur exprès, d'abord.** Écris la commande avec une majuscule :

```bash
Whoami
```

```text
bash: Whoami: command not found
```

Le terminal distingue majuscules et minuscules : `Whoami` n'existe pas. Et remarque ce qui s'est passé ensuite : il
t'a rendu l'invite. **Une erreur qui te rend l'invite ne t'a pas piégé.** Tu lis le message et tu
continues. Être piégé, c'est quand l'invite ne revient pas.

Maintenant, trois façons de rester piégé. Fais les trois.

**1. Un programme qui ne se termine pas.** `sleep 1000` attend mille secondes sans rien faire :

```bash
sleep 1000
```

L'invite ne revient pas. Appuie sur **Ctrl+C** (maintiens Ctrl et appuie sur C). `^C` apparaît et l'invite revient. Ctrl+C
dit au programme : « arrête-toi tout de suite ».

**2. Un programme qui attend que tu écrives.** `cat`, sans rien d'autre, répète ce que tu écris :

```bash
cat
```

Écris `hola` (« bonjour ») et Entrée : il répond `hola`. Il va continuer d'attendre indéfiniment. Appuie sur
**Ctrl+D** sur une ligne vide. L'invite revient. Ctrl+D dit au programme : « il n'y a plus rien à écrire ».

**3. Un programme plein écran.** `htop` montre les programmes qui tournent :

```bash
htop
```

Il prend tout l'écran. Sors avec **`q`**, comme dans `man`.

**Laquelle sert à quoi :**

| Touche | Dit au programme | Utilise-la quand |
|---|---|---|
| `q` | « Ferme-toi » | Tu es dans un programme qui se feuillette ou qui prend l'écran : `man`, `less`, `htop` |
| Ctrl+C | « Arrête-toi tout de suite » | Une commande tourne et ne se termine pas |
| Ctrl+D | « Il n'y a plus rien à écrire » | Un programme attend du texte de ta part |

Trois choses mesurées qu'il est bon de savoir :

- **Ctrl+C ne fait pas sortir de `man`.** La page reste. Pour en sortir, utilise `q`.
- **`q` sur l'invite ne fait rien d'utile.** Il répond `bash: q: command not found`. Cela sert à savoir que tu étais
  déjà dehors.
- **Ctrl+D sur l'invite vide te fait sortir du terminal d'entraînement.** L'invite est aussi un programme qui attend
  que tu écrives : il s'appelle bash. Ctrl+D à cet endroit équivaut à `exit`. Rien ne se perd : tu reviens avec
  `./practicar`.

**Ce que cela démontre** (objectif 0.4) : les cinq situations de la révision, dans « Pour conclure ».

---

## Trois notes sur ton terminal d'entraînement

**Ton Mac utilise zsh ; à l'intérieur, tu utilises bash.** Le programme qui lit tes commandes s'appelle *shell*.
Depuis macOS 10.15, celui des nouveaux comptes Mac est zsh, selon Apple. À l'intérieur du conteneur, c'est bash, répandu
sur de nombreux serveurs. C'est pourquoi tout le cours se fait à l'intérieur : tout le groupe s'entraîne
dans le même environnement. Le terminal de ton ordinateur ne sert qu'à lancer `./practicar`.

**`htop` et `ps` ne voient que le conteneur.** Quand tu as ouvert `htop`, tu n'as vu que deux programmes : bash
et htop lui-même. Tu ne vois pas ceux de ton ordinateur, parce que Docker donne à chaque conteneur sa propre liste
de processus. Pour apprendre à les lire et à les arrêter, ce qui est la leçon 10, cela suffit.

**Tes commandes de base sont écrites en Rust.** Depuis Ubuntu 25.10, des commandes comme `ls`, `cat` et `whoami` ne
viennent plus du projet GNU, qui est le classique, mais de `rust-coreutils`. C'est ce que dit la documentation d'Ubuntu,
et dans ton terminal cela se voit avec `man whoami` : sous *VERSION*, il est écrit `uutils coreutils`. Trois restent
de GNU : `cp`, `mv` et `rm`.

- **Ce que cela signifie pour toi :** si tu compares avec un tutoriel ou avec un serveur plus ancien, le texte de
  `man` et de `--help` peut être différent de celui que tu vois ici. Ce n'est pas une erreur de ta part ni du
  tutoriel.
- **Ce qui ne change pas dans ce cours :** les commandes s'appellent pareil et les options qui sont enseignées
  fonctionnent pareil. Les sorties que tu vois dans les leçons viennent de l'image testée les 18 et 19 nov. 2026.
  L'image se construit toujours sur l'Ubuntu 26.04 en vigueur, donc après une mise à jour une sortie peut varier un peu.

---

## Tes exercices

L'exercice 0.3 se fait **sans IA** et sans moteur de recherche : uniquement avec `man`.

| # | Tu fais | C'est fait quand |
|---|---|---|
| 0.1 | Les étapes 1 et 2 : tu entres, tu laisses ta marque, tu sors et tu reviens | `verifica 00` s'affiche entièrement au vert |
| 0.2 | L'étape 3 : tu prédis par écrit ce que répondent `whoami`, `hostname` et `pwd`, puis tu les lances | Tu as ta prédiction et la sortie, et tu sais expliquer celle qui ne coïncidait pas |
| 0.3 | Tu réponds aux trois questions ci-dessous, uniquement avec `man` | Tes trois réponses coïncident avec « Compare tes réponses » |
| 0.4 | L'étape 5 : tu te piéges exprès dans `sleep 1000`, `cat` et `htop`, et tu sors des trois | Tu es sorti des trois sans fermer la fenêtre |

**Les trois questions du 0.3.** Pour chacune : ouvre la page, cherche avec `/`, note la réponse et sors avec `q`.

1. Dans `man whoami` : combien d'options `whoami` accepte-t-il, et lesquelles ?
2. Dans `man ls`, cherche `--all` : quelle lettre courte est équivalente à `--all`, et que fait-elle ?
3. Dans `man man`, cherche `Executable` : le manuel est divisé en sections numérotées. Quel numéro a celle des
   programmes et des commandes ? Et celle des commandes d'administration du système ?

**Durée (estimée) :** de 60 à 90 minutes, sans compter l'installation de Docker. C'est une estimation : elle sera
ajustée avec le premier groupe qui suivra le cours.

---

## Ça ne marche pas ?

| Tu vois | Cause probable | Ce que tu fais |
|---|---|---|
| `❌ No encuentro el comando «docker» en esta computadora.` *(Je ne trouve pas la commande « docker » sur cet ordinateur.)* | Docker n'est pas installé | « Avant de commencer », selon ton système |
| `❌ Docker está instalado, pero no pude hablar con él.` *(Docker est installé, mais je n'ai pas pu lui parler.)* | Le plus courant : Docker est éteint, ou ton utilisateur n'a pas la permission. Cela peut être autre chose | Lance `docker info` et suis l'erreur exacte qu'il te donne |
| `no such file or directory: ./practicar` | Tu n'es pas dans le dossier du cours | `cd curso-terminal` et recommence |
| La première construction prend plusieurs minutes et des lignes qui commencent par `Ign:` s'affichent | Les serveurs de paquets d'Ubuntu répondent lentement. Cela est arrivé le 19 nov. 2026 : la même construction a pris 8 minutes, avec 3 lignes `Ign:`, contre une minute et demie le 18 nov. | Laisse-la tourner. `Ign:` n'est pas une erreur : apt réessaie. Si, à la fin, `❌ No se pudo construir la imagen.` apparaît, suis la ligne du dessous |
| `❌ No se pudo construir la imagen.` *(L'image n'a pas pu être construite.)* | Le réseau, l'espace disque, les permissions de Docker ou le matériel du cours | Lis la **première** erreur qui est sortie au-dessus du message, corrige-la et relance `./practicar` |
| `❌ ./practicar abre una terminal interactiva…` *(./practicar ouvre un terminal interactif…)* | Tu l'as lancé avec un tube ou une redirection | Lance-le seul : `./practicar` |
| `command not found: verifica` | Tu as lancé `verifica` sur ton ordinateur, pas à l'intérieur | Entre avec `./practicar` ; l'invite doit dire `alumno@practica` |
| `mkdir: /home/alumno/practica/00: File exists` | Tu avais déjà créé le dossier `00` | Ce n'est pas grave : continue avec la ligne de `date` |
| La dernière ligne de `verifica 00` est toujours en rouge après le retour | Tu n'as pas lancé `verifica 00` avant de sortir, donc il venait à peine de voir ta marque | Elle est déjà notée : sors avec `exit`, entre et lance `verifica 00` de nouveau |
| `❌ No sé en qué sesión estás…` *(Je ne sais pas dans quelle session tu es…)* | Tu es entré dans le conteneur sans le script du cours | Sors et entre avec `./practicar` |
| L'invite ne revient pas | Un programme a pris le terminal | Essaie `q` ; sinon Ctrl+C ; sinon Ctrl+D (étape 5) |
| Tu as fermé la fenêtre sans écrire `exit` | Le terminal d'entraînement a pu rester ouvert à l'intérieur | Ouvre une autre fenêtre et entre avec `./practicar` : ton dossier est toujours là |
| `❌ No borré nada: tu carpeta de práctica está en uso.` *(Je n'ai rien supprimé : ton dossier d'entraînement est en cours d'utilisation.)* | Un terminal d'entraînement est ouvert, peut-être dans une fenêtre que tu as fermée sans `exit` | Sors-en avec `exit`. Si tu n'as plus cette fenêtre, le même message te donne la ligne `docker stop …` pour le fermer toi-même |
| Le cours a été mis à jour et `verifica` ne connaît pas une leçon | Ton image est d'avant | `./practicar reconstruir` |

---

## Pour conclure

**Réponds sans relire.**

**Cinq situations : quelle touche ?** Tu réussis avec 4 sur 5.

1. Tu as ouvert `man ls` pour chercher une option et tu l'as déjà trouvée.
2. Tu as lancé une commande qui tourne depuis dix minutes et ne se termine pas.
3. Tu as écrit `cat` et Entrée par erreur, et maintenant tout ce que tu écris se répète.
4. Tu as ouvert `htop` pour voir ce qui tourne et tu en as assez vu.
5. Tu as fini pour aujourd'hui : tu es dans `alumno@practica:~$` et tu veux revenir à ton ordinateur.

**Et trois questions :**

6. Tu vois `soporte@caja-07:~$`. Sans rien lancer : qui es-tu, sur quelle machine et dans quel dossier ?
7. Tu as créé un fichier en dehors de `~/practica` et tu es sorti. Sera-t-il là quand tu reviendras ?
   Pourquoi ?
8. Tu ne te rappelles plus ce que fait une option d'une commande et tu n'as pas internet. Quelles deux choses
   essaies-tu ?

**Ce qui vient ensuite :** tu as maintenant un endroit où t'entraîner et tu sais où tu te trouves. Dans la leçon 01,
tu te déplaces dans les dossiers sans les voir.

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

**Étape 3 (exercice 0.2).** `whoami` répond `alumno`. `hostname` répond `practica`. `pwd` répond `/home/alumno`. Voilà le
piège : l'invite dit `~`, et `~` est l'abréviation de ton dossier personnel, qui sur cette machine est `/home/alumno`.
`~` et `/home/alumno` désignent le même endroit : l'invite utilise la forme courte.

**Exercice 0.3.**

1. Deux : `-h` ou `--help`, qui affiche l'aide, et `-V` ou `--version`, qui affiche la version.
2. `-a`. Il fait que `ls` ne cache pas les fichiers cachés, qui sont ceux dont le nom commence par un point.
3. La section 1 est celle des programmes et des commandes (*Executable programs or shell commands*). La 8 est celle des
   commandes d'administration du système (*System administration commands*). C'est pourquoi la page dit `whoami(1)` : le
   numéro entre parenthèses est la section.

**Pour conclure.**

1. `q`.
2. Ctrl+C.
3. Ctrl+D, sur une ligne vide. Ctrl+C l'arrête aussi.
4. `q`.
5. `exit`, ou Ctrl+D sur l'invite vide.
6. Tu es `soporte`, sur la machine `caja-07`, dans ton dossier personnel.
7. Non. En sortant, le conteneur est jeté et renaît propre. Seul `~/practica` est gardé à part, dans un volume.
8. La commande avec `--help`, et `man` suivi de la commande.

---

## Sources de la leçon

Consultées le 18 nov. 2026.

- Ubuntu. *Ubuntu 26.04 LTS release notes* (Resolute Raccoon ; publiée le 23 avril 2026). https://documentation.ubuntu.com/release-notes/26.04/
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, section « rust-coreutils » (les commandes de base viennent de `rust-coreutils` depuis la 25.10 ; `cp`, `mv` et `rm` restent de GNU). https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- Ubuntu Manpages. *man(1)*, d'Ubuntu 26.04 (sections du manuel ; `-k`). https://manpages.ubuntu.com/manpages/resolute/en/man1/man.1.html
- Docker. *Install Docker Engine on Ubuntu* (versions d'Ubuntu prises en charge ; Linux Mint sans prise en charge officielle). https://docs.docker.com/engine/install/ubuntu/
- Docker. *Install Docker Desktop on Mac* (installateur selon le processeur ; versions de macOS prises en charge). https://docs.docker.com/desktop/setup/install/mac-install/
- Docker. *Docker Desktop WSL 2 backend on Windows* (intégration avec la distribution WSL). https://docs.docker.com/desktop/features/wsl/
- Microsoft. *Installer WSL* (`wsl --install` ; versions de Windows). https://learn.microsoft.com/fr-fr/windows/wsl/install
- Docker. *Volumes* (un volume garde des données en dehors du conteneur et survit à sa suppression). https://docs.docker.com/engine/storage/volumes/
- Docker. *docker container run* (`-i`, `-t`, `--rm`, `-h`, `--network` ; chaque conteneur a sa propre liste de processus). https://docs.docker.com/reference/cli/docker/container/run/
- Apple. *Utiliser zsh comme shell par défaut sur un Mac* (zsh par défaut depuis macOS 10.15). https://support.apple.com/fr-fr/102360
- Nos propres mesures, 18 et 19 nov. 2026, sur un Mac avec Apple silicon (Docker 29.2.1, linux/arm64) et sur un ordinateur
  sous Linux Mint 22.3 (x86_64, Docker 29.10.0). Ce qui a été lancé sur chacun et comment chaque chiffre a été mesuré
  figure dans les deux tableaux de « Avant de commencer ». Les écrans de cette leçon et les trois touches de l'étape 5
  ont été pris sur le Mac. La révision du 19 nov. — le conteneur sans réseau, les étiquettes du cours et le `verifica`
  qui n'utilise plus l'horloge — a été testée sur les deux.
