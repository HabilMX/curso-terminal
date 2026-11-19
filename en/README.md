# The Terminal from Scratch

**By Dorian Chávez, founder of Hábil and integration architect.**

**Who it is for:** people who write code and have never worked in a terminal. No Linux is assumed.

**What you will know by the end:** how to get into a Linux terminal with no screen and work there without fear. You
move through folders; you create, copy and delete knowing what you are doing; you read logs, find files and text,
chain commands, edit with vi, change text with sed, read and change permissions, and watch and stop processes with
htop.

**What you need before you start:** a computer with Docker. [Lesson 00](00-tu-terminal-de-practica.md) tells you how
to get it on Linux, Mac and Windows, and leaves your practice terminal ready.

## Where you practice

In a **container**: a small Linux computer that lives inside yours. It is Ubuntu 26.04 LTS, it starts with a single
command (`./practicar`) and it is separate from your files: no folder of your computer is connected to it, and inside
there is no network. If you break it, you throw it away and start over. Everyone practices there, including people who
already have Linux or a Mac: that way the whole group practices in the same environment.

## How you learn here

- **One new idea per lesson.** Everything else is given and explained.
- **You predict, you run, you compare.** Before each command you write down what you think will happen; afterwards you
  check it and explain it in your own words.
- **It checks itself.** Each lesson comes with a `verifica` that inspects the state of your practice terminal and tells
  you what is missing. It does not grade what you predict or what you explain: you compare that yourself, at the end of
  each lesson.
- **With AI, in its place.** An AI assistant, such as Claude Code, can explain a command to you. But in every lesson
  there is at least one exercise done without AI, and every command that deletes or changes permissions you type
  yourself, by hand.

## The lessons

Do them in order: each one starts from what the previous one left. Start with
[00](00-tu-terminal-de-practica.md), which leaves your practice terminal ready.

| # | Lesson | The new idea |
|---|---|---|
| 00 | [Your practice terminal](00-tu-terminal-de-practica.md) | This terminal has no desktop and no windows, only text: it tells you who you are and where you are on every line, and you talk to it with commands |
| 01 | [Moving around without seeing](01-moverte-sin-ver.md) | The folder tree, and the absolute path versus the relative one |
| 02 | [Create, copy, move and delete without fear](02-crear-copiar-mover-borrar.md) | The terminal has no trash can: you think before you delete |
| 03 | Seeing what is inside | Almost everything on a server is text, and you read it without opening an editor |
| 04 | Finding files: find | `find` walks the tree and filters by whatever you tell it |
| 05 | Searching inside: grep | `grep` searches for text inside files; `find` searches for the files |
| 06 | Pipes and redirection | The output of one command can be the input of the next |
| 07 | Editing on the server: vi | vi has modes; half the problems come from not knowing which one you are in |
| 08 | Changing text without opening it: sed | Changing many lines in one go, testing before you write |
| 09 | Basic permissions | Every file says who can read it, write it and run it |
| 10 | Processes: ps, htop and kill | A running program is a process, with a number and an owner, and you can ask it to stop |
| 11 | Put it all together: the log of the problem | No new idea: you use everything before it to find and fix a failure |

The lessons without a link are in preparation and are published in order.

## What comes after

This course is the doorway to our [Docker](https://www.habil.mx/en/courses/docker/) and
[Kubernetes](https://www.habil.mx/en/courses/kubernetes/) courses.
