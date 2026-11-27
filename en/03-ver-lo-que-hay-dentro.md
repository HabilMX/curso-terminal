# Lesson 03 — Seeing what is inside

**By Dorian Chávez, founder of Hábil and integration architect.**

**Before you read, five words.** One line each; the rest come in the step where they are used.

- **Log:** the file where a program keeps writing down what it does, one line per thing and with its time. In the
  course's Spanish names and messages it is called *bitácora*; on servers you will also see it simply as *log*.
- **Line:** one line of a file. It ends with a line break, the Enter you cannot see. When a command counts lines, it
  counts those breaks.
- **Text and binary:** a text file has letters, numbers and signs that you can read. A binary one has bytes meant for a
  program, not for your eyes: a photo, a program, a compressed file.
- **Pager:** a program that shows you a text one screen at a time and lets you go back and forth through it. The one in
  this lesson is called `less`.
- **Compressed:** a file that a program, such as `gzip`, made smaller. To read it you have to decompress it.

**What you take away:** you show a whole file with `cat`, also with the number of each line; you pull out the
beginning, the end or one exact line with `head` and `tail`; you count lines with `wc -l`; you browse and search in
`less` and get out without getting lost; and before you open a file you do not know, you ask `file` what it is.

**This lesson's new idea (the only one that is assessed):** on a server almost everything is text, and you read it
without opening an editor. Before reading you **decide which piece you need**: all of it, the beginning, the end, one
line, how many there are, or browsing while you look for something. The question is not which commands exist, but which
piece each situation calls for.

**In what order it goes:** you set up an on-call room (step 1), you read whole files (step 2), you pull out pieces and
count (step 3), you browse and search (step 4), you ask what a file is before opening it (step 5) and at the end you
check everything with `verifica` (step 6).

**A note on the language of the on-call room.** As in lessons 00, 01 and 02, the course's messages and the room's names
are in Spanish and are part of the code, so they are not translated. The messages appear exactly as the terminal prints
them, with a gloss in English, in italics, below each block or in parentheses inside a table. The names of the room are
glossed once, in step 1: `ensayo` («rehearsal»), `corto` («short»), `cien` («one hundred»), `pagos` («payments»),
`arranque` («startup»), `reportes` («reports»), `sucursales` («branches») and the rest. `renglón` is «line». The seven
keys of the answer sheet are glossed in step 1 too, and `espera_segundos` («wait_seconds») in step 2. The commands
`practicar`, `prepara`, `verifica` and `anota` keep their names: you already know them from the earlier lessons.

---

## The scene

*An invented scenario. The screens in the lesson were copied from the practice terminal.*

Thursday, 3:12 a.m. Lucía's phone buzzes on the table.

«The branches are rejecting every card. It started a while ago, we do not know when. What does the payments log say?»

Lucía already knows how to get to the folder without seeing it, and that nothing gets deleted there without looking
first. What she has never done is **read** a file on a machine with no windows. There is no double-click. There is the
same line as always, waiting.

The log has hundreds of lines, and on a real server it could have millions. Somewhere in that file is the answer: the
line where it all began, and the last one, the most recent thing that got written. Next to it there is another one,
yesterday's, with a name that ends in `.gz` and that does not let itself be read like the others. On the other end,
someone is waiting for a time, a number and a sentence to decide what to do about hundreds of charges.

She has something in her favor: logs are usually text. *The Twelve-Factor App*, Adam Wiggins's guide to building
applications that run on servers, describes it like this: in its raw form, many logs are text, in time order, and
usually carry one event per line; a common exception is an error trace, which can take up several. If it is text, you
read it with the commands in this lesson.

This lesson is that half hour, in an on-call room made for reading with no consequences.

---

## Before you start

**What you need:** lesson 02 done; the course material up to date; and pen and paper for your predictions.

**Update your practice terminal.** As in the earlier lessons, from your computer, in the course folder:

```bash
git pull
./practicar reconstruir
```

This time the image adds three new packages: `file`, `gzip` and `ncurses-bin`. The rebuild from scratch was not
measured in this version: depending on Docker and on what you already have stored on your machine, it may need network
access and take longer than in 02. It must end with `✅ Imagen lista.` («Image ready.») Your `~/practica` folder is not
touched. After that, get in with `./practicar`.

**Where this lesson was tested:**

| System | Status |
|---|---|
| Linux | Tested on Nov 25, 2026 on the `leccion/03` branch (3bf1787), measured on a Linux server: x86_64 host, Docker 29.10.0; the image was built in 9 s and weighs 232,656,309 bytes; Ubuntu 26.04, `cat` (uutils coreutils) 0.10.0, `less` 668 and `file-5.46`. `prepara 03` ran, and so did every command of steps 1 to 6, and `verifica 03` in red and in green, back when the sheet had eight keys. `verifica 03` with today's seven keys was measured again the same day (branch at 1a3a7b5): red with 7 ❌, green with 7 ✅, green also after `simula 03`, and `anota 03 arranque` refuses. Not measured: `./practicar reconstruir` (the image was built with `docker build`) and `prepara 03` on a room that is already set up |
| Mac with Apple silicon, Ubuntu, Windows with Docker Desktop and WSL 2 | Not tested yet with this lesson |

**What will match your screen and what will not.** The names, the contents and the dates of the room match, and so do
the messages of the commands. The arrangement in columns, the colors and the size of your window change, and the size of
the window decides how many lines fit on the screen.

---

## Step 1 · Set up the on-call room

Inside your practice terminal:

```bash
prepara 03
```

```text
✅ Sala de guardia lista en /home/alumno/practica/03 (medí: 3 carpetas y 8 archivos, cada uno con
   su contenido; las dos bitácoras, por su huella).
   Te dejé la hoja de respuestas, con sus claves vacías: /home/alumno/practica/respuestas/03.md
   Empieza por ahí:  cd ~/practica/03
```

*On-call room ready in /home/alumno/practica/03 (I measured: 3 folders and 8 files, each with its contents; the two
logs, by their fingerprint). I left you the answer sheet, with its keys empty: /home/alumno/practica/respuestas/03.md.
Start there: `cd ~/practica/03`.*

`prepara 03` works like the one in 02: it has no randomness and it touches only `~/practica/03` and your answer sheet.
If the room already exists, `prepara 03` will explain to you before replacing it; that case was not measured in this
version. The logs are not written line by line: they come from a fixed rule, and `prepara` checks at the end that each
one has its *fingerprint*, a kind of signature of the contents. If a single character changes, the fingerprint no
longer matches.

**The room, seen from above:**

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

*The arrows say: «to try the commands out on», «three lines», «a hundred numbered lines», «the service's configuration»,
«today's log, the long one», «yesterday's log, compressed», «the one from the service's restart» and «the branches that
reported rejections». The names, in Spanish: `LEEME.txt` («README.txt»), `ensayo` («rehearsal»), `corto.txt`
(«short.txt»), `cien.txt` («hundred.txt»), `pagos` («payments»), `pagos.conf` («payments.conf»), `pagos.log`
(«payments.log»), `pagos.log.1.gz` («payments.log.1.gz»), `arranque.log` («startup.log»), `reportes` («reports») and
`sucursales.csv` («branches.csv»).*

The demonstrations go in `ensayo`; the exercises, in `pagos` and `reportes`. **In this room nothing is edited or
deleted:** `verifica 03` checks that everything is still as `prepara` left it, and if something changed it tells you.

Your answer sheet has seven keys, one for each thing you are going to read. It is filled in the same way as in the
earlier lessons, with `anota`. The keys, in Spanish: `espera` («wait»), `folio-137` («ticket-137»), `ultima-hora`
(«last-hour»), `renglones` («lines»), `sucursales` («branches»), `primer-error` («first-error») and `tipo-respaldo`
(«backup-type»).

**What it shows:** nothing yet. This step leaves the material ready.

---

## Step 2 · All in one go: `cat`

Go into the rehearsal folder:

```bash
cd ~/practica/03/ensayo
```

`cat` comes from *concatenate*, «to chain together»: its manual says it joins one or more files and writes them on the
screen. With a single file, it writes it whole:

```bash
cat corto.txt
```

```text
primer renglón
segundo renglón
tercer renglón, el último
```

*First line / second line / third line, the last one.*

In lesson 00, with no file, it stayed waiting. With a file it lets everything out and gives you the prompt back.

**With two files, it chains them.** **Predict:** what comes out of `cat corto.txt corto.txt`?

```text
primer renglón
segundo renglón
tercer renglón, el último
primer renglón
segundo renglón
tercer renglón, el último
```

One right after the other, with nothing in between. That is why it has that name.

### `cat -n`: with the number of each line

The `-n` option, from *number*, puts its number on each line:

```bash
cat -n corto.txt
```

```text
     1	primer renglón
     2	segundo renglón
     3	tercer renglón, el último
```

The number **is not part of the file**: `cat` adds it when it shows it. It is useful for answering «which line is such a
thing on?», which they are going to ask you on call.

### What does not fit on the screen

**Predict before you run it:** `cien.txt` has a hundred lines, and your window has fewer. What are you going to see with
`cat cien.txt`?

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

*«renglón 078 de 100» is «line 078 of 100».*

You see the end, up to `renglón 100 de 100`. The beginning went off the screen: `cat` knows nothing about screens, it
writes everything and what does not fit goes up. With a hundred lines you can scroll up with the mouse wheel; with a
million, you cannot.

**This is where this lesson's rule comes from:** `cat` is for short files, the ones you know will fit. For long ones you
ask for a piece (step 3) or you browse them (step 4). And to read a log on a server, **choose a reading command**: it
does not modify the file and it starts better with long logs. An editor can change the file if you press one key too
many, and a huge one can take a long time to open.

### Your turn: the configuration

Go back to the room with `cd ~/practica/03`. The payments service's configuration is short: `cat` is the right tool.

1. Show it whole, **with line numbers**.
2. The lines that start with `#` are comments, notes for people: the service does not read them. Notice which ones they
   are.
3. Find the line that says how many seconds the service waits between one retry and the next: its name carries the
   word `espera_segundos` («wait_seconds»). Write down **the number of that line**:

   ```bash
   anota 03 espera 1
   ```

   There, the `1` is an example: in its place goes the number you read.

**What it shows** (objective 3.1): the `espera` («wait») mark of `verifica 03`.

---

## Step 3 · Just a piece: `head`, `tail` and `wc`

Go back to the rehearsal folder with `cd ~/practica/03/ensayo`.

### `head`: the beginning

`head` means «head». According to its manual, if you do not tell it how many, it writes the first ten lines.
**Predict:** which is the last one you are going to see with `head cien.txt`?

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

From `renglón 001` to `renglón 010`. Ten, as the manual says. With `-n` and a number you tell it how many:

```bash
head -n 3 cien.txt
```

```text
renglón 001 de 100
renglón 002 de 100
renglón 003 de 100
```

### `tail`: the end

`tail` means «tail». It works like `head`, but from the end: ten lines if you do not tell it otherwise, and as many as
you ask for with `-n`.

```bash
tail -n 2 cien.txt
```

```text
renglón 099 de 100
renglón 100 de 100
```

In a log that is in order, the end is the most recent thing, and **`tail` is usually the first look on call**: it tells
you the last thing that *got recorded*. Read it with its time and use it as a clue; on a real call, you then confirm
the state with the signal that fits the service.

### One exact line, with what you already have

They ask you for line 42. **Predict:** which is the last line that `head -n 42` writes?

```bash
head -n 42 cien.txt
```

Trimmed output: the last three of the 42 lines.

```text
renglón 040 de 100
renglón 041 de 100
renglón 042 de 100
```

The last one that appears is `renglón 042 de 100`. `head -n 42` writes lines 1 to 42, so the one at the very bottom,
right above the prompt, **is number 42**. You do not need to count anything: you read the last one. In lesson 06 you
will see how to keep only that one, without the 41 above it. For now, this trick is enough.

### `wc`: counting without reading

`wc` comes from *word count*, although it counts three things. With no options it writes three numbers and the name of
the file:

```bash
wc cien.txt
```

```text
 100  400 2000 cien.txt
```

**Predict before you read the explanation:** `cien.txt` has a hundred lines, and each one says something like
`renglón 001 de 100`. How many words do you think it has in total?

The three numbers, in order, are: lines, words and bytes. For `cien.txt` they should be 100, 400 and 2000. Each line
has four words, because for `wc` a word is any group of characters without spaces, according to its manual: `001` and
`100` count as words. And the bytes are the size, the same number you saw in `ls -l` in lesson 01.

In this lesson one is enough for you: the lines. They are asked for with `-l`, from *lines*:

```bash
wc -l cien.txt
```

```text
100 cien.txt
```

**How it counts lines.** The `wc` manual says that `-l` counts **line breaks**. If the last line of a file does not end
with its break, `wc -l` does not count it. In this room every file ends with its break, but on a server not always: if
`wc -l` gives you one less than what you see, that is the cause.

**Your command, not the manual's.** Here `cat`, `head`, `tail` and `wc` come from `rust-coreutils`, like `ls` in lesson
01; according to the Ubuntu documentation, only `cp`, `mv` and `rm` are still GNU's. The forms you see in this lesson
were tested; the GNU manuals it cites explain the idea. If you try another option, look first at `command --help` in
**that** container.

### Your turn: the log and the report

Go back to the room with `cd ~/practica/03`. There are four questions. **Before each one, write on your paper which
piece you need and with which command you are going to pull it out**; then you run it and write it down.

1. **The ticket number on line 137** of `pagos/pagos.log`. Each charge carries a ticket number: an F, a hyphen and six
   digits. Write it down with `anota 03 folio-137` and the ticket number, exactly as it is.
2. **The time of the last line** of `pagos/pagos.log`: only the time, `hh:mm:ss`, without the date. It is written down
   with `anota 03 ultima-hora`. Read the whole line: it is what you are going to tell the person on the phone.
3. **How many lines** `pagos/pagos.log` has. Only the number: `anota 03 renglones`.
4. **How many branches** `reportes/sucursales.csv` has. It is a CSV: a table written as text, one line per row and
   commas between columns. First look at its first three lines with `head`. **Prediction D:** is the number of branches
   the same as the number of lines? Write down your answer with `anota 03 sucursales`.

**What it shows** (objective 3.2): the marks `folio-137`, `ultima-hora`, `renglones` and `sucursales` of `verifica 03`,
and your prediction D, which you compare yourself at the end.

---

## Step 4 · Browse and search: `less`

`head` and `tail` work when you know where what you are looking for is. When you do not, you browse with `less`, a
*pager*: according to its author, a program that shows text files and **not an editor**. `less file` is for looking and
does not change that file. Stick to the keys in the table below; stay away from `v` and `!`, which can open other
programs.

Its manual says something else that matters on a server: `less` does not need to read the whole file before it starts,
so with big files it starts faster than an editor.

### You already know the keys

Go back to the rehearsal folder and open `cien.txt` with `-N`, in uppercase: each line appears with its number, as in
`cat -n`, but browsing.

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

The screen fills with the beginning of the file, and the number of each line is on the left: you no longer count. You
are **inside** `less`, just as you were inside `man` in lesson 00, and the keys are the same ones you used there, plus a
few more:

| Key | What it does |
|---|---|
| Space bar | Down one screen |
| `g` | To the beginning of the file |
| `G` | To the end of the file |
| `/` and some text, then Enter | Searches that text downward |
| `n` | The next time it appears |
| `h` | The help of `less`, with all the keys |
| `q` | Quits and returns you to the prompt |

Try each one: a few screens with the space bar, then `G`, then `g`. Quit with `q`.

**Ctrl+C does not get you out.** In `less`, Ctrl+C stops whatever it was doing, for example a long search, and leaves
it waiting for your next key; it does not close it, according to its author's FAQ. As in `man`: you leave `less` with
`q`.

### Searching tells uppercase from lowercase

Inside `less -N cien.txt`, type `/042` and Enter. It jumps to line 42. Type `n`: there is no other one.

**Predict:** what happens if you search for `/RENGLÓN`, in uppercase?

At the bottom of the screen `Pattern not found  (press RETURN)` appears: it does not find it, even though the word is
in all hundred lines. The `less` manual says so: **by default, the search distinguishes uppercase from lowercase**. In
a log that matters: `ERROR` and `error` do not have to be the same thing. Press Enter and quit with `q`.

### Your turn: the first `ERROR`

What you need is in the table above and in the help of `less`, with `h`. Try it first with that; if you get stuck,
explain what you looked for and which screen you saw before asking for a hint.

1. Go back to the room, `cd ~/practica/03`, and open `pagos/pagos.log` in `less`, **with line numbers**.
2. Before searching, browse: a few screens with the space bar, the end with `G`, the beginning with `g`. See what a
   normal line looks like and what one that warns of something looks like.
3. **Prediction E:** the log has `INFO`, `WARN` and `ERROR` lines. Do you expect the first `ERROR` to be near the
   beginning or near the end? Write it down.
4. From the beginning of the file, search for the **first line that says `ERROR`, in uppercase**. Read that whole line
   and the ones after it: it says why the rejections started.
5. Quit with `q` and write down the **number** of that line with `anota 03 primer-error`.

**What it shows** (objective 3.3): the `primer-error` («first-error») mark of `verifica 03`, and your prediction E.

---

## Step 5 · Before you open it, ask what it is: `file`

Yesterday's log is left, `pagos/pagos.log.1.gz`. Its name carries `.log`, plus a `.1` and a `.gz`. Is it text? So you
do not have to guess there is `file`: it looks at the **contents** of a file, not at its name, and says what type of
file it is. According to its manual, it looks near the beginning for the fixed marks of each known format, and if it
does not fit any of them, it checks whether it is text.

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

`../pagos/*` is a pattern, like the ones in lesson 02: the terminal replaces it with all the names in `pagos`. They all
say *text*, except one. **The name does not decide:** `file` recognizes yesterday's log by its contents, and says it is
gzip.

`file` is a first signal, not a guarantee: its manual says that what it marks as *text* **probably** can be read
without trouble in the terminal. If it says *text*, you can normally read it; if not, do not use `cat` until you know
what format it is.

### Why you do not `cat` a binary

`cat` does not ask what a file is: it writes its bytes on the screen, just as they are. With a binary they are bytes
that are not letters, and among them there may be some that the terminal understands as commands. **Do not run `cat`
directly on this `.gz`, nor on any binary, in a production terminal.**

If the terminal ever ends up **scrambled** (what you type comes out as strange symbols, or is not visible, or Enter
stops moving down a line), the file did not break and neither did the machine. **Type `reset` and press Enter**, even
if you cannot see what you type. According to its manual, `reset` returns the terminal to its normal form after a
program leaves it in an abnormal state, and it warns that sometimes Enter does not respond: in that case, use
**Ctrl+J**.

Now, a safe sample. The bar `|` passes what one command writes on to the next one; you will see it in depth in 
lesson 06. `head -c 300` pulls out only the first 300 bytes of the file and the bar hands them to `cat -v`, which changes every
byte that is not a letter into signs that can be written. **Predict:** what are you going to see?

```bash
head -c 300 ../pagos/pagos.log.1.gz | cat -v
```

Trimmed output: the beginning of one very long line.

```text
^_M-^K^H^@^@^@^@^@^B^CM-^LM-^]]M-.^^7vEM-_3M-^J;^A^WM-HM-sM-Csh
```

None of that is letters from a log: they are the compressed bytes, translated into signs. Run `reset` once, to get to
know it:

```bash
reset
```

You will not see any reply text: `reset` sends control commands to the terminal and may clear the screen; then the
prompt comes back.

### Your turn: what yesterday's log is

With `file`, ask what `pagos/pagos.log.1.gz` is and write down **the first word** of what it answers, after the colon,
with `anota 03 tipo-respaldo`.

**What it shows** (objective 3.4): the `tipo-respaldo` («backup-type») mark of `verifica 03`, and your answers to
questions 8 and 9 of «To wrap up».

---

## Step 6 · Check with `verifica 03`

Run `verifica 03` whenever you want, from any folder. First it checks that the room is still intact, file by file; if
something changed, it stops and asks you to set it up again. Then it checks your seven answers **by reading the
files**: the good answer is not written in `verifica`, it comes out of the file itself.

Right after the room is set up, the first mark comes out in green and the seven answers in red, because they are empty.
This is how a red is read when the answer is wrong and not empty; for example, if you wrote down for `ultima-hora` the
time of the **first** line. Trimmed output: that red and the final line.

```text
❌ ultima-hora · 00:00:04 es la hora del PRIMER renglón (medí: la hora del renglón 1). head muestra el principio; el final lo muestra otro comando

❌ Lección 03: falta lo marcado con ❌. Arréglalo y corre  verifica 03  otra vez.
```

*❌ ultima-hora · 00:00:04 is the time of the FIRST line (I measured: the time of line 1). head shows the beginning;
another command shows the end.*

*❌ Lesson 03: something marked with ❌ is missing. Fix it and run verifica 03 again.*

It says what you wrote down, what it measured and where you got mixed up, but not what the good answer is.

Correct and repeat until you see this at the end:

```text
✅ Lección 03: todo en verde.
```

*Lesson 03: everything in green.*

---

## Your exercises

Exercise 3.3 is tried first with the `less` table and its help, with `h`; if you get stuck, explain what you looked for
and which screen you saw before asking for a hint.

| # | You do | It is done when |
|---|---|---|
| 3.1 | You read the whole configuration with line numbers and find `espera_segundos` | `verifica 03` marks `espera` in green |
| 3.2 | You pull out line 137, the last line and the total of the log, and you count the CSV's branches, with your predictions written beforehand | `verifica 03` marks the four in green, and you compared your prediction D |
| 3.3 | You browse the log in `less` and find the first `ERROR` | `verifica 03` marks `primer-error` in green, and you compared your prediction E |
| 3.4 | You ask `file` what yesterday's log is, you look at a safe sample with `head -c 300` and `cat -v`, and you use `reset` | `verifica 03` comes out all in green, and you answered questions 8 and 9 of «To wrap up» |

**Time (estimated):** 60 to 90 minutes. It is an estimate: it will be adjusted with the first group that takes the
course.

---

## Not running?

| You see | Probable cause | What you do |
|---|---|---|
| `Todavía no hay prepara para la lección «03».` *(There is no prepara for lesson «03» yet.)* | Your image is from before this lesson | Get out with `exit` and run `./practicar reconstruir` |
| `Ya existe /home/alumno/practica/03.` *(It already exists: /home/alumno/practica/03.)* and it asks you to type `si` | You had already set up the room, and setting it up again deletes it entirely, with no undo; your answer sheet stays | To start from zero, type `si` («yes») and Enter. With anything else it answers `No se borró nada.` *(Nothing was deleted.)* |
| The `less` screen does not go away and the prompt does not come back | You are still inside `less` | `q`. Ctrl+C does not close it |
| What you type comes out as strange symbols, or is not visible | You ran `cat` on a file that is not text | Type `reset` and Enter, even if you cannot see it. If Enter does not respond, use Ctrl+J |
| `❌ La sala no está intacta` *(The room is not intact)* | A file in the room changed | `prepara 03`. Your answer sheet is not touched |

---

## To wrap up

**Answer without reading again.**

**Four situations: which piece do you need, and with which command do you pull it out?** You pass with 3 out of 4.

1. They ask you what is going on right now in a service, according to its log.
2. They give you a configuration file of fifteen lines and ask you to review all of it.
3. They ask you how many records a CSV of forty thousand lines has, with a header.
4. They tell you the problem started «at some point in the early morning» and the log has ten thousand lines.

**And five questions:**

5. How do you pull out line 250 of a file with what you saw in this lesson?
6. You searched for `/error` in `less` and it found nothing, but you know there are errors. What could be going on?
7. `wc -l` tells you 99, and you count a hundred lines on the screen. What could be going on?
8. You come across a file called `respaldo.txt` («backup.txt»). What do you run before you `cat` it, and why is its name
   not enough?
9. You ran `cat` on a binary and now what you type is not visible. What do you do?

**What comes next:** you now read a file in pieces, without opening an editor. In lesson 04 you look for **files** in a
tree with `find`, when you do not even know which folder they are in.

**What comes later:** watching a log grow with `tail -f` arrives in lesson 10, and reading compressed files, in 06.

---

## Compare your answers

**Prediction D (the branches).** It is not the same number. The first line of the CSV is the **header**: it says what
each column is called (`sucursal,ciudad,terminales,hora_del_reporte`, «branch, city, terminals, report_time») and it is
not a branch. `wc -l` counts all the lines, header included, so the branches are one less. That is why it was worth
looking with `head` before counting: the number from `wc` only means something when you know what is on each line.

**Prediction E (the first `ERROR`).** Either one is fine if you wrote it beforehand: what matters is that you checked
it. If you browsed before searching, you saw that the log warns with `WARN` several times before the first `ERROR`:
problems almost never begin with an error, they begin with warnings that nobody read. And if along the way you came
across the word `error` in lowercase, that is another line, with another story: that is why the exercise asked for
`ERROR` exactly as written.

**Exercise 3.3.** The check is `verifica 03`; the number is not given here. To know whether you are on track: you
opened it with `less -N`, you went to the beginning with `g`, you searched with `/` and the exact text, and you read
the number on the left of the line, without counting.

**To wrap up.**

1. The end: `tail` on the log, or `tail -n` with as many lines as you want. It tells you the last thing that got
   recorded: read it with its time, use it as a clue, and then confirm the state with the signal that fits the service.
2. All of it: `cat`, better with `-n`. Fifteen lines fit on the screen.
3. How many there are: `wc -l`, minus one for the header. Before that, `head` to confirm that the first line is the
   header.
4. Browse and search: `less`, better with `-N`, searching with `/` for whatever distinguishes the line of the problem,
   such as `ERROR`.
5. `head -n 250`, and the last line it writes is number 250.
6. The `less` search distinguishes uppercase from lowercase: the errors may be written `ERROR` or `Error`. Try another
   form, or check the help of `less` with `h`.
7. The last line is missing its line break. `wc -l` counts breaks, not lines you can see.
8. `file respaldo.txt`. The name is put there by a person and may not tell the truth; `file` looks at the contents. If
   it does not say *text*, do not use `cat` until you know what format it is.
9. You type `reset` and Enter, even if you cannot see it. If Enter does not respond, Ctrl+J instead.

---

## Lesson sources

Opened on Nov 24, 2026. All of them are in English.

- man7.org, *The Linux man-pages project*. *cat(1)*, from GNU coreutils (chains files together; `-n` numbers the
  lines). https://man7.org/linux/man-pages/man1/cat.1.html
- man7.org. *head(1)*, from GNU coreutils (the first ten lines by default; `-n`).
  https://man7.org/linux/man-pages/man1/head.1.html
- man7.org. *tail(1)*, from GNU coreutils (the last ten lines by default; `-n`).
  https://man7.org/linux/man-pages/man1/tail.1.html
- man7.org. *wc(1)*, from GNU coreutils (lines, words and bytes, in that order; `-l` counts line breaks; what a word
  is). https://man7.org/linux/man-pages/man1/wc.1.html
- Ubuntu Manpages. *less(1)*, Ubuntu 26.04, less 668 (does not read the whole file before it starts; space bar, `g`,
  `G`, `/`, `n`, `h`, `q`, `-N`; the search distinguishes uppercase by default).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/less.1.html
- Mark Nudelman, author of `less`. *Less FAQ* (`less` is a pager and not an editor; Ctrl+C does not close it).
  https://www.greenwoodsoftware.com/less/faq.html
- Ubuntu Manpages. *file(1)*, Ubuntu 26.04, file 5.46 (how it decides the type from the contents; what *text* means:
  probably safe to read in the terminal). https://manpages.ubuntu.com/manpages/resolute/en/man1/file.1.html
- man7.org. *tset(1)*, from ncurses (`reset` returns the terminal to normal values after a program leaves it in an
  abnormal state; what you type may not be visible and Ctrl+J may be needed instead of Enter).
  https://man7.org/linux/man-pages/man1/tset.1.html
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, section «rust-coreutils» (the basic utilities come from
  `rust-coreutils`; `cp`, `mv` and `rm` are still GNU's).
  https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- Adam Wiggins. *The Twelve-Factor App*, «XI. Logs» (in its raw form, a log is usually text, in time order, with one
  event per line; an error trace can take up several). https://12factor.net/logs
