# Lesson 01 — Moving around without seeing

**By Dorian Chávez, founder of Hábil and integration architect.**

**Before you read, five words.** One line each; the rest come in the step where they are used.

- **Folder:** a place that holds files and other folders. In the terminal and in manuals you will see it as
  *directory*. It is the same thing.
- **Tree:** the way the folders are arranged: one inside another, like the branches of a tree.
- **Root:** the folder at the very top, the one that contains all the others. It is written with a single slash: `/`.
- **Current folder:** the folder where you are standing right now. It is the one `pwd` answers with.
- **Path:** the directions for getting to a folder or a file: folder names separated by slashes. For example,
  `pagos/bitacoras`.

**What you take away:** you move through folders without seeing them, with `cd`; you know which one you will end up in
before you press Enter; you see what is in a folder with `ls`, hidden things included; you read each column of
`ls -l`; and you write the way to a folder in two ways.

**This lesson's new idea (the only one that is assessed):** folders form a tree, and you reach a place in that tree
with two kinds of path: the **absolute** one, which starts from the root, and the **relative** one, which starts from
where you are standing.

**In what order it goes:** you set up a maze of folders (step 1), you walk through it with relative paths (step 2) and
with absolute paths (step 3), you look at what is in a folder and read its columns (step 4), and at the end you write
down four answers that `verifica` checks (step 5).

**A note on the language of the maze.** As in lesson 00, the course's messages and the maze's names are in Spanish
and are part of the code, so they are not translated. The messages appear exactly as the terminal prints them, with a
gloss in English, in italics, below each block or in parentheses inside a table. The names of the maze are glossed
once, in step 1: `pagos` («payments»), `bitacoras` («logs»), `guardia` («on call»), `marzo.log` («March log»),
`.pendiente` («pending») and the rest. So are the four keys of the answer sheet, in step 5: `mas-grande` («largest»),
`oculto` («hidden»), `ruta-absoluta` («absolute path») and `ruta-relativa` («relative path»).

---

## The scene

*An invented scenario; the screens that appear in the lesson are real.*

Tuesday, 11:47 p.m. Lucía is already in the server. She has already read the prompt: she knows who she is in there and
which folder she is standing in. On the other end of the phone, they ask her for this:

«Go to the payments logs folder and tell me which file is the biggest.»

«Go.» On her computer, going to a folder means double-clicking until you get there. Here there is nothing to click on.
There is the same line as before, waiting:

```text
operaciones@pagos-03:~$
```

Lucía has to reach a folder she cannot see, by a path she cannot see, and say which file is the biggest without
getting it wrong, because on the other end they are going to act on what she says.

This lesson is that trip, in a maze made for getting lost with no consequences.

---

## Before you start

**What you need:** lesson 00 done, with `verifica 00` in green; the course material up to date; and pen and paper for
your predictions.

**Update your practice terminal.** The image you built in lesson 00 does not have what this lesson needs. From your
computer, in the course folder:

```bash
git pull
./practicar reconstruir
```

`git pull` brings the new files from the repository. `./practicar reconstruir` («rebuild») puts them into the image
and normally reuses the Ubuntu and the packages that were already built; it needs the network only if one is missing.
On the test Mac, with everything before it already built, it took less than two seconds. It ends with the line
`✅ Imagen lista.` («Image ready.») If you do not use `git`, download the course ZIP again and run `./practicar reconstruir` in
the new folder.

Rebuilding does not delete your work: your `~/practica` folder lives apart, in the volume from lesson 00. After that,
get in as you already know, with `./practicar`.

**Where this lesson was tested, and where it has not been:**

| System | Status |
|---|---|
| Mac with Apple silicon, Docker 29.2.1 running with Colima | Tested on Nov 19, 2026: `./practicar reconstruir`, `prepara 01` twice, every command of the five steps, `verifica 01` in red and in green, and the screens you see here |
| Linux Mint 22.3 (x86_64), Docker Engine 29.10.0 | Tested on Nov 19, 2026: `./practicar reconstruir-limpio` («clean rebuild»), `./practicar reconstruir` with the package layer reused, `prepara 01` twice, `verifica 01` in green, a wrong path and a shortcut in red, and the six forms of `ls` identical to GNU's |
| Ubuntu, Mac with Docker Desktop, Windows with Docker Desktop and WSL 2 | Not tested yet with this lesson |

**What will match your screen and what will not.** The names, the file sizes, the dates and the paths match. The
arrangement in columns, the colors, the `total` line and the size of the folders may vary, because they depend on your
window and on where Docker keeps the volume.

---

## Step 1 · Set up the maze and look at it from above

Inside your practice terminal, run:

```bash
prepara 01
```

```text
✅ Laberinto listo en /home/alumno/practica/01 (medí: 10 carpetas y 12 archivos, cada archivo con su tamaño).
   Te dejé la hoja de respuestas, con sus claves vacías: /home/alumno/practica/respuestas/01.md
   Empieza por ahí:  cd ~/practica/01
```

*Maze ready in /home/alumno/practica/01 (I measured: 10 folders and 12 files, each file with its size). I left you the
answer sheet, with its keys empty: /home/alumno/practica/respuestas/01.md. Start there: `cd ~/practica/01`.*

`prepara` («prepare») is a course command: it sets up the practice material for a lesson. Three things about it:

- **It has no randomness.** The names, the sizes and the dates come from a fixed list. In the tests it was set up
  several times and came out identical.
- **It touches two things and nothing else:** the `~/practica/01` folder, and your answer sheet, which it creates only
  if it does not exist.
- **It can be run again.** If you get lost or break something, `prepara 01` leaves the maze as on the first day. To do
  that it deletes the one that is there, so it asks you to type `si` («yes»). That is why it is separate from
  `verifica`, which only looks.

**The maze, seen from above.** This drawing is not the output of any command: it is what you will have to imagine as
you walk. In the terminal you will only see the folder where you are standing.

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

*The arrows say: «the root», «your home folder: ~», «the maze starts here». The names, in Spanish: `LEEME.txt` («README.txt»),
`pagos` («payments»), `bitacoras` («logs»), `enero`, `febrero`, `marzo`, `abril` and `mayo` («January» to «May»),
`viejas` («old ones»), `config`, `plantillas` («templates»), `recibo` («receipt»), `entregas` («deliveries»),
`guardia` («on call»), `respaldos` («backups»), `semana` («week») and `copia` («copy»). `home` and `alumno` («student»)
are the home folder of your practice user, and `practica` is «practice».*

Read it from top to bottom. At the very top is the root, `/`. Inside the root there is a folder `home`; inside `home`,
a folder `alumno`; and so on. Each level to the right is «inside of». Going up is going toward the root; going down is
going into a folder that is inside yours. The files are filler: what matters is their name, their size and their date.

Now stand at the entrance of the maze. `cd` is the command for changing folders, and you will look at it closely in the
next step. Today this line is a recipe to copy as is:

```bash
cd ~/practica/01
```

```text
alumno@practica:~/practica/01$
```

`cd` did not answer anything: as in lesson 00, silence means it went well. What changed is the prompt: before it said
`~` and now it says `~/practica/01`. **The prompt keeps telling you which folder you are standing in.** Check it with
`pwd`:

```text
/home/alumno/practica/01
```

The prompt uses the short form, with `~`. `pwd` answers with the full form.

**What it shows:** nothing yet. This step leaves the material ready for the three exercises.

---

## Step 2 · Walk: `cd` with relative paths

`cd` comes from *change directory*. You write `cd`, a space and the path to where you want to go.

To know where you can go, you need to see what is there. `ls`, alone, lists what is in the current folder; you will see
it in depth in step 4.

```bash
ls
```

```text
LEEME.txt  pagos  respaldos
```

It matches the drawing: in `01` there is one file and two folders.

**Predict first.** You are going to run `cd pagos`. Write on your paper what the prompt will say afterwards.

```bash
cd pagos
```

```text
alumno@practica:~/practica/01/pagos$
```

You went down one level. `pagos` is a **relative path**: it does not say where `pagos` is in the whole tree, it only
says «the `pagos` folder that is here, where I am standing». It worked because in `01` there is a folder `pagos`.

Go down another level and ask where you ended up:

```bash
cd bitacoras
pwd
```

```text
/home/alumno/practica/01/pagos/bitacoras
```

**To go up: two dots.** `..` means «the folder above where I am». **Predict:** you are in `bitacoras`. Where do you
end up after `cd ..`?

```bash
cd ..
pwd
```

```text
/home/alumno/practica/01/pagos
```

**Several levels in one path.** The folders of a path are separated with a slash. You can go down two levels in one go,
and go up two with `..` twice:

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

**An error on purpose.** You are in `pagos`. Ask for a folder that does not exist there:

```bash
cd nada
```

```text
bash: cd: nada: No such file or directory
```

«No such file or directory.» The message gave you your prompt back, so it did not trap you. And the prompt still says
`pagos`: **when `cd` fails, you stay where you were.** The same happens with `cd Bitacoras`: the folder is called
`bitacoras`, and the terminal tells uppercase from lowercase in names, just as it does in commands.

All the paths in this step are relative: **their result depends on where you are standing.** The same `cd pagos` that
worked in `01` fails in a folder that has no `pagos` inside.

**What it shows** (objective 1.1, first half): your predictions from this step, compared with the prompt and with
`pwd`.

---

## Step 3 · The other way: absolute paths

A relative path works as long as you know where you are. There is another way of writing a route, which does not depend
on that.

**Go all the way up.** The root is written with a single slash:

```bash
cd /
ls
```

```text
bin   dev  home  media  opt   root  sbin  sys  usr
boot  etc  lib   mnt    proc  run   srv   tmp  var
```

The prompt now says `alumno@practica:/$`. You are in the root, and those are the folders of a Linux system. Today one
matters to you, `home`, because your home folder is inside it.

**Predict:** you are in the root. What happens if you run `cd pagos`, the same command that worked in step 2?

```text
bash: cd: pagos: No such file or directory
```

It failed, and rightly so: in the root there is no folder `pagos`.

**An absolute path starts with `/`.** It gives the complete route from the root, folder by folder. That is why it works
the same wherever you are standing. This one leads to the logs:

```bash
cd /home/alumno/practica/01/pagos/bitacoras
```

```text
alumno@practica:~/practica/01/pagos/bitacoras$
```

Read it next to the drawing from step 1: from the root to `home`, from `home` to `alumno`, from there to `practica`, to
`01`, to `pagos` and to `bitacoras`. It is the same thing `pwd` answers when you are there: **`pwd` answers with the
absolute path of the current folder.**

**The slash at the beginning changes everything.** `cd pagos` asks for the `pagos` folder that is inside where I am.
`cd /pagos` asks for the `pagos` folder that is in the root. Check it where there is a `pagos`:

```bash
cd ~/practica/01
cd /pagos
```

```text
bash: cd: /pagos: No such file or directory
```

There is a `pagos` one step away from you, but you asked for another one: one that would be in the root, and that one
does not exist.

**Your home folder has an abbreviation: `~`.** At the beginning of a path, the terminal replaces `~` with
`/home/alumno` before running the command. That is why `cd ~/practica/01` and `cd /home/alumno/practica/01` lead to
the same place, and `cd` alone, with no path, takes you to your home folder. But `~` is an abbreviation of the terminal
and not part of the tree: in another account, `~` is another folder. When you are asked for an absolute path, write it
from the root, with `/`.

**The four starting points,** together:

| The path starts with | It starts from | Example | Depends on where you are |
|---|---|---|---|
| `/` | The root | `/home/alumno/practica/01` | No |
| `~` | Your home folder | `~/practica/01` | No |
| `..` | The folder above the current one | `../config` | Yes |
| A name | The current folder | `pagos/bitacoras` | Yes |

**Which one suits?** The relative one is short: it suits when the place is nearby. The absolute one leaves no doubt: it
suits when you are dictating a route to someone who does not know which folder you are standing in, like Lucía on the
phone.

**What it shows** (objective 1.1 complete): exercise 1.1, below. And it is half of objective 1.3.

---

## Step 4 · Look at what is there: `ls` and its columns

Go to the logs folder, by whatever path you like, and list it:

```bash
cd ~/practica/01/pagos/bitacoras
ls
```

```text
abril.log  enero.log  febrero.log  marzo.log  mayo.log  viejas
```

`ls` comes from *list*. Alone it gives only the names, in alphabetical order. It gives no sizes or dates: for that
there are the options, the words with a dash that change what a command does. Today there are three.

### `-a`: hidden things too

**Predict:** the `ls` above showed six names. Do you think that is everything in the folder?

```bash
ls -a
```

```text
.   .pendiente  enero.log    marzo.log  viejas
..  abril.log   febrero.log  mayo.log
```

Three names that start with a dot appeared. In Linux, **a file whose name starts with a dot is a hidden file**: plain
`ls` does not list it. It is not protected or secret; it just does not get in the way of the everyday list. `-a` comes
from *all*. `.pendiente` is a hidden file of the maze. `..` you already know: it is the folder above. And `.` is the
current folder.

### `-l`: the long list

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

`-l` comes from *long*: one line per file or folder, with its data in columns. Read the line of `marzo.log` from left
to right:

| Column | In `marzo.log` | What it says |
|---|---|---|
| **Type** | `-` | The first letter of the line. A dash is a normal file. A `d`, from *directory*, is a folder: look at the line of `viejas`. There are other types; in this maze there are only these two |
| **Permissions** | `rw-r--r--` | The nine letters that follow. They say who can read, write and run the file. Today you only give them a name: they are read and changed in lesson 09 |
| **Links** | `1` | How many names point to that same file. You do not use it in this course |
| **Owner** | `alumno` | The user the file belongs to. Here it is you |
| **Group** | `alumno` | The group of users it belongs to. It comes up again in lesson 09 |
| **Size** | `1200000` | How big the file is, in bytes |
| **Date** | `Mar 31  2025` | When it was last modified. `Mar` is March and `Apr` is April |
| **Name** | `marzo.log` | What it is called |

Three details:

- **The `total` at the top is not the sum of the size column.** It is the space that what is listed takes up on disk, in
  blocks of 1024 bytes. You do not use it in this course.
- **The size of a folder is not what it holds.** `viejas` says `4096` and inside it has a file of 20,000 bytes. To find
  out how much is inside, you go in and look.
- **The date changes form according to age.** If the file was modified more than six months ago, you see the year, as
  here. If it is recent, instead of the year you see the time, in universal time (UTC) and not your clock's. You will
  notice it in step 5.

### `-h`: sizes you can read

Which is bigger, `998000` or `1200000`? You have to count digits. `-h` comes from *human-readable*: together with `-l`,
it writes the size abbreviated and with a letter.

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

Without a letter, it is bytes. `K` is kilobytes: 1,024 bytes. `M` is megabytes: 1,024 K. `G` is gigabytes: 1,024 M.

**The trap of this column: you read the letter first and then the number.** `975K` has a bigger number than `1.2M`, and
even so it is the smaller of the two files, because one M is worth more than a thousand K. And one limit: `-h` rounds.
`marzo.log` is 1,200,000 bytes, which is 1.14 M, and `ls` writes `1.2M`. When you need the exact number, drop the `h`.

### Options combine, and `ls` accepts a path

Several one-letter options can be written stuck together: `ls -lah` is the same as `ls -l -a -h`. Run it: it is the long
list, with hidden things and readable sizes. There `.pendiente` already has its line.

And you do not need to go to a folder to see what it has: `ls` accepts a path, relative or absolute, just like `cd`.
`ls -lh ..` shows you the folder above and you stay standing in `bitacoras`. Looking does not move you.

### With Claude Code, in this order

In this course an AI assistant always comes in the same way: **you predict → you ask → you read → you run → you
explain.**

1. **You predict.** With your `ls -lah` in front of you, write on your paper which is the biggest file in `bitacoras`.
2. **You ask.** On your computer, to Claude Code: «Give me an `ls` command that sorts a folder by size, from biggest to
   smallest, and explain each option to me». Claude Code does not see your maze: it runs outside.
3. **You read.** Before running what it gave you, look up each option in `ls --help`, as in lesson 00.
4. **You run.** You type the command yourself, in your practice terminal.
5. **You explain.** In your own words: why did it come out in that order? Does it match your prediction?

**What it shows** (objective 1.2): exercise 1.2, below.

---

## Step 5 · Write down your answers and check them

Up to here you checked everything yourself, with the prompt and with `pwd`. Now it is `verifica`'s turn. So that it can
check what you found, you write it down in your **answer sheet**, `~/practica/respuestas/01.md`, which `prepara` has
already left for you. It is plain text, with four lines: a key, a colon and your answer.

| Key | What goes there |
|---|---|
| `mas-grande` («largest») | The name of the biggest file in the maze. Only the name, as `ls` writes it, without its folder |
| `oculto` («hidden») | The name of the hidden file in the maze. Only the name, with its dot |
| `ruta-absoluta` («absolute path») | The absolute path of the destination folder |
| `ruta-relativa` («relative path») | The relative path to the destination folder, starting from `bitacoras` |

Run `verifica 01` before writing anything down, to see where you start from. The first two lines come out like this:

```text
✅ Tu laberinto está completo (medí: las 22 carpetas y archivos de la lista existen en ~/practica/01, ninguno es un enlace y cada archivo tiene su tamaño)
❌ Tu renglón «mas-grande:» está vacío (medí: lo que hay después de los dos puntos). Anótalo con  anota 01 mas-grande  y el nombre del archivo
```

*Your maze is complete (I measured: the 22 folders and files on the list exist in ~/practica/01, none is a link and
each file has its size). Your line «mas-grande:» is empty (I measured: what comes after the colon). Write it down with
`anota 01 mas-grande` and the name of the file.*

Three identical reds follow, one for each key that is missing. It is normal: the sheet is empty.

**How to write it down.** You have not seen a text editor yet; it comes in lesson 07. That is why the course brings a
command, `anota` («note down»): you type the lesson number, the key and your answer. Suppose you think the biggest is
`enero.log`:

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

*Noted. Your sheet is now like this: `mas-grande: enero.log`, and the other three keys still empty.*

`anota` does not know whether you got it right: it only writes. That is what `verifica 01` says, and now its second line
changes:

```text
❌ «enero.log» no es el archivo más grande del laberinto (medí: comparé sus bytes contra los de los demás archivos de la lista; hay uno con más). Vuelve a leer la columna del tamaño con  ls -lh : fíjate en la letra, no solo en el número
```

*«enero.log» is not the biggest file in the maze (I measured: I compared its bytes against those of the other files on
the list; there is one with more). Read the size column again with `ls -lh`: look at the letter, not only the number.*

This is how a red is read: it says what you wrote down, what was measured and with which command to look again. It does
not give the answer. To correct it, you write down the same key again: `anota` changes that line. And `anota 01`, with
nothing else, shows you the sheet.

**The two paths of exercise 1.3.** The destination folder is `guardia`: it is inside `entregas`, which is inside
`pagos`. Find it in the drawing from step 1. You have to reach it in two ways: with an **absolute path**, and with a
**relative path** that starts from the `bitacoras` folder.

Before writing down each path, walk it: stand where it corresponds, go in with `cd` and ask with `pwd` whether you got
to `guardia`. When a path takes you there, write it down just as you typed it. `verifica` does the same test you do.

When you have the four lines, run `verifica 01`. Correct and repeat until you see this at the end:

```text
✅ Lección 01: todo en verde.
```

*Lesson 01: everything in green.*

Now that the sheet has your answers, look at its line with `ls -l ~/practica/respuestas`:

```text
total 4
-rw-r--r-- 1 alumno alumno 139 Nov 19 07:35 01.md
```

It is the detail that was left pending in step 4: your sheet is recent, and instead of the year it has the time. The
size and the date of yours will be different.

**What it shows** (objectives 1.2 and 1.3): `verifica 01` all in green.

---

## Your exercises

Exercise 1.1 is done **without AI** and without running anything before predicting: only your paper and the drawing
from step 1.

| # | You do | It is done when |
|---|---|---|
| 1.1 | You predict in writing which folder you end up in after each of the ten `cd` below, and then you run them | You have your ten predictions and the ten outputs of `pwd`, and you can explain each one that did not match |
| 1.2 | In `bitacoras`: you find the biggest file and the hidden one and write them in your sheet; and you fill in on your paper the table of columns below | `verifica 01` marks `mas-grande` and `oculto` in green, and your table matches «Compare your answers» |
| 1.3 | You reach the `guardia` folder with an absolute path and with a relative one that starts from `bitacoras`, and write down both | `verifica 01` comes out all in green |

**The ten `cd` of 1.1.** First write the ten predictions, with the full path you think `pwd` will answer. Do not run
anything yet. If you think a `cd` is going to fail, write it that way, and write where you stay. You start standing in
`~/practica/01`, and each command starts from where the previous one left you.

| # | Command | # | Command |
|---|---|---|---|
| 1 | `cd pagos/config` | 6 | `cd pagos` |
| 2 | `cd plantillas` | 7 | `cd home` |
| 3 | `cd ../../bitacoras/viejas` | 8 | `cd ~` |
| 4 | `cd ../..` | 9 | `cd ../..` |
| 5 | `cd /` | 10 | `cd ..` |

When you have all ten, go to the entrance with `cd ~/practica/01` and run the commands one by one, with a `pwd` after
each one. There is no passing number: what counts is that, for each one you missed, you can say what you read wrong:
whether the path was absolute or relative, or which folder you were standing in.

**The table of columns of 1.2.** Run `ls -la` in `bitacoras` and fill in these two lines on your paper. `verifica` does
not check this part: you compare it yourself.

| Name | File or folder? | Permissions (the nine letters) | Owner | Size in bytes | Date |
|---|---|---|---|---|---|
| `.pendiente` | | | | | |
| `viejas` | | | | | |

**Time (estimated):** 60 to 90 minutes. It is an estimate: it will be adjusted with the first group that takes the
course.

---

## If you want more

*Optional. Nothing in this box is assessed or needed for the exercises.*

- **`cd -` takes you back to the previous folder.** It is a dash after `cd`. It is useful for going back and forth
  between two places, and it tells you where it took you back to.
- **The Tab key completes names.** In `pagos`, type `cd bit` and press Tab: the terminal completes `bitacoras/`,
  because it is the only folder there that starts that way. Fewer letters, fewer typing mistakes.
- **`ls -lhS` sorts by size.** It is one possible answer to what you asked Claude Code. In `ls --help`, the `S` says
  `Sort by file size, largest first.` This is how it came out in `bitacoras`:

```text
total 2.2M
-rw-r--r-- 1 alumno alumno 1.2M Mar 31  2025 marzo.log
-rw-r--r-- 1 alumno alumno 975K Apr 30  2025 abril.log
-rw-r--r-- 1 alumno alumno 9.6K Feb 28  2025 febrero.log
drwxr-xr-x 2 alumno alumno 4.0K Jan  2  2025 viejas
-rw-r--r-- 1 alumno alumno  980 Jan 31  2025 enero.log
-rw-r--r-- 1 alumno alumno    0 May 31  2025 mayo.log
```

  `viejas` ends up in the middle because its line says `4.0K`, the size of the folder and not of what it holds. And the
  hidden file does not appear, because the command has no `-a`.
- **The `ls` of this terminal and the one elsewhere.** Here `ls` comes from `rust-coreutils` and not from the GNU
  project, as you saw in lesson 00. In the forms in this lesson the output was identical to that of GNU's `ls`, which
  Ubuntu leaves installed as `gnuls`: compare `gnuls -lah` with your `ls -lah`. What changes is the text of
  `ls --help` and of `man ls`.

---

## Not running?

| You see | Probable cause | What you do |
|---|---|---|
| `bash: prepara: command not found`, or `Todavía no hay verifica para la lección «01».` *(There is no verifica for lesson «01» yet.)* | Your image is from before this lesson | Get out with `exit` and run `./practicar reconstruir` |
| `Ya existe /home/alumno/practica/01.` and a question *(It already exists: /home/alumno/practica/01.)* | You had already set up the maze, and setting it up again deletes it | To start from zero, type `si` («yes»). If not, press Enter: nothing is deleted |
| After `prepara 01`, `ls` shows nothing, or `error retrieving current directory` comes out | You were standing inside the maze when it was set up again: that folder no longer exists | `cd ~/practica/01` |
| `❌ No preparo nada: … es un enlace…` *(I prepare nothing: … it is a link…)* | In your practice folder there is a link where one of your folders should be | From your computer, `./practicar desde-cero` («from scratch») |
| `bash: cd: …: No such file or directory` | The folder does not exist with that name in the place where you asked for it | `pwd` to know where you are and `ls` to see what is there. Check the uppercase letters and whether the path should have started with `/` |
| `❌ Tu ruta relativa … no existe saliendo de …` *(Your relative path … does not exist starting from …)* | You tried it standing in another folder | Go to `bitacoras` and try your path there with `cd` and `pwd` |
| `❌ Tu laberinto no está completo…` *(Your maze is not complete…)* | You deleted or changed something in the maze | `prepara 01`. Your answer sheet is not lost |

---

## To wrap up

**Answer without reading again.**

**Five paths: absolute or relative?** You pass with 4 out of 5.

1. `config/plantillas`
2. `/home/alumno/practica`
3. `../respaldos`
4. `/`
5. `..`

**And five questions:**

6. You are in `/home/alumno/practica/01/respaldos/semana-1`. Which relative path takes you to `semana-2`?
7. You run `ls` in a folder and nothing comes out. Can you already say it is empty? What do you run to be sure?
8. In `ls -lh` you see `812K` on one line and `3.1M` on another. Which file is bigger, and how do you know?
9. A line of `ls -l` starts with `d`. What does that letter tell you? And what does the number in the size column say
   about its contents?
10. You are going to dictate to a colleague the route to a folder, and you do not know which one he is standing in. Do
    you dictate an absolute path or a relative one? Why?

**What comes next:** you now know how to move through folders and see what they hold. In lesson 02 you start changing
what is there: you create, copy, move and delete, in a terminal where there is no trash can.

---

> **What Hábil does**
>
> We operate and modernize the infrastructure where systems that cannot fail run: Linux servers, containers,
> Kubernetes and continuous delivery, for banking, insurance and regulated companies. If your servers depend on one
> person who «knows where everything is», that can be put in order without stopping operations.
>
> [**CI/CD, DevSecOps and cloud without surprises**](https://www.habil.mx/en/cloud-native-architecture/)
>
> First conversation, at no cost.

---

## Compare your answers

**Exercise 1.1.** What `pwd` answers after each command:

| # | Command | You end up in | Why |
|---|---|---|---|
| 1 | `cd pagos/config` | `/home/alumno/practica/01/pagos/config` | Relative: goes down two levels from `01` |
| 2 | `cd plantillas` | `/home/alumno/practica/01/pagos/config/plantillas` | Relative: goes down one |
| 3 | `cd ../../bitacoras/viejas` | `/home/alumno/practica/01/pagos/bitacoras/viejas` | Goes up two, to `pagos`, and down two along another branch |
| 4 | `cd ../..` | `/home/alumno/practica/01/pagos` | Goes up two |
| 5 | `cd /` | `/` | Absolute: the root, wherever you were |
| 6 | `cd pagos` | `/` | Fails: `No such file or directory`. In the root there is no `pagos`, and when `cd` fails you stay where you were |
| 7 | `cd home` | `/home` | Relative: in the root there is a folder `home` |
| 8 | `cd ~` | `/home/alumno` | Your home folder, wherever you were |
| 9 | `cd ../..` | `/` | Goes up two: from `alumno` to `home`, and from `home` to the root |
| 10 | `cd ..` | `/` | Above the root there is nothing: `..` in the root is the root itself. It gives no error and does not move you |

**Exercise 1.2, the biggest and the hidden one.** `verifica 01` tells you whether you got it right. The reasoning that
should be behind it:

- **The biggest** is read in the size column of `ls -lh`, the letter first and then the number: the only line with `M`
  beats the ones with `K`. Without `-h` you get to the same place by counting digits.
- **The hidden one** appears only with `-a`. It is the only name that starts with a dot and is neither `.` nor `..`.
  In your sheet it goes with its dot and everything: the dot is part of the name.

**Exercise 1.2, the table of columns.**

| Name | File or folder? | Permissions | Owner | Size in bytes | Date |
|---|---|---|---|---|---|
| `.pendiente` | File: the line starts with a dash | `rw-r--r--` | `alumno` | `48000` | `May 12  2025`: May 12, 2025 |
| `viejas` | Folder: the line starts with `d` | `rwxr-xr-x` | `alumno` | `4096` in the test; on your computer it may be another | `Jan  2  2025`: January 2, 2025 |

If in the size of `.pendiente` you wrote `47K`, you ran `ls -lah`: it is the same file, with the size abbreviated. If in
owner you hesitated between the two columns that say `alumno`: the first is the owner and the second is the group.

**Exercise 1.3.** There is no written answer here: the check is `verifica 01`. To know whether you are on track:

- Your absolute path starts with `/` and is, letter by letter, what `pwd` answers when you are standing in `guardia`.
  After the root it has seven names.
- Your relative path does not start with `/` or with `~`. The shortest one goes up one level and down two. There are
  other longer ones that also get there, and `verifica` accepts them: what it measures is that the path written, folder
  by folder, leads to `guardia`.

**To wrap up.**

1. Relative.
2. Absolute.
3. Relative.
4. Absolute: it is the root.
5. Relative.
6. `../semana-2`: you go up to `respaldos` and down to `semana-2`.
7. Not yet: plain `ls` does not list hidden things. You run `ls -a`. If only `.` and `..` come out, now it is empty.
8. The `3.1M` one. You read the letter first: one M is worth more than a thousand K.
9. That it is a folder. It says nothing about the contents: that number is the size of the folder itself.
10. An absolute one. A relative one only works if both know which folder he is standing in; the absolute one starts from
    the root.

---

## Lesson sources

Opened on Nov 19, 2026. All of them are in English.

- The Open Group. *POSIX.1-2024, Base Definitions, chapter 3* (3.2 «Absolute Pathname», 3.311 «Relative Pathname» and 3.111 «Dot-Dot»). https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap03.html
- The Open Group. *POSIX.1-2024, cd utility* (`cd` without a path uses the home folder; `cd -` goes to the previous folder and prints it). https://pubs.opengroup.org/onlinepubs/9799919799/utilities/cd.html
- The Open Group. *POSIX.1-2024, ls utility* (the order of the `-l` columns; time or year depending on the last six months). https://pubs.opengroup.org/onlinepubs/9799919799/utilities/ls.html
- GNU. *Bash Reference Manual, «Bourne Shell Builtins»* (`cd` and `pwd`; `pwd` prints the absolute path of the current folder). https://www.gnu.org/software/bash/manual/html_node/Bourne-Shell-Builtins.html
- GNU. *Bash Reference Manual, «Tilde Expansion»* (`~` is replaced with the home folder). https://www.gnu.org/software/bash/manual/html_node/Tilde-Expansion.html
- GNU. *Coreutils, «What information is listed»* (the `-l` columns, the type letters, the `total` line and `-h` with powers of 1024). https://www.gnu.org/software/coreutils/manual/html_node/What-information-is-listed.html
- GNU. *Coreutils, «Which files are listed»* (`ls` does not list names that start with a dot; `-a` does). https://www.gnu.org/software/coreutils/manual/html_node/Which-files-are-listed.html
- Ubuntu Manpages. *ls(1)*, from Ubuntu 26.04 (the `ls` of `rust-coreutils` 0.10.0: `-a`, `-l`, `-h`, `-S`). https://manpages.ubuntu.com/manpages/resolute/en/man1/ls.1.html
- Ubuntu Manpages. *hier(7)*, from Ubuntu 26.04 (the folders at the root of a Linux system; `/home`). https://manpages.ubuntu.com/manpages/resolute/en/man7/hier.7.html
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, section «rust-coreutils» (the GNU commands remain installed with the `gnu` prefix, such as `gnuls`). https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- Our own measurement, Nov 19, 2026, on a Mac with Apple silicon (Docker 29.2.1 with Colima, linux/arm64), with the
  course image on Ubuntu 26.04.1: `ls` from uutils coreutils 0.10.0, `gnuls` from GNU coreutils 9.7 and bash 5.3.9. The
  screens, the comparison of `ls` against `gnuls`, the rounding of `-h`, the Tab key and the time of
  `./practicar reconstruir` come from there. On Linux this lesson is not tested yet.
