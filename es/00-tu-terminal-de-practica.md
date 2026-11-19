# Lección 00 — Tu terminal de práctica

**Por Dorian Chávez, fundador de Hábil y arquitecto de integración.**

**Antes de leer, seis palabras.** Una línea cada una; las demás llegan en la lección donde se usan.

- **Terminal:** la ventana donde escribes órdenes con el teclado y la computadora te contesta con texto.
- **Comando:** una orden escrita. Por ejemplo, `whoami` («¿quién soy?»). Se escribe y se presiona Enter.
- **Indicador:** el texto que la terminal pone al principio de la línea cuando espera tu siguiente comando. En
  inglés, *prompt*.
- **Docker:** el programa que crea y corre contenedores.
- **Contenedor:** una computadora Linux chiquita que vive dentro de la tuya, separada de tus archivos.
- **Imagen:** el molde del que sale un contenedor. Se construye una vez y se usa muchas.

**Qué te llevas:** tu terminal de práctica funcionando, que es un Ubuntu 26.04 dentro de un contenedor; sabes entrar,
salir y volver sin perder tu trabajo; lees el indicador; le pides ayuda a la propia máquina; y sales de cualquier
programa que te atrape.

**Idea nueva de esta lección (la única que se evalúa):** en esta terminal no hay escritorio ni ventanas, solo
texto: te dice quién eres y dónde estás en cada línea, y se le habla con comandos.

**En qué orden va:** primero levantas la terminal (pasos 1 y 2), luego lees lo que te dice (paso 3), luego aprendes
a pedirle ayuda (paso 4) y al final a salir de donde te atores (paso 5).

---

## La escena

*Escenario inventado; las pantallas que aparecen en la lección sí son reales.*

Martes, 23:40. Lucía lleva tres semanas en su primer trabajo y hoy le toca guardia. Suena el teléfono: el servicio de
pagos dejó de contestar. Le pasan una dirección y una frase: «entra al servidor y dime qué ves».

Entra. No hay escritorio, ni iconos, ni ratón. Hay una pantalla negra y una sola línea:

```text
operaciones@pagos-03:~$
```

El cursor parpadea. Del otro lado del teléfono esperan.

Lucía no lo sabe todavía, pero esa línea ya le contestó dos de las tres preguntas que le van a hacer esta noche:
quién es ahí adentro y en qué carpeta está parada. La tercera, qué hacer, se le pregunta a la propia máquina. Y si
algo se queda trabado, hay tres teclas para salir.

Esta lección te da esas cuatro cosas en una máquina hecha para equivocarse: la tuya, de práctica.

---

## Antes de empezar

**Qué necesitas:**

| | Qué | Notas |
|---|---|---|
| Sistema | Linux, Mac o Windows | Abajo dice en cuáles ya se probó y en cuáles falta |
| Docker | Instalado y respondiendo | Abajo dice cómo, por sistema |
| Disco | Menos de 300 MB para la imagen del curso | Va un tope y no una cifra, porque cada herramienta de Docker mide distinto (abajo) |
| Red | Para descargar Ubuntu y unos paquetes la primera vez | Solo la usa tu computadora al construir; adentro de la terminal de práctica no hay red |
| Saber | Abrir la terminal de tu computadora y pegar una línea | No se supone nada de Linux ni de Docker |

**Dónde se probó, y dónde falta:**

| Sistema | Estado | Qué se corrió |
|---|---|---|
| Linux Mint 22.3 (x86_64), Docker Engine 29.10.0 | Probado el 19-nov-2026 | `./practicar` construyó la imagen en unos 90 segundos, `verifica 00` dio sus cinco marcas en verde y `./practicar desde-cero` borró la carpeta. Ojo: Docker no da soporte oficial a Linux Mint; su guía dice que «puede funcionar» |
| Mac con Apple silicon, Docker 29.2.1 corriendo con Colima | Probado el 18 y el 19-nov-2026 | Todo lo de esta lección: entrar, salir, volver, `verifica 00`, `desde-cero` y las pantallas que ves aquí |
| Ubuntu | Sin probar | Docker sí le da soporte oficial. `[pendiente: medir en Ubuntu]` |
| Mac con Docker Desktop | Sin probar | Instrucciones de la guía oficial. `[pendiente: medir en Mac con Docker Desktop]` |
| Windows con Docker Desktop y WSL 2 | Sin probar | Instrucciones de las guías oficiales. `[pendiente: medir en Windows con Docker Desktop y WSL2]` |

**Las cifras de esta lección, y cómo se midieron:**

| Cifra | Cómo se midió |
|---|---|
| Construir la imagen: 42 segundos | Mac con Apple silicon, 18-nov-2026, con `time ./practicar reconstruir`: sin caché y con la imagen de Ubuntu ya descargada |
| Construir la imagen: 1 minuto 38 segundos | La misma Mac, 18-nov-2026, con `docker build --no-cache`, sobre una versión anterior del Dockerfile |
| Construir la imagen: unos 90 segundos | Linux Mint 22.3 (x86_64), 19-nov-2026, con `./practicar` |
| Tamaño: 221 MB | Linux Mint, 18-nov-2026, con `docker image inspect` |
| Tamaño: 277 MB | Mac, 18-nov-2026, con `docker image ls` |

**Instala Docker, según tu sistema.** Es lo único que se instala en tu computadora.

- **Linux Mint o Ubuntu:** Docker Engine, desde el repositorio oficial de Docker. La
  [lección 1 del curso de Docker](https://www.habil.mx/es/cursos/docker/01-instalacion/) de esta casa lo hace paso a
  paso en Linux Mint y explica cada error. Docker no da soporte oficial a Mint, y esa lección dice qué hacer al
  respecto. En la computadora de la prueba, el usuario estaba en el grupo `docker` y
  usó Docker sin `sudo`; esa misma lección explica qué implica entrar a ese grupo.
- **Mac:** Docker Desktop, con el instalador de tu procesador (Apple silicon o Intel), desde la
  [guía oficial](https://docs.docker.com/desktop/setup/install/mac-install/). Ábrelo y espera a que diga que está
  corriendo. En la Mac de la prueba, Docker corría con otra herramienta, Colima, y no con Docker Desktop: por eso
  esta vía está en la tabla como «sin probar».
- **Windows:** la vía documentada es Docker Desktop con WSL 2. WSL es la pieza de Windows que corre un Linux adentro.
  Según Microsoft, en Windows 10 (versión 2004 o posterior) y Windows 11 se instala con `wsl --install` en un
  PowerShell abierto como administrador, y trae Ubuntu. Según Docker, después se instala Docker Desktop y se enciende
  la integración en *Settings › Resources › WSL Integration*. Todo lo que sigue lo corres en la terminal de ese
  Ubuntu, no en PowerShell. También está en la tabla como «sin probar».

**Comprueba que Docker responde.** Abre la terminal de tu computadora y corre:

```bash
docker version
```

Vas bien si ves dos bloques, `Client` y `Server`, cada uno con su versión. En las pruebas fueron Docker 29.10.0 en
Linux y 29.2.1 en la Mac. Si aparece `Client` y, en lugar de `Server`, un error, el programa `docker` no pudo hablar
con su motor. Corre `docker info` y sigue el error exacto que te dé; las causas más comunes están en «¿No te corre?».

**Trae el material del curso.** Está en un **repositorio**: una carpeta con todos los archivos del curso, publicada
en GitHub. El programa `git` la descarga con la primera línea; la segunda te mete a esa carpeta:

```bash
git clone https://github.com/HabilMX/curso-terminal.git
cd curso-terminal
```

**Sin `git`:** abre esa dirección en tu navegador, elige *Code › Download ZIP*, descomprime el archivo y abre tu
terminal en la carpeta que se creó. No copies el `cd curso-terminal` de arriba: esa carpeta puede llamarse distinto.

---

## Paso 1 · Levanta tu terminal y entra

**Predice primero.** Escribe en un papel: cuando entres, ¿qué crees que vas a ver? ¿Una ventana nueva? ¿Un
escritorio? ¿Otra cosa?

Desde la carpeta del curso:

```bash
./practicar
```

`practicar` es un guion del curso. La primera vez construye la imagen: descarga Ubuntu 26.04 y le pone lo que el
curso usa. En las pruebas tardó entre 42 segundos y 1 minuto 38 segundos. Las siguientes veces ya no descarga ni
reconstruye: en la Mac de la prueba entró en menos de dos segundos. Tu tiempo depende de Docker y de tu equipo.

Verás pasar mucho texto y, al final, esto:

```text
✅ Imagen lista.
Entrando a tu terminal de práctica. Para salir, escribe  exit  (o Ctrl+D).
alumno@practica:~$
```

No se abrió ninguna ventana. **Sigues en la misma ventana, pero ya estás en otra máquina.** La última línea es el
indicador de tu terminal de práctica, y la vas a leer con calma en el paso 3. Compara con lo que predijiste.

**Qué tan separada está de tu computadora.** Para estos ejercicios no se conecta ninguna carpeta de tu computadora,
la terminal corre como `alumno`, sin privilegios de administrador, y adentro no hay red. Tu trabajo se guarda solo
en la carpeta `~/practica`. Lo que el contenedor sí usa de tu computadora es procesador, memoria y disco.

Corre tu primer comando adentro:

```bash
verifica 00
```

`verifica` revisa cómo está tu terminal de práctica y te dice qué falta. En cada marca dice qué midió. Ahora debe
salir así:

```text
✅ Es Ubuntu 26.04 (medí: ID y VERSION_ID en /etc/os-release)
✅ Eres el usuario alumno (medí: id -un)
✅ /home/alumno/practica existe y es una carpeta (medí: que exista como carpeta)
✅ Puedes escribir en /home/alumno/practica (medí: creé un archivo vacío ahí y lo borré)
❌ Falta /home/alumno/practica/00/aqui-estuve.txt (medí: que el archivo exista). Créalo con el paso 2 de la lección, corre  verifica 00, sal con exit y vuelve a entrar

❌ Lección 00: falta lo marcado con ❌. Arréglalo y corre  verifica 00  otra vez.
```

Cuatro verdes y un rojo. El rojo es normal: es lo que haces en el paso 2.

**Lo demuestra** (objetivo 0.1, primera mitad): las cuatro marcas verdes.

---

## Paso 2 · Deja una marca, sal y vuelve

Un contenedor se tira al salir. Entonces, ¿dónde queda tu trabajo? En una sola carpeta: `practica`. Docker la guarda
aparte, en lo que llama un **volumen**, y te la vuelve a conectar cada vez que entras. Todo lo demás vuelve a nacer
limpio.

**Predice primero.** Vas a crear un archivo dentro de `practica`, salir y volver a entrar. ¿Seguirá ahí? ¿Y si lo
hubieras creado fuera de `practica`?

Hoy estas dos líneas son una receta dada. Escríbelas tal cual:

```bash
mkdir ~/practica/00
date > ~/practica/00/aqui-estuve.txt
```

- `mkdir` crea una carpeta: aquí, la carpeta `00` dentro de `practica`.
- `date` contesta la fecha y la hora. El signo `>` manda esa respuesta a un archivo en lugar de a la pantalla.
- `~` es la abreviatura de tu carpeta personal. La ves con calma en el paso 3.

En las lecciones 02 y 06 entiendes cada pieza a detalle. Ninguna de las dos líneas contesta nada: en la terminal,
**el silencio suele significar que salió bien**.

Corre `verifica 00` otra vez, **antes de salir**. El último renglón cambió:

```text
❌ Tu marca aqui-estuve.txt ya existe, y es la primera vez que la veo: fue en ESTA sesión (medí: no había apunte de una sesión anterior; lo acabo de dejar en 00/.sesion-verifica). Te falta salir con exit, volver a entrar con ./practicar y correr  verifica 00  otra vez
```

`verifica` ya vio tu marca y apuntó en qué sesión la vio. Una **sesión** es cada vez que entras. El apunte queda en
tu carpeta de práctica, en un archivo que se llama `.sesion-verifica`. Ahora hazle caso. Sal:

```bash
exit
```

```text
logout
Saliste de la terminal de práctica.
Lo que dejaste en ~/practica sigue guardado. Para volver a entrar:  ./practicar
```

Fíjate en el indicador: cambió. Ya no dice `alumno@practica`. Estás de vuelta en tu computadora. Vuelve a entrar con
`./practicar` y corre `verifica 00`:

```text
✅ Saliste y volviste sin perder tu carpeta (medí: tu marca ya existía en una sesión anterior a esta, según el apunte de 00/.sesion-verifica)

✅ Lección 00: todo en verde.
```

`verifica` no usa el reloj. Compara dos cosas: la sesión en que vio tu marca por primera vez y la sesión de ahora.
Son distintas y la marca sigue ahí: saliste, volviste y tu carpeta sobrevivió.

**Empezar de cero.** Si un día quieres tu carpeta de práctica vacía otra vez, desde tu computadora (no desde
adentro):

```bash
./practicar desde-cero
```

Pide que escribas `si` antes de borrar, porque no se puede deshacer. Borra solo tu carpeta de práctica. Si tienes la
terminal de práctica abierta en otra ventana, no borra nada y te pide salir primero. Es un comando que borra:
escríbelo tú, a mano. Si lo corres ahora, tendrás que repetir este paso.

**Lo demuestra** (objetivo 0.1 completo): `verifica 00` todo en verde.

---

## Paso 3 · Lee el indicador

Esta es la línea que tienes enfrente:

```text
alumno@practica:~$
```

Tiene cuatro piezas:

| Pieza | Qué dice |
|---|---|
| `alumno` | Quién eres: tu usuario en esta máquina |
| `practica` | En qué máquina estás. Va después de la `@`, como en un correo: «alumno, en practica» |
| `~` | En qué carpeta estás. `~` es la abreviatura de «tu carpeta personal» |
| `$` | Fin del indicador: de aquí en adelante escribes tú. `$` quiere decir usuario normal |

**Predice primero.** Hay un comando para preguntar cada cosa: `whoami` (¿quién soy?), `hostname` (¿cómo se llama esta
máquina?) y `pwd` (¿en qué carpeta estoy?). Antes de correrlos, escribe en tu papel qué va a contestar cada uno.
Para dos basta leer el indicador. El tercero tiene truco.

Ahora córrelos, uno por uno:

```bash
whoami
hostname
pwd
```

Compara con tu papel. Las respuestas están al final, en «Compara tus respuestas»; no las mires antes de predecir.

**Por qué importa.** En un trabajo real vas a tener abiertas tres terminales que se ven iguales: tu computadora, un
servidor de pruebas y uno de producción. El indicador es lo único que te dice en cuál estás a punto de escribir.
**Léelo antes de cada comando que cambie algo.**

**Lo demuestra** (objetivo 0.2): tu predicción escrita, comparada con la salida.

---

## Paso 4 · Pídele ayuda a la máquina

Nadie se sabe los comandos de memoria. Lo que sí se sabe es dónde preguntar, y la respuesta viene dentro de la misma
máquina. Hay dos formas.

**La corta: `--help`.** Casi todos los comandos aceptan `--help` y contestan con un resumen:

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

En esta terminal la ayuda aparece en inglés. Tiene tres partes: qué hace, cómo se usa (*Usage*) y qué opciones
acepta (*Options*). Una **opción** es una palabra con guiones que cambia lo que hace el comando.

**La larga: `man`.** Es el manual (`man`, de *manual*). Cada comando tiene su página:

```bash
man whoami
```

Ahora la pantalla es distinta. Ya no ves el indicador, y abajo dice:

```text
 Manual page whoami(1) line 1 (press h for help or q to quit)
```

Estás **dentro** del manual. Lo que teclees ya no son comandos: son teclas del manual. Estas cinco alcanzan:

| Tecla | Qué hace |
|---|---|
| Flecha abajo, o barra espaciadora | Baja: la flecha, un renglón; la barra, una pantalla |
| Flecha arriba | Sube un renglón |
| `/` y una palabra, luego Enter | Busca esa palabra hacia abajo |
| `n` | Va a la siguiente aparición |
| `q` | Sale del manual y te regresa al indicador |

Sal con `q`. Volvió el indicador.

**Con Claude Code, en este orden.** En este curso un asistente de IA entra siempre igual: **predices → corres → lees
lo que salió → explicas con tus palabras.** Aquí puedes pedirle a Claude Code que te explique qué dice una página de
`man` que no entendiste. Pero la página la abres tú y la respuesta la compruebas tú, en tu terminal. El ejercicio 0.3
se hace sin IA: el punto es que sepas preguntarle a la máquina cuando no hay nadie más.

**Lo demuestra** (objetivo 0.3): ejercicio 0.3, abajo.

---

## Paso 5 · Sal de lo que te atrapó

Hace un momento estuviste «atrapado» en `man` y saliste con `q`. Le pasa a todos: un programa toma la pantalla y el
indicador no vuelve. Hay tres teclas, y cada una sirve para algo distinto.

**Un error a propósito, antes.** Escribe el comando con mayúscula:

```bash
Whoami
```

```text
bash: Whoami: command not found
```

La terminal distingue mayúsculas de minúsculas: `Whoami` no existe. Y fíjate en lo que pasó después: te devolvió el
indicador. **Un error que te regresa el indicador no te atrapó.** Lees el mensaje y sigues. Atrapado es cuando el
indicador no vuelve.

Ahora sí, tres maneras de quedar atrapado. Haz las tres.

**1. Un programa que no termina.** `sleep 1000` espera mil segundos sin hacer nada:

```bash
sleep 1000
```

El indicador no vuelve. Presiona **Ctrl+C** (mantén Ctrl y toca la C). Aparece `^C` y vuelve el indicador. Ctrl+C le
dice al programa: «detente ya».

**2. Un programa que espera que escribas.** `cat`, sin nada más, repite lo que le escribas:

```bash
cat
```

Escribe `hola` y Enter: contesta `hola`. Va a seguir esperando para siempre. Presiona **Ctrl+D** en una línea vacía.
Vuelve el indicador. Ctrl+D le dice al programa: «ya no hay más que escribir».

**3. Un programa de pantalla completa.** `htop` muestra los programas que están corriendo:

```bash
htop
```

Toma toda la pantalla. Sal con **`q`**, igual que en `man`.

**Cuál sirve para qué:**

| Tecla | Le dice al programa | Úsala cuando |
|---|---|---|
| `q` | «Ciérrate» | Estás en un programa que se hojea o toma la pantalla: `man`, `less`, `htop` |
| Ctrl+C | «Detente ya» | Un comando está corriendo y no termina |
| Ctrl+D | «Ya no hay más que escribir» | Un programa espera texto tuyo |

Tres cosas medidas que conviene saber:

- **Ctrl+C no saca de `man`.** La página se queda. De ahí se sale con `q`.
- **`q` en el indicador no hace nada útil.** Contesta `bash: q: command not found`. Sirve para saber que ya estabas
  afuera.
- **Ctrl+D en el indicador vacío te saca de la terminal de práctica.** El indicador también es un programa que espera
  que escribas: se llama bash. Ctrl+D ahí es lo mismo que `exit`. No se pierde nada: vuelves con `./practicar`.

**Lo demuestra** (objetivo 0.4): las cinco situaciones del repaso, en «Para cerrar».

---

## Tres notas sobre tu terminal de práctica

**Tu Mac usa zsh; adentro usas bash.** El programa que lee tus comandos se llama *shell*. Desde macOS 10.15, el de
las cuentas nuevas de Mac es zsh, según Apple. Adentro del contenedor es bash, que es común en muchos
servidores. Por eso todo el curso se hace adentro: todo el grupo practica en el mismo entorno.
La terminal de tu computadora solo sirve para correr `./practicar`.

**`htop` y `ps` solo ven el contenedor.** Cuando abriste `htop` viste solo dos programas: bash y el propio htop. No
ves los de tu computadora, porque Docker le da a cada contenedor su propia lista de procesos. Para aprender a leerlos
y detenerlos, que es la lección 10, alcanza.

**Tus comandos básicos están escritos en Rust.** Desde Ubuntu 25.10, comandos como `ls`, `cat` y `whoami` ya no
vienen del proyecto GNU, que es el clásico, sino de `rust-coreutils`. Lo dice la documentación de Ubuntu, y en tu
terminal se ve en `man whoami`: bajo *VERSION* dice `uutils coreutils`. Tres siguen siendo de GNU: `cp`, `mv` y `rm`.

- **Qué significa para ti:** si comparas con un tutorial o con un servidor más viejo, el texto de `man` y de
  `--help` puede ser distinto del que ves aquí. No es error tuyo ni del tutorial.
- **Qué no cambia en este curso:** los comandos se llaman igual y las opciones que se enseñan funcionan igual. Las
  salidas que ves en las lecciones son de la imagen probada el 18 y el 19-nov-2026. La imagen se construye siempre
  sobre el Ubuntu 26.04 vigente, así que tras una actualización una salida puede variar un poco.

---

## Tus ejercicios

El ejercicio 0.3 se hace **sin IA** y sin buscador: solo con `man`.

| # | Haces | Está hecho cuando |
|---|---|---|
| 0.1 | Los pasos 1 y 2: entras, dejas tu marca, sales y vuelves | `verifica 00` sale todo en verde |
| 0.2 | El paso 3: predices por escrito qué contestan `whoami`, `hostname` y `pwd`, y luego los corres | Tienes tu predicción y la salida, y sabes explicar la que no coincidió |
| 0.3 | Contestas las tres preguntas de abajo, solo con `man` | Tus tres respuestas coinciden con «Compara tus respuestas» |
| 0.4 | El paso 5: te atrapas a propósito en `sleep 1000`, `cat` y `htop`, y sales de los tres | Saliste de los tres sin cerrar la ventana |

**Las tres preguntas del 0.3.** Para cada una: abre la página, busca con `/`, anota la respuesta y sal con `q`.

1. En `man whoami`: ¿cuántas opciones acepta `whoami` y cuáles son?
2. En `man ls`, busca `--all`: ¿qué letra corta es lo mismo que `--all`, y qué hace?
3. En `man man`, busca `Executable`: el manual se divide en secciones numeradas. ¿Qué número tiene la de los
   programas y comandos? ¿Y la de los comandos de administración del sistema?

**Tiempo (estimado):** de 60 a 90 minutos, sin contar la instalación de Docker. Es una estimación: se ajusta con el
primer grupo que tome el curso.

---

## ¿No te corre?

| Ves | Causa probable | Qué haces |
|---|---|---|
| `❌ No encuentro el comando «docker» en esta computadora.` | Docker no está instalado | «Antes de empezar», según tu sistema |
| `❌ Docker está instalado, pero no pude hablar con él.` | Lo más común: Docker está apagado, o tu usuario no tiene permiso. Puede ser otra cosa | Corre `docker info` y sigue el error exacto que te dé |
| `no such file or directory: ./practicar` | No estás en la carpeta del curso | `cd curso-terminal` y repite |
| `❌ No se pudo construir la imagen.` | La red, el espacio en disco, los permisos de Docker o el material del curso | Lee el **primer** error que salió arriba del mensaje, arréglalo y corre `./practicar` otra vez |
| `❌ ./practicar abre una terminal interactiva…` | Lo corriste con una tubería o una redirección | Córrelo solo: `./practicar` |
| `command not found: verifica` | Corriste `verifica` en tu computadora, no adentro | Entra con `./practicar`; el indicador debe decir `alumno@practica` |
| `mkdir: /home/alumno/practica/00: File exists` | Ya habías creado la carpeta `00` | No pasa nada: sigue con la línea de `date` |
| El último renglón de `verifica 00` sigue en rojo después de volver | No corriste `verifica 00` antes de salir, así que apenas vio tu marca | Ya quedó apuntada: sal con `exit`, entra y corre `verifica 00` otra vez |
| `❌ No sé en qué sesión estás…` | Entraste al contenedor sin el guion del curso | Sal y entra con `./practicar` |
| El indicador no vuelve | Un programa tomó la terminal | Prueba `q`; si no, Ctrl+C; si no, Ctrl+D (paso 5) |
| Cerraste la ventana sin escribir `exit` | La terminal de práctica pudo quedarse abierta por dentro | Abre otra ventana y entra con `./practicar`: tu carpeta sigue ahí |
| `❌ No borré nada: tu carpeta de práctica está en uso.` | Hay una terminal de práctica abierta, quizá de una ventana que cerraste sin `exit` | Sal de ella con `exit`. Si ya no tienes esa ventana, el mismo mensaje te da la línea `docker stop …` para cerrarla tú |
| El curso se actualizó y `verifica` no conoce una lección | Tu imagen es de antes | `./practicar reconstruir` |

---

## Para cerrar

**Contesta sin volver a leer.**

**Cinco situaciones: ¿qué tecla?** Pasas con 4 de 5.

1. Abriste `man ls` para buscar una opción y ya la encontraste.
2. Corriste un comando que lleva diez minutos y no termina.
3. Escribiste `cat` y Enter por error, y ahora todo lo que escribes se repite.
4. Abriste `htop` para ver qué está corriendo y ya viste suficiente.
5. Terminaste por hoy: estás en `alumno@practica:~$` y quieres volver a tu computadora.

**Y tres preguntas:**

6. Ves `soporte@caja-07:~$`. Sin correr nada: ¿quién eres, en qué máquina y en qué carpeta?
7. Creaste un archivo fuera de `~/practica` y saliste. ¿Va a estar cuando vuelvas? ¿Por qué?
8. No recuerdas qué hace una opción de un comando y no tienes internet. ¿Qué dos cosas pruebas?

**Lo que sigue:** ya tienes dónde practicar y sabes dónde estás parado. En la lección 01 te mueves por las carpetas
sin verlas.

---

> **Qué hace Hábil**
>
> Operamos y modernizamos la infraestructura donde corren sistemas que no pueden fallar: servidores Linux,
> contenedores, Kubernetes y entrega continua, para banca, seguros y empresas reguladas. Si sus servidores dependen
> de una persona que «sabe dónde está todo», eso se puede ordenar sin detener la operación.
>
> **Nube y DevSecOps** `[pendiente: confirmar liga con pages]`
>
> Primera conversación, sin costo.

---

## Compara tus respuestas

**Paso 3 (ejercicio 0.2).** `whoami` contesta `alumno`. `hostname` contesta `practica`. `pwd` contesta
`/home/alumno`. Ese es el truco: el indicador dice `~`, y `~` es la abreviatura de tu carpeta personal, que en esta
máquina es `/home/alumno`. `~` y `/home/alumno` nombran el mismo lugar: el indicador usa la forma corta.

**Ejercicio 0.3.**

1. Dos: `-h` o `--help`, que muestra la ayuda, y `-V` o `--version`, que muestra la versión.
2. `-a`. Hace que `ls` no esconda los archivos ocultos, que son los que tienen un nombre que empieza con punto.
3. La sección 1 es la de programas y comandos (*Executable programs or shell commands*). La 8 es la de comandos de
   administración del sistema (*System administration commands*). Por eso la página dice `whoami(1)`: el número entre
   paréntesis es la sección.

**Para cerrar.**

1. `q`.
2. Ctrl+C.
3. Ctrl+D, en una línea vacía. Ctrl+C también lo detiene.
4. `q`.
5. `exit`, o Ctrl+D en el indicador vacío.
6. Eres `soporte`, en la máquina `caja-07`, en tu carpeta personal.
7. No. Al salir, el contenedor se tira y vuelve a nacer limpio. Solo `~/practica` se guarda aparte, en un volumen.
8. El comando con `--help`, y `man` seguido del comando.

---

## Fuentes de la lección

Abiertas el 18-nov-2026.

- Ubuntu. *Ubuntu 26.04 LTS release notes* (Resolute Raccoon; publicada el 23 de abril de 2026). https://documentation.ubuntu.com/release-notes/26.04/
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, sección «rust-coreutils» (los comandos básicos vienen de `rust-coreutils` desde la 25.10; `cp`, `mv` y `rm` siguen siendo de GNU). https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- Ubuntu Manpages. *man(1)*, de Ubuntu 26.04 (secciones del manual; `-k`). https://manpages.ubuntu.com/manpages/resolute/en/man1/man.1.html
- Docker. *Install Docker Engine on Ubuntu* (versiones de Ubuntu con soporte; Linux Mint sin soporte oficial). https://docs.docker.com/engine/install/ubuntu/
- Docker. *Install Docker Desktop on Mac* (instalador por procesador; versiones de macOS con soporte). https://docs.docker.com/desktop/setup/install/mac-install/
- Docker. *Docker Desktop WSL 2 backend on Windows* (integración con la distribución de WSL). https://docs.docker.com/desktop/features/wsl/
- Microsoft. *How to install Linux on Windows with WSL* (`wsl --install`; versiones de Windows). https://learn.microsoft.com/en-us/windows/wsl/install
- Docker. *Volumes* (un volumen guarda datos fuera del contenedor y sobrevive a que este se borre). https://docs.docker.com/engine/storage/volumes/
- Docker. *docker container run* (`-i`, `-t`, `--rm`, `-h`, `--network`; cada contenedor tiene su propia lista de procesos). https://docs.docker.com/reference/cli/docker/container/run/
- Apple. *Use zsh as the default shell on Mac* (zsh por omisión desde macOS 10.15). https://support.apple.com/en-us/102360
- Medición propia, 18 y 19-nov-2026, en una Mac con Apple silicon (Docker 29.2.1, linux/arm64) y en una computadora
  con Linux Mint 22.3 (x86_64, Docker 29.10.0). Qué se corrió en cada una y cómo se midió cada cifra está en las dos
  tablas de «Antes de empezar». Las pantallas de esta lección y las tres teclas del paso 5 se tomaron en la Mac. La
  revisión del 19-nov —el contenedor sin red, las etiquetas del curso y el `verifica` que ya no usa el reloj— se
  probó en la Mac. `[pendiente: repetir en Linux Mint la revisión del 19-nov]`
