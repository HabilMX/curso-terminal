# Lição 02 — Criar, copiar, mover e apagar sem medo

**Por Dorian Chávez, fundador da Hábil e arquiteto de integração.**

**Antes de ler, quatro palavras.** Uma linha cada uma; as demais chegam no passo em que são usadas.

- **Origem e destino:** ao copiar ou mover, a origem é o que já se tem e o destino é para onde vai. Em um comando, a
  origem se escreve primeiro e o destino no final.
- **Sobrescrever:** escrever por cima de um arquivo que já existia. O que ele tinha antes se perde.
- **Lixeira:** o lugar onde o computador guarda o que foi apagado, para o caso de arrependimento. No terminal não há.
- **Padrão:** um nome com curingas, como `*.log`, que pega vários nomes de uma vez. O `*` quer dizer «qualquer coisa».

**O que fica com quem termina:** criam-se pastas e arquivos com `mkdir` e `touch`; copia-se, move-se e renomeia-se com
`cp` e `mv`, e sabe-se o que acontece quando o destino já existe; apaga-se com `rm` e `rmdir`, vendo antes o que vai
embora; e sabe-se explicar por que uma variável vazia transforma um apagamento pequeno em um enorme.

**Ideia nova desta lição (a única que se avalia):** no terminal não há lixeira. O que o `rm` apaga não se recupera, e o
`cp` e o `mv` sobrescrevem sem perguntar. Por isso **pensa-se antes de apagar**: primeiro se olha o que vai embora, e
depois se apaga.

**Em que ordem vai:** monta-se uma oficina de prática (passo 1), cria-se (passo 2), copia-se, move-se e renomeia-se
(passo 3), apaga-se com cuidado (passo 4), vê-se o perigo da variável vazia sem rodá-lo (passo 5) e, no fim, confere-se
tudo com o `verifica` (passo 6).

**Uma nota sobre o idioma da oficina.** Como nas lições 00 e 01, as mensagens do curso e os nomes da oficina estão em
espanhol e fazem parte do código, então não se traduzem. As mensagens aparecem exatamente como o terminal as imprime,
com uma glosa em português, em itálico, abaixo de cada bloco, ou entre parênteses dentro de uma tabela. Os nomes da
oficina são glosados uma só vez, no passo 1: `taller` («oficina»), `config` («configuração»), `plantillas` («modelos»),
`respaldo` («backup»), `bitacoras` («logs»), `entrada` («entrada»), `reportes` («relatórios»), `limpieza` («limpeza»),
`ensayo` («ensaio») e os demais. A chave da folha de respostas é glosada no passo 4: `patron` («padrão»). Os comandos
`practicar`, `prepara`, `verifica` e `anota` também mantêm o nome: já se conhecem das lições 00 e 01.

---

## A cena

*Cenário inventado; as telas que aparecem na lição são reais.*

Quarta-feira, 00h07. Lucía encontrou o log de pagamentos. A última linha diz:

```text
2025-06-02 00:09 pagos: disco lleno, no puedo escribir
```

*2025-06-02 00:09 pagamentos: disco cheio, não consigo escrever.*

Do outro lado do telefone dão a ela três instruções:

«Antes de mexer em qualquer coisa, faça o backup da configuração. Depois deixe o relatório de junho na pasta certa. E
apague os temporários do cache, que foram os que encheram o disco.»

No cache há arquivos que terminam em `.tmp`, e também um, `cache-indice.txt` («cache-índice.txt»), sem o qual o
serviço não volta a subir. Lucía sabe uma coisa sobre o comando de apagar: ele não pergunta, e não há lixeira de onde
tirar o que ele levar.

Esta lição é essa meia hora, em uma oficina feita para errar sem consequências.

---

## Antes de começar

**O que é preciso:** a lição 01 feita; o material do curso atualizado; e papel e lápis para as previsões.

**Atualize o terminal de prática.** Como na lição 01, a partir do computador, na pasta do curso:

```bash
git pull
./practicar reconstruir
```

Isso basta para receber esta lição: o `./practicar reconstruir` reaproveita o Ubuntu e os pacotes já construídos e só
coloca os arquivos novos. No Mac do teste levou menos de dois segundos e terminou com `✅ Imagen lista.` («Imagem
pronta.») A pasta `~/practica` não é tocada. Depois, entra-se com `./practicar`.

**Onde esta lição foi testada:**

| Sistema | Estado |
|---|---|
| Mac com Apple silicon, Docker 29.2.1 rodando com Colima | Testado em 19/11/2026: `./practicar reconstruir`, `prepara 02` duas vezes com o mesmo resultado, cada comando dos passos 1 a 6, e `verifica 02` em vermelho e em verde |
| Linux Mint 22.3 (x86_64), Docker Engine 29.10.0 | Testado em 19/11/2026: `./practicar reconstruir`, `prepara 02` duas vezes com o mesmo resultado do Mac, as mensagens do `mkdir`, do `touch` e do `rmdir`, as perguntas do `cp -i`, do `mv -i`, do `rm -i` e do `rm -I`, a lição completa como aluno, e `verifica 02` em verde e em vermelho com um link físico |
| Ubuntu, Mac com Docker Desktop, Windows com Docker Desktop e WSL 2 | Ainda sem testar com esta lição |

**O que vai coincidir com a tela e o que não.** Coincidem os nomes, os conteúdos, as datas da oficina e as mensagens
dos comandos. Mudam a hora do que for criado ou tocado, a disposição em colunas e as cores.

---

## Passo 1 · Monte a oficina

Dentro do terminal de prática:

```bash
prepara 02
```

```text
✅ Taller listo en /home/alumno/practica/02 (medí: 12 carpetas y 22 archivos, cada archivo con su contenido).
   Te dejé la hoja de respuestas, con su clave vacía: /home/alumno/practica/respuestas/02.md
   Empieza por ahí:  cd ~/practica/02
```

*Oficina pronta em `/home/alumno/practica/02` (medi: 12 pastas e 22 arquivos, cada arquivo com o próprio conteúdo).
Deixei a folha de respostas, com a chave vazia: `/home/alumno/practica/respuestas/02.md`. Comece por aí:
`cd ~/practica/02`.*

O `prepara 02` funciona como o da 01: não tem nada de aleatório, mexe só em `~/practica/02` e na folha de respostas, e
para montar a oficina de novo ele a apaga, então pede que se digite `si` («sim»). Cada arquivo traz uma linha de texto
diferente: pelo conteúdo se percebe se uma cópia é a boa ou se algo foi sobrescrito.

**A oficina, vista de cima:**

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

*As setas dizem: «o que entra no backup», «o backup, com uma versão antiga do pagos.conf», «o relatório novo», «já há
um relatório com o mesmo nome: o rascunho antigo», «o do exercício de apagar» e «para testar os comandos antes de usá-los
para valer». Os nomes, em espanhol: `LEEME.txt` («LEIAME.txt»), `config` («configuração»), `pagos.conf`
(«pagamentos.conf»), `plantillas` («modelos»), `aviso de corte.txt` («aviso de corte.txt»), `aviso.txt`
(«aviso.txt»), `recibo.txt` («recibo.txt»), `respaldo` («backup»), `bitacoras` («logs»), `ayer` («ontem»), `hoy`
(«hoje»), `entrada` («entrada»), `reporte-junio.txt` («relatório-junho.txt»), `reportes` («relatórios»), `limpieza`
(«limpeza»), `cache-indice.txt` («cache-índice.txt»), `tmp-notas.txt` («tmp-notas.txt»), `sesiones-activas` («sessões
ativas»), `sesiones-viejas` («sessões antigas»), `vacia` («vazia»), `ensayo` («ensaio»), `uno`, `dos` e `tres` («um»,
«dois» e «três») e `caja` («caixa»). `adentro.txt` é «dentro.txt».*

As demonstrações vão em `ensayo`; os exercícios, no resto.

**Comprova:** ainda nada. Este passo deixa o material pronto.

---

## Passo 2 · Criar: `mkdir` e `touch`

Entre no ensaio:

```bash
cd ~/practica/02/ensayo
ls
```

```text
caja  dos.txt  tres.log  uno.txt
```

### `mkdir`: uma pasta nova

O `mkdir` vem de *make directory*, «criar pasta». Escreve-se `mkdir` e o nome:

```bash
mkdir nueva
```

Silêncio: deu certo. (`nueva` é «nova».) **Preveja:** o que acontece se a mesma linha for rodada outra vez?

```text
mkdir: nueva: File exists
```

«Já existe.» O `mkdir` não sobrescreve uma pasta: recusa, e nada se perde.

**Várias pastas, uma dentro da outra.** **Preveja:** `uno` não existe. O que acontece com `mkdir uno/dos/tres`?

```text
mkdir: No such file or directory
```

Falhou, porque o `mkdir` cria a última pasta do caminho e espera que as anteriores já existam. Para criar o caminho
inteiro de uma vez existe a opção `-p`, de *parents*, «as de cima também»:

```bash
mkdir -p uno/dos/tres
ls uno/dos
```

```text
tres
```

Com o `-p`, se a pasta já existia, ele não reclama.

### `touch`: um arquivo vazio

O `touch` quer dizer «tocar». Se o arquivo não existe, cria-o vazio:

```bash
touch nota.txt
ls -l nota.txt
```

```text
-rw-r--r-- 1 alumno alumno 0 Nov 19 09:28 nota.txt
```

Zero bytes, e a hora de agora. (`nota.txt` é «nota.txt».) **Preveja:** `uno.txt` já existe e diz `uno`. Se for tocado,
perde o conteúdo?

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

Não perde: sobre um arquivo que existe, o `touch` só muda a data dele para a de agora. O `cat` mostra o que um arquivo
tem; vê-se a fundo na lição 03.

### O que se veria diferente em outro Linux

Nesta imagem, `mkdir`, `touch` e `rmdir` vêm do `rust-coreutils`, como o `ls` da lição 01; `cp`, `mv` e `rm` continuam
sendo do GNU. Dizem-no o `--version` e a documentação do Ubuntu. O que se nota é o texto de dois erros do `mkdir`. O do
GNU continua instalado como `gnumkdir`, e respondeu assim:

| Comando | O `mkdir` desta imagem | O `gnumkdir`, o do GNU |
|---|---|---|
| Criar `nueva`, que já existe | `mkdir: nueva: File exists` | `gnumkdir: cannot create directory ‘nueva’: File exists` |
| Criar `uno/dos/tres`, sem `-p` | `mkdir: No such file or directory` | `gnumkdir: cannot create directory ‘uno/dos/tres’: No such file or directory` |

Em um Linux com GNU, o texto que apareceria é o segundo, começando com `mkdir:`. O que importa: aqui o erro **não diz
qual caminho falhou**. Se acontecer com várias pastas na mesma linha, confira cada uma com `ls`. Os erros do `rmdir` e
do `touch` saíram iguais aos do GNU.

### Hora de fazer: a árvore do plantão

Volte à oficina com `cd ~/practica/02` e crie, exatamente assim, a pasta do plantão da Lucía (`guardia`, «plantão»):

```text
~/practica/02
└── guardia
    └── 2025-06-02
        ├── bitacoras/
        ├── reportes/
        │   └── resumen.txt
        └── pendientes.txt
```

As que terminam em `/` são pastas; as outras duas, arquivos vazios. (`resumen.txt` é «resumo.txt» e `pendientes.txt`,
«pendentes.txt».) Dica: `mkdir -p` e `touch` aceitam vários caminhos na mesma linha. No fim, `ls guardia/2025-06-02`
deve mostrar `bitacoras  pendientes.txt  reportes`.

**Comprova** (objetivo 2.1): a primeira marca do `verifica 02`, que confere a árvore **exatamente**: cada pasta e cada
arquivo, do tipo certo, e nada a mais. Um erro de digitação como `reprotes` sai em vermelho.

---

## Passo 3 · Copiar, mover e renomear: `cp` e `mv`

Volte ao ensaio com `cd ~/practica/02/ensayo`.

### `cp`: copiar

O `cp` vem de *copy*. Escreve-se `cp`, a origem e o destino. Veja primeiro o que diz o `dos.txt`:

```bash
cat dos.txt
```

```text
dos
```

**Preveja antes de rodar a próxima linha.** `dos.txt` já existe. Se `uno.txt` for copiado por cima, ele pergunta alguma
coisa? O que diz o `dos.txt` depois?

```bash
cp uno.txt dos.txt
cat dos.txt
```

```text
uno
```

Não perguntou nada. **Sobrescreveu**, em silêncio. O `dos` que havia ali dentro já não existe em lugar nenhum.

**Para que pergunte: `-i`,** de *interactive*. Pergunta antes de sobrescrever, e se a resposta for `n`, não copia:

```bash
cp -i tres.log uno.txt
```

```text
cp: overwrite 'uno.txt'? n
```

«Sobrescrevo `uno.txt`?» Com `n`, o `uno.txt` continua dizendo `uno`. Com `y`, de *yes*, copia.

**Copiar para uma pasta.** Se o destino é uma pasta que já existe, o `cp` deixa dentro dela a cópia, com o mesmo nome:

```bash
cp caja/adentro.txt nueva
ls nueva
```

```text
adentro.txt
```

### `cp -r`: copiar uma pasta inteira

**Preveja:** o `cp` copia uma pasta como copia um arquivo?

```bash
cp caja otra
```

```text
cp: -r not specified; omitting directory 'caja'
```

«Faltou o `-r`; pulo `caja`.» (`otra` é «outra».) Para copiar uma pasta com tudo o que ela tem, vai o `-r`, de
*recursive*, «até o fundo»:

```bash
cp -r caja otra
ls otra
```

```text
adentro.txt
```

**A armadilha do `cp -r`.** **Preveja:** roda-se exatamente a mesma linha outra vez. `otra` já existe. O que sobra?

```bash
cp -r caja otra
ls otra
```

```text
adentro.txt  caja
```

Na segunda vez o destino já era uma pasta, então o `cp` pôs a cópia **dentro**: há uma `caja` dentro de `otra`. A mesma
linha faz duas coisas, conforme o destino exista ou não. Antes de copiar uma pasta, veja com `ls` se o destino já está
ali.

### `mv`: mover e renomear

O `mv` vem de *move*. Funciona como o `cp`, com uma diferença: **a origem desaparece**.

**Renomear é mover no mesmo lugar:**

```bash
mv uno.txt primero.txt
ls
```

```text
caja  dos.txt  nota.txt  nueva  otra  primero.txt  tres.log  uno
```

Já não há `uno.txt`. É o mesmo arquivo, com outro nome. (`primero` é «primeiro».)

**Preveja:** `dos.txt` existe. O que acontece se `tres.log` for movido por cima?

```bash
mv tres.log dos.txt
cat dos.txt
```

```text
tres
```

Como o `cp`: **sobrescreveu sem perguntar**, e `tres.log` já não existe. O `mv -i` pergunta antes:

```bash
mv -i primero.txt dos.txt
```

```text
mv: overwrite 'dos.txt'? n
```

*(«Sobrescrevo `dos.txt`?» Com `n`, nada é movido.)*

E se o destino é uma pasta, o `mv` põe o arquivo dentro dela, como o `cp`:

```bash
mv nota.txt nueva
ls nueva
```

```text
adentro.txt  nota.txt
```

**Resumo:**

| Comando | A origem | Se o destino é um arquivo que existe | Se o destino é uma pasta |
|---|---|---|---|
| `cp` | Fica | Sobrescreve sem perguntar | Copia para dentro |
| `cp -r` | Fica | Igual ao `cp` | Copia para dentro, também na segunda vez |
| `mv` | Desaparece | Sobrescreve sem perguntar | Move para dentro |
| `cp -i`, `mv -i` | Igual | Pergunta antes | Igual |

### Hora de fazer: o backup e o relatório

Volte a `~/practica/02`. São quatro tarefas. **Antes de cada uma, escreva no papel o que se espera que aconteça**; as
três previsões marcadas se comparam no fim, em «Compare as respostas».

1. **Copie** `config/pagos.conf` por cima de `respaldo/pagos.conf`. Veja antes o que diz o backup, com `cat`.
   *Previsão A:* vai perguntar? O que dirá `respaldo/pagos.conf` no fim?
2. **Copie** a pasta `config/plantillas` inteira para `respaldo/plantillas`, **uma única vez**.
   *Previsão B:* se for copiada duas vezes com a mesma linha, o que sobra em `respaldo/plantillas`?
3. **Renomeie** `bitacoras/hoy.log` para `bitacoras/2025-06-02.log`.
4. **Mova** `entrada/reporte-junio.txt` para `reportes`, onde já há um `reporte-junio.txt`.
   *Previsão C:* o que acontece com o rascunho antigo de `reportes`? E com a pasta `entrada`?

Os comandos são montados por quem estuda, com o que se viu no ensaio.

**Com o Claude Code.** Neste curso um assistente de IA entra de um único jeito: prevê-se, pede-se, lê-se, roda-se e
explica-se. Para apagar há mais uma regra: nunca se pede o comando; pede-se que ele explique o comando que se
escreveu. O exercício com o Claude Code desta lição, sobre o `mv -n`, está em «Se quiser saber mais».

**Comprova** (objetivo 2.2): as marcas dois a cinco do `verifica 02`, e as previsões A, B e C, que se comparam por
conta própria. O `verifica` compara **conteúdos**, não só nomes: sabe se o backup traz a versão boa ou a antiga, e se a
cópia de `plantillas` tem o mesmo que o original.

---

## Passo 4 · Apagar: `rm` e `rmdir`

Antes de escrever um único `rm`, o que torna esta parte diferente:

- **Não há lixeira.** O `rm` não manda para lugar nenhum: apaga.
- **Nesta oficina, o `rm` normalmente não pergunta:** os arquivos são de quem estuda e podem ser escritos. Em outros
  casos o `rm` do GNU pode perguntar, mas não se deve depender dessa pergunta: olha-se primeiro com `ls`, e usa-se
  `rm -i` quando se quer que ele confirme.
- **Todo comando que apaga é escrito à mão**, também os exemplos deste passo: nem copiado da lição nem pedido a uma IA.

Continue no ensaio, `cd ~/practica/02/ensayo`.

### `rm`: apagar um arquivo

O `rm` vem de *remove*, «remover»:

```bash
rm primero.txt
ls
```

```text
caja  dos.txt  nueva  otra  uno
```

Sem perguntar e sem mensagem. `primero.txt` já não existe.

**Preveja:** o `rm` apaga uma pasta?

```bash
rm caja
```

```text
rm: cannot remove 'caja': Is a directory
```

«Não posso: é uma pasta.» Assim vem por padrão: o `rm` só apaga arquivos.

### `rmdir` e `rm -r`: apagar pastas

O `rmdir` vem de *remove directory*. **Preveja:** `caja` tem um arquivo dentro. Ele a apaga?

```bash
rmdir caja
```

```text
rmdir: failed to remove 'caja': Directory not empty
```

Não. O `rmdir` **só apaga pastas vazias**, e isso o torna seguro: se havia algo dentro, ele se recusa. Com uma vazia
funciona: `rmdir uno/dos/tres` apaga a `tres` criada no passo 2, em silêncio.

*(«Falhou ao remover `caja`: diretório não vazio.»)*

Para apagar uma pasta **com tudo o que ela tem dentro** existe o `rm -r`, o mesmo `-r` do `cp`:

```bash
rm -r caja
```

Sem perguntar, `caja` e o que ela tinha foram embora. O `rm -r` é o comando mais perigoso desta lição: não olha quanto há
dentro.

### `rm -i`: que pergunte

```bash
rm -i dos.txt
```

```text
rm: remove regular file 'dos.txt'? n
```

«Apago o arquivo normal `dos.txt`?» Com `n`, ele fica. Com um arquivo vazio, diz `regular empty file`.

### Os padrões: ver antes o que vai embora

Crie três arquivos de teste:

```bash
touch a.bak b.bak c.bak.txt
```

**Preveja:** quais o padrão `*.bak` pega? O `*` é «qualquer coisa», então `*.bak` é «qualquer nome que termine em
`.bak`».

```bash
ls *.bak
```

```text
a.bak  b.bak
```

`c.bak.txt` não entrou: termina em `.txt`. **Um padrão é exato**, e por isso é preciso olhá-lo antes de usá-lo.

**O padrão não é lido pelo `ls`: é lido pelo terminal.** Antes de rodar o comando, o bash troca `*.bak` pela lista de
nomes que ele pega, e o `ls` recebe essa lista. O `echo`, que repete o que se escreve, deixa ver:

```bash
echo *.bak
```

```text
a.bak b.bak
```

Isso é exatamente o que o `rm *.bak` receberia: `rm a.bak b.bak`. **Daí sai o hábito desta lição:**

1. Escreva o padrão com `ls` e leia a lista.
2. Se for justo o que se quer apagar, escreva `rm` com **o mesmo padrão**, letra por letra.
3. Se a lista trouxer algo a mais, mude o padrão e volte ao 1.

E quando o padrão não pega nada, o bash o deixa como está:

```bash
ls *.zip
```

```text
ls: cannot access '*.zip': No such file or directory
```

*(«O `ls` não consegue acessar `*.zip`: não existe esse arquivo ou pasta.»)*

O `echo *.zip` responde `*.zip`. Não há arquivos `.zip`: o `ls` procurou um que se chamasse, literalmente, `*.zip`.

**Com o `-i`, o padrão pergunta um por um.** Digite-o à mão: `rm -i *.bak`. Responda `y` ao primeiro e `n` ao segundo:

```text
rm: remove regular empty file 'a.bak'? y
rm: remove regular empty file 'b.bak'? n
```

*(«Removo o arquivo normal vazio `a.bak`?» e o mesmo para `b.bak`: o primeiro foi apagado, o segundo ficou.)*

Com trezentos arquivos, o hábito de cima vale mais que o `-i`.

### Hora de fazer, sem IA: a limpeza

Este exercício se faz **sem IA e sem copiar comandos**: nem desta lição nem de qualquer outro lugar. Vai-se a
`~/practica/02/limpieza` e faz-se o que pediram à Lucía:

1. **Apague os arquivos cujo nome termina em `.tmp`, e nada mais.** Primeiro escreva um padrão com `ls` e leia o que ele
   pega. Atenção com os vizinhos: `cache-indice.txt` e `tmp-notas.txt` ficam.
2. **Anote o padrão** com `anota`, **entre aspas simples**. Para um padrão `ejemplo*` («exemplo*») seria
   `anota 02 patron 'ejemplo*'`. (`patron` é a chave da folha de respostas: «padrão».) Sem aspas, o terminal trocaria o
   padrão pelos nomes que ele pega antes que o `anota` o visse.
3. **Apague os `.tmp`** com um `rm` escrito à mão, com o mesmo padrão.
4. **Apague a pasta `sesiones-viejas`**, com tudo o que ela tem, e **a pasta `vacia`**. Comece com `rmdir` nas duas e leia
   o que cada uma responde: ele diz qual precisa de outra coisa. `sesiones-activas` fica.
5. Rode `verifica 02`.

Se algo for apagado a mais **nesta oficina**, não acontece nada: o `prepara 02` o restaura. Use-o para praticar a pausa
de olhar antes de apagar.

**Comprova** (objetivo 2.3, primeira parte): as marcas seis a oito do `verifica 02`. A seis testa o padrão contra os oito
nomes que o `prepara` pôs em `limpieza`. A sete confere que o que foi pedido já não exista. A oito, que **todo o resto
continue ali, com o conteúdo**. Essas duas importam igual: apagar de menos se conserta; apagar a mais, não.

---

## Passo 5 · A variável vazia, vista só com `echo`

⛔ **Neste passo nenhum `rm` é rodado.** Tudo se vê com `echo` na frente, que só escreve o comando na tela. Se uma
destas linhas for digitada sem o `echo`, não se está fazendo o exercício.

### O apagamento que cresce sozinho

Os scripts de manutenção guardam a pasta que limpam em uma **variável**, `CARPETA` («pasta»; o mínimo, no quadro no fim
deste passo), e apagam com uma linha assim: `rm -rf "$CARPETA/"`. O `-f`, de *force*, diz ao `rm` que não pergunte e que
não reclame se algo não existir.

**Preveja:** se `CARPETA` estiver vazia, o que essa linha pede para apagar? Escreva isto, com o `echo`:

```bash
CARPETA=
echo rm -rf "$CARPETA/"
```

```text
rm -rf /
```

A variável vazia desapareceu, e a barra que vinha depois ficou sozinha. **`/` é a raiz: o sistema inteiro.** Um script
que devia apagar uma pasta de prática pede para apagar tudo. Se no fim da linha houver um `*`, é pior:

```bash
echo rm -rf "$CARPETA"/*
```

```text
rm -rf /bin /boot /dev /etc /home /lib /media /mnt /opt /proc /root /run /sbin /srv /sys /tmp /usr /var
```

Cada pasta da raiz. Segundo o manual, o `rm` do GNU se recusa a apagar `/` assim, tal qual (`--preserve-root`); com o `*`
ele já não recebe `/`, e sim esta lista. Não o testamos, e não é para testar.

**Um caso relatado.** Em janeiro de 2015, o issue público #3671 do Steam para Linux descreveu uma perda de arquivos
após a execução de `steam.sh`. Comentários do tópico apontaram `rm -rf "$STEAMROOT/"*` e o risco caso `STEAMROOT`
ficasse vazia. É uma razão para validar variáveis; não é preciso testar o incidente para aprender a regra.

### Por que não se testa, nem no contêiner

**O contêiner é descartável. A pasta `~/practica`, não:** ela vive em um volume que sobrevive ao contêiner. Veja de quem
é cada coisa:

```bash
ls -ld / /usr /etc ~/practica
```

```text
drwxr-xr-x 1 root   root   4096 Nov 19 09:28 /
drwxr-xr-x 1 root   root   4096 Nov 19 09:28 /etc
drwxr-xr-x 4 alumno alumno 4096 Nov 19 09:28 /home/alumno/practica
drwxr-xr-x 1 root   root   4096 Nov  6 10:27 /usr
```

As pastas do sistema são do `root`, o administrador: `touch /usr/prueba` («teste») responde `Permission denied`
(«permissão negada»). Mas `~/practica` é de quem estuda, e o que é de quem estuda pode ser apagado. As respostas dessa
pasta não voltam com o `./practicar`.

### O freio: `${CARPETA:?}`

Os scripts se protegem com um freio que tem nome: `${CARPETA:?}`. Se a variável estiver vazia, o terminal se recusa a
rodar a linha. Com `echo` na frente, como sempre neste passo:

```bash
CARPETA=
echo rm -rf "${CARPETA:?}/"
```

```text
bash: CARPETA: parameter null or not set
```

«A variável está vazia ou não existe.» E o `echo` **não rodou**: segundo o manual do bash, o terminal cancela o comando,
e com um `rm` também não rodaria. Para quem estuda basta isto: **se um `rm` leva uma variável, a variável vai como
`${NOME:?}`**.

> **O mínimo de uma variável.** `CARPETA=ensayo` guarda `ensayo` com o nome `CARPETA`: tudo junto, sem espaços. Com `$`
> na frente, `$CARPETA`, o terminal troca o nome pelo valor antes de rodar o comando, como troca um padrão pela lista.
> Uma variável vazia, ou que nunca guardou nada, vale nada, **sem dar erro**. Este curso não vai além disso.

**Comprova** (objetivo 2.3, segunda parte): a explicação das perguntas 9 e 10 de «Para fechar», comparada com «Compare
as respostas».

---

## Passo 6 · Confira com `verifica 02`

Rode `verifica 02` quando quiser, de qualquer pasta. Com a oficina recém-montada, sete marcas saem em vermelho e uma em
verde: a última, porque ainda não se apagou nada que devia ficar.

Assim se lê um vermelho da marca do padrão, com o `ejemplo*` usado acima como amostra:

```text
❌ 2.3 · Tu patrón «ejemplo*» deja fuera «cache-01.tmp», que sí se borra (medí: probé tu patrón contra los 8 nombres que prepara puso en limpieza)
```

*O padrão «ejemplo*» deixa de fora «cache-01.tmp», que é apagado (medi: testei o padrão contra os 8 nomes que o prepara
pôs em limpieza).*

Diz o que foi anotado, o que foi medido e o que falhou, mas não qual é o padrão certo.

**O que não o engana:** um link, o atalho da lição 01. Um link para `config/pagos.conf` «diz» o mesmo que uma cópia, mas
não é um backup: se o original se perde, o link não tem nada. O `verifica 02` confere o caminho real de cada coisa, e um
link no lugar de um arquivo ou de uma pasta sai em vermelho. Também não o engana um **link físico** (`ln`): um segundo
nome para o mesmo arquivo, que parece um arquivo normal. Se um mudar, o outro muda. O `verifica` percebe porque os dois
nomes compartilham o *inode*, o número com que o disco conhece o arquivo.

Corrija e repita até ver isto no final:

```text
✅ Lección 02: todo en verde.
```

*Lição 02: tudo em verde.*

---

## Os exercícios

O exercício 2.3 se faz **sem IA**, e todos os comandos de apagar são escritos à mão.

| # | Faz-se | Está feito quando |
|---|---|---|
| 2.1 | Cria-se a árvore do plantão do passo 2 | O `verifica 02` marca a árvore em verde |
| 2.2 | As quatro tarefas do passo 3, com as previsões A, B e C escritas antes | O `verifica 02` marca as quatro em verde, e as previsões foram comparadas com «Compare as respostas» |
| 2.3 | A limpeza do passo 4: olha-se com o padrão, anota-se e apaga-se à mão | O `verifica 02` sai todo em verde, e as perguntas 9 e 10 de «Para fechar» foram respondidas |

**Tempo (estimado):** de 60 a 90 minutos. É uma estimativa: ajusta-se com o primeiro grupo que fizer o curso.

---

## Se quiser saber mais

*Opcional. Nada deste quadro se avalia nem é preciso para os exercícios.*

- **O `-v` conta o que foi feito.** Com `cp -v`, `mv -v`, `rm -v` e `mkdir -v`, cada comando escreve uma linha por coisa
  que tocou. O `rm -v` respondeu, por exemplo, `removed 'viaje/vuelta/dos.txt'` («removido»; `viaje` é «viagem» e
  `vuelta`, «volta»).
- **Chaves para várias pastas.** `mkdir -p viaje/{ida,vuelta}` cria `viaje/ida` e `viaje/vuelta`. As chaves são
  expandidas pelo bash, como o `*`: confira com `echo viaje/{ida,vuelta}`.
- **Com o Claude Code: `mv -n`.** Prevê-se no papel o que o `mv -n` faz. Pede-se ao Claude Code, no computador: «O que faz
  o `mv -n` e em que ele se distingue do `mv -i`?». Lê-se o `-n` em `mv --help` e compara-se. Roda-se, no ensaio:
  `touch a.txt b.txt`, `mv -n a.txt b.txt` e `ls`. Explica-se com palavras próprias se coincidiu. O que aconteceu no
  teste está em «Compare as respostas».
- **O `rm -I` pergunta uma única vez.** Com mais de três arquivos, ou com `-r`, pergunta antes de começar: `rm -I x*`
  sobre quatro arquivos respondeu `rm: remove 4 arguments?` («remover 4 argumentos?»). Com `n` não apagou nenhum.

---

## Não funcionou?

| Aparece | Causa provável | O que fazer |
|---|---|---|
| `bash: prepara: …` ou `Todavía no hay prepara para la lección «02».` *(Ainda não há prepara para a lição «02».)* | A imagem é de antes desta lição | Sair com `exit` e rodar `./practicar reconstruir` |
| `Ya existe /home/alumno/practica/02.` e uma pergunta *(Já existe /home/alumno/practica/02.)* | A oficina já tinha sido montada, e montá-la de novo a apaga | Para começar do zero, digitar `si` («sim»). Se não, apertar Enter: nada é apagado |
| `mkdir: No such file or directory` | Falta uma pasta do meio, e este `mkdir` não diz qual | Usar `mkdir -p`, ou conferir cada trecho do caminho com `ls` |
| `rmdir: failed to remove …: Directory not empty` | A pasta tem algo dentro | Ver o que ela tem com `ls -a` antes de decidir |
| `❌ … son nombres, no un patrón` *(… são nomes, não um padrão)* | O padrão foi anotado sem aspas, e o terminal o trocou pelos nomes | `anota 02 patron` e o padrão entre aspas simples |
| `❌ … Copiaste dos veces` *(… Copiou-se duas vezes)* | Rodou-se o mesmo `cp -r` duas vezes | Apagar `respaldo/plantillas` com cuidado e copiá-la uma vez, ou `prepara 02` |
| `❌ … ya no está, y no se pedía borrarlo` *(… já não está, e não se pedia apagá-lo)* | O padrão ou o `rm -r` levaram a mais | Nesta oficina não acontece nada: o `prepara 02` restaura, e repete-se |
| `❌ … es un enlace…` *(… é um link…)* | Há um atalho onde devia haver um arquivo ou uma pasta de verdade | Fazer com `cp` ou `mv`, ou `prepara 02` |
| `❌ … es un enlace duro…` *(… é um link físico…)* | Esse arquivo é o mesmo que o original, com outro nome | Apagá-lo com cuidado e copiá-lo com `cp` |

---

## Para fechar

**Responda sem reler.**

**Cinco situações: o que acontece?** Passa-se com 4 de 5.

1. `cp a.txt b.txt`, e `b.txt` já existia com outro texto.
2. `mv a.txt docs`, e `docs` é uma pasta que existe.
3. `mkdir x/y`, e `x` não existe.
4. `rmdir fotos`, e `fotos` tem doze fotos dentro.
5. `touch notas.txt`, e `notas.txt` já tinha três páginas de notas.

**E cinco perguntas:**

6. Que diferença há entre `cp` e `mv` no que acontece com a origem?
7. Vai-se apagar com `rm *.log`. O que se roda antes, e o que se procura na resposta?
8. Escreveu-se `anota 02 patron *.tmp`, sem aspas, parado em `limpieza`. O que foi anotado, e por quê?
9. Explique com palavras próprias, em três linhas, por que `rm -rf "$CARPETA/"` é perigoso se `CARPETA` está vazia.
10. Como se vê, sem rodar nada perigoso, o que essa linha apagaria? E como se protege a linha?

**O que vem a seguir:** já se sabe criar, copiar, mover e apagar sem perder o que não se queria. Na lição 03 abre-se o
que há dentro dos arquivos: leem-se logs longos sem abrir um editor.

---

> **O que a Hábil faz**
>
> Operamos e modernizamos a infraestrutura onde rodam sistemas que não podem falhar: servidores Linux, contêineres,
> Kubernetes e entrega contínua, para bancos, seguros e empresas reguladas. Se os servidores da empresa dependem de
> uma pessoa que «sabe onde está tudo», isso pode ser organizado sem parar a operação.
>
> [**CI/CD, DevSecOps e nuvem sem sobressaltos**](https://www.habil.mx/pt/arquitetura-cloud-native/)
>
> Primeira conversa, sem custo.

---

## Compare as respostas

**Previsão A (copiar sobre o backup).** Não pergunta. No fim, `respaldo/pagos.conf` diz
`puerto=8443 version=2025-06-02 (la buena, la de hoy)`, igual ao `config/pagos.conf`. A versão da porta 8080 se
perdeu: aqui era o que se queria, mas com outro arquivo poderia ser a única coisa que não se queria perder.

**Previsão B (`cp -r` duas vezes).** A primeira vez cria `respaldo/plantillas` com os três arquivos. A segunda, como
`respaldo/plantillas` já existe, põe outra cópia dentro: fica `respaldo/plantillas/plantillas`. Assim o `ls` respondeu
no teste:

```text
'aviso de corte.txt'   aviso.txt   plantillas   recibo.txt
```

As aspas não fazem parte do nome: o `ls` as põe porque o nome tem espaços.

**Com o Claude Code: `mv -n`.** No teste, `mv -n a.txt b.txt` não moveu nada e não disse nada: o `ls` continuou
mostrando `a.txt  b.txt`. O `-n` é «nunca sobrescreva», em silêncio; o `-i` pergunta. Se o Claude Code disse outra
coisa, vale o que foi visto no terminal.

**Previsão C (mover sobre o rascunho).** O `mv` não pergunta: o rascunho antigo de `reportes` é sobrescrito e se perde,
e em `reportes/reporte-junio.txt` fica a versão nova. O arquivo desaparece de `entrada`, mas a pasta `entrada` fica,
vazia: o `mv` move o que se pede e nada mais.

**Exercício 2.3.** O controle é o `verifica 02`; aqui não vai o comando. Para saber se está indo bem:

- O padrão pega os três `.tmp` e nenhum vizinho. O `cache*` leva o `cache-indice.txt`; o `*tmp*` leva o
  `tmp-notas.txt`. O bom repara em **como termina** o nome.
- O `rmdir` apagou `vacia` e se recusou com `sesiones-viejas`. Essa precisava do `rm -r`, e antes foi olhada com `ls`.

**Para fechar.**

1. `b.txt` fica com o texto de `a.txt`, sem perguntar. O que ele tinha antes se perde.
2. `a.txt` fica dentro da pasta: `docs/a.txt`. Já não está onde estava.
3. Falha com `No such file or directory`, e nesta imagem sem dizer qual caminho. Com `mkdir -p x/y` criam-se as duas.
4. Recusa-se: `Directory not empty`. O `rmdir` só apaga pastas vazias, e nada se perde.
5. As notas continuam ali. O `touch` só muda a data para a de agora.
6. Com o `cp` a origem fica e aparece uma cópia. Com o `mv` a origem desaparece: o arquivo muda de lugar ou de nome.
7. `ls *.log`, com o mesmo padrão. Procura-se que traga só o que se quer apagar; se trouxer a mais, muda-se o padrão.
8. Os nomes dos três `.tmp`. O bash trocou `*.tmp` pela lista antes de rodar o `anota`, como com o `rm` e com o `ls`.
   O `verifica` detecta isso e pede as aspas.
9. Algo assim: se `CARPETA` está vazia, o terminal a troca por nada e a linha fica `rm -rf /`, que pede para apagar a
   raiz, o sistema inteiro. O `-r` entra em todas as pastas e o `-f` não pergunta. Com um `*` no fim, ele recebe uma por
   uma as pastas da raiz, e o `--preserve-root` já não o detém.
10. Vê-se com `echo` na frente: `echo rm -rf "$CARPETA/"` escreve o comando sem rodá-lo. Protege-se com
    `"${CARPETA:?}/"`: se a variável está vazia, o bash cancela o comando antes de rodá-lo.

---

## Fontes da lição

Abertas em 19/11/2026. Todas estão em inglês.

- Ubuntu Manpages. *rm(1)*, Ubuntu 26.04, GNU coreutils 9.7 (sem `-r` não apaga pastas; `-i`, `-I`, `-r`, `-f`;
  `--preserve-root` por padrão; a pergunta diante de arquivos que não podem ser escritos).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/rm.1.html
- Ubuntu Manpages. *cp(1)*, Ubuntu 26.04, GNU coreutils 9.7 (`-i`, `-r`).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/cp.1.html
- Ubuntu Manpages. *mv(1)*, Ubuntu 26.04, GNU coreutils 9.7 (renomeia ou move para uma pasta; `-i`, `-n`).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/mv.1.html
- Ubuntu Manpages. *mkdir(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (`-p`).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/mkdir.1.html
- Ubuntu Manpages. *touch(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (muda a data; cria o arquivo se não existe).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/touch.1.html
- Ubuntu Manpages. *rmdir(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (só pastas vazias).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/rmdir.1.html
- Ubuntu Manpages. *bash(1)*, Ubuntu 26.04, bash 5.3, o manual do bash: `${parameter:?word}` (com a variável vazia, um
  terminal interativo não roda o comando), *Pathname Expansion* (o bash troca o padrão pela lista de nomes, ou o deixa
  como está se não pega nada). https://manpages.ubuntu.com/manpages/resolute/en/man1/bash.1.html
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, seção «rust-coreutils» (`cp`, `mv` e `rm` continuam sendo do GNU).
  https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- GitHub, repositório ValveSoftware/steam-for-linux, issue 3671, aberto em 14 de janeiro de 2015: o relato de um usuário
  e os comentários do tópico, que apontam `rm -rf "$STEAMROOT/"*`. https://github.com/ValveSoftware/steam-for-linux/issues/3671
- Ubuntu Manpages. *ln(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (cria links físicos por padrão; com `-s`,
  simbólicos). https://manpages.ubuntu.com/manpages/resolute/man1/ln.1.html
- Medição nossa, 19/11/2026, em um Mac com Apple silicon (Docker 29.2.1 com Colima, linux/arm64), com a imagem do curso
  sobre o Ubuntu 26.04.1: `mkdir`, `touch` e `rmdir` do uutils coreutils 0.10.0, `cp`, `mv` e `rm` do GNU coreutils 9.7,
  e bash 5.3.9. Daí saem as telas, a tabela do `mkdir` e o tempo do `./practicar reconstruir`.
