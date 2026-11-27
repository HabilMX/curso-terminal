# Lição 03 — Ver o que há dentro

**Por Dorian Chávez, fundador da Hábil e arquiteto de integração.**

**Antes de ler, cinco palavras.** Uma linha cada uma; as demais chegam no passo em que são usadas.

- **Log:** o arquivo onde um programa vai anotando o que faz, uma linha por coisa e com a hora. Em espanhol se diz
  *bitácora*, e nos servidores é assim, *log*, que se vê escrito.
- **Linha:** uma linha de um arquivo. Termina com uma quebra de linha, o Enter que não se vê. Quando um comando conta
  linhas, conta essas quebras.
- **Texto e binário:** um arquivo de texto tem letras, números e sinais que dá para ler. Um binário tem bytes pensados
  para um programa, não para os olhos: uma foto, um programa, um arquivo comprimido.
- **Paginador:** um programa que mostra um texto por telas e permite ir e voltar por ele. O desta lição se chama `less`.
- **Comprimido:** um arquivo que um programa, como o `gzip`, deixou menor. Para lê-lo é preciso descomprimi-lo.

**O que fica com quem termina:** mostra-se um arquivo completo com `cat`, também com o número de cada linha; tiram-se o
começo, o fim ou uma linha exata com `head` e `tail`; contam-se linhas com `wc -l`; folheia-se e busca-se no `less`, e
sai-se dele sem se perder; e, antes de abrir um arquivo desconhecido, pergunta-se ao `file` o que ele é.

**Ideia nova desta lição (a única que se avalia):** em um servidor quase tudo é texto, e se lê sem abrir um editor.
Antes de ler, **decide-se que pedaço é preciso**: tudo, o começo, o fim, uma linha, quantas são, ou folhear procurando
algo. A pergunta não é quais comandos existem, e sim que pedaço cada situação pede.

**Em que ordem vai:** monta-se uma sala de plantão (passo 1), leem-se arquivos completos (passo 2), tiram-se pedaços e
contam-se linhas (passo 3), folheia-se e busca-se (passo 4), pergunta-se o que é um arquivo antes de abri-lo (passo 5)
e, no fim, confere-se tudo com o `verifica` (passo 6).

**Uma nota sobre o idioma da sala.** Como nas lições 00, 01 e 02, as mensagens do curso e os nomes da sala de plantão
estão em espanhol e fazem parte do código, então não se traduzem. As mensagens aparecem exatamente como o terminal as
imprime, com uma glosa em português, em itálico, abaixo de cada bloco, ou entre parênteses dentro de uma tabela. Os
nomes da sala são glosados uma só vez, no passo 1: `ensayo` («ensaio»), `pagos` («pagamentos»), `reportes`
(«relatórios»), `sucursales` («filiais») e os demais. Os comandos `practicar`, `prepara`, `verifica` e `anota` também
mantêm o nome: já se conhecem das lições anteriores.

---

## A cena

*Cenário inventado. As telas da lição foram copiadas do terminal de prática.*

Quinta-feira, 03h12. O telefone de Lucía vibra sobre a mesa.

—As filiais estão recusando todos os cartões. Começou faz um tempo, não sabemos quando. O que diz o log de
pagamentos?

Lucía já sabe chegar à pasta sem vê-la, e que ali não se apaga nada sem olhar. O que ela nunca fez foi **ler** um
arquivo em uma máquina sem janelas. Não há clique duplo. Há a mesma linha de sempre, esperando.

O log tem centenas de linhas, e em um servidor de verdade poderia ter milhões. Em algum lugar desse arquivo está a
resposta: a linha em que tudo começou, e a última, o mais recente que ficou escrito. Ao lado dele há outro, o de ontem,
com um nome que termina em `.gz` e que não se deixa ler como os demais. Do outro lado, alguém espera uma hora, um número
e uma frase para decidir o que fazer com centenas de cobranças.

Há algo a favor dela: os logs costumam ser texto. *The Twelve-Factor App*, o guia de Adam Wiggins sobre como construir
aplicações que rodam em servidores, descreve assim: em estado bruto, muitos logs são texto, em ordem de tempo, e
costumam trazer um evento por linha; uma exceção comum é um rastro de erro, que pode ocupar várias. Se é texto, lê-se com
os comandos desta lição.

Esta lição é essa meia hora, em uma sala de plantão feita para ler sem consequências.

---

## Antes de começar

**O que é preciso:** a lição 02 feita; o material do curso atualizado; e papel e lápis para as previsões.

**Atualize o terminal de prática.** Como nas lições anteriores, a partir do computador, na pasta do curso:

```bash
git pull
./practicar reconstruir
```

Desta vez a imagem incorpora três pacotes novos: `file`, `gzip` e `ncurses-bin`. Nesta versão não se mediu a
reconstrução do zero: conforme o Docker e o que já estiver guardado na máquina, pode precisar de rede e demorar mais
que na 02. Deve terminar com `✅ Imagen lista.` («Imagem pronta.») A pasta `~/practica` não é tocada. Depois, entra-se
com `./practicar`.

**Onde esta lição foi testada:**

| Sistema | Estado |
|---|---|
| Linux | Testada em 25/11/2026 na ramificação `leccion/03` (3bf1787), medida em um servidor Linux: host x86_64, Docker 29.10.0; a imagem foi construída em 9 s e pesa 232.656.309 bytes; Ubuntu 26.04, `cat` (uutils coreutils) 0.10.0, `less` 668 e `file-5.46`. Rodaram `prepara 03`, cada comando dos passos 1 a 6, e `verifica 03` em vermelho e em verde, quando a folha tinha oito chaves. O `verifica 03` com as sete chaves de agora foi medido de novo no mesmo dia (ramificação em 1a3a7b5): vermelho com 7 ❌, verde com 7 ✅, verde também depois de `simula 03`, e o `anota 03 arranque` se recusa. Sem medir: `./practicar reconstruir` (a imagem foi construída com `docker build`) e `prepara 03` sobre uma sala já montada |
| Mac com Apple silicon, Ubuntu, Windows com Docker Desktop e WSL 2 | Ainda sem testar com esta lição |

**O que vai coincidir com a tela e o que não.** Coincidem os nomes, os conteúdos e as datas da sala, e as mensagens dos
comandos. Mudam a disposição em colunas, as cores e o tamanho da janela, que decide quantas linhas cabem na tela.

---

## Passo 1 · Monte a sala de plantão

Dentro do terminal de prática:

```bash
prepara 03
```

```text
✅ Sala de guardia lista en /home/alumno/practica/03 (medí: 3 carpetas y 8 archivos, cada uno con
   su contenido; las dos bitácoras, por su huella).
   Te dejé la hoja de respuestas, con sus claves vacías: /home/alumno/practica/respuestas/03.md
   Empieza por ahí:  cd ~/practica/03
```

*Sala de plantão pronta em `/home/alumno/practica/03` (medi: 3 pastas e 8 arquivos, cada um com o próprio conteúdo; os
dois logs, pela impressão digital). Deixei a folha de respostas, com as chaves vazias:
`/home/alumno/practica/respuestas/03.md`. Comece por aí: `cd ~/practica/03`.*

O `prepara 03` funciona como o da 02: não tem nada de aleatório e mexe só em `~/practica/03` e na folha de respostas. Se
a sala já existe, o `prepara 03` explicará antes de substituí-la; esse caso não foi medido nesta versão. Os logs não vêm
escritos linha por linha: saem de uma regra fixa, e o `prepara` confere no final que cada um tenha *impressão
digital* própria, uma espécie de assinatura do conteúdo. Se um único caractere mudar, a impressão digital já não coincide.

**A sala, vista de cima:**

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

*As setas dizem: «para testar os comandos», «três linhas», «cem linhas numeradas», «a configuração do serviço», «o log
de hoje, o longo», «o log de ontem, comprimido», «o da reinicialização do serviço» e «as filiais que reportaram
recusas». Os nomes, em espanhol: `LEEME.txt` («LEIAME.txt»), `ensayo` («ensaio»), `corto` («curto»), `cien` («cem»),
`pagos` («pagamentos»), `arranque` («partida»), `reportes` («relatórios») e `sucursales` («filiais»).*

As demonstrações vão em `ensayo`; os exercícios, em `pagos` e `reportes`. **Nesta sala nada se edita nem se apaga:** o
`verifica 03` confere que tudo continue como o `prepara` deixou, e se algo mudou ele avisa.

A folha de respostas traz sete chaves, uma para cada coisa que se vai ler. Preenche-se como nas lições anteriores, com
o `anota`.

**Comprova:** ainda nada. Este passo deixa o material pronto.

---

## Passo 2 · Tudo de uma vez: `cat`

Entre no ensaio:

```bash
cd ~/practica/03/ensayo
```

O `cat` vem de *concatenate*, «encadear»: o manual dele diz que cola um ou vários arquivos e os escreve na tela. Com um
só arquivo, escreve-o completo:

```bash
cat corto.txt
```

```text
primer renglón
segundo renglón
tercer renglón, el último
```

*«Primeira linha», «segunda linha», «terceira linha, a última».*

Na lição 00, sem arquivo, ele ficava esperando. Com um arquivo solta tudo e devolve o prompt.

**Com dois arquivos, ele os encadeia.** **Preveja:** o que sai com `cat corto.txt corto.txt`?

```text
primer renglón
segundo renglón
tercer renglón, el último
primer renglón
segundo renglón
tercer renglón, el último
```

Um atrás do outro, sem separação. Por isso tem esse nome.

### `cat -n`: com o número de cada linha

A opção `-n`, de *number*, põe o número em cada linha:

```bash
cat -n corto.txt
```

```text
     1	primer renglón
     2	segundo renglón
     3	tercer renglón, el último
```

O número **não faz parte do arquivo**: o `cat` o acrescenta ao mostrar. Serve para responder «em que linha está tal
coisa?», que em um plantão vão perguntar.

### O que não cabe na tela

**Preveja antes de rodar:** `cien.txt` tem cem linhas, e a janela tem menos. O que se vai ver com `cat cien.txt`?

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

Vê-se o final, até o `renglón 100 de 100`. O começo saiu da tela: o `cat` não sabe de telas, escreve tudo e o que não
cabe vai para cima. Com cem linhas dá para subir com a roda do mouse; com um milhão, não.

**Daqui sai a regra desta lição:** o `cat` é para arquivos curtos, os que se sabe que cabem. Para os longos, pede-se um
pedaço (passo 3) ou folheiam-se (passo 4). E, para ler um log em um servidor, **escolha um comando de leitura**: ele não
modifica o arquivo e começa melhor com logs longos. Um editor pode alterar o arquivo se uma tecla a mais for tocada, e
um enorme pode demorar muito para abrir.

### Hora de fazer: a configuração

Volte à sala com `cd ~/practica/03`. A configuração do serviço de pagamentos é curta: o `cat` é a ferramenta certa.

1. Mostre-a completa, **com números de linha**.
2. As linhas que começam com `#` são comentários, notas para as pessoas: o serviço não as lê. Repare quais são.
3. Procure a linha que diz quantos segundos o serviço espera entre uma nova tentativa e outra: o nome dela traz a
   palavra `espera_segundos`. Anote **o número dessa linha**:

   ```bash
   anota 03 espera 1
   ```

   Ali, o `1` é um exemplo: no lugar dele vai o número que se leu.

**Comprova** (objetivo 3.1): a marca `espera` do `verifica 03`.

---

## Passo 3 · Só um pedaço: `head`, `tail` e `wc`

Volte ao ensaio com `cd ~/practica/03/ensayo`.

### `head`: o começo

`head` quer dizer «cabeça». Conforme o manual, se não se diz quantas, ele escreve as dez primeiras linhas.
**Preveja:** qual é a última que se vai ver com `head cien.txt`?

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

Do `renglón 001` ao `renglón 010`. Dez, como diz o manual. Com `-n` e um número se diz quantas:

```bash
head -n 3 cien.txt
```

```text
renglón 001 de 100
renglón 002 de 100
renglón 003 de 100
```

### `tail`: o fim

`tail` quer dizer «cauda». Funciona como o `head`, mas a partir do final: dez linhas se não se disser outra coisa, e as
que se pedirem com `-n`.

```bash
tail -n 2 cien.txt
```

```text
renglón 099 de 100
renglón 100 de 100
```

Em um log ordenado, o final é o mais recente, e **o `tail` costuma ser o primeiro olhar em um plantão**: diz o que foi
*registrado* por último. Leia-o com a hora e use-o como pista; em um plantão de verdade, depois se confirma o estado com
o sinal que couber ao serviço.

### Uma linha exata, com o que já se tem

Pedem a linha 42. **Preveja:** qual é a última linha que o `head -n 42` escreve?

```bash
head -n 42 cien.txt
```

Saída recortada: as três últimas das 42 linhas.

```text
renglón 040 de 100
renglón 041 de 100
renglón 042 de 100
```

A última que aparece é o `renglón 042 de 100`. O `head -n 42` escreve as linhas de 1 a 42, então a que fica embaixo,
bem em cima do prompt, **é a 42**. Não é preciso contar nada: lê-se a última. Na lição 06 se verá como ficar só com
essa, sem as 41 de cima. Por ora, esse truque basta.

### `wc`: contar sem ler

`wc` vem de *word count*, «contar palavras», embora conte três coisas. Sem opções, escreve três números e o nome do
arquivo:

```bash
wc cien.txt
```

```text
 100  400 2000 cien.txt
```

**Preveja antes de ler a explicação:** `cien.txt` tem cem linhas, e cada uma diz algo como `renglón 001 de 100`. Quantas
palavras se acha que tem ao todo?

Os três números, em ordem, são: linhas, palavras e bytes. Para `cien.txt` devem ser 100, 400 e 2000. Cada linha tem
quatro palavras, porque para o `wc` uma palavra é qualquer grupo de caracteres sem espaços, segundo o manual dele:
`001` e `100` contam como palavras. E os bytes são o tamanho, o mesmo número que se viu no `ls -l` na lição 01.

Nesta lição basta um: as linhas. Pedem-se com `-l`, de *lines*:

```bash
wc -l cien.txt
```

```text
100 cien.txt
```

**Como ele conta as linhas.** O manual do `wc` diz que o `-l` conta as **quebras de linha**. Se a última linha de um
arquivo não termina com quebra de linha, o `wc -l` não a conta. Nesta sala todos os arquivos terminam com a quebra, mas em
um servidor nem sempre: se o `wc -l` der uma a menos do que se vê, essa é a causa.

**O comando desta imagem, não o do manual.** Aqui `cat`, `head`, `tail` e `wc` são do `rust-coreutils`, como o `ls` na
lição 01; segundo a documentação do Ubuntu, só `cp`, `mv` e `rm` continuam sendo do GNU. Foram testadas as formas que
aparecem nesta lição; os manuais GNU citados explicam a ideia. Se for testar outra opção, olhe primeiro `comando --help`
**nesse** contêiner.

### Hora de fazer: o log e o relatório

Volte à sala com `cd ~/practica/03`. São quatro perguntas. **Antes de cada uma, escreva no papel que pedaço é preciso e
com que comando se vai tirá-lo**; depois se roda e se anota.

1. **O folio da linha 137** de `pagos/pagos.log`. Cada cobrança traz um folio, um F, um hífen e seis algarismos. Anote-o
   com `anota 03 folio-137` e o folio, tal qual.
2. **A hora da última linha** de `pagos/pagos.log`: só a hora, `hh:mm:ss`, sem a data. Anota-se com
   `anota 03 ultima-hora`. Leia a linha completa: é o que se vai dizer a quem está ao telefone.
3. **Quantas linhas tem** `pagos/pagos.log`. Só o número: `anota 03 renglones`.
4. **Quantas filiais** traz `reportes/sucursales.csv`. É um CSV: uma tabela escrita como texto, uma linha por fileira e
   vírgulas entre as colunas. Veja primeiro as três primeiras linhas com `head`. **Previsão D:** o número de filiais é o
   mesmo que o número de linhas? Anote a resposta com `anota 03 sucursales`.

**Comprova** (objetivo 3.2): as marcas `folio-137`, `ultima-hora`, `renglones` e `sucursales` do `verifica 03`, e a
previsão D, que se compara por conta própria no fim.

---

## Passo 4 · Folhear e buscar: `less`

`head` e `tail` servem quando se sabe onde está o que se procura. Quando não, folheia-se com o `less`, um *paginador*:
segundo o autor, um programa que mostra arquivos de texto e **não um editor**. `less arquivo` serve para olhar e não
muda esse arquivo. Fique nas teclas da tabela abaixo; evite `v` e `!`, que podem abrir outros programas.

O manual dele diz outra coisa que importa em um servidor: o `less` não precisa ler o arquivo inteiro antes de começar,
então com arquivos grandes ele começa mais rápido que um editor.

### As teclas já são conhecidas

Volte ao ensaio e abra `cien.txt` com `-N`, em maiúscula: cada linha aparece com número, como no `cat -n`, mas
folheando.

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

A tela se enche com o começo do arquivo, e o número de cada linha está à esquerda: já não se conta. Está-se **dentro**
do `less`, como se esteve dentro do `man` na lição 00, e as teclas são as mesmas que se usaram ali, mais algumas:

| Tecla | O que faz |
|---|---|
| Barra de espaço | Desce uma tela |
| `g` | Ao começo do arquivo |
| `G` | Ao final do arquivo |
| `/` e um texto, depois Enter | Busca esse texto para baixo |
| `n` | A próxima vez que aparece |
| `h` | A ajuda do `less`, com todas as teclas |
| `q` | Sai e devolve ao prompt |

Teste cada uma: algumas telas com a barra, depois `G`, depois `g`. Saia com `q`.

**Ctrl+C não tira daí.** No `less`, Ctrl+C interrompe o que ele estivesse fazendo, por exemplo uma busca longa, e o
deixa esperando a próxima tecla; não o fecha, segundo as perguntas frequentes do autor. Como no `man`: do `less` se sai
com `q`.

### A busca diferencia maiúsculas

Dentro de `less -N cien.txt`, digite `/042` e Enter. Ele salta para a linha 42. Digite `n`: não há outra.

**Preveja:** o que acontece se for buscado `/RENGLÓN`, em maiúsculas?

Embaixo da tela aparece `Pattern not found  (press RETURN)`: não encontra, embora a palavra esteja nas cem linhas. O
manual do `less` diz: **por padrão, a busca diferencia maiúsculas de minúsculas**. Em um log isso importa: `ERROR` e
`error` não têm por que ser a mesma coisa. Aperte Enter e saia com `q`.

### Hora de fazer: o primeiro `ERROR`

O que é preciso está na tabela de cima e na ajuda do `less`, com `h`. Tente primeiro com isso; se travar, explique o que
buscou e que tela viu antes de pedir uma dica.

1. Volte à sala, `cd ~/practica/03`, e abra `pagos/pagos.log` no `less`, **com números de linha**.
2. Antes de buscar, folheie: algumas telas com a barra, o final com `G`, o começo com `g`. Veja como é uma linha normal
   e como é uma que avisa algo.
3. **Previsão E:** o log tem linhas `INFO`, `WARN` e `ERROR`. Espera-se que o primeiro `ERROR` esteja perto do começo ou
   perto do final? Escreva.
4. A partir do começo do arquivo, busque a **primeira linha que diz `ERROR`, em maiúsculas**. Leia essa linha completa
   e as seguintes: dizem por que as recusas começaram.
5. Saia com `q` e anote o **número** dessa linha com `anota 03 primer-error`.

**Comprova** (objetivo 3.3): a marca `primer-error` do `verifica 03`, e a previsão E.

---

## Passo 5 · Antes de abrir, pergunte o que é: `file`

Falta o log de ontem, `pagos/pagos.log.1.gz`. O nome dele termina em `.log`, mais um `.1` e um `.gz`. É texto? Para não
adivinhar existe o `file`: olha o **conteúdo** de um arquivo, não o nome, e diz que tipo de arquivo é. Segundo o manual
dele, procura perto do começo as marcas fixas de cada formato conhecido, e se não se encaixa em nenhum, confere se é
texto.

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

`../pagos/*` é um padrão, como os da lição 02: o terminal o troca por todos os nomes de `pagos`. Todos dizem *text*,
menos um. **O nome não decide:** o `file` reconhece o log de ontem pelo conteúdo, e diz que é gzip.

O `file` é um primeiro sinal, não uma garantia: o manual dele diz que o que ele marca como *text* **provavelmente** se
pode ler sem problema no terminal. Se diz *text*, normalmente se pode lê-lo; se não, não use `cat` até saber que formato
é.

### Por que não se faz `cat` em um binário

O `cat` não pergunta o que é um arquivo: escreve os bytes dele na tela, tal qual. Com um binário são bytes que não são
letras, e entre eles pode haver alguns que o terminal entende como ordens. **Não rode `cat` diretamente sobre este
`.gz`, nem sobre nenhum binário em um terminal de produção.**

Se algum dia o terminal ficar **desarranjado** (o que se digita sai em símbolos estranhos, ou não se vê, ou o Enter
deixa de descer de linha), não quebrou o arquivo nem a máquina. **Digite `reset` e aperte Enter**, mesmo sem ver o que se
digita. Segundo o manual dele, o `reset` devolve o terminal à forma normal depois que um programa o deixa em um estado
anormal, e avisa que às vezes o Enter não responde: nesse caso, use **Ctrl+J**.

Agora sim, uma amostra segura. A barra `|` passa o que um comando escreve para o seguinte; será vista a fundo na 
lição 06. O `head -c 300` tira só os primeiros 300 bytes do arquivo e a barra os passa ao `cat -v`, que troca cada byte que
não é letra por sinais que podem ser escritos. **Preveja:** o que se vai ver?

```bash
head -c 300 ../pagos/pagos.log.1.gz | cat -v
```

Saída recortada: o começo de uma única linha muito longa.

```text
^_M-^K^H^@^@^@^@^@^B^CM-^LM-^]]M-.^^7vEM-_3M-^J;^A^WM-HM-sM-Csh
```

Nada disso são letras de um log: são os bytes comprimidos, traduzidos em sinais. Rode `reset` uma vez, para conhecê-lo:

```bash
reset
```

Não aparecerá texto de resposta: o `reset` envia ordens de controle ao terminal e pode limpar a tela; depois volta o
prompt.

### Hora de fazer: o que é o log de ontem

Com o `file`, pergunte o que é `pagos/pagos.log.1.gz` e anote **a primeira palavra** do que ele responde, depois dos
dois-pontos, com `anota 03 tipo-respaldo`.

**Comprova** (objetivo 3.4): a marca `tipo-respaldo` do `verifica 03`, e as respostas às perguntas 8 e 9 de «Para
fechar».

---

## Passo 6 · Confira com `verifica 03`

Rode `verifica 03` quando quiser, de qualquer pasta. Primeiro ele confere que a sala continue intacta, arquivo por
arquivo; se algo mudou, para e pede que se monte de novo. Depois confere as sete respostas **lendo os arquivos**: a
resposta certa não está escrita no `verifica`, sai do próprio arquivo.

Com a sala recém-montada, a primeira marca sai em verde e as sete respostas em vermelho, porque estão vazias. Assim se lê
um vermelho quando a resposta está errada e não vazia; por exemplo, se em `ultima-hora` se anotasse a hora da **primeira**
linha. Saída recortada: esse vermelho e a linha final.

```text
❌ ultima-hora · 00:00:04 es la hora del PRIMER renglón (medí: la hora del renglón 1). head muestra el principio; el final lo muestra otro comando

❌ Lección 03: falta lo marcado con ❌. Arréglalo y corre  verifica 03  otra vez.
```

*ultima-hora · 00:00:04 é a hora da PRIMEIRA linha (medi: a hora da linha 1). O head mostra o começo; o final, outro
comando mostra.* E a última: *Lição 03: falta o que está marcado com ❌. Corrija e rode `verifica 03` outra vez.*

Diz o que foi anotado, o que foi medido e em que houve confusão, mas não qual é a resposta certa.

Corrija e repita até ver isto no final:

```text
✅ Lección 03: todo en verde.
```

*Lição 03: tudo em verde.*

---

## Os exercícios

O exercício 3.3 se tenta primeiro com a tabela do `less` e a ajuda dele, com `h`; se travar, explique o que buscou e que
tela viu antes de pedir uma dica.

| # | Faz-se | Está feito quando |
|---|---|---|
| 3.1 | Lê-se a configuração completa com números de linha e encontra-se `espera_segundos` | O `verifica 03` marca `espera` em verde |
| 3.2 | Tiram-se a linha 137, a última linha e o total do log, e contam-se as filiais do CSV, com as previsões escritas antes | O `verifica 03` marca as quatro em verde, e a previsão D foi comparada |
| 3.3 | Folheia-se o log no `less` e encontra-se o primeiro `ERROR` | O `verifica 03` marca `primer-error` em verde, e a previsão E foi comparada |
| 3.4 | Pergunta-se ao `file` o que é o log de ontem, olha-se uma amostra segura com `head -c 300` e `cat -v`, e usa-se o `reset` | O `verifica 03` sai todo em verde, e as perguntas 8 e 9 de «Para fechar» foram respondidas |

**Tempo (estimado):** de 60 a 90 minutos. É uma estimativa: ajusta-se com o primeiro grupo que fizer o curso.

---

## Não funcionou?

| Aparece | Causa provável | O que fazer |
|---|---|---|
| `Todavía no hay prepara para la lección «03».` *(Ainda não há prepara para a lição «03».)* | A imagem é de antes desta lição | Sair com `exit` e rodar `./practicar reconstruir` |
| `Ya existe /home/alumno/practica/03.` *(Já existe /home/alumno/practica/03.)* e pede que se digite `si` | A sala já tinha sido montada, e montá-la de novo a apaga inteira, sem desfazer; a folha de respostas fica | Para começar do zero, digitar `si` («sim») e Enter. Com qualquer outra coisa responde `No se borró nada.` *(Nada foi apagado.)* |
| A tela do `less` não sai e o prompt não volta | Continua-se dentro do `less` | `q`. Ctrl+C não o fecha |
| O que se digita sai em símbolos estranhos, ou não se vê | Fez-se `cat` em um arquivo que não é texto | Digitar `reset` e Enter, mesmo sem ver. Se o Enter não responde, usar Ctrl+J |
| `❌ La sala no está intacta` *(A sala não está intacta)* | Um arquivo da sala mudou | `prepara 03`. A folha de respostas não é tocada |

---

## Para fechar

**Responda sem reler.**

**Quatro situações: que pedaço é preciso, e com que comando se tira?** Passa-se com 3 de 4.

1. Perguntam o que está acontecendo agora em um serviço, segundo o log dele.
2. Entregam um arquivo de configuração de quinze linhas e pedem que seja revisado inteiro.
3. Perguntam quantos registros traz um CSV de quarenta mil linhas, com cabeçalho.
4. Dizem que o problema começou «em algum momento da madrugada» e o log tem dez mil linhas.

**E cinco perguntas:**

5. Como se tira a linha 250 de um arquivo com o que se viu nesta lição?
6. Buscou-se `/error` no `less` e não se achou nada, mas se sabe que há erros. O que pode estar acontecendo?
7. O `wc -l` diz 99, e se contam cem linhas na tela. O que pode estar acontecendo?
8. Encontra-se um arquivo chamado `respaldo.txt`. O que se roda antes de fazer `cat` nele, e por que o nome não basta?
9. Fez-se `cat` em um binário e agora o que se digita não se vê. O que se faz?

**O que vem a seguir:** já se lê um arquivo por pedaços, sem abrir um editor. Na lição 04 buscam-se **arquivos** em uma
árvore com o `find`, quando nem se sabe em que pasta estão.

**Mais adiante:** ver um log crescer com `tail -f` chega na lição 10, e ler arquivos comprimidos, na 06.

---

## Compare as respostas

**Previsão D (as filiais).** Não é o mesmo número. A primeira linha do CSV é o **cabeçalho**: diz como se chama cada
coluna (`sucursal,ciudad,terminales,hora_del_reporte`) e não é uma filial. O `wc -l` conta todas as linhas, cabeçalho
incluído, então as filiais são uma a menos. Por isso convinha olhar com o `head` antes de contar: o número do `wc` só
significa algo quando se sabe o que há em cada linha.

**Previsão E (o primeiro `ERROR`).** Qualquer uma das duas vale se foi escrita antes: o que importa é que se comprovou.
Se se folheou antes de buscar, viu-se que o log avisa com `WARN` várias vezes antes do primeiro `ERROR`: os problemas
quase nunca começam com um erro, começam com avisos que ninguém leu. E se no caminho apareceu a palavra `error` em
minúsculas, é outra linha, com outra história: por isso o exercício pedia `ERROR` tal qual.

**Exercício 3.3.** O controle é o `verifica 03`; aqui não vai o número. Para saber se está indo bem: abriu-se com
`less -N`, foi-se ao começo com `g`, buscou-se com `/` e o texto exato, e o número se leu à esquerda da linha, sem
contar.

**Para fechar.**

1. O final: `tail` sobre o log, ou `tail -n` com as linhas que se quiser. Diz o que foi registrado por último: leia-o
   com a hora, use-o como pista e depois confirme o estado com o sinal que couber ao serviço.
2. Tudo: `cat`, melhor com `-n`. Quinze linhas cabem na tela.
3. Quantos são: `wc -l`, menos um pelo cabeçalho. Antes, `head` para confirmar que a primeira linha é o cabeçalho.
4. Folhear e buscar: `less`, melhor com `-N`, buscando com `/` o que distinga a linha do problema, como `ERROR`.
5. `head -n 250` e a última linha que ele escreve é a 250.
6. A busca do `less` diferencia maiúsculas de minúsculas: os erros podem estar escritos `ERROR` ou `Error`. Tente com
   outra forma, ou confira a ajuda do `less` com `h`.
7. À última linha falta a quebra de linha. O `wc -l` conta quebras, não linhas à vista.
8. `file respaldo.txt`. O nome é posto por uma pessoa e pode não dizer a verdade; o `file` olha o conteúdo. Se não diz
   *text*, não se usa `cat` até saber que formato é.
9. Digita-se `reset` e Enter, mesmo sem ver. Se o Enter não responde, Ctrl+J no lugar.

---

## Fontes da lição

Abertas em 24/11/2026. Todas estão em inglês.

- man7.org, *The Linux man-pages project*. *cat(1)*, do GNU coreutils (encadeia arquivos; `-n` numera as
  linhas). https://man7.org/linux/man-pages/man1/cat.1.html
- man7.org. *head(1)*, do GNU coreutils (as dez primeiras linhas por padrão; `-n`).
  https://man7.org/linux/man-pages/man1/head.1.html
- man7.org. *tail(1)*, do GNU coreutils (as dez últimas linhas por padrão; `-n`).
  https://man7.org/linux/man-pages/man1/tail.1.html
- man7.org. *wc(1)*, do GNU coreutils (linhas, palavras e bytes, nessa ordem; `-l` conta quebras de linha; o que é uma
  palavra). https://man7.org/linux/man-pages/man1/wc.1.html
- Ubuntu Manpages. *less(1)*, Ubuntu 26.04, less 668 (não lê o arquivo inteiro antes de começar; barra, `g`, `G`,
  `/`, `n`, `h`, `q`, `-N`; a busca diferencia maiúsculas por padrão).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/less.1.html
- Mark Nudelman, autor do `less`. *Less FAQ* (o `less` é um paginador e não um editor; Ctrl+C não o fecha).
  https://www.greenwoodsoftware.com/less/faq.html
- Ubuntu Manpages. *file(1)*, Ubuntu 26.04, file 5.46 (como decide o tipo pelo conteúdo; o que quer dizer
  *text*: provavelmente seguro de ler no terminal). https://manpages.ubuntu.com/manpages/resolute/en/man1/file.1.html
- man7.org. *tset(1)*, do ncurses (o `reset` devolve o terminal a valores normais depois que um programa o deixa
  em um estado anormal; pode não se ver o que se digita e pode ser preciso Ctrl+J no lugar do Enter).
  https://man7.org/linux/man-pages/man1/tset.1.html
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, seção «rust-coreutils» (os utilitários básicos vêm do
  `rust-coreutils`; `cp`, `mv` e `rm` continuam sendo do GNU).
  https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- Adam Wiggins. *The Twelve-Factor App*, «XI. Logs» (em estado bruto, um log costuma ser texto, em ordem de tempo, com
  um evento por linha; um rastro de erro pode ocupar várias).
  https://12factor.net/logs
