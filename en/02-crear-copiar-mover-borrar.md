# Lesson 02 — Create, copy, move and delete without fear

**By Dorian Chávez, founder of Hábil and integration architect.**

**Before you read, four words.** One line each; the rest come in the step where they are used.

- **Source and destination:** when you copy or move, the source is what you already have and the destination is where
  it goes. In a command, the source is written first and the destination last.
- **Overwrite:** to write on top of a file that already existed. What it held before is lost.
- **Trash can:** the place where your computer keeps what you deleted, in case you change your mind. In the terminal
  there is none.
- **Pattern:** a name with wildcards, such as `*.log`, that catches several names in one go. The `*` means «anything».

**What you take away:** you create folders and files with `mkdir` and `touch`; you copy, move and rename with `cp` and
`mv`, and you know what happens when the destination already exists; you delete with `rm` and `rmdir`, seeing first
what is going to go; and you can explain why an empty variable turns a small deletion into an enormous one.

**This lesson's new idea (the only one that is assessed):** in the terminal there is no trash can. What `rm` deletes
does not come back, and `cp` and `mv` overwrite without asking. That is why **you think before you delete**: first you
look at what is going to go, and then you delete.

**In what order it goes:** you set up a practice workshop (step 1), you create (step 2), you copy, move and rename
(step 3), you delete with care (step 4), you see the danger of the empty variable without running it (step 5), and at
the end you check everything with `verifica` (step 6).

**A note on the language of the workshop.** As in lessons 00 and 01, the course's messages and the workshop's names are
in Spanish and are part of the code, so they are not translated. The messages appear exactly as the terminal prints
them, with a gloss in English, in italics, below each block or in parentheses inside a table. The names of the workshop
are glossed once, in step 1: `taller` («workshop»), `config`, `plantillas` («templates»), `respaldo` («backup»),
`bitacoras` («logs»), `entrada` («incoming»), `reportes` («reports»), `limpieza` («cleanup»), `ensayo` («rehearsal»)
and the rest. The key of the answer sheet is glossed in step 4: `patron` («pattern»). The commands `practicar`,
`prepara`, `verifica` and `anota` keep their names too: you already know them from lessons 00 and 01.

---

## The scene

*An invented scenario; the screens that appear in the lesson are real.*

Wednesday, 12:07 a.m. Lucía found the payments log. The last line says:

```text
2025-06-02 00:09 pagos: disco lleno, no puedo escribir
```

*2025-06-02 00:09 payments: disk full, I cannot write.*

On the other end of the phone, they give her three instructions:

«Before touching anything, back up the configuration. Then put the June report in its folder. And delete the cache
temp files, which are the ones that filled the disk.»

In the cache there are files that end in `.tmp`, and also one, `cache-indice.txt` («cache-index.txt»), without which the
service will not start again. Lucía knows one thing about the command for deleting: it does not ask, and there is no
trash can to pull out what it takes away.

This lesson is that half hour, in a workshop made for making mistakes with no consequences.

---

## Before you start

**What you need:** lesson 01 done; the course material up to date; and pen and paper for your predictions.

**Update your practice terminal.** As in lesson 01, from your computer, in the course folder:

```bash
git pull
./practicar reconstruir
```

That is enough to receive this lesson: `./practicar reconstruir` («rebuild») reuses the Ubuntu and the packages that
were already built and only puts in the new files. On the test Mac it took less than two seconds and ended with
`✅ Imagen lista.` («Image ready.») Your `~/practica` folder is not touched. After that, get in with `./practicar`.

**Where this lesson was tested:**

| System | Status |
|---|---|
| Mac with Apple silicon, Docker 29.2.1 running with Colima | Tested on Nov 19, 2026: `./practicar reconstruir`, `prepara 02` twice with the same result, every command of steps 1 to 6, and `verifica 02` in red and in green |
| Linux Mint 22.3 (x86_64), Docker Engine 29.10.0 | Tested on Nov 19, 2026: `./practicar reconstruir`, `prepara 02` twice with the same result as on the Mac, the messages of `mkdir`, `touch` and `rmdir`, the questions of `cp -i`, `mv -i`, `rm -i` and `rm -I`, the whole lesson as a student, and `verifica 02` in green and in red with a hard link |
| Ubuntu, Mac with Docker Desktop, Windows with Docker Desktop and WSL 2 | Not tested yet with this lesson |

**What will match your screen and what will not.** The names, the contents, the dates of the workshop and the messages
of the commands match. The time of what you create or touch, the arrangement in columns and the colors change.

---

## Step 1 · Set up the workshop

Inside your practice terminal:

```bash
prepara 02
```

```text
✅ Taller listo en /home/alumno/practica/02 (medí: 12 carpetas y 22 archivos, cada archivo con su contenido).
   Te dejé la hoja de respuestas, con su clave vacía: /home/alumno/practica/respuestas/02.md
   Empieza por ahí:  cd ~/practica/02
```

*Workshop ready in /home/alumno/practica/02 (I measured: 12 folders and 22 files, each file with its contents). I left
you the answer sheet, with its key empty: /home/alumno/practica/respuestas/02.md. Start there: `cd ~/practica/02`.*

`prepara 02` works like the one in lesson 01: it has no randomness, it touches only `~/practica/02` and your answer
sheet, and to set the workshop up again it deletes it, so it asks you to type `si` («yes»). Each file holds a different
line of text: from the contents you can tell whether a copy is the right one or whether something was overwritten.

**The workshop, seen from above:**

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

*The arrows say: «what gets backed up», «the backup, with an old version of pagos.conf», «the new report», «there is
already a report with the same name: the old draft», «the delete exercise» and «to try the commands out before using
them for real». The names, in Spanish: `LEEME.txt` («README.txt»), `config` («configuration»), `pagos.conf`
(«payments.conf»), `plantillas` («templates»), `aviso de corte.txt` («cut-off notice.txt»), `aviso.txt`
(«notice.txt»), `recibo.txt` («receipt.txt»), `respaldo` («backup»), `bitacoras` («logs»), `ayer` («yesterday»),
`hoy` («today»), `entrada` («incoming»), `reporte-junio.txt` («june-report.txt»), `reportes` («reports»), `limpieza`
(«cleanup»), `cache-indice.txt` («cache-index.txt»), `tmp-notas.txt` («tmp-notes.txt»), `sesiones-activas` («active
sessions»), `sesiones-viejas` («old sessions»), `vacia` («empty»), `ensayo` («rehearsal»), `uno`, `dos` and `tres`
(«one», «two» and «three») and `caja` («box»). `adentro.txt` is «inside.txt».*

The demonstrations go in `ensayo`; the exercises, in the rest.

**What it shows:** nothing yet. This step leaves the material ready.

---

## Step 2 · Create: `mkdir` and `touch`

Go into the rehearsal folder:

```bash
cd ~/practica/02/ensayo
ls
```

```text
caja  dos.txt  tres.log  uno.txt
```

### `mkdir`: a new folder

`mkdir` comes from *make directory*. You write `mkdir` and the name:

```bash
mkdir nueva
```

Silence: it went well. (`nueva` is «new».) **Predict:** what happens if you run the same line again?

```text
mkdir: nueva: File exists
```

«It already exists.» `mkdir` does not overwrite a folder: it refuses, and nothing is lost.

**Several folders, one inside another.** **Predict:** `uno` does not exist. What happens with `mkdir uno/dos/tres`?

```text
mkdir: No such file or directory
```

It failed, because `mkdir` creates the last folder of the path and expects the earlier ones to exist already. To create
the whole route in one go there is the `-p` option, from *parents*, «the ones above too»:

```bash
mkdir -p uno/dos/tres
ls uno/dos
```

```text
tres
```

With `-p`, if the folder already existed, it does not complain.

### `touch`: an empty file

`touch` means «to touch». If the file does not exist, it creates it empty:

```bash
touch nota.txt
ls -l nota.txt
```

```text
-rw-r--r-- 1 alumno alumno 0 Nov 19 09:28 nota.txt
```

Zero bytes, and the time of right now. (`nota.txt` is «note.txt».) **Predict:** `uno.txt` already exists and says
`uno`. If you touch it, does it lose its contents?

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

It does not lose them: on a file that exists, `touch` only changes its date to the one of right now. `cat` shows what a
file holds; you will see it in depth in lesson 03.

### What you would see differently on another Linux

In this image, `mkdir`, `touch` and `rmdir` come from `rust-coreutils`, like the `ls` of lesson 01; `cp`, `mv` and `rm`
are still GNU's. `--version` and the Ubuntu documentation say so. What you notice is the text of two `mkdir` errors.
GNU's is still installed as `gnumkdir`, and this is how it answered:

| Command | The `mkdir` of this image | `gnumkdir`, GNU's |
|---|---|---|
| Create `nueva`, which already exists | `mkdir: nueva: File exists` | `gnumkdir: cannot create directory ‘nueva’: File exists` |
| Create `uno/dos/tres`, without `-p` | `mkdir: No such file or directory` | `gnumkdir: cannot create directory ‘uno/dos/tres’: No such file or directory` |

On a Linux with GNU you would see the second text, starting with `mkdir:`. What matters: here the error **does not say
which path failed**. If it happens with several folders on the same line, check each one with `ls`. The errors of
`rmdir` and `touch` came out the same as GNU's.

### Your turn: the on-call tree

Go back to the workshop with `cd ~/practica/02` and create, exactly as shown, Lucía's on-call folder (`guardia`, «on
call»):

```text
~/practica/02
└── guardia
    └── 2025-06-02
        ├── bitacoras/
        ├── reportes/
        │   └── resumen.txt
        └── pendientes.txt
```

The ones that end in `/` are folders; the other two are empty files. (`resumen.txt` is «summary.txt» and
`pendientes.txt` is «pending.txt».) Hint: `mkdir -p` and `touch` accept several paths on the same line. At the end,
`ls guardia/2025-06-02` must show `bitacoras  pendientes.txt  reportes`.

**What it shows** (objective 2.1): the first mark of `verifica 02`, which checks the tree **exactly**: each folder and
each file, of its type, and nothing extra. A typo such as `reprotes` comes out in red.

---

## Step 3 · Copy, move and rename: `cp` and `mv`

Go back to the rehearsal folder with `cd ~/practica/02/ensayo`.

### `cp`: copy

`cp` comes from *copy*. You write `cp`, the source and the destination. First look at what `dos.txt` says:

```bash
cat dos.txt
```

```text
dos
```

**Predict before you run the next line.** `dos.txt` already exists. If you copy `uno.txt` on top of it, does it ask you
anything? What does `dos.txt` say afterwards?

```bash
cp uno.txt dos.txt
cat dos.txt
```

```text
uno
```

It asked nothing. It **overwrote**, silently. The `dos` that was inside no longer exists anywhere.

**To make it ask: `-i`,** from *interactive*. It asks before overwriting, and if you answer `n`, it does not copy:

```bash
cp -i tres.log uno.txt
```

```text
cp: overwrite 'uno.txt'? n
```

«Do I overwrite `uno.txt`?» With `n`, `uno.txt` still says `uno`. With `y`, from *yes*, it copies.

**Copying into a folder.** If the destination is a folder that already exists, `cp` leaves the copy inside, with the
same name:

```bash
cp caja/adentro.txt nueva
ls nueva
```

```text
adentro.txt
```

### `cp -r`: copy a whole folder

**Predict:** does `cp` copy a folder the same way as a file?

```bash
cp caja otra
```

```text
cp: -r not specified; omitting directory 'caja'
```

«You did not put `-r`; I am skipping `caja`.» (`otra` is «another».) To copy a folder with everything in it you use
`-r`, from *recursive*, «all the way down»:

```bash
cp -r caja otra
ls otra
```

```text
adentro.txt
```

**The trap of `cp -r`.** **Predict:** you run exactly the same line again. `otra` already exists. What is left?

```bash
cp -r caja otra
ls otra
```

```text
adentro.txt  caja
```

The second time the destination was already a folder, so `cp` put the copy **inside**: there is a `caja` inside `otra`.
The same line does two different things depending on whether the destination exists or not. Before copying a folder,
check with `ls` whether the destination is already there.

### `mv`: move and rename

`mv` comes from *move*. It works like `cp`, with one difference: **the source disappears**.

**Renaming is moving in the same place:**

```bash
mv uno.txt primero.txt
ls
```

```text
caja  dos.txt  nota.txt  nueva  otra  primero.txt  tres.log  uno
```

There is no `uno.txt` any more. It is the same file, with another name. (`primero` is «first».)

**Predict:** `dos.txt` exists. What happens if you move `tres.log` on top of it?

```bash
mv tres.log dos.txt
cat dos.txt
```

```text
tres
```

Just like `cp`: it **overwrote without asking**, and `tres.log` no longer exists. `mv -i` asks first:

```bash
mv -i primero.txt dos.txt
```

```text
mv: overwrite 'dos.txt'? n
```

And if the destination is a folder, `mv` puts the file inside, like `cp`:

```bash
mv nota.txt nueva
ls nueva
```

```text
adentro.txt  nota.txt
```

**In summary:**

| Command | The source | If the destination is a file that exists | If the destination is a folder |
|---|---|---|---|
| `cp` | Stays | Overwrites it without asking | Copies inside |
| `cp -r` | Stays | Same as `cp` | Copies inside, the second time too |
| `mv` | Disappears | Overwrites it without asking | Moves inside |
| `cp -i`, `mv -i` | Same | Asks first | Same |

### Your turn: the backup and the report

Go back to `~/practica/02`. There are four tasks. **Before each one, write on your paper what you expect to happen**;
the three marked predictions are compared at the end, in «Compare your answers».

1. **Copy** `config/pagos.conf` on top of `respaldo/pagos.conf`. First look at what the backup says with `cat`.
   *Prediction A:* is it going to ask you? What will `respaldo/pagos.conf` say at the end?
2. **Copy** the whole folder `config/plantillas` to `respaldo/plantillas`, **only once**.
   *Prediction B:* if you copy it twice with the same line, what is left in `respaldo/plantillas`?
3. **Rename** `bitacoras/hoy.log` to `bitacoras/2025-06-02.log`.
4. **Move** `entrada/reporte-junio.txt` to `reportes`, where there is already a `reporte-junio.txt`.
   *Prediction C:* what happens to the old draft in `reportes`? And to the `entrada` folder?

You put the commands together yourself, with what you saw in the rehearsal folder.

**With Claude Code.** In this course an AI assistant comes in only one way: you predict, you ask, you read, you run and
you explain. For deleting there is one more rule: you never ask it for the command; you ask it to explain yours. The
exercise with Claude Code for this lesson, about `mv -n`, is in «If you want more».

**What it shows** (objective 2.2): marks two to five of `verifica 02`, and your predictions A, B and C, which you
compare yourself. `verifica` compares **contents**, not just names: it knows whether the backup holds the good version
or the old one, and whether the copy of `plantillas` has the same as the original.

---

## Step 4 · Delete: `rm` and `rmdir`

Before you type a single `rm`, what makes this part different:

- **There is no trash can.** `rm` does not send anything anywhere: it deletes.
- **In this workshop, `rm` normally does not ask:** the files are yours and can be written to. In other cases GNU's
  `rm` may ask, but do not depend on that question: look first with `ls`, and use `rm -i` when you want it to confirm.
- **Every command that deletes, you type by hand**, the examples in this step too: neither copied from the lesson nor
  asked of an AI.

Stay in the rehearsal folder, `cd ~/practica/02/ensayo`.

### `rm`: delete a file

`rm` comes from *remove*:

```bash
rm primero.txt
ls
```

```text
caja  dos.txt  nueva  otra  uno
```

No question and no message. `primero.txt` no longer exists.

**Predict:** does `rm` delete a folder?

```bash
rm caja
```

```text
rm: cannot remove 'caja': Is a directory
```

«I cannot: it is a folder.» That is how it comes by default: `rm` only deletes files.

### `rmdir` and `rm -r`: delete folders

`rmdir` comes from *remove directory*. **Predict:** `caja` has a file inside. Does it delete it?

```bash
rmdir caja
```

```text
rmdir: failed to remove 'caja': Directory not empty
```

No. `rmdir` **only deletes empty folders**, and that makes it safe: if there was something inside, it refuses. With an
empty one it does work: `rmdir uno/dos/tres` deletes the `tres` you created in step 2, silently.

To delete a folder **with everything inside it** there is `rm -r`, the same `-r` as in `cp`:

```bash
rm -r caja
```

No question: `caja` and what it held are gone. `rm -r` is the most dangerous command in this lesson: it does not check
how much is inside.

### `rm -i`: make it ask

```bash
rm -i dos.txt
```

```text
rm: remove regular file 'dos.txt'? n
```

«Do I delete the regular file `dos.txt`?» With `n`, it stays. With an empty file it says `regular empty file`.

### Patterns: see first what is going to go

Create three test files:

```bash
touch a.bak b.bak c.bak.txt
```

**Predict:** which ones does the pattern `*.bak` catch? The `*` is «anything», so `*.bak` is «any name that ends in
`.bak`».

```bash
ls *.bak
```

```text
a.bak  b.bak
```

`c.bak.txt` did not fall in: it ends in `.txt`. **A pattern is exact**, and that is why you have to look at it before
you use it.

**The pattern is not read by `ls`: it is read by the terminal.** Before running the command, bash replaces `*.bak` with
the list of names it catches, and `ls` receives that list. `echo`, which repeats whatever you write, lets you see it:

```bash
echo *.bak
```

```text
a.bak b.bak
```

That is exactly what `rm *.bak` would receive: `rm a.bak b.bak`. **From there comes the habit of this lesson:**

1. Write the pattern with `ls` and read the list.
2. If it is exactly what you want to delete, write `rm` with **the same pattern**, letter by letter.
3. If the list has something extra, change the pattern and go back to 1.

And when the pattern catches nothing, bash leaves it as it is:

```bash
ls *.zip
```

```text
ls: cannot access '*.zip': No such file or directory
```

`echo *.zip` answers `*.zip`. There are no `.zip` files: `ls` looked for one that was literally called `*.zip`.

**With `-i`, the pattern asks one by one.** Type it yourself: `rm -i *.bak`. Answer `y` to the first and `n` to the
second:

```text
rm: remove regular empty file 'a.bak'? y
rm: remove regular empty file 'b.bak'? n
```

With three hundred files, the habit above is worth more than `-i`.

### Your turn, without AI: the cleanup

This exercise is done **without AI and without copying commands**: neither from this lesson nor from anywhere else. You
go to `~/practica/02/limpieza` and do what they asked Lucía to do:

1. **Delete the files whose name ends in `.tmp`, and nothing else.** First write a pattern with `ls` and read what it
   catches. Watch out for the neighbors: `cache-indice.txt` and `tmp-notas.txt` stay.
2. **Write down your pattern** with `anota`, **in single quotes**. For a pattern `ejemplo*` («example*») it would be
   `anota 02 patron 'ejemplo*'`. (`patron` is the key of your answer sheet: «pattern».) Without quotes, the terminal
   would replace your pattern with the names it catches before `anota` saw it.
3. **Delete the `.tmp` files** with an `rm` that you type yourself, with the same pattern.
4. **Delete the folder `sesiones-viejas`**, with everything in it, **and the folder `vacia`**. Start with `rmdir` on
   both and read what each one answers: it tells you which one needs something else. `sesiones-activas` stays.
5. Run `verifica 02`.

If you delete something extra **in this workshop**, nothing happens: `prepara 02` restores it. Use that to practice the
pause of looking before you delete.

**What it shows** (objective 2.3, first part): marks six to eight of `verifica 02`. Mark six tests your pattern against
the eight names that `prepara` put in `limpieza`. Mark seven checks that what was asked for no longer exists. Mark
eight, that **everything else is still there, with its contents**. Those two matter equally: deleting too little can be
fixed; deleting too much cannot.

---

## Step 5 · The empty variable, seen only with `echo`

⛔ **In this step no `rm` is run.** Everything is seen with `echo` in front, which only writes the command on the
screen. If you type one of these lines without the `echo`, you are not doing the exercise.

### The deletion that grows by itself

Maintenance scripts keep the folder they clean in a **variable**, `CARPETA` («folder»; the minimum, in the box at the
end of this step), and they delete with a line like this: `rm -rf "$CARPETA/"`. `-f`, from *force*, tells `rm` not to
ask and not to complain if something does not exist.

**Predict:** if `CARPETA` is empty, what does that line ask to delete? Type this, with the `echo`:

```bash
CARPETA=
echo rm -rf "$CARPETA/"
```

```text
rm -rf /
```

The empty variable vanished, and the slash that came after it was left alone. **`/` is the root: the whole system.** A
script that was supposed to delete a practice folder asks to delete everything. If there is a `*` at the end of the
line, it is worse:

```bash
echo rm -rf "$CARPETA"/*
```

```text
rm -rf /bin /boot /dev /etc /home /lib /media /mnt /opt /proc /root /run /sbin /srv /sys /tmp /usr /var
```

Every folder in the root. According to its manual, GNU's `rm` refuses to delete `/` as such (`--preserve-root`); with
the `*` it no longer receives `/`, but this list. We did not test it and you are not going to either.

**A reported case.** In January 2015, the public issue #3671 of Steam for Linux described a loss of files after running
`steam.sh`. Comments in the thread pointed to `rm -rf "$STEAMROOT/"*` and to the risk if `STEAMROOT` was left empty. It
is a reason to validate variables; you do not need to test the incident to learn the rule.

### Why it is not tested, not even in the container

**The container is disposable. Your `~/practica` folder is not:** it lives in a volume that outlives the container. See
who owns what:

```bash
ls -ld / /usr /etc ~/practica
```

```text
drwxr-xr-x 1 root   root   4096 Nov 19 09:28 /
drwxr-xr-x 1 root   root   4096 Nov 19 09:28 /etc
drwxr-xr-x 4 alumno alumno 4096 Nov 19 09:28 /home/alumno/practica
drwxr-xr-x 1 root   root   4096 Nov  6 10:27 /usr
```

The system folders belong to `root`, the administrator: `touch /usr/prueba` answers `Permission denied`. But
`~/practica` is yours, and what is yours you can delete. Your answers do not come back with `./practicar`.

### The brake: `${CARPETA:?}`

Scripts protect themselves with a brake that has a name: `${CARPETA:?}`. If the variable is empty, the terminal refuses
to run the line. With `echo` in front, as always in this step:

```bash
CARPETA=
echo rm -rf "${CARPETA:?}/"
```

```text
bash: CARPETA: parameter null or not set
```

«The variable is empty or does not exist.» And the `echo` **did not run**: according to the bash manual, the terminal
cancels the command, and with an `rm` it would not run either. For you this is enough: **if an `rm` carries a variable,
the variable goes as `${NAME:?}`**.

> **The minimum about a variable.** `CARPETA=ensayo` stores `ensayo` under the name `CARPETA`: all stuck together, with
> no spaces. With a `$` in front, `$CARPETA`, the terminal replaces the name with its value before running the command,
> just as it replaces a pattern with the list. A variable that is empty, or that never stored anything, is worth
> nothing, **with no error**. This course goes no further than that.

**What it shows** (objective 2.3, second part): your explanation of questions 9 and 10 of «To wrap up», compared with
«Compare your answers».

---

## Step 6 · Check with `verifica 02`

Run `verifica 02` whenever you want, from any folder. Right after the workshop is set up, seven marks come out in red
and one in green: the last one, because you have not yet deleted anything that was supposed to stay.

This is how a red on the pattern mark is read, with the `ejemplo*` used above as a sample:

```text
❌ 2.3 · Tu patrón «ejemplo*» deja fuera «cache-01.tmp», que sí se borra (medí: probé tu patrón contra los 8 nombres que prepara puso en limpieza)
```

*Your pattern «ejemplo*» leaves out «cache-01.tmp», which does get deleted (I measured: I tested your pattern against
the 8 names that prepara put in limpieza).*

It says what you wrote down, what was measured and what failed, but not which pattern is the right one.

**What it cannot be fooled by:** a link, the shortcut from lesson 01. A link to `config/pagos.conf` «says» the same as
a copy, but it is not a backup: if the original is lost, the link has nothing. `verifica 02` checks the real path of
each thing, and a link in place of a file or of a folder comes out in red. Nor is it fooled by a **hard link** (`ln`): a
second name for the same file, which looks like a normal file. If you change one, the other changes. `verifica` notices
because the two names share the *inode*, the number by which the disk knows the file.

Correct and repeat until you see this at the end:

```text
✅ Lección 02: todo en verde.
```

*Lesson 02: everything in green.*

---

## Your exercises

Exercise 2.3 is done **without AI**, and you type all of its delete commands yourself.

| # | You do | It is done when |
|---|---|---|
| 2.1 | You create the on-call tree from step 2 | `verifica 02` marks the tree in green |
| 2.2 | The four tasks of step 3, with your predictions A, B and C written beforehand | `verifica 02` marks the four in green, and you compared your predictions with «Compare your answers» |
| 2.3 | The cleanup of step 4: you look with your pattern, write it down and delete by hand | `verifica 02` comes out all in green, and you answered questions 9 and 10 of «To wrap up» |

**Time (estimated):** 60 to 90 minutes. It is an estimate: it will be adjusted with the first group that takes the
course.

---

## If you want more

*Optional. Nothing in this box is assessed or needed for the exercises.*

- **`-v` tells you what it did.** With `cp -v`, `mv -v`, `rm -v` and `mkdir -v`, each command writes one line for each
  thing it touched. `rm -v` answered, for example, `removed 'viaje/vuelta/dos.txt'`.
- **Braces for several folders.** `mkdir -p viaje/{ida,vuelta}` creates `viaje/ida` and `viaje/vuelta`. (`viaje` is
  «trip», `ida` «outbound» and `vuelta` «return».) The braces are expanded by bash, just like the `*`: check it with
  `echo viaje/{ida,vuelta}`.
- **With Claude Code: `mv -n`.** You predict on your paper what `mv -n` does. You ask Claude Code, on your computer:
  «What does `mv -n` do and how does it differ from `mv -i`?». You read `-n` in `mv --help` and compare. You run, in the
  rehearsal folder: `touch a.txt b.txt`, `mv -n a.txt b.txt` and `ls`. You explain in your own words whether it matched.
  What happened in the test is in «Compare your answers».
- **`rm -I` asks only once.** With more than three files, or with `-r`, it asks before it starts: `rm -I x*` on four
  files answered `rm: remove 4 arguments?`. With `n` it deleted none.

---

## Not running?

| You see | Probable cause | What you do |
|---|---|---|
| `bash: prepara: …` or `Todavía no hay prepara para la lección «02».` *(There is no prepara for lesson «02» yet.)* | Your image is from before this lesson | Get out with `exit` and run `./practicar reconstruir` |
| `Ya existe /home/alumno/practica/02.` and a question *(It already exists: /home/alumno/practica/02.)* | You had already set up the workshop, and setting it up again deletes it | To start from zero, type `si` («yes»). If not, press Enter: nothing is deleted |
| `mkdir: No such file or directory` | A folder in the middle is missing, and this `mkdir` does not say which | Use `mkdir -p`, or check each stretch of the path with `ls` |
| `rmdir: failed to remove …: Directory not empty` | The folder has something inside | See what it has with `ls -a` before you decide |
| `❌ … son nombres, no un patrón` *(… they are names, not a pattern)* | You wrote the pattern down without quotes, and the terminal replaced it with the names | `anota 02 patron` and your pattern in single quotes |
| `❌ … Copiaste dos veces` *(… You copied twice)* | You ran the same `cp -r` twice | Delete `respaldo/plantillas` with care and copy it once, or `prepara 02` |
| `❌ … ya no está, y no se pedía borrarlo` *(… is no longer there, and deleting it was not asked for)* | Your pattern or your `rm -r` took too much | In this workshop nothing happens: `prepara 02` restores it, and you repeat |
| `❌ … es un enlace…` *(… it is a link…)* | There is a shortcut where a real file or folder should be | Do it with `cp` or `mv`, or `prepara 02` |
| `❌ … es un enlace duro…` *(… it is a hard link…)* | That file is the same as the original, with another name | Delete it with care and copy it with `cp` |

---

## To wrap up

**Answer without reading again.**

**Five situations: what happens?** You pass with 4 out of 5.

1. `cp a.txt b.txt`, and `b.txt` already existed with another text.
2. `mv a.txt docs`, and `docs` is a folder that exists.
3. `mkdir x/y`, and `x` does not exist.
4. `rmdir fotos`, and `fotos` has twelve photos inside.
5. `touch notas.txt`, and `notas.txt` already had three pages of notes.

**And five questions:**

6. What is the difference between `cp` and `mv` in what happens to the source?
7. You are going to delete with `rm *.log`. What do you run first, and what do you look for in what it answers?
8. You typed `anota 02 patron *.tmp`, without quotes, standing in `limpieza`. What got written down, and why?
9. Explain in your own words, in three lines, why `rm -rf "$CARPETA/"` is dangerous if `CARPETA` is empty.
10. How do you see, without running anything dangerous, what that line would delete? And how do you protect it?

**What comes next:** you now know how to create, copy, move and delete without losing what you did not want to lose. In
lesson 03 you open what is inside the files: you read long logs without opening an editor.

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

**Prediction A (copying over the backup).** It does not ask. At the end `respaldo/pagos.conf` says
`puerto=8443 version=2025-06-02 (la buena, la de hoy)` («the good one, today's»), the same as `config/pagos.conf`. The
version with port 8080 was lost: here that was what you wanted, but with another file it could be the only one you did
not want to lose.

**Prediction B (`cp -r` twice).** The first time it creates `respaldo/plantillas` with its three files. The second
time, since `respaldo/plantillas` already exists, it puts another copy inside: you end up with
`respaldo/plantillas/plantillas`. This is how `ls` answered in the test:

```text
'aviso de corte.txt'   aviso.txt   plantillas   recibo.txt
```

The quotes are not part of the name: `ls` adds them because the name has spaces.

**With Claude Code: `mv -n`.** In the test, `mv -n a.txt b.txt` moved nothing and said nothing: `ls` kept showing
`a.txt  b.txt`. `-n` is «never overwrite», silently; `-i` asks. If Claude Code told you something else, what you saw
in your terminal wins.

**Prediction C (moving over the draft).** `mv` does not ask: the old draft in `reportes` is overwritten and lost, and in
`reportes/reporte-junio.txt` the new version remains. The file disappears from `entrada`, but the `entrada` folder
stays, empty: `mv` moves what you ask for and nothing else.

**Exercise 2.3.** The check is `verifica 02`; the command is not given here. To know whether you are on track:

- Your pattern catches the three `.tmp` files and no neighbor. `cache*` takes `cache-indice.txt`; `*tmp*` takes
  `tmp-notas.txt`. The right one looks at **how the name ends**.
- `rmdir` deleted `vacia` and refused with `sesiones-viejas`. That one needed `rm -r`, and you looked at it with `ls`
  first.

**To wrap up.**

1. `b.txt` ends up with the text of `a.txt`, without asking. What it had before is lost.
2. `a.txt` ends up inside the folder: `docs/a.txt`. It is no longer where it was.
3. It fails with `No such file or directory`, and in this image without saying which path. With `mkdir -p x/y` both are
   created.
4. It refuses: `Directory not empty`. `rmdir` only deletes empty folders, and nothing is lost.
5. The notes are still there. `touch` only changes its date to the one of right now.
6. With `cp` the source stays and a copy appears. With `mv` the source disappears: the file changes place or name.
7. `ls *.log`, with the same pattern. You look for it to bring only what you want to delete; if it brings extra, you
   change the pattern.
8. The names of the three `.tmp` files. Bash replaced `*.tmp` with the list before running `anota`, just as with `rm`
   and with `ls`. `verifica` detects it and asks you for the quotes.
9. Something like this: if `CARPETA` is empty, the terminal replaces it with nothing and the line becomes `rm -rf /`,
   which asks to delete the root, the whole system. `-r` goes into every folder and `-f` does not ask. With a `*` at the
   end, it receives the folders of the root one by one, and `--preserve-root` no longer stops it.
10. You see it with `echo` in front: `echo rm -rf "$CARPETA/"` writes the command without running it. You protect it
    with `"${CARPETA:?}/"`: if the variable is empty, bash cancels the command before running it.

---

## Lesson sources

Opened on Nov 19, 2026. All of them are in English.

- Ubuntu Manpages. *rm(1)*, Ubuntu 26.04, GNU coreutils 9.7 (without `-r` it does not delete folders; `-i`, `-I`, `-r`,
  `-f`; `--preserve-root` by default; the question for files that cannot be written to).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/rm.1.html
- Ubuntu Manpages. *cp(1)*, Ubuntu 26.04, GNU coreutils 9.7 (`-i`, `-r`).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/cp.1.html
- Ubuntu Manpages. *mv(1)*, Ubuntu 26.04, GNU coreutils 9.7 (renames or moves into a folder; `-i`, `-n`).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/mv.1.html
- Ubuntu Manpages. *mkdir(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (`-p`).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/mkdir.1.html
- Ubuntu Manpages. *touch(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (changes the date; creates the file if it does not
  exist). https://manpages.ubuntu.com/manpages/resolute/en/man1/touch.1.html
- Ubuntu Manpages. *rmdir(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (only empty folders).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/rmdir.1.html
- Ubuntu Manpages. *bash(1)*, Ubuntu 26.04, bash 5.3, the bash manual: `${parameter:?word}` (with the variable empty, an
  interactive terminal does not run the command), *Pathname Expansion* (bash replaces the pattern with the list of
  names, or leaves it as it is if it catches nothing). https://manpages.ubuntu.com/manpages/resolute/en/man1/bash.1.html
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, section «rust-coreutils» (`cp`, `mv` and `rm` are still GNU's).
  https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- GitHub, repository ValveSoftware/steam-for-linux, issue 3671, opened on January 14, 2015: a user's report and the
  thread comments, which point to `rm -rf "$STEAMROOT/"*`. https://github.com/ValveSoftware/steam-for-linux/issues/3671
- Ubuntu Manpages. *ln(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (creates hard links by default; with `-s`, symbolic
  ones). https://manpages.ubuntu.com/manpages/resolute/man1/ln.1.html
- Our own measurement, Nov 19, 2026, on a Mac with Apple silicon (Docker 29.2.1 with Colima, linux/arm64), with the
  course image on Ubuntu 26.04.1: `mkdir`, `touch` and `rmdir` from uutils coreutils 0.10.0, `cp`, `mv` and `rm` from
  GNU coreutils 9.7, and bash 5.3.9. The screens, the `mkdir` table and the time of `./practicar reconstruir` come from
  there.
