# Lesson 00 — Your practice terminal

**By Dorian Chávez, founder of Hábil and integration architect.**

**Before you read, six words.** One line each; the rest come in the lesson where they are used.

- **Terminal:** the window where you type instructions with the keyboard and the computer answers you with text.
- **Command:** a typed instruction. For example, `whoami` («who am I?»). You type it and press Enter.
- **Prompt:** the text the terminal puts at the start of the line when it is waiting for your next command. In
  Spanish, *indicador*.
- **Docker:** the program that creates and runs containers.
- **Container:** a small Linux computer that lives inside yours, separate from your files.
- **Image:** the mold a container comes from. You build it once and use it many times.

**What you take away:** your practice terminal working, which is an Ubuntu 26.04 inside a container; you know how to
get in, get out and come back without losing your work; you read the prompt; you ask the machine itself for help; and
you get out of any program that traps you.

**This lesson's new idea (the only one that is assessed):** in this terminal there is no desktop and no windows, only
text: it tells you who you are and where you are on every line, and you talk to it with commands.

**In what order it goes:** first you start the terminal (steps 1 and 2), then you read what it tells you (step 3),
then you learn to ask it for help (step 4) and, last, to get out of wherever you get stuck (step 5).

**A note on the language of the script.** The course's script (`./practicar`) and its checker (`verifica`) speak
Spanish, and so do the messages they print. In this lesson those messages appear exactly as the terminal prints them,
and below each block there is a short gloss in English, in italics. Names such as `practicar`, `verifica`,
`alumno` («student»), `practica` («practice») and `aqui-estuve.txt` («I was here») are part of the code and are not
translated.

---

## The scene

*An invented scenario; the screens that appear in the lesson are real.*

Tuesday, 11:40 p.m. Lucía has been at her first job for three weeks and tonight she is on call. The phone rings: the
payments service has stopped answering. They give her an address and a sentence: «get into the server and tell me
what you see».

She gets in. There is no desktop, no icons, no mouse. There is a black screen and a single line:

```text
operaciones@pagos-03:~$
```

*(In Spanish, «operaciones» is «operations» and «pagos» is «payments».)*

The cursor blinks. On the other end of the phone, they wait.

Lucía does not know it yet, but that line has already answered two of the three questions they are going to ask her
tonight: who she is in there and which folder she is standing in. The third, what to do, you ask the machine itself.
And if something gets stuck, there are three keys to get out.

This lesson gives you those four things on a machine made for making mistakes: yours, for practice.

---

## Before you start

**What you need:**

| | What | Notes |
|---|---|---|
| System | Linux, Mac or Windows | Below it says on which ones it has been tested and on which it has not |
| Docker | Installed and responding | Below it says how, by system |
| Disk | Less than 300 MB for the course image | A ceiling and not a figure, because each Docker tool measures differently (below) |
| Network | To download Ubuntu and a few packages the first time | Only your computer uses it, while building; inside the practice terminal there is no network |
| Know-how | Open your computer's terminal and paste a line | No Linux and no Docker is assumed |

**Where it was tested, and where it has not been:**

| System | Status | What was run |
|---|---|---|
| Linux Mint 22.3 (x86_64), Docker Engine 29.10.0 | Tested on Nov 19, 2026 | `./practicar` built the image in about 90 seconds, `verifica 00` gave its fifth mark in red before exiting and all five in green on coming back; inside there is only the internal network `lo`; `./practicar desde-cero` refused with the session open and, once it was closed, deleted the folder. Note: Docker does not officially support Linux Mint; its guide says it «may work» |
| Mac with Apple silicon, Docker 29.2.1 running with Colima | Tested on Nov 18 and 19, 2026 | Everything in this lesson: getting in, getting out, coming back, `verifica 00`, `desde-cero` and the screens you see here |
| Ubuntu | Not tested yet | Docker does officially support it; the installation is the one in its guide |
| Mac with Docker Desktop | Not tested yet | The installation is the one in the official Docker Desktop guide |
| Windows with Docker Desktop and WSL 2 | Not tested yet | The installation is the one in the official Microsoft and Docker guides |

If you test it on one of these, tell us how it went.

**The figures in this lesson, and how they were measured:**

| Figure | How it was measured |
|---|---|
| Building the image: 42 seconds | Mac with Apple silicon, Nov 18, 2026, with `time ./practicar reconstruir`: without cache and with the Ubuntu image already downloaded |
| Building the image: 1 minute 38 seconds | The same Mac, Nov 18, 2026, with `docker build --no-cache`, on an earlier version of the Dockerfile |
| Building the image: about 90 seconds | Linux Mint 22.3 (x86_64), Nov 19, 2026, with `./practicar` |
| Size: 221 MB | Linux Mint, Nov 18, 2026, with `docker image inspect` |
| Size: 277 MB | Mac, Nov 18, 2026, with `docker image ls` |

**Install Docker, according to your system.** It is the only thing that gets installed on your computer.

- **Linux Mint or Ubuntu:** Docker Engine, from Docker's official repository. [Lesson 1 of our Docker
  course](https://www.habil.mx/en/courses/docker/01-instalacion/) does it step by step on Linux Mint
  and explains each error. Docker does not officially support Mint, and that lesson says what to do about it. On the
  test computer, the user was in the `docker` group and used Docker without `sudo`; that same lesson explains what
  joining that group implies.
- **Mac:** Docker Desktop, with the installer for your processor (Apple silicon or Intel), from the
  [official guide](https://docs.docker.com/desktop/setup/install/mac-install/). Open it and wait for it to say it is
  running. On the test Mac, Docker ran with another tool, Colima, and not with Docker Desktop: that is why this route
  is in the table as «not tested».
- **Windows:** the documented route is Docker Desktop with WSL 2. WSL is the piece of Windows that runs a Linux inside.
  According to Microsoft, on Windows 10 (version 2004 or later) and Windows 11 it is installed with `wsl --install` in
  a PowerShell opened as administrator, and it brings Ubuntu. According to Docker, you then install Docker Desktop and
  turn on the integration in *Settings › Resources › WSL Integration*. Everything that follows you run in that Ubuntu's
  terminal, not in PowerShell. It is also in the table as «not tested».

**Check that Docker responds.** Open your computer's terminal and run:

```bash
docker version
```

You are fine if you see two blocks, `Client` and `Server`, each with its version. In the tests they were Docker 29.10.0
on Linux and 29.2.1 on the Mac. If `Client` appears and, instead of `Server`, an error, the `docker` program could not
talk to its engine. Run `docker info` and follow the exact error it gives you; the most common causes are in «Not
running?».

**Get the course material.** It is in a **repository**: a folder with all the course files, published on GitHub. The
`git` program downloads it with the first line; the second puts you inside that folder:

```bash
git clone https://github.com/HabilMX/curso-terminal.git
cd curso-terminal
```

**Without `git`:** open that address in your browser, choose *Code › Download ZIP*, unzip the file and open your
terminal in the folder that was created. Do not copy the `cd curso-terminal` above: that folder may have a different
name.

---

## Step 1 · Start your terminal and get in

**Predict first.** Write on a piece of paper: when you get in, what do you think you will see? A new window? A desktop?
Something else?

From the course folder:

```bash
./practicar
```

`practicar` is a course script. The first time it builds the image: it downloads Ubuntu 26.04 and puts in what the
course uses. In the tests it took between 42 seconds and 1 minute 38 seconds. The following times it no longer
downloads or rebuilds: on the test Mac it got in in under two seconds. Your time depends on Docker and your machine.

You will see a lot of text go by and, at the end, this:

```text
✅ Imagen lista.
Entrando a tu terminal de práctica. Para salir, escribe  exit  (o Ctrl+D).
alumno@practica:~$
```

*Image ready. Getting into your practice terminal. To get out, type `exit` (or Ctrl+D).*

No window opened. **You are in the same window, but you are now on another machine.** The last line is the prompt of
your practice terminal, and you will read it closely in step 3. Compare it with what you predicted.

**How separate it is from your computer.** For these exercises no folder of your computer is connected, the terminal
runs as `alumno`, without administrator privileges, and inside there is no network. Your work is saved only in the
`~/practica` folder. What the container does use from your computer is processor, memory and disk.

Run your first command inside:

```bash
verifica 00
```

`verifica` inspects the state of your practice terminal and tells you what is missing. Each mark says what it measured.
It should now come out like this:

```text
✅ Es Ubuntu 26.04 (medí: ID y VERSION_ID en /etc/os-release)
✅ Eres el usuario alumno (medí: id -un)
✅ /home/alumno/practica existe y es una carpeta (medí: que exista como carpeta)
✅ Puedes escribir en /home/alumno/practica (medí: creé un archivo vacío ahí y lo borré)
❌ Falta /home/alumno/practica/00/aqui-estuve.txt (medí: que el archivo exista). Créalo con el paso 2 de la lección, corre  verifica 00, sal con exit y vuelve a entrar

❌ Lección 00: falta lo marcado con ❌. Arréglalo y corre  verifica 00  otra vez.
```

*It is Ubuntu 26.04 (I measured: ID and VERSION_ID in `/etc/os-release`). You are the user alumno (I measured: `id -un`).
`/home/alumno/practica` exists and is a folder (I measured: that it exists as a folder). You can write in
`/home/alumno/practica` (I measured: I created an empty file there and deleted it). Missing
`/home/alumno/practica/00/aqui-estuve.txt` (I measured: that the file exists). Create it with step 2 of the lesson, run
`verifica 00`, get out with `exit` and come back in. Lesson 00: what is marked with ❌ is missing. Fix it and run
`verifica 00` again.*

Four greens and a red. The red is normal: it is what you do in step 2.

**What it shows** (objective 0.1, first half): the four green marks.

---

## Step 2 · Leave a mark, get out and come back

A container is thrown away when you leave. So, where does your work stay? In a single folder: `practica`. Docker keeps
it apart, in what it calls a **volume**, and connects it back every time you get in. Everything else is born clean
again.

**Predict first.** You are going to create a file inside `practica`, get out and get back in. Will it still be there?
And if you had created it outside `practica`?

Today these two lines are a recipe to copy as is. Type them exactly:

```bash
mkdir ~/practica/00
date > ~/practica/00/aqui-estuve.txt
```

- `mkdir` creates a folder: here, the folder `00` inside `practica`.
- `date` answers the date and time. The `>` sign sends that answer to a file instead of to the screen.
- `~` is the abbreviation for your home folder. You see it closely in step 3.

In lessons 02 and 06 you will understand each piece in detail. Neither of the two lines answers anything: in the terminal,
**silence usually means it went well**.

Run `verifica 00` again, **before leaving**. The last line changed:

```text
❌ Tu marca aqui-estuve.txt ya existe, y es la primera vez que la veo: fue en ESTA sesión (medí: no había apunte de una sesión anterior; lo acabo de dejar en 00/.sesion-verifica). Te falta salir con exit, volver a entrar con ./practicar y correr  verifica 00  otra vez
```

*Your mark aqui-estuve.txt already exists, and it is the first time I see it: it was in THIS session (I measured: there
was no note of a previous session; I have just left it in `00/.sesion-verifica`). You still need to get out with `exit`,
get back in with `./practicar` and run `verifica 00` again.*

`verifica` has already seen your mark and noted in which session it saw it. A **session** is each time you get in. The
note stays in your practice folder, in a file called `.sesion-verifica`. Now do as it says. Get out:

```bash
exit
```

```text
logout
Saliste de la terminal de práctica.
Lo que dejaste en ~/practica sigue guardado. Para volver a entrar:  ./practicar
```

*You left the practice terminal. What you left in `~/practica` is still saved. To get back in: `./practicar`.*

Notice the prompt: it changed. It no longer says `alumno@practica`. You are back on your computer. Get back in with
`./practicar` and run `verifica 00`:

```text
✅ Saliste y volviste sin perder tu carpeta (medí: tu marca ya existía en una sesión anterior a esta, según el apunte de 00/.sesion-verifica)

✅ Lección 00: todo en verde.
```

*You got out and came back without losing your folder (I measured: your mark already existed in a session before this
one, according to the note in `00/.sesion-verifica`). Lesson 00: all green.*

`verifica` does not use the clock. It compares two things: the session in which it saw your mark for the first time and
the current session. They are different and the mark is still there: you got out, you came back and your folder
survived.

**Starting from zero.** If one day you want your practice folder empty again, from your computer (not from inside):

```bash
./practicar desde-cero
```

It asks you to type `si` («yes») before deleting, because it cannot be undone. It deletes only your practice folder. If
you have the practice terminal open in another window, it deletes nothing and asks you to get out first. It is a
command that deletes: type it yourself, by hand. If you run it now, you will have to repeat this step.

**What it shows** (objective 0.1 complete): `verifica 00` all green.

---

## Step 3 · Read the prompt

This is the line you have in front of you:

```text
alumno@practica:~$
```

It has four pieces:

| Piece | What it says |
|---|---|
| `alumno` | Who you are: your user on this machine |
| `practica` | Which machine you are on. It goes after the `@`, as in an email: «alumno, at practica» |
| `~` | Which folder you are in. `~` is the abbreviation for «your home folder» |
| `$` | End of the prompt: from here on you type. `$` means normal user |

**Predict first.** There is a command to ask each thing: `whoami` (who am I?), `hostname` (what is this machine
called?) and `pwd` (which folder am I in?). Before running them, write on your paper what each will answer. For two,
reading the prompt is enough. The third has a trick.

Now run them, one by one:

```bash
whoami
hostname
pwd
```

Compare with your paper. The answers are at the end, in «Compare your answers»; do not look at them before predicting.

**Why it matters.** In a real job you will have three terminals open that look the same: your computer, a test server
and a production one. The prompt is the only thing that tells you which one you are about to type in. **Read it before
every command that changes something.**

**What it shows** (objective 0.2): your written prediction, compared with the output.

---

## Step 4 · Ask the machine for help

Nobody knows the commands by heart. What they do know is where to ask, and the answer comes inside the machine itself.
There are two ways.

**The short one: `--help`.** Almost every command accepts `--help` and answers with a summary:

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

In this terminal the help appears in English. It has three parts: what it does, how it is used (*Usage*) and which
options it accepts (*Options*). An **option** is a word with dashes that changes what the command does.

**The long one: `man`.** It is the manual (`man`, for *manual*). Every command has its page:

```bash
man whoami
```

Now the screen is different. You no longer see the prompt, and at the bottom it says:

```text
 Manual page whoami(1) line 1 (press h for help or q to quit)
```

You are **inside** the manual. What you type is no longer commands: it is the manual's keys. These five are enough:

| Key | What it does |
|---|---|
| Down arrow, or space bar | Goes down: the arrow, one line; the space bar, one screen |
| Up arrow | Goes up one line |
| `/` and a word, then Enter | Searches for that word downward |
| `n` | Goes to the next occurrence |
| `q` | Leaves the manual and returns you to the prompt |

Leave with `q`. The prompt is back.

**With Claude Code, in this order.** In this course an AI assistant always comes in the same way: **you predict → you
run → you read what came out → you explain in your own words.** Here you can ask Claude Code to explain what a `man`
page you did not understand says. But you open the page and you check the answer, in your terminal. Exercise 0.3 is
done without AI: the point is that you know how to ask the machine when there is nobody else.

**What it shows** (objective 0.3): exercise 0.3, below.

---

## Step 5 · Get out of what trapped you

A moment ago you were «trapped» in `man` and left with `q`. It happens to everyone: a program takes the screen and the
prompt does not come back. There are three keys, and each one is for something different.

**An error on purpose, first.** Type the command with a capital letter:

```bash
Whoami
```

```text
bash: Whoami: command not found
```

The terminal tells uppercase from lowercase: `Whoami` does not exist. And notice what happened next: it gave you back
the prompt. **An error that gives you back the prompt did not trap you.** You read the message and carry on. Trapped is
when the prompt does not come back.

Now, three ways of getting trapped. Do all three.

**1. A program that does not end.** `sleep 1000` waits a thousand seconds doing nothing:

```bash
sleep 1000
```

The prompt does not come back. Press **Ctrl+C** (hold Ctrl and tap C). `^C` appears and the prompt comes back. Ctrl+C
tells the program: «stop now».

**2. A program that waits for you to type.** `cat`, with nothing else, repeats whatever you type:

```bash
cat
```

Type `hola` («hello») and Enter: it answers `hola`. It will keep waiting forever. Press **Ctrl+D** on an empty line. The
prompt comes back. Ctrl+D tells the program: «there is nothing more to type».

**3. A full-screen program.** `htop` shows the programs that are running:

```bash
htop
```

It takes the whole screen. Leave with **`q`**, just like in `man`.

**Which one is for what:**

| Key | It tells the program | Use it when |
|---|---|---|
| `q` | «Close yourself» | You are in a program you page through or that takes the screen: `man`, `less`, `htop` |
| Ctrl+C | «Stop now» | A command is running and does not end |
| Ctrl+D | «There is nothing more to type» | A program is waiting for text from you |

Three measured things worth knowing:

- **Ctrl+C does not get you out of `man`.** The page stays. You leave it with `q`.
- **`q` at the prompt does nothing useful.** It answers `bash: q: command not found`. It is useful for knowing you were
  already outside.
- **Ctrl+D at the empty prompt gets you out of the practice terminal.** The prompt is also a program waiting for you to
  type: it is called bash. Ctrl+D there is the same as `exit`. Nothing is lost: you come back with `./practicar`.

**What it shows** (objective 0.4): the five situations in the review, in «To wrap up».

---

## Three notes about your practice terminal

**Your Mac uses zsh; inside you use bash.** The program that reads your commands is called a *shell*. Since macOS 10.15,
the one on new Mac accounts is zsh, according to Apple. Inside the container it is bash, which is common on many
servers. That is why the whole course is done inside: the whole group practices in the same environment. Your
computer's terminal is only for running `./practicar`.

**`htop` and `ps` only see the container.** When you opened `htop` you saw only two programs: bash and htop itself. You
do not see those of your computer, because Docker gives each container its own list of processes. To learn to read and
stop them, which is lesson 10, that is enough.

**Your basic commands are written in Rust.** Since Ubuntu 25.10, commands such as `ls`, `cat` and `whoami` no longer
come from the GNU project, which is the classic one, but from `rust-coreutils`. Ubuntu's documentation says so, and in
your terminal you see it in `man whoami`: under *VERSION* it says `uutils coreutils`. Three are still from GNU: `cp`,
`mv` and `rm`.

- **What it means for you:** if you compare with a tutorial or with an older server, the text of `man` and of `--help`
  may differ from what you see here. It is not your fault or the tutorial's.
- **What does not change in this course:** the commands are called the same and the options that are taught work the
  same. The outputs you see in the lessons come from the image tested on Nov 18 and 19, 2026. The image is always built
  on the current Ubuntu 26.04, so after an update an output may vary a little.

---

## Your exercises

Exercise 0.3 is done **without AI** and without a search engine: only with `man`.

| # | You do | It is done when |
|---|---|---|
| 0.1 | Steps 1 and 2: you get in, leave your mark, get out and come back | `verifica 00` comes out all green |
| 0.2 | Step 3: you predict in writing what `whoami`, `hostname` and `pwd` answer, and then run them | You have your prediction and the output, and you can explain the one that did not match |
| 0.3 | You answer the three questions below, only with `man` | Your three answers match «Compare your answers» |
| 0.4 | Step 5: you trap yourself on purpose in `sleep 1000`, `cat` and `htop`, and get out of all three | You got out of all three without closing the window |

**The three questions of 0.3.** For each one: open the page, search with `/`, write down the answer and leave with `q`.

1. In `man whoami`: how many options does `whoami` accept and which are they?
2. In `man ls`, search for `--all`: which short letter is the same as `--all`, and what does it do?
3. In `man man`, search for `Executable`: the manual is divided into numbered sections. What number is the one for
   programs and commands? And the one for system administration commands?

**Time (estimated):** 60 to 90 minutes, not counting the Docker installation. It is an estimate: it will be adjusted
with the first group that takes the course.

---

## Not running?

| You see | Probable cause | What you do |
|---|---|---|
| `❌ No encuentro el comando «docker» en esta computadora.` *(I can't find the «docker» command on this computer.)* | Docker is not installed | «Before you start», according to your system |
| `❌ Docker está instalado, pero no pude hablar con él.` *(Docker is installed, but I could not talk to it.)* | Most commonly: Docker is off, or your user does not have permission. It can be something else | Run `docker info` and follow the exact error it gives you |
| `no such file or directory: ./practicar` | You are not in the course folder | `cd curso-terminal` and repeat |
| `❌ No se pudo construir la imagen.` *(The image could not be built.)* | The network, disk space, Docker's permissions or the course material | Read the **first** error that came out above the message, fix it and run `./practicar` again |
| `❌ ./practicar abre una terminal interactiva…` *(./practicar opens an interactive terminal…)* | You ran it with a pipe or a redirection | Run it alone: `./practicar` |
| `command not found: verifica` | You ran `verifica` on your computer, not inside | Get in with `./practicar`; the prompt must say `alumno@practica` |
| `mkdir: /home/alumno/practica/00: File exists` | You had already created the `00` folder | Nothing wrong: carry on with the `date` line |
| The last line of `verifica 00` is still red after coming back | You did not run `verifica 00` before leaving, so it had only just seen your mark | It is already noted: get out with `exit`, get in and run `verifica 00` again |
| `❌ No sé en qué sesión estás…` *(I don't know which session you are in…)* | You got into the container without the course script | Get out and get in with `./practicar` |
| The prompt does not come back | A program took the terminal | Try `q`; if not, Ctrl+C; if not, Ctrl+D (step 5) |
| You closed the window without typing `exit` | The practice terminal may have stayed open inside | Open another window and get in with `./practicar`: your folder is still there |
| `❌ No borré nada: tu carpeta de práctica está en uso.` *(I deleted nothing: your practice folder is in use.)* | A practice terminal is open, perhaps from a window you closed without `exit` | Get out of it with `exit`. If you no longer have that window, the same message gives you the `docker stop …` line to close it yourself |
| The course was updated and `verifica` does not know a lesson | Your image is from before | `./practicar reconstruir` |

---

## To wrap up

**Answer without reading again.**

**Five situations: which key?** You pass with 4 out of 5.

1. You opened `man ls` to look for an option and you already found it.
2. You ran a command that has been going for ten minutes and does not end.
3. You typed `cat` and Enter by mistake, and now everything you type is repeated.
4. You opened `htop` to see what is running and you have seen enough.
5. You are done for today: you are at `alumno@practica:~$` and you want to go back to your computer.

**And three questions:**

6. You see `soporte@caja-07:~$`. Without running anything: who are you, on which machine and in which folder?
7. You created a file outside `~/practica` and got out. Will it be there when you come back? Why?
8. You do not remember what an option of a command does and you have no internet. What two things do you try?

**What comes next:** you now have somewhere to practice and you know where you are standing. In lesson 01 you move
through the folders without seeing them.

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

**Step 3 (exercise 0.2).** `whoami` answers `alumno`. `hostname` answers `practica`. `pwd` answers `/home/alumno`. That
is the trick: the prompt says `~`, and `~` is the abbreviation for your home folder, which on this machine is
`/home/alumno`. `~` and `/home/alumno` name the same place: the prompt uses the short form.

**Exercise 0.3.**

1. Two: `-h` or `--help`, which shows the help, and `-V` or `--version`, which shows the version.
2. `-a`. It makes `ls` not hide hidden files, which are those with a name that starts with a dot.
3. Section 1 is the one for programs and commands (*Executable programs or shell commands*). Section 8 is the one for
   system administration commands (*System administration commands*). That is why the page says `whoami(1)`: the number
   in parentheses is the section.

**To wrap up.**

1. `q`.
2. Ctrl+C.
3. Ctrl+D, on an empty line. Ctrl+C also stops it.
4. `q`.
5. `exit`, or Ctrl+D at the empty prompt.
6. You are `soporte`, on the machine `caja-07`, in your home folder.
7. No. On exit, the container is thrown away and is born clean again. Only `~/practica` is saved apart, in a volume.
8. The command with `--help`, and `man` followed by the command.

---

## Lesson sources

Opened on Nov 18, 2026.

- Ubuntu. *Ubuntu 26.04 LTS release notes* (Resolute Raccoon; published on April 23, 2026). https://documentation.ubuntu.com/release-notes/26.04/
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, section «rust-coreutils» (the basic commands come from `rust-coreutils` since 25.10; `cp`, `mv` and `rm` are still from GNU). https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- Ubuntu Manpages. *man(1)*, from Ubuntu 26.04 (manual sections; `-k`). https://manpages.ubuntu.com/manpages/resolute/en/man1/man.1.html
- Docker. *Install Docker Engine on Ubuntu* (supported Ubuntu versions; Linux Mint without official support). https://docs.docker.com/engine/install/ubuntu/
- Docker. *Install Docker Desktop on Mac* (installer by processor; supported macOS versions). https://docs.docker.com/desktop/setup/install/mac-install/
- Docker. *Docker Desktop WSL 2 backend on Windows* (integration with the WSL distribution). https://docs.docker.com/desktop/features/wsl/
- Microsoft. *How to install Linux on Windows with WSL* (`wsl --install`; Windows versions). https://learn.microsoft.com/en-us/windows/wsl/install
- Docker. *Volumes* (a volume stores data outside the container and survives its deletion). https://docs.docker.com/engine/storage/volumes/
- Docker. *docker container run* (`-i`, `-t`, `--rm`, `-h`, `--network`; each container has its own list of processes). https://docs.docker.com/reference/cli/docker/container/run/
- Apple. *Use zsh as the default shell on Mac* (zsh by default since macOS 10.15). https://support.apple.com/en-us/102360
- Our own measurement, Nov 18 and 19, 2026, on a Mac with Apple silicon (Docker 29.2.1, linux/arm64) and on a computer
  with Linux Mint 22.3 (x86_64, Docker 29.10.0). What was run on each and how each figure was measured is in the two
  tables of «Before you start». The screens of this lesson and the three keys of step 5 were taken on the Mac. The
  Nov 19 revision —the container without a network, the course's labels and the `verifica` that no longer uses the
  clock— was tested on both.
