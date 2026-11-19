# Lição 00 — O terminal de prática

**Por Dorian Chávez, fundador da Hábil e arquiteto de integração.**

**Antes de ler, seis palavras.** Uma linha cada uma; as demais chegam na lição em que são usadas.

- **Terminal:** a janela em que se digitam instruções no teclado e o computador responde com texto.
- **Comando:** uma instrução escrita. Por exemplo, `whoami` («quem sou eu?»). Digita-se e aperta-se Enter.
- **Prompt:** o texto que o terminal coloca no começo da linha quando espera o próximo comando. Em espanhol,
  *indicador*.
- **Docker:** o programa que cria e roda contêineres.
- **Contêiner:** um computador Linux pequeno que vive dentro do computador de quem estuda, separado dos arquivos pessoais.
- **Imagem:** o molde de que sai um contêiner. Constrói-se uma vez e usa-se muitas.

**O que fica com quem termina:** o terminal de prática funcionando, que é um Ubuntu 26.04 dentro de um contêiner; saber
entrar, sair e voltar sem perder o trabalho; ler o prompt; pedir ajuda à própria máquina; e sair de qualquer programa
que prenda.

**Ideia nova desta lição (a única que se avalia):** neste terminal não há área de trabalho nem janelas, só texto: ele
diz quem se é e onde se está em cada linha, e se fala com ele por meio de comandos.

**Em que ordem vai:** primeiro sobe-se o terminal (passos 1 e 2), depois lê-se o que ele diz (passo 3), depois aprende-se
a pedir ajuda a ele (passo 4) e, por fim, a sair de onde se travar (passo 5).

**Uma nota sobre o idioma do script.** O script do curso (`./practicar`) e o verificador dele (`verifica`) falam
espanhol, e as mensagens que imprimem também. Nesta lição essas mensagens aparecem exatamente como o terminal as
imprime, e abaixo de cada bloco há uma glosa curta em português, em itálico. Nomes como `practicar`, `verifica`,
`alumno` («aluno»), `practica` («prática») e `aqui-estuve.txt` («aqui estive») fazem parte do código e não se
traduzem.

---

## A cena

*Cenário inventado; as telas que aparecem na lição são reais.*

Terça-feira, 23h40. Lucía está há três semanas no primeiro emprego e hoje é dia de plantão. O telefone toca: o serviço
de pagamentos parou de responder. Passam a ela um endereço e uma frase: «entre no servidor e diga o que vê».

Ela entra. Não há área de trabalho, nem ícones, nem mouse. Há uma tela preta e uma única linha:

```text
operaciones@pagos-03:~$
```

*(Em espanhol, «operaciones» é «operações» e «pagos» é «pagamentos».)*

O cursor pisca. Do outro lado do telefone, esperam.

Lucía ainda não sabe, mas essa linha já respondeu duas das três perguntas que vão fazer a ela esta noite: quem ela é
ali dentro e em que pasta está parada. A terceira, o que fazer, pergunta-se à própria máquina. E se algo travar, há
três teclas para sair.

Esta lição dá essas quatro coisas em uma máquina em que errar não custa nada: o terminal de prática, próprio de cada pessoa.

---

## Antes de começar

**O que é preciso:**

| | O quê | Notas |
|---|---|---|
| Sistema | Linux, Mac ou Windows | Abaixo diz em quais já foi testado e em quais falta |
| Docker | Instalado e respondendo | Abaixo diz como, por sistema |
| Disco | Menos de 300 MB para a imagem do curso | Vai um teto e não um número, porque cada ferramenta do Docker mede de um jeito (abaixo) |
| Rede | Para baixar o Ubuntu e alguns pacotes na primeira vez | Só o computador a usa, ao construir; dentro do terminal de prática não há rede |
| Conhecimento | Abrir o terminal do computador e colar uma linha | Não se supõe nada de Linux nem de Docker |

**Onde foi testado, e onde falta:**

| Sistema | Estado | O que se rodou |
|---|---|---|
| Linux Mint 22.3 (x86_64), Docker Engine 29.10.0 | Testado em 19/11/2026 | O `./practicar` construiu a imagem em cerca de 90 segundos, o `verifica 00` deu a quinta marca em vermelho antes de sair e as cinco em verde na volta; lá dentro só existe a rede interna `lo`; o `./practicar desde-cero` se recusou com a sessão aberta e, já fechada, apagou a pasta. Atenção: o Docker não dá suporte oficial ao Linux Mint; o guia dele diz que «pode funcionar» |
| Mac com Apple silicon, Docker 29.2.1 rodando com Colima | Testado em 18 e 19/11/2026 | Tudo o que há nesta lição: entrar, sair, voltar, `verifica 00`, `desde-cero` e as telas que aparecem aqui |
| Ubuntu | Ainda sem testar | O Docker dá suporte oficial a ele; a instalação é a do guia dele |
| Mac com Docker Desktop | Ainda sem testar | A instalação é a do guia oficial do Docker Desktop |
| Windows com Docker Desktop e WSL 2 | Ainda sem testar | A instalação é a dos guias oficiais da Microsoft e do Docker |

Quem testar em um desses sistemas, conte-nos como foi.

**Os números desta lição, e como foram medidas:**

| Número | Como foi medida |
|---|---|
| Construir a imagem: 42 segundos | Mac com Apple silicon, 18/11/2026, com `time ./practicar reconstruir`, que naquele dia construía sem cache (hoje isso é feito por `./practicar reconstruir-limpio`), e com a imagem do Ubuntu já baixada |
| Construir a imagem: 1 minuto e 38 segundos | O mesmo Mac, 18/11/2026, com `docker build --no-cache`, sobre uma versão anterior do Dockerfile |
| Construir a imagem: cerca de 90 segundos | Linux Mint 22.3 (x86_64), 19/11/2026, com `./practicar` |
| Tamanho: 221 MB | Linux Mint, 18/11/2026, com `docker image inspect` |
| Tamanho: 277 MB | Mac, 18/11/2026, com `docker image ls` |

**Instale o Docker, conforme o sistema.** É a única coisa que se instala no computador.

- **Linux Mint ou Ubuntu:** Docker Engine, a partir do repositório oficial do Docker. A
  [lição 1 do curso de Docker](https://www.habil.mx/pt/cursos/docker/01-instalacion/) da Hábil faz isso passo a
  passo no Linux Mint e explica cada erro. O Docker não dá suporte oficial ao Mint, e essa lição diz o que fazer a
  respeito. No computador do teste, o usuário estava no grupo `docker` e usou o Docker sem `sudo`; essa mesma lição
  explica o que implica entrar nesse grupo.
- **Mac:** Docker Desktop, com o instalador do processador (Apple silicon ou Intel), a partir do
  [guia oficial](https://docs.docker.com/desktop/setup/install/mac-install/). Abra-o e espere até que diga que está
  rodando. No Mac do teste, o Docker rodava com outra ferramenta, o Colima, e não com o Docker Desktop: por isso esta
  via está na tabela como «ainda sem testar».
- **Windows:** a via documentada é o Docker Desktop com WSL 2. O WSL é a peça do Windows que roda um Linux dentro dele.
  Segundo a Microsoft, no Windows 10 (versão 2004 ou posterior) e no Windows 11 ele se instala com `wsl --install` em um
  PowerShell aberto como administrador, e traz o Ubuntu. Segundo o Docker, depois se instala o Docker Desktop e se liga
  a integração em *Settings › Resources › WSL Integration*. Tudo o que vem a seguir roda-se no terminal desse Ubuntu,
  não no PowerShell. Também está na tabela como «ainda sem testar».

**Confira se o Docker responde.** Abra o terminal do computador e rode:

```bash
docker version
```

Está tudo certo se aparecerem dois blocos, `Client` e `Server`, cada um com a versão dele. Nos testes foram o Docker
29.10.0 no Linux e o 29.2.1 no Mac. Se aparecer `Client` e, no lugar de `Server`, um erro, o programa `docker` não
conseguiu falar com o motor dele. Rode `docker info` e siga o erro exato que ele der; as causas mais comuns estão em
«Não funcionou?».

**Traga o material do curso.** Ele está em um **repositório**: uma pasta com todos os arquivos do curso, publicada no
GitHub. O programa `git` a baixa com a primeira linha; a segunda entra nessa pasta:

```bash
git clone https://github.com/HabilMX/curso-terminal.git
cd curso-terminal
```

**Sem `git`:** abra esse endereço no navegador, escolha *Code › Download ZIP*, descompacte o arquivo e abra o terminal
na pasta que foi criada. Não copie o `cd curso-terminal` de cima: essa pasta pode ter outro nome.

---

## Passo 1 · Suba o terminal e entre

**Preveja primeiro.** Escreva em um papel: ao entrar, o que deve aparecer? Uma janela nova? Uma área de
trabalho? Outra coisa?

A partir da pasta do curso:

```bash
./practicar
```

O `practicar` é um script do curso. Na primeira vez ele constrói a imagem: baixa o Ubuntu 26.04 e coloca nela o que o
curso usa. Nos testes levou entre 42 segundos e 1 minuto e 38 segundos. Nas vezes seguintes já não baixa nem
reconstrói: no Mac do teste, entrou em menos de dois segundos. O tempo depende do Docker e da máquina.

Vai passar muito texto e, no fim, isto:

```text
✅ Imagen lista.
Entrando a tu terminal de práctica. Para salir, escribe  exit  (o Ctrl+D).
alumno@practica:~$
```

*Imagem pronta. Entrando no terminal de prática. Para sair, digite `exit` (ou Ctrl+D).*

Nenhuma janela se abriu. **Continua-se na mesma janela, mas já se está em outra máquina.** A última linha é o prompt do
terminal de prática, e será lida com calma no passo 3. Compare com o que se previu.

**Quão separado ele está do computador.** Para estes exercícios não se conecta nenhuma pasta do computador, o terminal
roda como `alumno`, sem privilégios de administrador, e lá dentro não há rede. O trabalho se guarda somente na pasta
`~/practica`. O que o contêiner usa, sim, do computador é processador, memória e disco.

Rode o primeiro comando lá dentro:

```bash
verifica 00
```

O `verifica` confere como está o terminal de prática e diz o que falta. Em cada marca diz o que mediu. Agora deve sair
assim:

```text
✅ Es Ubuntu 26.04 (medí: ID y VERSION_ID en /etc/os-release)
✅ Eres el usuario alumno (medí: id -un)
✅ /home/alumno/practica existe y es una carpeta (medí: que exista como carpeta)
✅ Puedes escribir en /home/alumno/practica (medí: creé un archivo vacío ahí y lo borré)
❌ Falta /home/alumno/practica/00/aqui-estuve.txt (medí: que el archivo exista). Créalo con el paso 2 de la lección, corre  verifica 00, sal con exit y vuelve a entrar

❌ Lección 00: falta lo marcado con ❌. Arréglalo y corre  verifica 00  otra vez.
```

*É Ubuntu 26.04 (medi: ID e VERSION_ID em `/etc/os-release`). O usuário é alumno (medi: `id -un`).
`/home/alumno/practica` existe e é uma pasta (medi: que exista como pasta). Pode-se escrever em
`/home/alumno/practica` (medi: criei um arquivo vazio ali e o apaguei). Falta `/home/alumno/practica/00/aqui-estuve.txt`
(medi: que o arquivo exista). Crie-o com o passo 2 da lição, rode `verifica 00`, saia com `exit` e entre de novo.
Lição 00: falta o que está marcado com ❌. Corrija e rode `verifica 00` outra vez.*

Quatro verdes e um vermelho. O vermelho é normal: é o que se faz no passo 2.

**Comprova** (objetivo 0.1, primeira metade): as quatro marcas verdes.

---

## Passo 2 · Deixe uma marca, saia e volte

Um contêiner é descartado ao sair. Então, onde fica o trabalho? Em uma única pasta: `practica`. O Docker a guarda à
parte, no que chama de **volume**, e a conecta de novo toda vez que se entra. Todo o resto nasce limpo outra vez.

**Preveja primeiro.** Vai-se criar um arquivo dentro de `practica`, sair e voltar a entrar. Ele continuará lá? E se
tivesse sido criado fora de `practica`?

Hoje estas duas linhas são uma receita pronta. Digite-as exatamente assim:

```bash
mkdir ~/practica/00
date > ~/practica/00/aqui-estuve.txt
```

- `mkdir` cria uma pasta: aqui, a pasta `00` dentro de `practica`.
- `date` responde a data e a hora. O sinal `>` manda essa resposta para um arquivo em vez de para a tela.
- `~` é a abreviação da pasta pessoal. Ela é vista com calma no passo 3.

Nas lições 02 e 06 entende-se cada peça em detalhe. Nenhuma das duas linhas responde nada: no terminal, **o silêncio
costuma significar que deu certo**.

Rode `verifica 00` outra vez, **antes de sair**. A última linha mudou:

```text
❌ Tu marca aqui-estuve.txt ya existe, y es la primera vez que la veo: fue en ESTA sesión (medí: no había apunte de una sesión anterior; lo acabo de dejar en 00/.sesion-verifica). Te falta salir con exit, volver a entrar con ./practicar y correr  verifica 00  otra vez
```

*A marca aqui-estuve.txt já existe, e é a primeira vez que o verificador a vê: foi nesta sessão (medi: não havia
registro de uma sessão anterior; acabo de deixá-lo em `00/.sesion-verifica`). Falta sair com `exit`, voltar a entrar
com `./practicar` e rodar `verifica 00` outra vez.*

O `verifica` já viu a marca e anotou em que sessão a viu. Uma **sessão** é cada vez que se entra. A anotação fica na
pasta de prática, em um arquivo chamado `.sesion-verifica`. Agora faça o que ele diz. Saia:

```bash
exit
```

```text
logout
Saliste de la terminal de práctica.
Lo que dejaste en ~/practica sigue guardado. Para volver a entrar:  ./practicar
```

*Saiu-se do terminal de prática. O que ficou em `~/practica` continua guardado. Para voltar a entrar: `./practicar`.*

Repare no prompt: mudou. Já não diz `alumno@practica`. Voltou-se ao computador. Entre de novo com `./practicar` e rode
`verifica 00`:

```text
✅ Saliste y volviste sin perder tu carpeta (medí: tu marca ya existía en una sesión anterior a esta, según el apunte de 00/.sesion-verifica)

✅ Lección 00: todo en verde.
```

*Saiu-se e voltou-se sem perder a pasta (medi: a marca já existia em uma sessão anterior a esta, segundo a anotação de
`00/.sesion-verifica`). Lição 00: tudo em verde.*

O `verifica` não usa o relógio. Compara duas coisas: a sessão em que viu a marca pela primeira vez e a sessão de agora.
São diferentes e a marca continua ali: saiu-se, voltou-se e a pasta sobreviveu.

**Começar do zero.** Se um dia a pasta de prática precisar ficar vazia outra vez, a partir do computador (não de
dentro):

```bash
./practicar desde-cero
```

Ele pede que se digite `si` antes de apagar, porque não dá para desfazer. Apaga só a pasta de prática. Se o terminal de
prática estiver aberto em outra janela, não apaga nada e pede que se saia primeiro. É um comando que apaga: digite-o à
mão. Se for rodado agora, será preciso repetir este passo.

**Comprova** (objetivo 0.1 completo): `verifica 00` tudo em verde.

---

## Passo 3 · Leia o prompt

Esta é a linha que está à frente:

```text
alumno@practica:~$
```

Tem quatro peças:

| Peça | O que diz |
|---|---|
| `alumno` | Quem se é: o usuário nesta máquina |
| `practica` | Em que máquina se está. Vai depois do `@`, como em um e-mail: «alumno, em practica» |
| `~` | Em que pasta se está. `~` é a abreviação de «a pasta pessoal» |
| `$` | Fim do prompt: daqui em diante quem escreve é a pessoa ao teclado. `$` quer dizer usuário normal |

**Preveja primeiro.** Há um comando para perguntar cada coisa: `whoami` (quem sou eu?), `hostname` (como se chama esta
máquina?) e `pwd` (em que pasta estou?). Antes de rodá-los, escreva no papel o que cada um vai responder. Para dois,
basta ler o prompt. O terceiro tem pegadinha.

Agora rode-os, um por um:

```bash
whoami
hostname
pwd
```

Compare com o papel. As respostas estão no fim, em «Compare as respostas»; não olhe antes de prever.

**Por que importa.** Em um trabalho real haverá três terminais abertos que parecem iguais: o computador, um servidor de
testes e um de produção. O prompt é a única coisa que diz em qual deles se está prestes a escrever. **Leia-o antes de
cada comando que mude algo.**

**Comprova** (objetivo 0.2): a previsão escrita, comparada com a saída.

---

## Passo 4 · Peça ajuda à máquina

Ninguém sabe os comandos de cor. O que se sabe é onde perguntar, e a resposta vem dentro da própria máquina. Há duas
formas.

**A curta: `--help`.** Quase todos os comandos aceitam `--help` e respondem com um resumo:

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

Neste terminal a ajuda aparece em inglês. Tem três partes: o que faz, como se usa (*Usage*) e que opções aceita
(*Options*). Uma **opção** é uma palavra com hífens que muda o que o comando faz.

**A longa: `man`.** É o manual (`man`, de *manual*). Cada comando tem uma página própria:

```bash
man whoami
```

Agora a tela é diferente. O prompt já não aparece, e embaixo diz:

```text
 Manual page whoami(1) line 1 (press h for help or q to quit)
```

Está-se **dentro** do manual. O que se digita já não são comandos: são teclas do manual. Estas cinco bastam:

| Tecla | O que faz |
|---|---|
| Seta para baixo, ou barra de espaço | Desce: a seta, uma linha; a barra, uma tela |
| Seta para cima | Sobe uma linha |
| `/` e uma palavra, depois Enter | Busca essa palavra para baixo |
| `n` | Vai para a próxima ocorrência |
| `q` | Sai do manual e volta ao prompt |

Saia com `q`. O prompt voltou.

**Com o Claude Code, nesta ordem.** Neste curso um assistente de IA entra sempre do mesmo jeito: **prever → rodar → ler
o que saiu → explicar com palavras próprias.** Aqui se pode pedir ao Claude Code que explique o que diz uma página do
`man` que não ficou clara. Mas a página é aberta por quem estuda, e a resposta é comprovada por quem estuda, no próprio
terminal. O exercício 0.3 se faz sem IA: o ponto é saber perguntar à máquina quando não há mais ninguém.

**Comprova** (objetivo 0.3): exercício 0.3, abaixo.

---

## Passo 5 · Saia do que prendeu

Há pouco se ficou «preso» no `man` e se saiu com `q`. Acontece com todo mundo: um programa toma a tela e o prompt não
volta. Há três teclas, e cada uma serve para uma coisa diferente.

**Um erro de propósito, antes.** Digite o comando com maiúscula:

```bash
Whoami
```

```text
bash: Whoami: command not found
```

O terminal diferencia maiúsculas de minúsculas: `Whoami` não existe. E repare no que aconteceu depois: devolveu o
prompt. **Um erro que devolve o prompt não prendeu.** Lê-se a mensagem e segue-se. Preso é quando o prompt não volta.

Agora sim, três maneiras de ficar preso. Faça as três.

**1. Um programa que não termina.** O `sleep 1000` espera mil segundos sem fazer nada:

```bash
sleep 1000
```

O prompt não volta. Aperte **Ctrl+C** (mantenha o Ctrl e toque no C). Aparece `^C` e o prompt volta. O Ctrl+C diz ao
programa: «pare já».

**2. Um programa que espera que se escreva.** O `cat`, sem mais nada, repete o que se digitar:

```bash
cat
```

Digite `hola` («olá») e Enter: ele responde `hola`. Vai continuar esperando para sempre. Aperte **Ctrl+D** em uma linha vazia.
O prompt volta. O Ctrl+D diz ao programa: «não há mais nada a escrever».

**3. Um programa de tela cheia.** O `htop` mostra os programas que estão rodando:

```bash
htop
```

Ele toma a tela inteira. Saia com **`q`**, igual ao `man`.

**Qual serve para quê:**

| Tecla | Diz ao programa | Use quando |
|---|---|---|
| `q` | «Feche-se» | Está-se em um programa que se folheia ou que toma a tela: `man`, `less`, `htop` |
| Ctrl+C | «Pare já» | Um comando está rodando e não termina |
| Ctrl+D | «Não há mais nada a escrever» | Um programa espera texto digitado |

Três coisas medidas que convém saber:

- **O Ctrl+C não tira do `man`.** A página continua. De lá se sai com `q`.
- **O `q` no prompt não faz nada útil.** Responde `bash: q: command not found`. Serve para saber que já se estava
  fora.
- **O Ctrl+D no prompt vazio tira do terminal de prática.** O prompt também é um programa que espera texto: chama-se
  bash. O Ctrl+D ali é o mesmo que `exit`. Não se perde nada: volta-se com `./practicar`.

**Comprova** (objetivo 0.4): as cinco situações da revisão, em «Para fechar».

---

## Três notas sobre o terminal de prática

**O Mac usa zsh; lá dentro se usa bash.** O programa que lê os comandos se chama *shell*. Desde o macOS 10.15, o das
contas novas do Mac é o zsh, segundo a Apple. Dentro do contêiner é o bash, que é comum em muitos servidores. Por isso
o curso inteiro se faz lá dentro: o grupo todo pratica no mesmo ambiente. O terminal do computador serve apenas para
rodar `./practicar`.

**O `htop` e o `ps` só veem o contêiner.** Ao abrir o `htop` apareceram só dois programas: o bash e o próprio htop.
Os do computador não aparecem, porque o Docker dá a cada contêiner uma lista de processos própria. Para aprender a
lê-los e a pará-los, que é a lição 10, basta.

**Os comandos básicos estão escritos em Rust.** Desde o Ubuntu 25.10, comandos como `ls`, `cat` e `whoami` já não vêm
do projeto GNU, que é o clássico, e sim do `rust-coreutils`. Quem diz é a documentação do Ubuntu, e no terminal isso
se vê em `man whoami`: sob *VERSION* diz `uutils coreutils`. Três continuam sendo do GNU: `cp`, `mv` e `rm`.

- **O que isso significa:** quem compara com um tutorial ou com um servidor mais antigo pode ver um texto de `man` e de
  `--help` diferente do daqui. Não é erro de quem compara nem do tutorial.
- **O que não muda neste curso:** os comandos têm o mesmo nome e as opções que se ensinam funcionam igual. As saídas
  que aparecem nas lições são da imagem testada em 18 e 19/11/2026. A imagem é construída sempre sobre o Ubuntu 26.04
  vigente, então, depois de uma atualização, uma saída pode variar um pouco.

---

## Os exercícios

O exercício 0.3 se faz **sem IA** e sem buscador: só com o `man`.

| # | Faz-se | Está feito quando |
|---|---|---|
| 0.1 | Os passos 1 e 2: entra-se, deixa-se a marca, sai-se e volta-se | O `verifica 00` sai todo em verde |
| 0.2 | O passo 3: prevê-se por escrito o que respondem `whoami`, `hostname` e `pwd`, e depois se rodam | Há a previsão e a saída, e se sabe explicar a que não coincidiu |
| 0.3 | Respondem-se as três perguntas abaixo, só com o `man` | As três respostas coincidem com «Compare as respostas» |
| 0.4 | O passo 5: fica-se preso de propósito no `sleep 1000`, no `cat` e no `htop`, e sai-se dos três | Saiu-se dos três sem fechar a janela |

**As três perguntas do 0.3.** Para cada uma: abra a página, busque com `/`, anote a resposta e saia com `q`.

1. No `man whoami`: quantas opções o `whoami` aceita e quais são?
2. No `man ls`, busque `--all`: que letra curta é o mesmo que `--all`, e o que ela faz?
3. No `man man`, busque `Executable`: o manual se divide em seções numeradas. Que número tem a dos programas e
   comandos? E a dos comandos de administração do sistema?

**Tempo (estimado):** de 60 a 90 minutos, sem contar a instalação do Docker. É uma estimativa: ajusta-se com o primeiro
grupo que fizer o curso.

---

## Não funcionou?

| Aparece | Causa provável | O que fazer |
|---|---|---|
| `❌ No encuentro el comando «docker» en esta computadora.` *(Não encontro o comando «docker» neste computador.)* | O Docker não está instalado | «Antes de começar», conforme o sistema |
| `❌ Docker está instalado, pero no pude hablar con él.` *(O Docker está instalado, mas não consegui falar com ele.)* | O mais comum: o Docker está desligado, ou o usuário não tem permissão. Pode ser outra coisa | Rodar `docker info` e seguir o erro exato que ele der |
| `no such file or directory: ./practicar` | Não se está na pasta do curso | `cd curso-terminal` e repetir |
| A primeira construção leva muitos minutos e aparecem linhas que começam com `Ign:` | Os servidores de pacotes do Ubuntu respondem devagar. Aconteceu em 19/11/2026: a mesma construção levou 8 minutos, com 3 linhas `Ign:`, contra um minuto e meio em 18/11 | Deixá-la rodar. `Ign:` não é um erro: o apt tenta de novo. Se no fim sair `❌ No se pudo construir la imagen.` *(Não foi possível construir a imagem.)*, seguir a linha de baixo |
| `❌ No se pudo construir la imagen.` *(Não foi possível construir a imagem.)* | A rede, o espaço em disco, as permissões do Docker ou o material do curso | Ler o **primeiro** erro que saiu acima da mensagem, corrigi-lo e rodar `./practicar` outra vez |
| `❌ ./practicar abre una terminal interactiva…` *(O ./practicar abre um terminal interativo…)* | Foi rodado com um pipe ou um redirecionamento | Rodá-lo sozinho: `./practicar` |
| `command not found: verifica` | Rodou-se o `verifica` no computador, não lá dentro | Entrar com `./practicar`; o prompt deve dizer `alumno@practica` |
| `mkdir: /home/alumno/practica/00: File exists` | A pasta `00` já tinha sido criada | Sem problema: seguir com a linha do `date` |
| A última linha do `verifica 00` continua em vermelho depois de voltar | Não se rodou o `verifica 00` antes de sair, então ele acabava de ver a marca | Já ficou anotada: sair com `exit`, entrar e rodar `verifica 00` outra vez |
| `❌ No sé en qué sesión estás…` *(Não sei em que sessão está…)* | Entrou-se no contêiner sem o script do curso | Sair e entrar com `./practicar` |
| O prompt não volta | Um programa tomou o terminal | Tentar `q`; se não, Ctrl+C; se não, Ctrl+D (passo 5) |
| Fechou-se a janela sem digitar `exit` | O terminal de prática pode ter ficado aberto por dentro | Abrir outra janela e entrar com `./practicar`: a pasta continua lá |
| `❌ No borré nada: tu carpeta de práctica está en uso.` *(Não apaguei nada: a pasta de prática está em uso.)* | Há um terminal de prática aberto, talvez de uma janela fechada sem `exit` | Sair dele com `exit`. Se a janela já não existe, a mesma mensagem traz a linha `docker stop …` para fechá-lo à mão |
| O curso foi atualizado e o `verifica` não conhece uma lição | A imagem é de antes | `./practicar reconstruir` |

---

## Para fechar

**Responda sem reler.**

**Cinco situações: que tecla?** Passa-se com 4 de 5.

1. Abriu-se o `man ls` para buscar uma opção e ela já foi encontrada.
2. Rodou-se um comando que leva dez minutos e não termina.
3. Digitou-se `cat` e Enter por engano, e agora tudo o que se escreve é repetido.
4. Abriu-se o `htop` para ver o que está rodando e já se viu o bastante.
5. Acabou por hoje: está-se em `alumno@practica:~$` e quer-se voltar ao computador.

**E três perguntas:**

6. Aparece `soporte@caja-07:~$`. Sem rodar nada: quem é o usuário, em que máquina e em que pasta ele está?
7. Criou-se um arquivo fora de `~/practica` e saiu-se. Ele vai estar lá na volta? Por quê?
8. Não se lembra do que faz uma opção de um comando e não há internet. Que duas coisas se tentam?

**O que vem a seguir:** já há um lugar para praticar e já se sabe onde se está parado. Na lição 01 a pessoa se move pelas
pastas sem vê-las.

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

**Passo 3 (exercício 0.2).** `whoami` responde `alumno`. `hostname` responde `practica`. `pwd` responde `/home/alumno`.
Essa é a pegadinha: o prompt diz `~`, e `~` é a abreviação da pasta pessoal, que nesta máquina é `/home/alumno`. `~` e
`/home/alumno` nomeiam o mesmo lugar: o prompt usa a forma curta.

**Exercício 0.3.**

1. Duas: `-h` ou `--help`, que mostra a ajuda, e `-V` ou `--version`, que mostra a versão.
2. `-a`. Faz o `ls` não esconder os arquivos ocultos, que são os que têm um nome que começa com ponto.
3. A seção 1 é a dos programas e comandos (*Executable programs or shell commands*). A 8 é a dos comandos de
   administração do sistema (*System administration commands*). Por isso a página diz `whoami(1)`: o número entre
   parênteses é a seção.

**Para fechar.**

1. `q`.
2. Ctrl+C.
3. Ctrl+D, em uma linha vazia. O Ctrl+C também o interrompe.
4. `q`.
5. `exit`, ou Ctrl+D no prompt vazio.
6. O usuário é `soporte`, na máquina `caja-07`, na pasta pessoal.
7. Não. Ao sair, o contêiner é descartado e nasce limpo de novo. Só `~/practica` é guardada à parte, em um volume.
8. O comando com `--help`, e `man` seguido do comando.

---

## Fontes da lição

Abertas em 18/11/2026.

- Ubuntu. *Ubuntu 26.04 LTS release notes* (Resolute Raccoon; publicada em 23 de abril de 2026). https://documentation.ubuntu.com/release-notes/26.04/
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, seção «rust-coreutils» (os comandos básicos vêm do `rust-coreutils` desde o 25.10; `cp`, `mv` e `rm` continuam sendo do GNU). https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- Ubuntu Manpages. *man(1)*, do Ubuntu 26.04 (seções do manual; `-k`). https://manpages.ubuntu.com/manpages/resolute/en/man1/man.1.html
- Docker. *Install Docker Engine on Ubuntu* (versões do Ubuntu com suporte; Linux Mint sem suporte oficial). https://docs.docker.com/engine/install/ubuntu/
- Docker. *Install Docker Desktop on Mac* (instalador por processador; versões do macOS com suporte). https://docs.docker.com/desktop/setup/install/mac-install/
- Docker. *Docker Desktop WSL 2 backend on Windows* (integração com a distribuição do WSL). https://docs.docker.com/desktop/features/wsl/
- Microsoft. *How to install Linux on Windows with WSL* (`wsl --install`; versões do Windows). https://learn.microsoft.com/pt-br/windows/wsl/install
- Docker. *Volumes* (um volume guarda dados fora do contêiner e sobrevive a que ele seja apagado). https://docs.docker.com/engine/storage/volumes/
- Docker. *docker container run* (`-i`, `-t`, `--rm`, `-h`, `--network`; cada contêiner tem uma lista de processos própria). https://docs.docker.com/reference/cli/docker/container/run/
- Apple. *Use zsh as the default shell on Mac* (zsh por padrão desde o macOS 10.15). https://support.apple.com/pt-br/102360
- Medição nossa, 18 e 19/11/2026, em um Mac com Apple silicon (Docker 29.2.1, linux/arm64) e em um computador com
  Linux Mint 22.3 (x86_64, Docker 29.10.0). O que se rodou em cada um e como se mediu cada número está nas duas tabelas
  de «Antes de começar». As telas desta lição e as três teclas do passo 5 foram tiradas no Mac. A revisão de 19/11
  —o contêiner sem rede, as etiquetas do curso e o `verifica` que já não usa o relógio— foi testada nos dois.
