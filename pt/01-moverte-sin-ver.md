# Lição 01 — Mover-se sem ver

**Por Dorian Chávez, fundador da Hábil e arquiteto de integração.**

**Antes de ler, cinco palavras.** Uma linha cada uma; as demais chegam no passo em que são usadas.

- **Pasta:** um lugar que guarda arquivos e outras pastas. No terminal e nos manuais aparece como *directory*,
  «diretório». É a mesma coisa.
- **Árvore:** a forma como as pastas estão arrumadas: uma dentro da outra, como os galhos de uma árvore.
- **Raiz:** a pasta de cima de tudo, a que contém todas as demais. Escreve-se com uma única barra: `/`.
- **Pasta atual:** a pasta em que se está parado neste momento. É a que o `pwd` responde.
- **Caminho:** as indicações para chegar a uma pasta ou a um arquivo: nomes de pastas separados por barras. Por exemplo,
  `pagos/bitacoras`.

**O que fica com quem termina:** move-se por pastas sem vê-las, com `cd`; sabe-se em qual se vai ficar antes de apertar
Enter; vê-se o que há em uma pasta com `ls`, inclusive o que está oculto; lê-se cada coluna do `ls -l`; e escreve-se o
caminho até uma pasta de duas maneiras.

**Ideia nova desta lição (a única que se avalia):** as pastas formam uma árvore, e a um lugar dessa árvore se chega com
dois tipos de caminho: o **absoluto**, que sai da raiz, e o **relativo**, que sai de onde se está parado.

**Em que ordem vai:** monta-se um labirinto de pastas (passo 1), caminha-se por ele com caminhos relativos (passo 2) e
com caminhos absolutos (passo 3), olha-se o que há em uma pasta e leem-se as colunas dela (passo 4) e, no fim, anotam-se
quatro respostas que o `verifica` confere (passo 5).

**Uma nota sobre o idioma do labirinto.** Como na lição 00, as mensagens do curso e os nomes do labirinto estão em
espanhol e fazem parte do código, então não se traduzem. As mensagens aparecem exatamente como o terminal as imprime,
com uma glosa em português, em itálico, abaixo de cada bloco, ou entre parênteses dentro de uma tabela. Os nomes do
labirinto são glosados uma só vez, no passo 1: `pagos` («pagamentos»), `bitacoras` («logs»), `guardia` («plantão»),
`marzo.log` («log de março»), `.pendiente` («pendente») e os demais. As quatro chaves da folha de respostas também, no
passo 5: `mas-grande` («maior»), `oculto` («oculto»), `ruta-absoluta` («caminho absoluto») e `ruta-relativa` («caminho
relativo»).

---

## A cena

*Cenário inventado; as telas que aparecem na lição são reais.*

Terça-feira, 23h47. Lucía já entrou no servidor. Já leu o prompt: sabe quem é ali dentro e em que pasta está parada. Do
outro lado do telefone pedem a ela o seguinte:

—Vá até a pasta dos logs de pagamentos e diga qual arquivo pesa mais.

«Vá.» No computador dela, ir a uma pasta é dar duplo clique até chegar. Aqui não há nada em que clicar. Há a mesma linha
de antes, esperando:

```text
operaciones@pagos-03:~$
```

Lucía tem de chegar a uma pasta que não vê, por um caminho que não vê, e dizer qual arquivo é o maior sem errar, porque
do outro lado vão agir com o que ela disser.

Esta lição é esse percurso, em um labirinto feito para se perder sem consequências.

---

## Antes de começar

**O que é preciso:** a lição 00 feita, com `verifica 00` em verde; o material do curso atualizado; e papel e lápis para
as previsões.

**Atualize o terminal de prática.** A imagem construída na lição 00 não traz o que há nesta lição. A partir do
computador, na pasta do curso:

```bash
git pull
./practicar reconstruir
```

O `git pull` traz do repositório os arquivos novos. O `./practicar reconstruir` («reconstruir») os coloca na imagem e
normalmente reaproveita o Ubuntu e os pacotes que já estavam construídos; só precisa de rede se faltar algum. No Mac do
teste, com tudo o que vinha antes já construído, levou menos de dois segundos. Termina com `✅ Imagen lista.` («Imagem
pronta.»). Quem não usa `git` baixa de novo o ZIP do curso e roda `./practicar reconstruir` na pasta nova.

Reconstruir não apaga o trabalho: a pasta `~/practica` vive à parte, no volume da lição 00. Depois entra-se como já se
sabe, com `./practicar`.

**Onde esta lição foi testada, e onde falta:**

| Sistema | Estado |
|---|---|
| Mac com Apple silicon, Docker 29.2.1 rodando com Colima | Testado em 19/11/2026: `./practicar reconstruir`, `prepara 01` duas vezes, cada comando dos cinco passos, `verifica 01` em vermelho e em verde, e as telas que aparecem aqui |
| Linux Mint 22.3 (x86_64), Docker Engine 29.10.0 | Testado em 19/11/2026: `./practicar reconstruir-limpio` («reconstruir limpo»), `./practicar reconstruir` com a camada de pacotes reaproveitada, `prepara 01` duas vezes, `verifica 01` em verde, um caminho errado e um atalho em vermelho, e as seis formas de `ls` idênticas às do GNU |
| Ubuntu, Mac com Docker Desktop, Windows com Docker Desktop e WSL 2 | Ainda sem testar com esta lição |

**O que vai coincidir com a tela e o que não.** Coincidem os nomes, os tamanhos dos arquivos, as datas e os caminhos.
Podem variar a disposição em colunas, as cores, a linha `total` e o tamanho das pastas, que dependem da janela e de onde
o Docker guarda o volume.

---

## Passo 1 · Monte o labirinto e olhe-o de cima

Dentro do terminal de prática, rode:

```bash
prepara 01
```

```text
✅ Laberinto listo en /home/alumno/practica/01 (medí: 10 carpetas y 12 archivos, cada archivo con su tamaño).
   Te dejé la hoja de respuestas, con sus claves vacías: /home/alumno/practica/respuestas/01.md
   Empieza por ahí:  cd ~/practica/01
```

*Labirinto pronto em `/home/alumno/practica/01` (medi: 10 pastas e 12 arquivos, cada arquivo com tamanho). Deixei a
folha de respostas, com as chaves vazias: `/home/alumno/practica/respuestas/01.md`. Comece por aí: `cd ~/practica/01`.*

O `prepara` («preparar») é um comando do curso: monta o material de prática de uma lição. Três coisas sobre ele:

- **Não tem acaso.** Os nomes, os tamanhos e as datas vêm de uma lista fixa. No teste foi montado várias vezes e ficou
  idêntico.
- **Mexe em duas coisas e em nada mais:** a pasta `~/practica/01` e a folha de respostas, que ele cria só se não existir.
- **Pode ser rodado de novo.** Quem se perder ou estragar algo roda `prepara 01` e o labirinto volta a ser o do primeiro
  dia. Para isso ele apaga o que já está lá, então pede que se digite `si` («sim»). Por isso fica separado do `verifica`,
  que só olha.

**O labirinto, visto de cima.** Este desenho não é a saída de nenhum comando: é o que será preciso imaginar enquanto se
caminha. No terminal só se vê a pasta em que se está parado.

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

*As setas dizem: «a raiz», «a pasta pessoal: ~», «aqui começa o labirinto». Os nomes, em espanhol: `LEEME.txt`
(«LEIAME.txt»), `pagos` («pagamentos»), `bitacoras` («logs»), `enero`, `febrero`, `marzo`, `abril` e `mayo` («janeiro» a
«maio»), `viejas` («antigas»), `config`, `plantillas` («modelos»), `recibo` («recibo»), `entregas` («entregas»),
`guardia` («plantão»), `respaldos` («backups»), `semana` («semana») e `copia` («cópia»). `home` e `alumno` («aluno») são
a pasta pessoal do usuário de prática, e `practica` é «prática».*

Leia de cima para baixo. No alto está a raiz, `/`. Dentro da raiz há uma pasta `home`; dentro de `home`, uma pasta
`alumno`; e assim por diante. Cada nível para a direita é «dentro de». Subir é ir em direção à raiz; descer é entrar em
uma pasta que está dentro da atual. Os arquivos são de enchimento: importam o nome, o tamanho e a data.

Agora fique parado na entrada do labirinto. O `cd` é o comando para mudar de pasta, e ele é visto em detalhe no próximo
passo. Hoje esta linha é uma receita pronta:

```bash
cd ~/practica/01
```

```text
alumno@practica:~/practica/01$
```

O `cd` não respondeu nada: como na lição 00, o silêncio quer dizer que deu certo. O que mudou foi o prompt: antes dizia
`~` e agora diz `~/practica/01`. **O prompt vai dizendo em que pasta se está parado.** Confirme com `pwd`:

```text
/home/alumno/practica/01
```

O prompt usa a forma curta, com `~`. O `pwd` responde a forma completa.

**Comprova:** ainda nada. Este passo deixa pronto o material dos três exercícios.

---

## Passo 2 · Caminhe: `cd` com caminhos relativos

O `cd` vem de *change directory*, «mudar de pasta». Escreve-se `cd`, um espaço e o caminho até onde se quer ir.

Para saber aonde se pode ir é preciso ver o que há. O `ls`, sozinho, lista o que há na pasta atual; ele é visto a fundo
no passo 4.

```bash
ls
```

```text
LEEME.txt  pagos  respaldos
```

Coincide com o desenho: em `01` há um arquivo e duas pastas.

**Preveja primeiro.** Vai-se rodar `cd pagos`. Escreva no papel o que o prompt vai dizer depois.

```bash
cd pagos
```

```text
alumno@practica:~/practica/01/pagos$
```

Desceu-se um nível. `pagos` é um **caminho relativo**: não diz onde está `pagos` na árvore inteira, só diz «a pasta
`pagos` que está aqui, onde estou parado». Funcionou porque em `01` há uma pasta `pagos`.

Desça mais um nível e pergunte onde se foi parar:

```bash
cd bitacoras
pwd
```

```text
/home/alumno/practica/01/pagos/bitacoras
```

**Para subir: dois pontos.** `..` quer dizer «a pasta de cima de onde estou». **Preveja:** está-se em `bitacoras`. Onde
se vai parar depois de `cd ..`?

```bash
cd ..
pwd
```

```text
/home/alumno/practica/01/pagos
```

**Vários níveis em um único caminho.** As pastas de um caminho se separam com uma barra. Pode-se descer dois níveis de
uma vez, e subir dois com dois `..`:

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

**Um erro de propósito.** Está-se em `pagos`. Peça uma pasta que não existe ali:

```bash
cd nada
```

```text
bash: cd: nada: No such file or directory
```

«Não existe esse arquivo ou pasta.» A mensagem devolveu o prompt, então não prendeu. E o prompt continua dizendo
`pagos`: **quando o `cd` falha, fica-se onde se estava.** Acontece o mesmo com `cd Bitacoras`: a pasta se chama
`bitacoras`, e o terminal diferencia maiúsculas de minúsculas nos nomes, como nos comandos.

Todos os caminhos deste passo são relativos: **o resultado depende de onde se está parado.** O mesmo `cd pagos` que
funcionou em `01` falha em uma pasta que não tenha uma `pagos` dentro.

**Comprova** (objetivo 1.1, primeira metade): as previsões deste passo, comparadas com o prompt e com o `pwd`.

---

## Passo 3 · A outra maneira: caminhos absolutos

Um caminho relativo serve enquanto se sabe onde se está. Há outra maneira de escrever um caminho, que não depende disso.

**Suba até o alto.** A raiz se escreve com uma única barra:

```bash
cd /
ls
```

```text
bin   dev  home  media  opt   root  sbin  sys  usr
boot  etc  lib   mnt    proc  run   srv   tmp  var
```

O prompt agora diz `alumno@practica:/$`. Está-se na raiz, e essas são as pastas de um sistema Linux. Hoje importa uma,
`home`, porque dentro dela está a pasta pessoal.

**Preveja:** está-se na raiz. O que acontece se se rodar `cd pagos`, o mesmo comando que funcionou no passo 2?

```text
bash: cd: pagos: No such file or directory
```

Falhou, e com razão: na raiz não há nenhuma pasta `pagos`.

**Um caminho absoluto começa com `/`.** Diz o caminho completo a partir da raiz, pasta por pasta. Por isso funciona do
mesmo jeito, não importa onde se esteja parado. Este leva aos logs:

```bash
cd /home/alumno/practica/01/pagos/bitacoras
```

```text
alumno@practica:~/practica/01/pagos/bitacoras$
```

Leia-o junto com o desenho do passo 1: da raiz a `home`, de `home` a `alumno`, daí a `practica`, a `01`, a `pagos` e a
`bitacoras`. É o mesmo que o `pwd` responde quando se está ali: **o `pwd` responde o caminho absoluto da pasta atual.**

**A barra do começo muda tudo.** `cd pagos` pede a pasta `pagos` que está dentro de onde estou. `cd /pagos` pede a pasta
`pagos` que está na raiz. Confira onde de fato há uma `pagos`:

```bash
cd ~/practica/01
cd /pagos
```

```text
bash: cd: /pagos: No such file or directory
```

Há uma `pagos` a um passo de distância, mas pediu-se outra: uma que estaria na raiz, e essa não existe.

**A pasta pessoal tem abreviação: `~`.** No começo de um caminho, o terminal troca `~` por `/home/alumno` antes de rodar o
comando. Por isso `cd ~/practica/01` e `cd /home/alumno/practica/01` levam ao mesmo lugar, e `cd` sozinho, sem caminho,
leva à pasta pessoal. Mas `~` é uma abreviação do terminal e não parte da árvore: em outra conta, `~` é outra pasta.
Quando pedirem um caminho absoluto, escreva-o a partir da raiz, com `/`.

**Os quatro pontos de partida,** juntos:

| O caminho começa com | Sai de | Exemplo | Depende de onde se está |
|---|---|---|---|
| `/` | A raiz | `/home/alumno/practica/01` | Não |
| `~` | A pasta pessoal | `~/practica/01` | Não |
| `..` | A pasta de cima da atual | `../config` | Sim |
| Um nome | A pasta atual | `pagos/bitacoras` | Sim |

**Qual convém?** O relativo é curto: convém quando o lugar está perto. O absoluto não deixa dúvidas: convém quando se dita
um caminho a alguém que não sabe em que pasta se está parado, como a Lucía por telefone.

**Comprova** (objetivo 1.1 completo): exercício 1.1, abaixo. E é metade do objetivo 1.3.

---

## Passo 4 · Veja o que há: `ls` e as colunas dele

Vá até a pasta dos logs, com o caminho que quiser, e liste:

```bash
cd ~/practica/01/pagos/bitacoras
ls
```

```text
abril.log  enero.log  febrero.log  marzo.log  mayo.log  viejas
```

O `ls` vem de *list*, «listar». Sozinho, dá só os nomes, em ordem alfabética. Não dá tamanhos nem datas: para isso
existem as opções, as palavras com hífen que mudam o que um comando faz. Hoje são três.

### `-a`: também o que está oculto

**Preveja:** o `ls` de cima mostrou seis nomes. Será que é tudo o que há na pasta?

```bash
ls -a
```

```text
.   .pendiente  enero.log    marzo.log  viejas
..  abril.log   febrero.log  mayo.log
```

Apareceram três nomes que começam com ponto. No Linux, **um arquivo cujo nome começa com ponto é um arquivo oculto**: o
`ls` sozinho não o lista. Não está protegido nem é secreto; só não atrapalha a lista do dia a dia. O `-a` vem de *all*,
«tudo». O `.pendiente` é um arquivo oculto do labirinto. O `..` já se conhece: é a pasta de cima. E o `.` é a pasta atual.

### `-l`: a lista longa

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

O `-l` vem de *long*, «longa»: uma linha por arquivo ou pasta, com os dados em colunas. Leia a linha do `marzo.log` da
esquerda para a direita:

| Coluna | Em `marzo.log` | O que diz |
|---|---|---|
| **Tipo** | `-` | A primeira letra da linha. Um hífen é um arquivo normal. Um `d`, de *directory*, é uma pasta: veja a linha de `viejas`. Há outros tipos; neste labirinto só há estes dois |
| **Permissões** | `rw-r--r--` | As nove letras seguintes. Dizem quem pode ler, escrever e executar o arquivo. Hoje só se dá nome a elas: leem-se e mudam-se na lição 09 |
| **Links** | `1` | Quantos nomes apontam para esse mesmo arquivo. Neste curso não se usa |
| **Dono** | `alumno` | O usuário a quem o arquivo pertence. Aqui é a própria pessoa |
| **Grupo** | `alumno` | O grupo de usuários a que pertence. Volta a aparecer na lição 09 |
| **Tamanho** | `1200000` | Quanto o arquivo mede, em bytes |
| **Data** | `Mar 31  2025` | Quando foi modificado pela última vez. Os meses saem em inglês: `Mar` é março e `Apr` é abril |
| **Nome** | `marzo.log` | Como se chama |

Três detalhes:

- **O `total` de cima não é a soma da coluna de tamanho.** É o espaço que o listado ocupa em disco, em blocos de 1.024
  bytes. Neste curso não se usa.
- **O tamanho de uma pasta não é o que ela leva dentro.** `viejas` diz `4096` e dentro tem um arquivo de 20.000 bytes.
  Para saber quanto há dentro, entra-se e olha-se.
- **A data muda de forma conforme a idade.** Se o arquivo foi modificado há mais de seis meses, vê-se o ano, como aqui.
  Se é recente, no lugar do ano vê-se a hora, em hora universal (UTC) e não a do relógio de quem olha. Isso será notado no
  passo 5.

### `-h`: tamanhos que se leem

Qual é maior, `998000` ou `1200000`? É preciso contar os dígitos. O `-h` vem de *human-readable*, «legível para uma
pessoa»: junto com o `-l`, escreve o tamanho abreviado e com uma letra.

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

Sem letra são bytes. `K` são kilobytes: 1.024 bytes. `M` são megabytes: 1.024 K. `G` são gigabytes: 1.024 M.

**A pegadinha desta coluna: lê-se primeiro a letra e depois o número.** `975K` tem um número maior que `1.2M`, e mesmo
assim é o menor arquivo dos dois, porque um M vale mais de mil K. E um limite: o `-h` arredonda. O `marzo.log` mede
1.200.000 bytes, que são 1,14 M, e o `ls` escreve `1.2M`. Quando for preciso o número exato, tira-se o `h`.

### As opções se juntam, e o `ls` aceita um caminho

Várias opções de uma letra podem ser escritas coladas: `ls -lah` é o mesmo que `ls -l -a -h`. Rode-o: é a lista longa,
com o que está oculto e com tamanhos legíveis. Ali o `.pendiente` já tem uma linha.

E não é preciso ir a uma pasta para ver o que ela tem: o `ls` aceita um caminho, relativo ou absoluto, como o `cd`.
`ls -lh ..` mostra a pasta de cima e a pessoa continua parada em `bitacoras`. Olhar não move.

### Com o Claude Code, nesta ordem

Neste curso um assistente de IA entra de um único jeito: **prever → pedir → ler → rodar → explicar.**

1. **Prever.** Com o `ls -lah` à frente, anote no papel qual é o maior arquivo de `bitacoras`.
2. **Pedir.** No computador, ao Claude Code: «Dê-me um comando de `ls` que ordene uma pasta por tamanho, do maior para o
   menor, e explique cada opção». O Claude Code não vê o labirinto: roda do lado de fora.
3. **Ler.** Antes de rodar o que ele deu, busque cada opção em `ls --help`, como na lição 00.
4. **Rodar.** O comando é digitado por quem estuda, no terminal de prática.
5. **Explicar.** Com palavras próprias: por que ficou nessa ordem? Coincide com a previsão?

**Comprova** (objetivo 1.2): exercício 1.2, abaixo.

---

## Passo 5 · Anote as respostas e confira-as

Até aqui tudo foi conferido por quem estuda, com o prompt e com o `pwd`. Agora é a vez do `verifica`. Para que ele possa
revisar o que se encontrou, anota-se na **folha de respostas**, `~/practica/respuestas/01.md`, que o `prepara` já deixou.
É texto simples, com quatro linhas: uma chave, dois-pontos e a resposta.

| Chave | O que vai |
|---|---|
| `mas-grande` («maior») | O nome do maior arquivo do labirinto. Só o nome, como o `ls` o escreve, sem a pasta |
| `oculto` («oculto») | O nome do arquivo oculto do labirinto. Só o nome, com o ponto |
| `ruta-absoluta` («caminho absoluto») | O caminho absoluto da pasta de destino |
| `ruta-relativa` («caminho relativo») | O caminho relativo até a pasta de destino, saindo de `bitacoras` |

Rode `verifica 01` antes de anotar qualquer coisa, para ver de onde se parte. As duas primeiras linhas saem assim:

```text
✅ Tu laberinto está completo (medí: las 22 carpetas y archivos de la lista existen en ~/practica/01, ninguno es un enlace y cada archivo tiene su tamaño)
❌ Tu renglón «mas-grande:» está vacío (medí: lo que hay después de los dos puntos). Anótalo con  anota 01 mas-grande  y el nombre del archivo
```

*O labirinto está completo (medi: as 22 pastas e arquivos da lista existem em `~/practica/01`, nenhum é um link e cada
arquivo tem tamanho). A linha «mas-grande:» está vazia (medi: o que há depois dos dois-pontos). Anote-a com
`anota 01 mas-grande` e o nome do arquivo.*

Seguem-se três vermelhos iguais, um para cada chave que falta. É normal: a folha está vazia.

**Como se anota.** Ainda não se viu um editor de texto; ele chega na lição 07. Por isso o curso traz um comando, o
`anota` («anote»): digita-se o número da lição, a chave e a resposta. Suponha que se ache que o maior é o `enero.log`:

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

*Anotado. A folha ficou assim: `mas-grande: enero.log`, e as outras três chaves continuam vazias.*

O `anota` não sabe se se acertou: só escreve. Isso quem diz é o `verifica 01`, que agora muda a segunda linha:

```text
❌ «enero.log» no es el archivo más grande del laberinto (medí: comparé sus bytes contra los de los demás archivos de la lista; hay uno con más). Vuelve a leer la columna del tamaño con  ls -lh : fíjate en la letra, no solo en el número
```

*«enero.log» não é o maior arquivo do labirinto (medi: comparei os bytes dele com os dos demais arquivos da lista; há um
com mais). Leia de novo a coluna do tamanho com `ls -lh`: repare na letra, não só no número.*

Assim se lê um vermelho: diz o que se anotou, o que se mediu e com que comando voltar a olhar. Não diz a resposta. Para
corrigir, anota-se outra vez a mesma chave: o `anota` troca essa linha. E `anota 01`, sem mais nada, mostra a folha.

**Os dois caminhos do exercício 1.3.** A pasta de destino é `guardia`: está dentro de `entregas`, que está dentro de
`pagos`. Procure-a no desenho do passo 1. É preciso chegar a ela de duas maneiras: com um **caminho absoluto** e com um
**caminho relativo** que saia da pasta `bitacoras`.

Antes de anotar cada caminho, percorra-o: fique parado onde convém, entre com `cd` e pergunte com `pwd` se chegou a
`guardia`. Quando um caminho levar até lá, anote-o exatamente como foi escrito. O `verifica` faz o mesmo teste.

Quando as quatro linhas estiverem preenchidas, rode `verifica 01`. Corrija e repita até ver isto no final:

```text
✅ Lección 01: todo en verde.
```

*Lição 01: tudo em verde.*

Agora que a folha tem as respostas, veja a linha dela com `ls -l ~/practica/respuestas`:

```text
total 4
-rw-r--r-- 1 alumno alumno 139 Nov 19 07:35 01.md
```

É o detalhe que ficou pendente no passo 4: a folha é recente, e no lugar do ano traz a hora. O tamanho e a data da folha
de cada pessoa serão outros.

**Comprova** (objetivos 1.2 e 1.3): `verifica 01` tudo em verde.

---

## Os exercícios

O exercício 1.1 se faz **sem IA** e sem rodar nada antes de prever: só o papel e o desenho do passo 1.

| # | Faz-se | Está feito quando |
|---|---|---|
| 1.1 | Prevê-se por escrito em que pasta se fica depois de cada um dos dez `cd` abaixo, e depois se rodam | Há as dez previsões e as dez saídas do `pwd`, e se sabe explicar cada uma que não coincidiu |
| 1.2 | Em `bitacoras`: acha-se o maior arquivo e o oculto e anotam-se na folha; e preenche-se no papel a tabela de colunas abaixo | O `verifica 01` marca em verde `mas-grande` e `oculto`, e a tabela coincide com «Compare as respostas» |
| 1.3 | Chega-se à pasta `guardia` com um caminho absoluto e com um relativo que sai de `bitacoras`, e anotam-se os dois | O `verifica 01` sai todo em verde |

**Os dez `cd` do 1.1.** Primeiro escreva as dez previsões, com o caminho completo que se acha que o `pwd` vai responder.
Não rode nada ainda. Se se achar que um `cd` vai falhar, escreva isso, e escreva onde se fica. Começa-se parado em
`~/practica/01`, e cada comando sai de onde o anterior deixou.

| # | Comando | # | Comando |
|---|---|---|---|
| 1 | `cd pagos/config` | 6 | `cd pagos` |
| 2 | `cd plantillas` | 7 | `cd home` |
| 3 | `cd ../../bitacoras/viejas` | 8 | `cd ~` |
| 4 | `cd ../..` | 9 | `cd ../..` |
| 5 | `cd /` | 10 | `cd ..` |

Quando as dez estiverem prontas, vá até a entrada com `cd ~/practica/01` e rode os comandos um por um, com um `pwd`
depois de cada um. Não há um número para passar: o que conta é que, em cada previsão que falhou, se saiba dizer o que foi
lido errado: se o caminho era absoluto ou relativo, ou em que pasta se estava parado.

**A tabela de colunas do 1.2.** Rode `ls -la` em `bitacoras` e preencha no papel estas duas linhas. Esta parte o
`verifica` não revisa: compara-se por conta própria.

| Nome | Arquivo ou pasta? | Permissões (as nove letras) | Dono | Tamanho em bytes | Data |
|---|---|---|---|---|---|
| `.pendiente` | | | | | |
| `viejas` | | | | | |

**Tempo (estimado):** de 60 a 90 minutos. É uma estimativa: ajusta-se com o primeiro grupo que fizer o curso.

---

## Se quiser saber mais

*Opcional. Nada deste quadro se avalia nem é preciso para os exercícios.*

- **`cd -` volta à pasta anterior.** É um hífen depois do `cd`. Serve para ir e vir entre dois lugares, e responde para
  onde voltou.
- **A tecla Tab completa nomes.** Em `pagos`, escreva `cd bit` e aperte Tab: o terminal completa `bitacoras/`, porque é a
  única pasta dali que começa assim. Menos letras, menos erros de digitação.
- **`ls -lhS` ordena por tamanho.** É uma resposta possível ao que se pediu ao Claude Code. No `ls --help`, o `S` diz
  `Sort by file size, largest first.` («Ordenar por tamanho, do maior para o menor.») Assim saiu em `bitacoras`:

```text
total 2.2M
-rw-r--r-- 1 alumno alumno 1.2M Mar 31  2025 marzo.log
-rw-r--r-- 1 alumno alumno 975K Apr 30  2025 abril.log
-rw-r--r-- 1 alumno alumno 9.6K Feb 28  2025 febrero.log
drwxr-xr-x 2 alumno alumno 4.0K Jan  2  2025 viejas
-rw-r--r-- 1 alumno alumno  980 Jan 31  2025 enero.log
-rw-r--r-- 1 alumno alumno    0 May 31  2025 mayo.log
```

  `viejas` fica no meio porque a linha dela diz `4.0K`, o tamanho da pasta e não do que ela leva dentro. E o arquivo
  oculto não aparece, porque o comando não leva `-a`.
- **O `ls` deste terminal e o de outros lugares.** Aqui o `ls` vem do `rust-coreutils` e não do projeto GNU, como se viu
  na lição 00. Nas formas desta lição a saída foi idêntica à do `ls` do GNU, que o Ubuntu deixa instalado como `gnuls`:
  compare `gnuls -lah` com o `ls -lah`. O que muda é o texto de `ls --help` e de `man ls`.

---

## Não funcionou?

| Aparece | Causa provável | O que fazer |
|---|---|---|
| `bash: prepara: command not found`, ou `Todavía no hay verifica para la lección «01».` *(Ainda não há verifica para a lição «01».)* | A imagem é de antes desta lição | Sair com `exit` e rodar `./practicar reconstruir` |
| `Ya existe /home/alumno/practica/01.` e uma pergunta *(Já existe /home/alumno/practica/01.)* | O labirinto já tinha sido montado, e montá-lo de novo o apaga | Para começar do zero, digitar `si` («sim»). Se não, apertar Enter: nada é apagado |
| Depois de `prepara 01`, o `ls` não mostra nada, ou sai `error retrieving current directory` | Estava-se parado dentro do labirinto quando ele foi montado de novo: essa pasta já não existe | `cd ~/practica/01` |
| `❌ No preparo nada: … es un enlace…` *(Não preparo nada: … é um link…)* | Na pasta de prática há um link onde deveria haver uma pasta da própria pessoa | A partir do computador, `./practicar desde-cero` («do zero») |
| `bash: cd: …: No such file or directory` | A pasta não existe com esse nome no lugar em que foi pedida | `pwd` para saber onde se está e `ls` para ver o que há. Conferir as maiúsculas e se o caminho devia começar com `/` |
| `❌ Tu ruta relativa … no existe saliendo de …` *(O caminho relativo … não existe saindo de …)* | Foi testado parado em outra pasta | Ir até `bitacoras` e testar ali o caminho com `cd` e `pwd` |
| `❌ Tu laberinto no está completo…` *(O labirinto não está completo…)* | Algo do labirinto foi apagado ou mudado | `prepara 01`. A folha de respostas não se perde |

---

## Para fechar

**Responda sem reler.**

**Cinco caminhos: absoluto ou relativo?** Passa-se com 4 de 5.

1. `config/plantillas`
2. `/home/alumno/practica`
3. `../respaldos`
4. `/`
5. `..`

**E cinco perguntas:**

6. Está-se em `/home/alumno/practica/01/respaldos/semana-1`. Que caminho relativo leva a `semana-2`?
7. Rodou-se `ls` em uma pasta e não saiu nada. Já se pode dizer que ela está vazia? O que se roda para ter certeza?
8. No `ls -lh` aparece `812K` em uma linha e `3.1M` em outra. Qual arquivo é maior, e como se sabe?
9. Uma linha do `ls -l` começa com `d`. O que essa letra diz? E o que o número da coluna do tamanho diz sobre o conteúdo
   dela?
10. Vai-se ditar a um colega o caminho até uma pasta, e não se sabe em qual pasta ele está parado. Dita-se um caminho
    absoluto ou um relativo? Por quê?

**O que vem a seguir:** já se sabe mover-se pelas pastas e ver o que elas têm. Na lição 02 começa-se a mudar o que há:
cria-se, copia-se, move-se e apaga-se, em um terminal onde não há lixeira.

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

**Exercício 1.1.** O que o `pwd` responde depois de cada comando:

| # | Comando | Fica-se em | Por quê |
|---|---|---|---|
| 1 | `cd pagos/config` | `/home/alumno/practica/01/pagos/config` | Relativo: desce dois níveis a partir de `01` |
| 2 | `cd plantillas` | `/home/alumno/practica/01/pagos/config/plantillas` | Relativo: desce um |
| 3 | `cd ../../bitacoras/viejas` | `/home/alumno/practica/01/pagos/bitacoras/viejas` | Sobe dois, até `pagos`, e desce dois por outro ramo |
| 4 | `cd ../..` | `/home/alumno/practica/01/pagos` | Sobe dois |
| 5 | `cd /` | `/` | Absoluto: a raiz, não importa onde se estava |
| 6 | `cd pagos` | `/` | Falha: `No such file or directory`. Na raiz não há `pagos`, e quando o `cd` falha fica-se onde se estava |
| 7 | `cd home` | `/home` | Relativo: na raiz sim há uma pasta `home` |
| 8 | `cd ~` | `/home/alumno` | A pasta pessoal, não importa onde se estava |
| 9 | `cd ../..` | `/` | Sobe dois: de `alumno` a `home`, e de `home` à raiz |
| 10 | `cd ..` | `/` | Acima da raiz não há nada: `..` na raiz é a própria raiz. Não dá erro e não move |

**Exercício 1.2, o maior e o oculto.** O `verifica 01` diz se se acertou. O raciocínio que deve estar por trás:

- **O maior** lê-se na coluna do tamanho do `ls -lh`, primeiro a letra e depois o número: a única linha com `M` vence as
  que trazem `K`. Sem `-h` chega-se ao mesmo contando os dígitos.
- **O oculto** só aparece com `-a`. É o único nome que começa com ponto e não é nem `.` nem `..`. Na folha vai com o ponto
  e tudo: o ponto é parte do nome.

**Exercício 1.2, a tabela de colunas.**

| Nome | Arquivo ou pasta? | Permissões | Dono | Tamanho em bytes | Data |
|---|---|---|---|---|---|
| `.pendiente` | Arquivo: a linha começa com um hífen | `rw-r--r--` | `alumno` | `48000` | `May 12  2025`: 12 de maio de 2025 |
| `viejas` | Pasta: a linha começa com `d` | `rwxr-xr-x` | `alumno` | `4096` no teste; no computador de cada pessoa pode ser outro | `Jan  2  2025`: 2 de janeiro de 2025 |

Se no tamanho do `.pendiente` se escreveu `47K`, rodou-se `ls -lah`: é o mesmo arquivo, com o tamanho abreviado. Se no
dono houve dúvida entre as duas colunas que dizem `alumno`: a primeira é o dono e a segunda é o grupo.

**Exercício 1.3.** Aqui não vai a resposta escrita: o controle é o `verifica 01`. Para saber se está indo bem:

- O caminho absoluto começa com `/` e é, letra por letra, o que o `pwd` responde quando se está parado em `guardia`.
  Depois da raiz leva sete nomes.
- O caminho relativo não começa com `/` nem com `~`. O mais curto sobe um nível e desce dois. Há outros mais longos que
  também chegam, e o `verifica` os dá por bons: o que ele mede é se o caminho escrito, pasta por pasta, leva a `guardia`.

**Para fechar.**

1. Relativo.
2. Absoluto.
3. Relativo.
4. Absoluto: é a raiz.
5. Relativo.
6. `../semana-2`: sobe-se a `respaldos` e desce-se a `semana-2`.
7. Ainda não: o `ls` sozinho não lista o que está oculto. Roda-se `ls -a`. Se só saírem `.` e `..`, aí sim está vazia.
8. O de `3.1M`. Lê-se primeiro a letra: um M vale mais de mil K.
9. Que é uma pasta. Do conteúdo não diz nada: esse número é o tamanho da própria pasta.
10. Um absoluto. Um relativo só serve se os dois sabem em que pasta ele está parado; o absoluto sai da raiz.

---

## Fontes da lição

Abertas em 19/11/2026. Todas estão em inglês.

- The Open Group. *POSIX.1-2024, Base Definitions, capítulo 3* (3.2 «Absolute Pathname», 3.311 «Relative Pathname» e 3.111 «Dot-Dot»). https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap03.html
- The Open Group. *POSIX.1-2024, utilitário cd* (`cd` sem caminho usa a pasta pessoal; `cd -` vai à pasta anterior e a escreve). https://pubs.opengroup.org/onlinepubs/9799919799/utilities/cd.html
- The Open Group. *POSIX.1-2024, utilitário ls* (a ordem das colunas do `-l`; hora ou ano conforme os últimos seis meses). https://pubs.opengroup.org/onlinepubs/9799919799/utilities/ls.html
- GNU. *Bash Reference Manual, «Bourne Shell Builtins»* (`cd` e `pwd`; o `pwd` escreve o caminho absoluto da pasta atual). https://www.gnu.org/software/bash/manual/html_node/Bourne-Shell-Builtins.html
- GNU. *Bash Reference Manual, «Tilde Expansion»* (o `~` é trocado pela pasta pessoal). https://www.gnu.org/software/bash/manual/html_node/Tilde-Expansion.html
- GNU. *Coreutils, «What information is listed»* (as colunas do `-l`, as letras do tipo, a linha `total` e o `-h` com potências de 1024). https://www.gnu.org/software/coreutils/manual/html_node/What-information-is-listed.html
- GNU. *Coreutils, «Which files are listed»* (o `ls` não lista os nomes que começam com ponto; o `-a` lista). https://www.gnu.org/software/coreutils/manual/html_node/Which-files-are-listed.html
- Ubuntu Manpages. *ls(1)*, do Ubuntu 26.04 (o `ls` do `rust-coreutils` 0.10.0: `-a`, `-l`, `-h`, `-S`). https://manpages.ubuntu.com/manpages/resolute/en/man1/ls.1.html
- Ubuntu Manpages. *hier(7)*, do Ubuntu 26.04 (as pastas da raiz de um sistema Linux; `/home`). https://manpages.ubuntu.com/manpages/resolute/en/man7/hier.7.html
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, seção «rust-coreutils» (os comandos do GNU continuam instalados com o prefixo `gnu`, como `gnuls`). https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- Medição nossa, 19/11/2026, em um Mac com Apple silicon (Docker 29.2.1 com Colima, linux/arm64), com a imagem do curso
  sobre o Ubuntu 26.04.1: `ls` do uutils coreutils 0.10.0, `gnuls` do GNU coreutils 9.7 e bash 5.3.9. Daí saem as telas,
  a comparação do `ls` com o `gnuls`, o arredondamento do `-h`, a tecla Tab e o tempo do `./practicar reconstruir`. No
  Linux esta lição ainda não foi testada.
