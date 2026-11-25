# Lección 03 — Ver lo que hay dentro

**Por Dorian Chávez, fundador de Hábil y arquitecto de integración.**

**Antes de leer, cinco palabras.** Una línea cada una; las demás llegan en el paso donde se usan.

- **Bitácora:** el archivo donde un programa va apuntando lo que hace, un renglón por cosa y con su hora. En inglés,
  y en los servidores, la vas a ver como *log*.
- **Renglón:** una línea de un archivo. Termina con un salto de renglón, el Enter que no se ve. Cuando un comando
  cuenta renglones, cuenta esos saltos.
- **Texto y binario:** un archivo de texto tiene letras, números y signos que tú puedes leer. Uno binario tiene bytes
  pensados para un programa, no para tus ojos: una foto, un programa, un archivo comprimido.
- **Paginador:** un programa que te enseña un texto por pantallas y te deja ir y venir por él. El de esta lección se
  llama `less`.
- **Comprimido:** un archivo que un programa, como `gzip`, hizo más chico. Para leerlo hay que descomprimirlo.

**Qué te llevas:** muestras un archivo completo con `cat`, también con el número de cada renglón; sacas el
principio, el final o un renglón exacto con `head` y `tail`; cuentas renglones, palabras y bytes con `wc`; hojeas y
buscas en `less` y sales sin perderte; miras crecer una bitácora con `tail -f`; y antes de abrir un archivo que no
conoces, le preguntas a `file` qué es.

**Idea nueva de esta lección (la única que se evalúa):** en un servidor casi todo es texto, y se lee sin abrir un
editor. Antes de leer **decides qué pedazo necesitas**: todo, el principio, el final, un renglón, cuántos son, o
hojear buscando algo. La pregunta no es qué comandos existen, sino qué pedazo pide cada situación.

**En qué orden va:** armas una sala de guardia (paso 1), lees archivos completos (paso 2), sacas pedazos y cuentas
(paso 3), hojeas y buscas (paso 4), miras crecer una bitácora (paso 5), preguntas qué es un archivo antes de abrirlo
(paso 6) y al final compruebas todo con `verifica` (paso 7).

---

## La escena

*Escenario inventado. Las pantallas de la lección se copiaron de la terminal de práctica.*

Jueves, 03:12. El teléfono de Lucía vibra sobre la mesa.

—Las sucursales están rechazando todas las tarjetas. Empezó hace rato, no sabemos cuándo. ¿Qué dice la bitácora de
pagos?

Lucía ya sabe llegar a la carpeta sin verla, y que ahí no se borra nada sin mirar. Lo que nunca ha hecho es **leer**
un archivo en una máquina sin ventanas. No hay doble clic. Está la misma línea de siempre, esperando.

La bitácora tiene cientos de renglones, y en un servidor de verdad podría tener millones. En algún lugar de ese
archivo está la respuesta: el renglón donde todo empezó, y el último, que dice qué está pasando ahorita. Junto a ella
hay otra, la de ayer, con un nombre que termina en `.gz` y que no se deja leer como las demás. Del otro lado,
alguien espera una hora, un número y una frase para decidir qué hacer con cientos de cobros.

Hay algo a su favor: las bitácoras suelen ser texto. *The Twelve-Factor App*, la guía de Adam Wiggins sobre cómo
construir aplicaciones que corren en servidores, lo describe así: en crudo, una bitácora es normalmente texto, con un
evento por renglón y en orden de tiempo. Si es texto, se lee con los comandos de esta lección.

Esta lección es esa media hora, en una sala de guardia hecha para leer sin consecuencias.

---

## Antes de empezar

**Qué necesitas:** la lección 02 hecha; el material del curso actualizado; y papel y lápiz para tus predicciones.

**Actualiza tu terminal de práctica.** Igual que en las lecciones anteriores, desde tu computadora, en la carpeta del
curso:

```bash
git pull
./practicar reconstruir
```

Esta vez la imagen trae tres paquetes nuevos (`file`, `gzip` y `ncurses-bin`), así que `./practicar reconstruir`
necesita red y tarda un poco más que en la 02: reutiliza Ubuntu y los paquetes de antes, y solo baja los nuevos. Debe
terminar con `✅ Imagen lista.` Tu carpeta `~/practica` no se toca. Después entra con `./practicar`.

**Dónde se probó esta lección:**

| Sistema | Estado |
|---|---|
| Linux (mx1) | Probada el 25-nov-2026 en la rama `leccion/03` (3bf1787), medida por master: host x86_64, Docker 29.10.0; la imagen se construyó en 9 s y pesa 232,656,309 bytes; Ubuntu 26.04, `cat` (uutils coreutils) 0.10.0, `less` 668 y `file-5.46`. Corrieron `prepara 03`, cada comando de los pasos 1 a 7, `simula 03`, y `verifica 03` en rojo y en verde. Sin medir: `./practicar reconstruir` (la imagen se construyó con `docker build`), `prepara 03` sobre una sala ya armada, y `zless` |
| Mac con Apple silicon, Ubuntu, Windows con Docker Desktop y WSL 2 | Sin probar todavía con esta lección |

**Qué va a coincidir con tu pantalla y qué no.** Coinciden los nombres, los contenidos y las fechas de la sala, y los
mensajes de los comandos. Cambian el acomodo en columnas, los colores y el tamaño de tu ventana, que decide cuántos
renglones caben en pantalla.

---

## Paso 1 · Arma la sala de guardia

Adentro de tu terminal de práctica:

```bash
prepara 03
```

```text
✅ Sala de guardia lista en /home/alumno/practica/03 (medí: 3 carpetas y 8 archivos, cada uno con
   su contenido; las dos bitácoras, por su huella).
   Te dejé la hoja de respuestas, con sus claves vacías: /home/alumno/practica/respuestas/03.md
   Empieza por ahí:  cd ~/practica/03
```

`prepara 03` funciona como el de la 02: no tiene azar, toca solo `~/practica/03` y tu hoja de respuestas, y si la
sala ya existe te pide escribir `si` antes de borrarla. Las bitácoras no vienen escritas renglón por renglón: salen
de una regla fija, y `prepara` comprueba al final que cada una tenga su *huella*, una especie de firma del contenido.
Si un solo carácter cambia, la huella ya no coincide.

**La sala, vista desde arriba:**

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
│   └── arranque.log    ← la que vas a ver crecer en el paso 5
└── reportes
    └── sucursales.csv  ← las sucursales que reportaron rechazos
```

Las demostraciones van en `ensayo`; los ejercicios, en `pagos` y `reportes`. **En esta sala nada se edita ni se
borra:** `verifica 03` revisa que todo siga como lo dejó `prepara`, y si algo cambió te lo dice.

Tu hoja de respuestas trae ocho claves, una por cada cosa que vas a leer. Se llena igual que en las lecciones
anteriores, con `anota`.

**Lo demuestra:** todavía nada. Este paso deja listo el material.

---

## Paso 2 · Todo de un jalón: `cat`

Entra al ensayo:

```bash
cd ~/practica/03/ensayo
```

`cat` viene de *concatenate*, «encadenar»: su manual dice que pega uno o varios archivos y los escribe en la
pantalla. Con un solo archivo, lo escribe completo:

```bash
cat corto.txt
```

```text
primer renglón
segundo renglón
tercer renglón, el último
```

En la lección 00, sin archivo, se quedaba esperando. Con un archivo lo suelta todo y te devuelve el indicador.

**Con dos archivos, los encadena.** **Predice:** ¿qué sale con `cat corto.txt corto.txt`?

```text
primer renglón
segundo renglón
tercer renglón, el último
primer renglón
segundo renglón
tercer renglón, el último
```

Uno detrás del otro, sin separación. Por eso se llama así.

### `cat -n`: con el número de cada renglón

La opción `-n`, de *number*, le pone su número a cada renglón:

```bash
cat -n corto.txt
```

```text
     1	primer renglón
     2	segundo renglón
     3	tercer renglón, el último
```

El número **no es parte del archivo**: `cat` lo agrega al mostrarlo. Sirve para contestar «¿en qué renglón está tal
cosa?», que en una guardia te van a preguntar.

### Lo que no cabe en la pantalla

**Predice antes de correrlo:** `cien.txt` tiene cien renglones, y tu ventana tiene menos. ¿Qué vas a ver con
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

Ves el final, hasta el `renglón 100 de 100`. El principio se fue de la pantalla: `cat` no sabe de pantallas, escribe
todo y lo que no cabe se va hacia arriba. Con cien renglones puedes subir con la rueda del ratón; con un millón, no.

**De aquí sale la regla de esta lección:** `cat` es para archivos cortos, los que sabes que caben. Para los largos,
pides un pedazo (paso 3) o los hojeas (paso 4). Y en un servidor, **no abres una bitácora en un editor para leerla**.
Es regla del curso, por dos razones: un editor puede cambiar el archivo si tocas una tecla de más, y un archivo
enorme puede tardar mucho en abrir. Para leer, se usa un comando que solo lee.

### Tu turno: la configuración

Vuelve a la sala con `cd ~/practica/03`. La configuración del servicio de pagos es corta: `cat` es la herramienta
correcta.

1. Muéstrala completa, **con números de renglón**.
2. Los renglones que empiezan con `#` son comentarios, notas para las personas: el servicio no los lee. Fíjate cuáles
   son.
3. Busca el renglón que dice cuántos segundos espera el servicio entre un reintento y otro: su nombre trae la
   palabra `espera_segundos`. Anota **el número de ese renglón**:

   ```bash
   anota 03 espera 1
   ```

   Ahí, el `1` es un ejemplo: en su lugar va el número que leíste.

**Lo demuestra** (objetivo 3.1): la marca `espera` de `verifica 03`.

---

## Paso 3 · Solo un pedazo: `head`, `tail` y `wc`

Vuelve al ensayo con `cd ~/practica/03/ensayo`.

### `head`: el principio

`head` quiere decir «cabeza». Según su manual, si no le dices cuántos, escribe los primeros diez renglones.
**Predice:** ¿cuál es el último que vas a ver con `head cien.txt`?

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

Del `renglón 001` al `renglón 010`. Diez, como dice el manual. Con `-n` y un número le dices cuántos:

```bash
head -n 3 cien.txt
```

```text
renglón 001 de 100
renglón 002 de 100
renglón 003 de 100
```

### `tail`: el final

`tail` quiere decir «cola». Funciona igual que `head`, pero desde el final: diez renglones si no le dices otra cosa,
y los que pidas con `-n`.

```bash
tail -n 2 cien.txt
```

```text
renglón 099 de 100
renglón 100 de 100
```

En una bitácora, el final es lo más reciente. **`tail` es casi siempre lo primero que se corre en una guardia**: lo
último que pasó es lo que está pasando.

### Un renglón exacto, con lo que ya tienes

Te piden el renglón 42. **Predice:** ¿cuál es el último renglón que escribe `head -n 42`?

```bash
head -n 42 cien.txt
```

```text
…
renglón 040 de 100
renglón 041 de 100
renglón 042 de 100
```

El último que aparece es el `renglón 042 de 100`. `head -n 42` escribe los renglones del 1 al 42, así que el que
queda hasta abajo, justo encima del indicador, **es el 42**. No necesitas contar nada: lees el último. En la lección
06 vas a ver cómo quedarte solo con ese, sin los 41 de arriba. Por ahora, este truco alcanza.

### `wc`: contar sin leer

`wc` viene de *word count*, «contar palabras», aunque cuenta tres cosas. Sin opciones escribe tres números y el
nombre del archivo:

```bash
wc cien.txt
```

```text
 100  400 2000 cien.txt
```

**Predice antes de leer la explicación:** `cien.txt` tiene cien renglones, y cada uno dice algo como
`renglón 001 de 100`. ¿Cuántas palabras crees que tiene en total?

Los tres números, en orden, son: renglones, palabras y bytes. Para `cien.txt` deben ser 100, 400 y 2000. Cada
renglón tiene cuatro palabras, porque para `wc` una palabra es cualquier grupo de caracteres sin espacios, según su
manual: `001` y `100` cuentan como palabras. Y los bytes son el tamaño, el mismo número que viste en `ls -l` en la
lección 01.

Para pedir solo uno, cada cosa tiene su opción:

| Opción | Cuenta | Para acordarte |
|---|---|---|
| `wc -l` | Renglones | *lines* |
| `wc -w` | Palabras | *words* |
| `wc -c` | Bytes | el tamaño |

```bash
wc -l cien.txt
```

```text
100 cien.txt
```

**Cómo cuenta los renglones.** El manual de `wc` dice que `-l` cuenta los **saltos de renglón**. Si el último renglón
de un archivo no termina con su salto, `wc -l` no lo cuenta. En esta sala todos los archivos terminan con su salto,
pero en un servidor no siempre: si `wc -l` te da uno menos de lo que ves, esa es la causa.

**Tu comando, no el del manual.** Aquí `cat`, `head`, `tail` y `wc` son de `rust-coreutils`, como `ls` en la lección
01; según la documentación de Ubuntu, solo `cp`, `mv` y `rm` siguen siendo de GNU. Los manuales que cita esta lección
son los de GNU. Si algo no coincide con un tutorial, `cat --help` te dice lo que acepta **tu** `cat`.

### Tu turno: la bitácora y el reporte

Vuelve a la sala con `cd ~/practica/03`. Son cuatro preguntas. **Antes de cada una, escribe en tu papel qué pedazo
necesitas y con qué comando lo vas a sacar**; después lo corres y anotas.

1. **El folio del renglón 137** de `pagos/pagos.log`. Cada cobro trae un folio, una F, un guion y seis cifras.
   Anótalo con `anota 03 folio-137` y el folio, tal cual.
2. **La hora del último renglón** de `pagos/pagos.log`: solo la hora, `hh:mm:ss`, sin la fecha. Se anota con
   `anota 03 ultima-hora`. Lee el renglón completo: es lo que le vas a decir a quien está en el teléfono.
3. **Cuántos renglones tiene** `pagos/pagos.log`. Solo el número: `anota 03 renglones`.
4. **Cuántas sucursales** trae `reportes/sucursales.csv`. Es un CSV: una tabla escrita como texto, un renglón por
   fila y comas entre columnas. Mira primero sus tres primeros renglones con `head`. **Predicción D:** ¿el número de
   sucursales es el mismo que el número de renglones? Anota tu respuesta con `anota 03 sucursales`.

**Lo demuestra** (objetivo 3.2): las marcas `folio-137`, `ultima-hora`, `renglones` y `sucursales` de `verifica 03`,
y tu predicción D, que comparas tú al final.

---

## Paso 4 · Hojear y buscar: `less`

`head` y `tail` sirven cuando sabes dónde está lo que buscas. Cuando no, hojeas con `less`, un *paginador*: según su
autor, un programa que muestra archivos de texto y **no un editor**. No puedes descomponer el archivo con una tecla de
más.

Su manual dice otra cosa que importa en un servidor: `less` no necesita leer el archivo completo antes de empezar, así
que con archivos grandes arranca más rápido que un editor.

### Las teclas ya las conoces

Vuelve al ensayo y abre `cien.txt`:

```bash
cd ~/practica/03/ensayo
less cien.txt
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
renglón 011 de 100
renglón 012 de 100
renglón 013 de 100
renglón 014 de 100
renglón 015 de 100
renglón 016 de 100
renglón 017 de 100
renglón 018 de 100
renglón 019 de 100
renglón 020 de 100
renglón 021 de 100
renglón 022 de 100
renglón 023 de 100
cien.txt
```

La pantalla se llena con el principio del archivo. Estás **adentro** de
`less`, igual que estuviste adentro de `man` en la lección 00, y las teclas son las mismas que usaste ahí, más unas
cuantas:

| Tecla | Qué hace |
|---|---|
| Barra espaciadora | Baja una pantalla |
| `b` | Sube una pantalla (*back*) |
| Flechas arriba y abajo | Un renglón |
| `g` | Al principio del archivo |
| `G` | Al final del archivo |
| Un número y `g` | A ese renglón: `42g` te lleva al 42 |
| `/` y un texto, luego Enter | Busca ese texto hacia abajo |
| `n` | La siguiente vez que aparece |
| `N` | La anterior |
| `h` | La ayuda de `less`, con todas las teclas |
| `q` | Sale y te regresa al indicador |

Prueba cada una. Teclea `42` y luego `g`: te deja en el renglón 42. Luego `G`, luego `g`. Sal con `q`.

**Ctrl+C no te saca.** En `less`, Ctrl+C detiene lo que estuviera haciendo, por ejemplo una búsqueda larga, y lo deja
esperando tu siguiente tecla; no lo cierra, según las preguntas frecuentes de su autor. Como en `man`: de `less` se
sale con `q`.

### Con números de renglón: `less -N`

Con `-N`, en mayúscula, cada renglón aparece con su número, como en `cat -n`, pero hojeando:

```bash
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

Ya no cuentas: el número de cada renglón está a la izquierda.

### Buscar distingue mayúsculas

Adentro de `less -N cien.txt`, teclea `/042` y Enter. Salta al renglón 42. Teclea `n`: no hay otro.

**Predice:** ¿qué pasa si buscas `/RENGLÓN`, en mayúsculas?

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
Pattern not found  (press RETURN)
```

No lo encuentra, aunque la palabra está en los cien renglones. El manual de `less` lo dice: **por omisión, la
búsqueda distingue mayúsculas de minúsculas**. `RENGLÓN` y `renglón` son, para `less`, dos cosas distintas. En una
bitácora eso importa: `ERROR` y `error` no tienen por qué ser lo mismo. Sal con `q`.

### Tu turno, sin ayuda: el primer `ERROR`

Este ejercicio se hace **solo con tu terminal**: sin buscador y sin que nadie te dicte la tecla. Lo que necesitas está
en la tabla de arriba y en la ayuda de `less`, con `h`.

1. Vuelve a la sala, `cd ~/practica/03`, y abre `pagos/pagos.log` en `less`, **con números de renglón**.
2. Antes de buscar, hojea: unas cuantas pantallas con la barra, el final con `G`, el principio con `g`. Mira cómo
   es un renglón normal y cómo es uno que avisa algo.
3. **Predicción E:** la bitácora tiene renglones `INFO`, `WARN` y `ERROR`. ¿Esperas que el primer `ERROR` esté cerca
   del principio o cerca del final? Escríbelo.
4. Desde el principio del archivo, busca el **primer renglón que dice `ERROR`, en mayúsculas**. Lee ese renglón
   completo y los de después: dice por qué empezaron los rechazos.
5. Sal con `q` y anota el **número** de ese renglón con `anota 03 primer-error`.

**Lo demuestra** (objetivo 3.3): la marca `primer-error` de `verifica 03`, y tu predicción E.

---

## Paso 5 · Una bitácora que crece: `tail -f`

Una bitácora no es un archivo terminado: mientras el servicio corre, sigue escribiendo renglones al final. *The
Twelve-Factor App* lo dice así: una bitácora no tiene principio ni fin fijos, fluye mientras la aplicación opera, y se
puede mirar en vivo con `tail` en una terminal.

Para eso está `tail -f`. La `-f` es de *follow*, «seguir»: según su manual, `tail` escribe lo nuevo **conforme el
archivo crece**, y no termina solo.

En la escena, a las 03:20 alguien reinició el servicio de pagos. Su bitácora de arranque tiene, por ahora, dos
renglones:

```bash
cd ~/practica/03
tail pagos/arranque.log
```

```text
2025-06-12 03:20:00 INFO  pagos: me pidieron reiniciar; me detengo
2025-06-12 03:20:01 INFO  pagos: detenido
```

Para ver el arranque necesitas que algo escriba en ese archivo. En esta sala no hay un servicio de verdad, así que lo
hace un comando del curso, `simula 03`: espera cinco segundos y después agrega un renglón cada tres, como lo haría el
servicio al arrancar. Te regresa el indicador enseguida y escribe por su cuenta.

**Predice:** cuando corras `tail -f`, ¿el indicador va a volver solo?

Corre estas dos líneas, una tras otra, sin esperar entre ellas:

```bash
simula 03
tail -f pagos/arranque.log
```

```text
Empiezo en 5 segundos: agrego 7 renglones a ~/practica/03/pagos/arranque.log,
uno cada 3 segundos. Termino solo, en menos de un minuto. Ya puedes mirarlo.
2025-06-12 03:20:00 INFO  pagos: me pidieron reiniciar; me detengo
2025-06-12 03:20:01 INFO  pagos: detenido
2025-06-12 03:21:10 INFO  pagos: arranco con la versión 4.11.2
2025-06-12 03:21:11 INFO  pagos: leo la configuración de pagos.conf
2025-06-12 03:21:12 INFO  conciliacion: busco el lote del 11 de junio
2025-06-12 03:21:14 INFO  conciliacion: el lote del 11 de junio ya llegó (03:16); lo reviso
2025-06-12 03:21:19 INFO  conciliacion: el lote cuadra; los 168 rechazos quedan para cobrarse de nuevo
2025-06-12 03:21:20 INFO  pagos: conecto con el banco
2025-06-12 03:21:22 INFO  pagos: listo para cobrar; código de arranque A-7731
```

Primero `simula` te dice qué va a hacer. Después `tail -f` muestra los dos renglones que ya había, y se queda ahí.
Cada pocos segundos aparece un renglón nuevo abajo: tú no tocaste nada, el archivo creció y `tail` te lo fue
enseñando. El indicador no vuelve: `tail -f` sigue esperando más, para siempre.

Cuando aparezca el renglón que dice `listo para cobrar`, sal con **Ctrl+C**. Es la tecla de la lección 00 para «un
comando que no termina». Esta vez no es `q`: `tail` no es un paginador, es un comando que sigue corriendo.

**Mirar no es detener.** Ctrl+C detuvo a `tail`, no a lo que escribía: si sales antes de tiempo, `simula` sigue
agregando renglones, y los ves con otro `tail -f`. En un servidor, cerrar tu `tail -f` no le hace nada al servicio.

### Tu turno: el código de arranque

El último renglón del arranque trae un código: la letra `A`, un guion y cuatro cifras. Quien está en el teléfono lo
necesita para confirmar que el servicio que arrancó es el bueno. Anótalo con `anota 03 arranque` y el código.

Si se te pasó el momento, no pasa nada: `simula 03` no se repite sobre un archivo que ya creció. Para verlo crecer
otra vez, arma la sala de nuevo con `prepara 03`. Tus respuestas no se borran.

**Lo demuestra** (objetivo 3.4): la marca `arranque` de `verifica 03`.

---

## Paso 6 · Antes de abrirlo, pregunta qué es: `file`

Falta la bitácora de ayer, `pagos/pagos.log.1.gz`. Su nombre termina en `.log`, más un `.1` y un `.gz`. ¿Es texto?
Para no adivinar está `file`.

`file` mira el **contenido** de un archivo, no su nombre, y dice qué tipo de archivo es. Su manual explica cómo:
primero revisa qué clase de cosa es en el disco (una carpeta, un archivo vacío), luego busca cerca del principio las
marcas fijas que trae cada formato conocido, y si no encaja con ninguno, revisa si es texto y con qué letras está
escrito. Lo primero que encaja es lo que te dice.

En el ensayo:

```bash
cd ~/practica/03/ensayo
file corto.txt cien.txt
```

```text
corto.txt: Unicode text, UTF-8 text
cien.txt:  Unicode text, UTF-8 text
```

En los dos aparece la palabra **text**. Según el manual de `file`, esa palabra quiere decir que el archivo solo tiene
caracteres que se pueden escribir y unos pocos de control comunes, y que **probablemente es seguro leerlo en la
terminal**. Ahora la sala:

```bash
file ../pagos/* ../reportes/*
```

```text
../pagos/arranque.log:      Unicode text, UTF-8 text
../pagos/pagos.conf:        Unicode text, UTF-8 text
../pagos/pagos.log:         Unicode text, UTF-8 text
../pagos/pagos.log.1.gz:    gzip compressed data, max compression, from Unix, original size modulo 2^32 52807
../reportes/sucursales.csv: CSV Unicode text, UTF-8 text
```

`../pagos/*` es un patrón, como los de la lección 02: la terminal lo cambia por todos los nombres de `pagos`. Todos
dicen *text*, menos uno. **El nombre no decide:** `file` reconoce la bitácora de ayer por las primeras marcas de su
contenido, y dice qué formato es. No dice *text*.

### Por qué no se le hace `cat` a un binario

`cat` no pregunta qué es un archivo: escribe sus bytes en la pantalla, tal cual. Con un archivo de texto eso son
letras. Con uno binario son bytes que no son letras, y la terminal intenta mostrarlos como pueda. Por eso `file`
dice «seguro de leer» solo de los de texto.

Vas a verlo una vez, a propósito, en un lugar donde no pasa nada. **Predice:** ¿qué vas a ver?

```bash
cat ../pagos/pagos.log.1.gz
```

```text
^_M-^K^H^@^@^@^@^@^B^CM-^LM-^]]M-.^^7vEM-_3M-^J;^A^WM-HM-sM-CshM…
```

Arriba van solo los primeros bytes, y no como los ve tu pantalla: en la prueba pasaron por `cat -v`, que cambia
cada byte que no es letra por signos que sí se pueden escribir. Sin eso, en tu pantalla es basura: símbolos sueltos,
cuadritos, quizá un pitido. A veces, además, la terminal queda **descompuesta**: lo que
escribes sale en símbolos raros, o no se ve, o Enter deja de bajar de renglón. No se rompió el archivo ni la máquina:
entre esos bytes iban algunos que la terminal entiende como órdenes.

**Si te pasa, escribe `reset` y presiona Enter**, aunque no veas lo que escribes. `reset` regresa la terminal a su
forma normal: según su manual, sirve justo después de que un programa la deja en un estado anormal. El mismo manual
avisa dos cosas que pueden pasarte: que no se vea lo que tecleas, y que Enter no funcione; en ese caso, en lugar de
Enter, usa **Ctrl+J**. Si tu terminal quedó bien después del `cat`, corre `reset` de todos modos, para conocerlo.

```bash
reset
```

No escribe nada. En la prueba, lo único que mandó fueron las órdenes que regresan la terminal a su forma
normal, y la primera borra la pantalla: te queda el indicador, hasta arriba.

Dos hábitos salen de aquí: antes de abrir un archivo que no conoces, `file`; y si no dice *text*, no le haces `cat`,
averiguas con qué se lee. La de ayer se lee descomprimiéndola: lo ves en «Si quieres más».

### Tu turno: qué es la bitácora de ayer

Con `file`, pregunta qué es `pagos/pagos.log.1.gz` y anota **la primera palabra** de lo que contesta, después de los
dos puntos, con `anota 03 tipo-respaldo`.

**Lo demuestra** (objetivo 3.5): la marca `tipo-respaldo` de `verifica 03`, y tus respuestas a las preguntas 9 y 10 de
«Para cerrar».

---

## Paso 7 · Comprueba con `verifica 03`

Corre `verifica 03` cuando quieras, desde cualquier carpeta. Primero revisa que la sala siga intacta, archivo por
archivo; si algo cambió, se detiene y te pide armarla de nuevo. Después revisa tus ocho respuestas **leyendo los
archivos**: la respuesta buena no está escrita en `verifica`, sale del archivo mismo.

Recién armada la sala, la primera marca sale en verde y las ocho respuestas en rojo, porque están vacías. Así se lee
un rojo cuando la respuesta está mal y no vacía; por ejemplo, si a `ultima-hora` le anotaras la hora del **primer**
renglón:

```text
✅ La sala está intacta (medí: los 8 archivos de la lista existen, ninguno es un enlace, cada texto tiene sus renglones y cada bitácora su huella)
❌ espera · Tu renglón «espera:» está vacío (medí: lo que hay después de los dos puntos). Anótalo con  anota 03 espera  y tu respuesta
❌ folio-137 · Tu renglón «folio-137:» está vacío (medí: lo que hay después de los dos puntos). Anótalo con  anota 03 folio-137  y tu respuesta
❌ ultima-hora · 00:00:04 es la hora del PRIMER renglón (medí: la hora del renglón 1). head muestra el principio; el final lo muestra otro comando
❌ renglones · Tu renglón «renglones:» está vacío (medí: lo que hay después de los dos puntos). Anótalo con  anota 03 renglones  y tu respuesta
❌ sucursales · Tu renglón «sucursales:» está vacío (medí: lo que hay después de los dos puntos). Anótalo con  anota 03 sucursales  y tu respuesta
❌ primer-error · Tu renglón «primer-error:» está vacío (medí: lo que hay después de los dos puntos). Anótalo con  anota 03 primer-error  y tu respuesta
❌ arranque · Tu renglón «arranque:» está vacío (medí: lo que hay después de los dos puntos). Anótalo con  anota 03 arranque  y tu respuesta
❌ tipo-respaldo · Tu renglón «tipo-respaldo:» está vacío (medí: lo que hay después de los dos puntos). Anótalo con  anota 03 tipo-respaldo  y tu respuesta

❌ Lección 03: falta lo marcado con ❌. Arréglalo y corre  verifica 03  otra vez.
```

Dice qué anotaste, qué midió y en qué te confundiste, pero no cuál es la respuesta buena.

Corrige y repite hasta ver esto al final:

```text
✅ Lección 03: todo en verde.
```

---

## Tus ejercicios

El ejercicio 3.3 se hace **solo con tu terminal**: sin buscador y sin que nadie te dicte la tecla.

| # | Haces | Está hecho cuando |
|---|---|---|
| 3.1 | Lees la configuración completa con números de renglón y encuentras `espera_segundos` | `verifica 03` marca en verde `espera` |
| 3.2 | Sacas el renglón 137, el último renglón y el total de la bitácora, y cuentas las sucursales del CSV, con tus predicciones escritas antes | `verifica 03` marca en verde las cuatro, y comparaste tu predicción D |
| 3.3 | Hojeas la bitácora en `less` y encuentras el primer `ERROR` | `verifica 03` marca en verde `primer-error`, y comparaste tu predicción E |
| 3.4 | Miras crecer la bitácora del arranque con `tail -f` y sales con Ctrl+C | `verifica 03` marca en verde `arranque` |
| 3.5 | Preguntas con `file` qué es la bitácora de ayer, ves qué hace `cat` con ella y usas `reset` | `verifica 03` sale todo en verde, y contestaste las preguntas 9 y 10 de «Para cerrar» |

**Tiempo (estimado):** de 60 a 90 minutos. Es una estimación: se ajusta con el primer grupo que tome el curso.

---

## Si quieres más

*Opcional. Nada de este recuadro se evalúa ni hace falta para los ejercicios.*

- **Leer la bitácora de ayer sin descomprimirla en el disco.** Junto con `gzip` vienen `zcat`, que hace lo que `cat`
  pero descomprimiendo, y `zless`, lo mismo para `less`; `man zcat` te dice cómo se usan. **Predice** primero qué sale con
  `zcat pagos/pagos.log.1.gz`, y por qué no conviene sin más; luego prueba `zless pagos/pagos.log.1.gz` y sal con
  `q`. Lo que pasó en la prueba:

  ```text
  $ zcat pagos/pagos.log.1.gz | head -n 3
  2025-06-11 00:00:04 INFO  pagos: arranco el día con la versión 4.11.2 y la configuración de pagos.conf
  2025-06-11 00:02:04 INFO  conciliacion: el lote del 10 de junio llegó y cuadra
  2025-06-11 00:04:04 INFO  cobro F-127999 monto=791.99 resultado=aprobado
  $ zcat pagos/pagos.log.1.gz | wc -l
  720
  ```

- **Bytes no es lo mismo que letras.** `wc -m` cuenta caracteres, y `wc -c`, bytes. En `cien.txt` no dan lo mismo:
  compara los dos. La diferencia es de cien, uno por renglón. ¿Qué letra de cada renglón ocupa dos bytes?
- **`less` también puede seguir un archivo.** Su manual tiene la tecla `F`: se va al final y sigue leyendo conforme
  el archivo crece, como `tail -f`. Ahí sí, Ctrl+C deja de seguir y te deja en `less`; sales con `q`. Pruébalo con
  `arranque.log` después de armar la sala de nuevo y correr `simula 03`.

---

## ¿No te corre?

| Ves | Causa probable | Qué haces |
|---|---|---|
| `bash: simula: command not found`, o `Todavía no hay prepara para la lección «03».` | Tu imagen es de antes de esta lección | Sal con `exit` y corre `./practicar reconstruir` |
| `Ya existe /home/alumno/practica/03.` y una pregunta | Ya habías armado la sala, y armarla de nuevo la borra | Para empezar de cero, escribe `si`. Si no, presiona Enter: no se borra nada |
| La pantalla de `less` no se va y no vuelve el indicador | Sigues adentro de `less` | `q`. Ctrl+C no lo cierra |
| `tail -f` no termina | Así es: espera más renglones para siempre | Ctrl+C |
| `tail -f` no muestra nada nuevo | `simula 03` no está corriendo, o ya terminó | Corre `simula 03` y enseguida `tail -f` |
| `❌ … ya no está como lo dejó prepara` al correr `simula 03` | Ya lo habías corrido, y el archivo ya creció | Si quieres verlo otra vez, `prepara 03` |
| Lo que escribes sale en símbolos raros, o no se ve | Le hiciste `cat` a un archivo que no es texto | Escribe `reset` y Enter, aunque no lo veas. Si Enter no responde, usa Ctrl+J |
| `❌ La sala no está intacta` | Un archivo de la sala cambió | `prepara 03`. Tu hoja de respuestas no se toca |

---

## Para cerrar

**Contesta sin volver a leer.**

**Cinco situaciones: ¿qué pedazo necesitas, y con qué comando lo sacas?** Pasas con 4 de 5.

1. Te preguntan qué está pasando ahorita en un servicio, según su bitácora.
2. Te dan un archivo de configuración de quince renglones y te piden revisarlo entero.
3. Te preguntan cuántos registros trae un CSV de cuarenta mil renglones, con encabezado.
4. Te dicen que el problema empezó «en algún momento de la madrugada» y la bitácora tiene diez mil renglones.
5. Vas a reiniciar un servicio y quieres ver, mientras arranca, si llega a decir que está listo.

**Y cinco preguntas:**

6. ¿Cómo sacas el renglón 250 de un archivo con lo que viste en esta lección?
7. Buscaste `/error` en `less` y no encontró nada, pero sabes que hay errores. ¿Qué puede estar pasando?
8. `wc -l` te dice 99, y tú cuentas cien renglones en pantalla. ¿Qué puede estar pasando?
9. Te encuentras un archivo llamado `respaldo.txt`. ¿Qué corres antes de hacerle `cat`, y por qué no basta con su
   nombre?
10. Le hiciste `cat` a un binario y ahora lo que escribes no se ve. ¿Qué haces?

**Lo que sigue:** ya lees un archivo por pedazos, sin abrir un editor. En la lección 04 buscas **archivos** en un
árbol con `find`, cuando ni siquiera sabes en qué carpeta están.

---

> **Qué hace Hábil**
>
> Operamos y modernizamos la infraestructura donde corren sistemas que no pueden fallar: servidores Linux,
> contenedores, Kubernetes y entrega continua, para banca, seguros y empresas reguladas. Si sus servidores dependen
> de una persona que «sabe dónde está todo», eso se puede ordenar sin detener la operación.
>
> [**CI/CD, DevSecOps y nube sin sobresaltos**](https://www.habil.mx/es/arquitectura-cloud-native/)
>
> Primera conversación, sin costo.

---

## Compara tus respuestas

**Predicción D (las sucursales).** No es el mismo número. El primer renglón del CSV es el **encabezado**: dice cómo se
llama cada columna (`sucursal,ciudad,terminales,hora_del_reporte`) y no es una sucursal. `wc -l` cuenta todos los
renglones, encabezado incluido, así que las sucursales son uno menos. Por eso convenía mirar con `head` antes de
contar: el número de `wc` solo significa algo cuando sabes qué hay en cada renglón.

**Predicción E (el primer `ERROR`).** Cualquiera de las dos vale si la escribiste antes: lo que importa es que lo
comprobaste. Si hojeaste antes de buscar, viste que la bitácora avisa con `WARN` varias veces antes del primer
`ERROR`: los problemas casi nunca empiezan con un error, empiezan con avisos que nadie leyó. Y si en el camino te
topaste con la palabra `error` en minúsculas, es otro renglón, con otra historia: por eso el ejercicio pedía
`ERROR` tal cual.

**Ejercicio 3.3.** El control es `verifica 03`; aquí no va el número. Para saber si vas bien: lo abriste con `less -N`,
fuiste al principio con `g`, buscaste con `/` y el texto exacto, y el número lo leíste a la izquierda del renglón,
sin contar.

**Para cerrar.**

1. El final: `tail` sobre la bitácora, o `tail -n` con los renglones que quieras. Lo último que se escribió es lo que
   está pasando.
2. Todo: `cat`, mejor con `-n`. Quince renglones caben en la pantalla.
3. Cuántos son: `wc -l`, menos uno por el encabezado. Antes, `head` para confirmar que el primer renglón es el
   encabezado.
4. Hojear y buscar: `less`, mejor con `-N`, buscando con `/` lo que distinga al renglón del problema, como `ERROR`.
5. Lo nuevo, conforme llega: `tail -f`, y Ctrl+C para salir cuando ya viste lo que esperabas.
6. `head -n 250` y el último renglón que escribe es el 250. O en `less`, `250g`.
7. La búsqueda de `less` distingue mayúsculas de minúsculas: los errores pueden estar escritos `ERROR` o `Error`.
   Prueba con otra forma, o revisa la ayuda de `less` con `h`.
8. Al último renglón le falta su salto de renglón. `wc -l` cuenta saltos, no renglones a la vista.
9. `file respaldo.txt`. El nombre lo pone una persona y puede no decir la verdad; `file` mira el contenido. Si no
   dice *text*, no le haces `cat`.
10. Escribes `reset` y Enter, aunque no lo veas. Si Enter no responde, Ctrl+J en su lugar.

---

## Fuentes de la lección

Abiertas el 24-nov-2026. Todas están en inglés.

- man7.org, *The Linux man-pages project*. *cat(1)*, de GNU coreutils (encadena archivos; `-n` numera los
  renglones). https://man7.org/linux/man-pages/man1/cat.1.html
- man7.org. *head(1)*, de GNU coreutils (los primeros diez renglones por omisión; `-n`).
  https://man7.org/linux/man-pages/man1/head.1.html
- man7.org. *tail(1)*, de GNU coreutils (los últimos diez renglones por omisión; `-n`; `-f` escribe lo que se agrega
  conforme el archivo crece). https://man7.org/linux/man-pages/man1/tail.1.html
- man7.org. *wc(1)*, de GNU coreutils (renglones, palabras y bytes, en ese orden; `-l` cuenta saltos de renglón; qué
  es una palabra; `-m`). https://man7.org/linux/man-pages/man1/wc.1.html
- Ubuntu Manpages. *less(1)*, Ubuntu 26.04, less 668 (no lee el archivo completo antes de empezar; barra, `b`, `g`,
  `G`, un número y `g`, `/`, `n`, `N`, `q`, `-N`, `F`; la búsqueda distingue mayúsculas por omisión).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/less.1.html
- Mark Nudelman, autor de `less`. *Less FAQ* (`less` es un paginador y no un editor; Ctrl+C no lo cierra).
  https://www.greenwoodsoftware.com/less/faq.html
- Ubuntu Manpages. *file(1)*, Ubuntu 26.04, file 5.46 (las tres clases de pruebas y su orden; qué quiere decir
  *text*: probablemente seguro de leer en la terminal). https://manpages.ubuntu.com/manpages/resolute/en/man1/file.1.html
- man7.org. *tset(1)*, de ncurses (`reset` regresa la terminal a valores normales después de que un programa la deja
  en un estado anormal; puede no verse lo que escribes y puede hacer falta Ctrl+J en lugar de Enter).
  https://man7.org/linux/man-pages/man1/tset.1.html
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, sección «rust-coreutils» (las utilidades básicas vienen de
  `rust-coreutils`; `cp`, `mv` y `rm` siguen siendo de GNU).
  https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- Adam Wiggins. *The Twelve-Factor App*, «XI. Logs» (en crudo, una bitácora suele ser texto, un evento por renglón y
  en orden de tiempo; no tiene principio ni fin fijos; se puede mirar en vivo con `tail`).
  https://12factor.net/logs
