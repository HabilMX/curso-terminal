# O terminal do zero

**Por Dorian Chávez, fundador da Hábil e arquiteto de integração.**

**Para quem é:** quem programa e nunca viveu em um terminal. Não se supõe nada de Linux.

**O que se vai saber ao terminar:** entrar em um terminal Linux sem tela e trabalhar ali sem medo. Quem termina circula
por pastas; cria, copia e apaga sabendo o que faz; lê logs, encontra arquivos e texto, encadeia comandos, edita com o
vi, troca texto com o sed, lê e muda permissões, e vê e interrompe processos com o htop.

**O que é preciso antes de começar:** um computador com Docker. A [lição 00](00-tu-terminal-de-practica.md) explica
como tê-lo no Linux, no Mac e no Windows, e deixa pronto o terminal de prática.

## Onde se pratica

Em um **contêiner**: um computador Linux pequeno que vive dentro do computador de quem estuda. É um Ubuntu 26.04 LTS,
sobe com um único comando (`./practicar`) e fica separado dos arquivos: nenhuma pasta do computador é conectada a ele
e lá dentro não há rede. Se for desarrumado, descarta-se e recomeça-se do zero. Todos praticam ali, inclusive quem já
tem Linux ou Mac: assim o grupo inteiro pratica no mesmo ambiente.

## Como se aprende aqui

- **Uma ideia nova por lição.** O resto vem dado e explicado.
- **Prever, rodar, comparar.** Antes de cada comando anota-se o que se acha que vai acontecer; depois se comprova e se
  explica com palavras próprias.
- **Comprova-se sozinho.** Cada lição traz um `verifica` que confere o estado do terminal de prática e diz o que
  falta. Ele não avalia o que se prevê nem o que se explica: isso se compara por conta própria, no fim de cada lição.
- **Com IA, no lugar certo.** Um assistente de IA, como o Claude Code, pode explicar um comando. Mas em cada lição há
  pelo menos um exercício feito sem IA, e todo comando que apaga ou muda permissões se digita à mão.

## As lições

Fazem-se em ordem: cada uma parte do que a anterior deixou. Comece pela
[00](00-tu-terminal-de-practica.md), que deixa pronto o terminal de prática.

| # | Lição | A ideia nova |
|---|---|---|
| 00 | [O terminal de prática](00-tu-terminal-de-practica.md) | Neste terminal não há área de trabalho nem janelas, só texto: ele diz quem se é e onde se está em cada linha, e se fala com ele por meio de comandos |
| 01 | Mover-se sem ver | A árvore de pastas, e o caminho absoluto contra o relativo |
| 02 | Criar, copiar, mover e apagar sem medo | No terminal não há lixeira: pensa-se antes de apagar |
| 03 | Ver o que há dentro | Quase tudo em um servidor é texto, e se lê sem abrir um editor |
| 04 | Encontrar arquivos: find | O `find` percorre a árvore e filtra pelo que se mandar |
| 05 | Buscar dentro: grep | O `grep` busca texto dentro dos arquivos; o `find` busca os arquivos |
| 06 | Pipes e redirecionamento | A saída de um comando pode ser a entrada do seguinte |
| 07 | Editar no servidor: vi | O vi tem modos; metade dos problemas é não saber em qual se está |
| 08 | Mudar texto sem abri-lo: sed | Mudar muitas linhas de uma vez, testando antes de gravar |
| 09 | Permissões básicas | Cada arquivo diz quem pode lê-lo, escrevê-lo e executá-lo |
| 10 | Processos: ps, htop e kill | Um programa em execução é um processo, com número e dono, e se pode pedir que pare |
| 11 | Junte tudo: o log do problema | Sem ideia nova: usa-se tudo o que veio antes para encontrar e corrigir uma falha |

As lições sem link estão em preparação e são publicadas em ordem.

## O que vem depois

Este curso é a porta de entrada dos cursos de [Docker](https://www.habil.mx/pt/cursos/docker/) e de
[Kubernetes](https://www.habil.mx/pt/cursos/kubernetes/) desta casa.
