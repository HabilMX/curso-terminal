# Lección 01 — Moverte sin ver

**Por Dorian Chávez, fundador de Hábil y arquitecto de integración.**

**Antes de leer, cinco palabras.** Una línea cada una; las demás llegan en el paso donde se usan.

- **Carpeta:** un lugar que guarda archivos y otras carpetas. En la terminal y en los manuales la vas a ver como
  *directory*, «directorio». Es lo mismo.
- **Árbol:** la forma en que están acomodadas las carpetas: una adentro de otra, como las ramas de un árbol.
- **Raíz:** la carpeta de hasta arriba, la que contiene a todas las demás. Se escribe con una sola diagonal: `/`.
- **Carpeta actual:** la carpeta donde estás parado en este momento. Es la que contesta `pwd`.
- **Ruta:** las indicaciones para llegar a una carpeta o a un archivo: nombres de carpetas separados por diagonales.
  Por ejemplo, `pagos/bitacoras`.

**Qué te llevas:** te mueves por las carpetas sin verlas, con `cd`; sabes en cuál vas a quedar antes de presionar
Enter; ves lo que hay en una carpeta con `ls`, también lo oculto; lees cada columna de `ls -l`; y escribes el camino
a una carpeta de dos maneras.

**Idea nueva de esta lección (la única que se evalúa):** las carpetas forman un árbol, y a cualquier lugar de ese
árbol se llega con dos tipos de ruta: la **absoluta**, que sale de la raíz, y la **relativa**, que sale de donde
estás parado.

**En qué orden va:** primero armas un laberinto de carpetas para practicar (paso 1). Luego caminas por él con rutas
relativas (paso 2) y con rutas absolutas (paso 3). Después miras lo que hay en una carpeta y lees sus columnas
(paso 4). Al final anotas cuatro respuestas y `verifica` las revisa (paso 5).

---

## La escena

*Escenario inventado; las pantallas que aparecen en la lección sí son reales.*

Martes, 23:47. Lucía ya entró al servidor. Ya leyó el indicador: sabe quién es ahí adentro y en qué carpeta está
parada. Del otro lado del teléfono le piden lo siguiente:

—Ve a la carpeta de las bitácoras de pagos y dime cuál archivo pesa más.

«Ve.» En su computadora, ir a una carpeta es dar doble clic hasta llegar. Aquí no hay nada a qué darle clic. Está la
misma línea de antes, esperando:

```text
operaciones@pagos-03:~$
```

Lucía tiene que llegar a una carpeta que no ve, por un camino que no ve, y saber en todo momento dónde está parada.
Y cuando llegue, tiene que leer una lista de archivos y decir cuál es el más grande sin equivocarse, porque del otro
lado van a actuar con lo que ella diga.

Esta lección es ese recorrido, en un laberinto hecho para perderse sin consecuencias.

---

## Antes de empezar

**Qué necesitas:**

| | Qué | Notas |
|---|---|---|
| La lección 00 | Hecha: `verifica 00` en verde | De ahí vienen el indicador, `pwd`, `man`, `--help` y las teclas para salir |
| El material del curso | Actualizado | Esta lección trae un comando nuevo, `prepara`. Abajo dice cómo recibirlo |
| Papel y lápiz | Para tus predicciones | El ejercicio 1.1 se hace sin IA y sin correr nada antes de predecir |

**Actualiza tu terminal de práctica.** La imagen que construiste en la lección 00 no trae lo de esta lección. Desde
tu computadora, en la carpeta del curso, corre estas dos líneas:

```bash
git pull
./practicar reconstruir
```

- `git pull` trae del repositorio los archivos nuevos del curso.
- `./practicar reconstruir` vuelve a construir la imagen con ellos. Descarga otra vez Ubuntu y los paquetes, así que
  tarda como la primera vez: en las pruebas de la lección 00 fueron entre 42 segundos y 1 minuto 38 segundos. Termina
  con `✅ Imagen lista.` Ese tiempo depende de la red y del servidor de paquetes de Ubuntu: el 19-nov-2026, en la Mac
  de la prueba, ese servidor contestaba lento, y una construcción completa llevaba más de 11 minutos sin terminar
  cuando se canceló. Si a ti te tarda, mira si el texto sigue avanzando antes de interrumpirla.

**Sin `git`:** vuelve a descargar el ZIP del curso, descomprímelo, abre tu terminal en la carpeta nueva y corre ahí
`./practicar reconstruir`.

Reconstruir la imagen no borra tu trabajo. Tu carpeta `~/practica` vive aparte, en el volumen que viste en la
lección 00, y este comando no lo toca.

Después entra como ya sabes:

```bash
./practicar
```

**Dónde se probó esta lección, y dónde falta:**

| Sistema | Estado | Qué se corrió |
|---|---|---|
| Mac con Apple silicon, Docker 29.2.1 corriendo con Colima | Probado el 19-nov-2026 | Adentro de la terminal de práctica, todo: `prepara 01` dos veces, cada comando de los cinco pasos, `verifica 01` en rojo y en verde, y las pantallas que ves aquí. La imagen se construyó con `docker build`, reutilizando los pasos ya construidos; `./practicar reconstruir` completo no se terminó de correr ese día, por la red |
| Linux Mint 22.3 (x86_64) | Sin probar todavía con esta lección | Ahí se probó la lección 00. Lo que puede cambiar está en la nota de abajo |
| Ubuntu, Mac con Docker Desktop, Windows con Docker Desktop y WSL 2 | Sin probar todavía | Igual que en la lección 00 |

Una nota sobre lo que puede cambiar de una computadora a otra: el número que `ls -l` pone como tamaño de una
**carpeta**. En la prueba salió `4096`. Ese número lo decide el sistema donde Docker guarda el volumen, así que en tu
computadora puede ser otro. Los tamaños de los **archivos** del laberinto sí son los mismos en todas.

---

## Paso 1 · Arma el laberinto y míralo desde arriba

Adentro de tu terminal de práctica, corre:

```bash
prepara 01
```

```text
✅ Laberinto listo en /home/alumno/practica/01 (medí: 10 carpetas y 12 archivos, cada archivo con su tamaño).
   Empieza por ahí:  cd ~/practica/01
```

`prepara` es un comando del curso. Arma el material de práctica de una lección; el de la 01 es un laberinto de
carpetas. Tres cosas sobre él:

- **No tiene azar.** Los nombres, los tamaños y las fechas vienen de una lista fija. Por eso las pantallas de esta
  lección deben coincidir con las tuyas. En la prueba se armó dos veces seguidas y quedó idéntico. La excepción
  conocida es el tamaño de las carpetas, por lo que dice la nota de «Antes de empezar».
- **Solo toca la carpeta `~/practica/01`.** Nada más.
- **Se puede volver a correr.** Si te pierdes o descompones algo, `prepara 01` deja el laberinto como el primer día.
  Para eso tiene que borrar el que ya está, así que te avisa y te pide que escribas `si`. En la prueba tardó menos de
  un segundo.

¿Por qué `prepara` es un comando aparte y no una parte de `verifica`? Porque hacen cosas opuestas. `verifica` solo
mira y te dice qué falta. `prepara` crea y borra. Si estuvieran juntos, un día correrías uno queriendo el otro.

**El laberinto, visto desde arriba.** Así quedó. Este dibujo no es la salida de ningún comando: lo ponemos aquí
porque es lo que vas a tener que imaginar mientras caminas. En la terminal no vas a ver el árbol completo: solo vas
a ver la carpeta donde estás parado.

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

Léelo de arriba hacia abajo. Hasta arriba está la raíz, `/`. Adentro de la raíz hay una carpeta `home`; adentro de
`home`, una carpeta `alumno`; y así. Cada nivel hacia la derecha es «adentro de». Los archivos del laberinto no
tienen nada que leer: son de relleno, y lo que importa de ellos es su nombre, su tamaño y su fecha.

Hay dos palabras que se usan mucho al hablar del árbol. Si `bitacoras` está adentro de `pagos`, se dice que `pagos`
es la carpeta **de arriba** de `bitacoras`, o su carpeta *padre*. Subir es ir hacia la raíz. Bajar es entrar a una
carpeta que está adentro de la tuya.

Ahora párate en la entrada del laberinto. `cd` es el comando para cambiar de carpeta, y lo ves con calma en el
siguiente paso. Hoy esta línea es una receta dada:

```bash
cd ~/practica/01
```

```text
alumno@practica:~/practica/01$
```

`cd` no contestó nada: como en la lección 00, el silencio quiere decir que salió bien. Lo que cambió es el
indicador. Antes decía `~` y ahora dice `~/practica/01`. **El indicador te va diciendo en qué carpeta estás parado.**
Compruébalo con el comando que ya conoces:

```bash
pwd
```

```text
/home/alumno/practica/01
```

El indicador usa la forma corta, con `~`. `pwd` contesta la forma completa.

**Lo demuestra:** todavía nada. Este paso deja listo el material de los tres ejercicios.

---

## Paso 2 · Camina: `cd` con rutas relativas

`cd` viene de *change directory*, «cambiar de carpeta». Se escribe `cd`, un espacio y la ruta a donde quieres ir.

Para saber a dónde puedes ir necesitas ver qué hay. `ls`, a secas, lista lo que hay en la carpeta actual. Lo ves a
fondo en el paso 4; por ahora alcanza con esto:

```bash
ls
```

```text
LEEME.txt  pagos  respaldos
```

Coincide con el dibujo: en `01` hay un archivo y dos carpetas.

**Predice primero.** Vas a correr `cd pagos`. Escribe en tu papel qué va a decir el indicador después.

```bash
cd pagos
```

```text
alumno@practica:~/practica/01/pagos$
```

Bajaste un nivel. `pagos` es una **ruta relativa**: no dice dónde está `pagos` en todo el árbol, solo dice «la
carpeta `pagos` que está aquí, donde estoy parado». Funcionó porque estabas en `01`, y en `01` hay una carpeta
`pagos`.

Baja otro nivel y pregunta dónde quedaste:

```bash
cd bitacoras
pwd
```

```text
/home/alumno/practica/01/pagos/bitacoras
```

**Para subir: dos puntos.** `..` quiere decir «la carpeta de arriba de donde estoy». **Predice:** estás en
`bitacoras`. ¿Dónde quedas después de `cd ..`?

```bash
cd ..
pwd
```

```text
/home/alumno/practica/01/pagos
```

**Varios niveles en una sola ruta.** Las carpetas de una ruta se separan con una diagonal, `/`. Puedes bajar dos
niveles de un jalón:

```bash
cd config/plantillas
```

```text
alumno@practica:~/practica/01/pagos/config/plantillas$
```

Y subir dos niveles de un jalón, con dos veces `..`:

```bash
cd ../..
pwd
```

```text
/home/alumno/practica/01/pagos
```

**Un error a propósito.** Estás en `pagos`. Pide una carpeta que no existe ahí:

```bash
cd nada
```

```text
bash: cd: nada: No such file or directory
```

«No existe ese archivo o carpeta.» Fíjate en dos cosas. El mensaje te regresó el indicador, así que no te atrapó. Y
el indicador sigue diciendo `pagos`: **cuando `cd` falla, te quedas donde estabas.**

Otro error, que es el mismo:

```bash
cd Bitacoras
```

```text
bash: cd: Bitacoras: No such file or directory
```

La carpeta se llama `bitacoras`, con minúscula. La terminal distingue mayúsculas de minúsculas en los nombres de las
carpetas igual que en los comandos.

**Un atajo que no se evalúa.** No tienes que escribir los nombres completos. Escribe `cd bit` y presiona la tecla
**Tab**: la terminal completa el nombre, `bitacoras/`, porque es la única carpeta de aquí que empieza así. Menos
letras, menos errores de dedo. Si no completa nada, es que no hay ninguna que empiece así, o hay varias.

**Lo que acabas de usar,** en una tabla:

| Escribes | Quiere decir |
|---|---|
| `cd pagos` | Entra a la carpeta `pagos` que está aquí |
| `cd config/plantillas` | Entra a `config`, que está aquí, y luego a `plantillas`, que está adentro |
| `cd ..` | Sube a la carpeta de arriba |
| `cd ../..` | Sube dos niveles |

Todas son rutas relativas: **su resultado depende de dónde estás parado.** El mismo `cd pagos` que funcionó en `01`
falla en cualquier carpeta que no tenga una `pagos` adentro.

**Lo demuestra** (objetivo 1.1, primera mitad): tus predicciones de este paso, comparadas con el indicador y con
`pwd`.

---

## Paso 3 · La otra manera: rutas absolutas

Una ruta relativa sirve mientras sepas dónde estás. Hay otra manera de escribir un camino, que no depende de eso.

**Sube hasta arriba.** La raíz se escribe con una sola diagonal:

```bash
cd /
```

```text
alumno@practica:/$
```

El indicador dice `/`. Estás en la raíz del árbol, la carpeta que contiene a todas. Mira qué hay:

```bash
ls
```

```text
bin   dev  home  media  opt   root  sbin  sys  usr
boot  etc  lib   mnt    proc  run   srv   tmp  var
```

Son las carpetas de un sistema Linux. En este curso vas a entrar a pocas; hoy te importa una, `home`, porque adentro
está tu carpeta personal.

**Predice:** estás en la raíz. ¿Qué pasa si corres `cd pagos`, el mismo comando que funcionó en el paso 2?

```bash
cd pagos
```

```text
bash: cd: pagos: No such file or directory
```

Falló, y con razón: en la raíz no hay ninguna carpeta `pagos`. La ruta relativa `pagos` solo funciona parado en
`01`.

**Una ruta absoluta empieza con `/`.** Dice el camino completo desde la raíz, carpeta por carpeta. Por eso funciona
igual desde cualquier lugar. Esta lleva a las bitácoras:

```bash
cd /home/alumno/practica/01/pagos/bitacoras
```

```text
alumno@practica:~/practica/01/pagos/bitacoras$
```

Léela junto con el dibujo del paso 1: de la raíz a `home`, de `home` a `alumno`, de `alumno` a `practica`, de ahí a
`01`, a `pagos` y a `bitacoras`. Es exactamente lo que contesta `pwd` cuando estás ahí: **`pwd` contesta la ruta
absoluta de la carpeta actual.**

**La diagonal del principio cambia todo.** Estas dos rutas se parecen y piden cosas distintas:

| Escribes | Empieza con `/` | Quiere decir |
|---|---|---|
| `cd pagos` | No: es relativa | La carpeta `pagos` que está adentro de donde estoy |
| `cd /pagos` | Sí: es absoluta | La carpeta `pagos` que está en la raíz |

Compruébalo. Ve a `~/practica/01`, donde sí hay una `pagos`, y pide la de la raíz:

```bash
cd ~/practica/01
cd /pagos
```

```text
bash: cd: /pagos: No such file or directory
```

Hay una `pagos` a un paso de ti, pero pediste otra: una que estaría en la raíz, y esa no existe.

**Tu carpeta personal tiene abreviatura: `~`.** Ya la conoces del indicador. Al principio de una ruta, la terminal
cambia `~` por `/home/alumno` antes de correr el comando. Por eso `cd ~/practica/01` y
`cd /home/alumno/practica/01` llevan al mismo lugar. Una ruta que empieza con `~` no depende de dónde estás, igual
que una absoluta. Pero la abreviatura la entiende la terminal, no es parte del árbol: en otra cuenta, `~` es otra
carpeta. Cuando te pidan una ruta absoluta, escríbela desde la raíz, con `/`.

**`cd` a secas te lleva a casa.** Sin ruta, `cd` te lleva a tu carpeta personal:

```bash
cd
pwd
```

```text
/home/alumno
```

**Y `cd -` te regresa a la carpeta anterior.** Es un guion después de `cd`. Sirve para ir y venir entre dos lugares:

```bash
cd -
```

```text
/home/alumno/practica/01
alumno@practica:~/practica/01$
```

Este sí contesta: te dice a dónde te regresó.

**Los cuatro puntos de partida,** juntos:

| La ruta empieza con | Sale de | Ejemplo | Depende de dónde estás |
|---|---|---|---|
| `/` | La raíz | `/home/alumno/practica/01` | No |
| `~` | Tu carpeta personal | `~/practica/01` | No |
| `..` | La carpeta de arriba de la actual | `../config` | Sí |
| Un nombre | La carpeta actual | `pagos/bitacoras` | Sí |

Las dos primeras filas funcionan desde cualquier lugar. Las dos últimas son relativas.

**¿Cuál conviene?** Las dos se usan, para cosas distintas.

- La **relativa** es corta. Conviene cuando el lugar está cerca de donde estás: `cd ..`, `cd config`.
- La **absoluta** no deja dudas. Conviene cuando anotas un camino para alguien más, o para ti dentro de un mes: quien
  lo lea no sabe en qué carpeta estabas parado cuando lo escribiste.

Es lo que le pasa a Lucía por teléfono. Si le dictan «entra a `bitacoras`», solo sirve si los dos saben dónde está
parada ella. Si le dictan la ruta desde la raíz, sirve aunque esté en otro lado.

**Lo demuestra** (objetivo 1.1 completo): ejercicio 1.1, abajo. Y es la mitad del objetivo 1.3.

---

## Paso 4 · Mira lo que hay: `ls` y sus columnas

Ve a la carpeta de las bitácoras. Usa la ruta que quieras; ya tienes dos.

```bash
cd ~/practica/01/pagos/bitacoras
ls
```

```text
abril.log  enero.log  febrero.log  marzo.log  mayo.log  viejas
```

`ls` viene de *list*, «listar». A secas, da solo los nombres, en orden alfabético. En tu pantalla, `viejas` sale en
azul: el color marca que es una carpeta. No da nada más: ni tamaños ni fechas. Para eso están las opciones. Como
viste en la lección 00, una opción es una palabra con guion que cambia lo que hace el comando. Hoy son tres.

### `-a`: también lo oculto

**Predice:** el `ls` de arriba mostró seis nombres. ¿Crees que es todo lo que hay en la carpeta?

```bash
ls -a
```

```text
.   .pendiente  enero.log    marzo.log  viejas
..  abril.log   febrero.log  mayo.log
```

Aparecieron tres nombres que empiezan con punto. En Linux, **un archivo cuyo nombre empieza con punto es un archivo
oculto**: `ls` a secas no lo lista. No está protegido ni es secreto; solo no estorba en la lista de todos los días.
`-a` viene de *all*, «todo».

- `.pendiente` es un archivo oculto del laberinto.
- `..` ya lo conoces: es la carpeta de arriba.
- `.` es la carpeta actual. Por eso `cd .` no te mueve.

Los dos últimos no son archivos que alguien haya creado: toda carpeta los tiene, y son la razón de que `cd ..`
funcione en cualquier parte.

### `-l`: la lista larga

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

`-l` viene de *long*, «larga»: un renglón por archivo o carpeta, con sus datos en columnas. Toma el renglón de
`marzo.log` y léelo de izquierda a derecha:

```text
-rw-r--r-- 1 alumno alumno 1200000 Mar 31  2025 marzo.log
```

| Columna | En el ejemplo | Qué dice |
|---|---|---|
| **Tipo** | `-` | La primera letra del renglón. Un guion es un archivo normal. Una `d`, de *directory*, es una carpeta: mira el renglón de `viejas`. Hay otros tipos; en este laberinto solo hay estos dos |
| **Permisos** | `rw-r--r--` | Las nueve letras que siguen. Dicen quién puede leer, escribir y ejecutar el archivo. Hoy solo les pones nombre: se leen y se cambian en la lección 09 |
| **Enlaces** | `1` | Cuántos nombres apuntan a ese mismo archivo. En este curso no lo usas |
| **Dueño** | `alumno` | El usuario al que pertenece el archivo. Aquí eres tú |
| **Grupo** | `alumno` | El grupo de usuarios al que pertenece. Vuelve a salir en la lección 09, junto con los permisos |
| **Tamaño** | `1200000` | Cuánto mide el archivo, en bytes |
| **Fecha** | `Mar 31  2025` | Cuándo se modificó por última vez. Los meses salen en inglés: `Mar` es marzo y `Apr` es abril |
| **Nombre** | `marzo.log` | Cómo se llama |

Tres detalles que conviene saber:

- **El `total` de arriba no es la suma de la columna de tamaño.** Es el espacio que ocupa en disco lo que se listó,
  contado en bloques de 1024 bytes. En este curso no lo usas.
- **El tamaño de una carpeta no es lo que lleva adentro.** `viejas` dice `4096` y adentro tiene un archivo de
  20,000 bytes. El número de una carpeta es lo que ocupa su propia lista de nombres. Para saber cuánto hay adentro,
  entras y miras.
- **La fecha cambia de forma según la edad.** Si el archivo se modificó hace más de seis meses, ves el año, como
  aquí. Si es reciente, en lugar del año ves la hora. Lo vas a notar en el paso 5, en un archivo que vas a crear tú.
  Esa hora no es la de tu reloj: la terminal de práctica usa la hora universal, UTC.

### `-h`: tamaños que se leen

¿Cuál es más grande, `998000` o `1200000`? Hay que contar cifras. `-h` viene de *human-readable*, «legible para una
persona»: junto con `-l`, escribe el tamaño abreviado y con una letra.

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

| Letra | Unidad | Cuántos bytes |
|---|---|---|
| ninguna | bytes | Tal cual: `980` son 980 bytes |
| `K` | kilobytes | 1,024 |
| `M` | megabytes | 1,048,576, que son 1,024 K |
| `G` | gigabytes | 1,024 M |

**La trampa de esta columna: se lee primero la letra y después el número.** `975K` tiene un número más grande que
`1.2M`, y aun así es el archivo más chico de los dos, porque una M vale más de mil K. Quien lee solo el número se
equivoca de archivo.

Y un límite: `-h` redondea. `marzo.log` mide 1,200,000 bytes, que son 1.14 M, y `ls` escribe `1.2M`. Para ver rápido
cuál es grande y cuál es chico, `-h` alcanza. Cuando necesites el número exacto, quita la `h`.

### Las opciones se juntan

Varias opciones de una letra se pueden escribir pegadas, con un solo guion. `ls -lah` es lo mismo que
`ls -l -a -h`: lista larga, con lo oculto y con tamaños legibles.

```bash
ls -lah
```

```text
total 2.2M
drwxr-xr-x 3 alumno alumno 4.0K May 31  2025 .
drwxr-xr-x 5 alumno alumno 4.0K Jun  2  2025 ..
-rw-r--r-- 1 alumno alumno  47K May 12  2025 .pendiente
-rw-r--r-- 1 alumno alumno 975K Apr 30  2025 abril.log
-rw-r--r-- 1 alumno alumno  980 Jan 31  2025 enero.log
-rw-r--r-- 1 alumno alumno 9.6K Feb 28  2025 febrero.log
-rw-r--r-- 1 alumno alumno 1.2M Mar 31  2025 marzo.log
-rw-r--r-- 1 alumno alumno    0 May 31  2025 mayo.log
drwxr-xr-x 2 alumno alumno 4.0K Jan  2  2025 viejas
```

### `ls` también acepta una ruta

No hace falta ir a una carpeta para ver qué tiene. `ls` acepta una ruta, relativa o absoluta, igual que `cd`. Sin
moverte de `bitacoras`:

```bash
ls -lh ..
```

```text
total 12K
drwxr-xr-x 3 alumno alumno 4.0K May 31  2025 bitacoras
drwxr-xr-x 3 alumno alumno 4.0K Feb 10  2025 config
drwxr-xr-x 3 alumno alumno 4.0K Jun  2  2025 entregas
```

Viste la carpeta de arriba, `pagos`, y sigues parado en `bitacoras`. Mirar no te mueve.

### Con Claude Code, en este orden

En este curso un asistente de IA entra siempre igual. Aquí son cinco momentos: **predices → le pides → lees →
corres → explicas.**

1. **Predices.** Con lo que ya viste en `ls -lah`, anota en tu papel cuál es el archivo más grande de `bitacoras`.
   Todavía no le preguntas nada a nadie.
2. **Le pides.** En tu computadora, pídele a Claude Code algo así: «Dame un comando de `ls` que liste una carpeta
   ordenada por tamaño, del archivo más grande al más chico, y explícame cada opción». Claude Code corre en tu
   computadora, no adentro de tu terminal de práctica: no ve tu laberinto.
3. **Lees.** Antes de correr lo que te dio, comprueba cada opción en la propia máquina, como en la lección 00. Si te
   propone `ls -lhS`, busca la `S` en `ls --help`. Dice: `Sort by file size, largest first.` Si te da un comando con
   piezas que todavía no conoces, pídele otro que solo use opciones de `ls`.
4. **Corres.** El comando lo escribes tú, en tu terminal de práctica.
5. **Explicas.** Con tus palabras: ¿por qué quedó en ese orden? ¿Coincide con lo que predijiste? ¿Y por qué el
   archivo oculto no aparece en esa lista?

Así salió `ls -lhS` en la prueba:

```text
total 2.2M
-rw-r--r-- 1 alumno alumno 1.2M Mar 31  2025 marzo.log
-rw-r--r-- 1 alumno alumno 975K Apr 30  2025 abril.log
-rw-r--r-- 1 alumno alumno 9.6K Feb 28  2025 febrero.log
drwxr-xr-x 2 alumno alumno 4.0K Jan  2  2025 viejas
-rw-r--r-- 1 alumno alumno  980 Jan 31  2025 enero.log
-rw-r--r-- 1 alumno alumno    0 May 31  2025 mayo.log
```

El orden es el paso que te ahorra el asistente. Leer la columna y saber por qué `viejas` quedó en medio sigue siendo
trabajo tuyo.

**Lo demuestra** (objetivo 1.2): ejercicio 1.2, abajo.

---

## Paso 5 · Anota tus respuestas y compruébalas

Hasta aquí todo lo comprobaste tú, con el indicador y con `pwd`. Ahora le toca a `verifica`. Para que pueda revisar
lo que encontraste, lo anotas en un archivo: tu **hoja de respuestas**, `~/practica/respuestas/01.md`.

Córrelo antes de anotar nada, para ver de dónde partes:

```bash
verifica 01
```

```text
✅ Tu laberinto está completo (medí: las 22 carpetas y archivos de la lista existen en ~/practica/01, cada archivo con su tamaño)
❌ Falta tu hoja de respuestas, ~/practica/respuestas/01.md (medí: que el archivo exista). Créala con el paso 5 de la lección. Sin ella no puedo revisar tus cuatro respuestas

❌ Lección 01: falta lo marcado con ❌. Arréglalo y corre  verifica 01  otra vez.
```

Un verde y un rojo. El rojo es normal: la hoja todavía no existe.

**El formato de la hoja.** Son cuatro renglones. Cada uno lleva una clave, dos puntos y tu respuesta:

```text
mas-grande: el nombre del archivo más grande del laberinto
oculto: el nombre del archivo oculto del laberinto
ruta-absoluta: la ruta absoluta de la carpeta destino
ruta-relativa: la ruta relativa a la carpeta destino, saliendo de bitacoras
```

Las reglas, para que no haya sorpresas:

- **Las claves se escriben tal cual:** `mas-grande`, `oculto`, `ruta-absoluta` y `ruta-relativa`. `verifica` busca
  esas cuatro palabras al principio de un renglón, seguidas de dos puntos.
- **En las dos primeras va solo el nombre del archivo,** como lo escribe `ls`: con sus minúsculas y con su punto, si
  lo lleva.
- **Si una clave aparece varias veces, vale la última.** Para corregir una respuesta no borras nada: la escribes otra
  vez.
- **Lo demás no estorba.** Renglones vacíos, notas tuyas o espacios de más no cambian el resultado.

**Cómo se escribe en la hoja.** Todavía no has visto un editor de texto; llega en la lección 07. Hoy la hoja se
llena con dos recetas dadas. La primera crea la carpeta de las respuestas:

```bash
mkdir ~/practica/respuestas
```

La segunda agrega un renglón a la hoja. Cambia `AQUI-TU-RESPUESTA` por lo que encontraste:

```bash
echo "mas-grande: AQUI-TU-RESPUESTA" >> ~/practica/respuestas/01.md
```

- `echo` repite en la pantalla lo que le escribas entre comillas.
- `>>` manda esa respuesta al final de un archivo, en lugar de a la pantalla. Si el archivo no existe, lo crea.
- Ojo con la diferencia: el `>` de la lección 00, uno solo, reemplaza todo el archivo. Con `>>`, dos, se agrega al
  final. Aquí van dos. Las dos piezas se ven a detalle en la lección 06.

Repite la receta con cada clave: `oculto`, `ruta-absoluta` y `ruta-relativa`. Para ver cómo va quedando tu hoja:

```bash
cat ~/practica/respuestas/01.md
```

En la lección 00, `cat` a secas repetía lo que escribías. Seguido de un archivo, te muestra lo que tiene adentro.

**Las dos rutas del ejercicio 1.3.** La carpeta destino es `guardia`: está adentro de `entregas`, que está adentro
de `pagos`. Búscala en el dibujo del paso 1. Tienes que llegar a ella de dos maneras:

1. Con una **ruta absoluta**, que empiece en la raíz.
2. Con una **ruta relativa**, saliendo de la carpeta `bitacoras`.

Antes de anotar cada ruta, recórrela: párate donde corresponde, entra con `cd` y pregunta con `pwd` si llegaste a
`guardia`. Cuando una ruta te lleve, anótala en la hoja tal como la escribiste. `verifica` hace la misma prueba que
tú: entra por tu ruta absoluta y mira a dónde llega; luego se para en `bitacoras`, entra por tu ruta relativa y
mira a dónde llega.

Cuando tengas los cuatro renglones, corre `verifica 01`. Cada marca dice qué midió. Si alguna sale en rojo, léela
completa: dice qué anotaste, qué se midió y con qué comando volver a mirar. No dice la respuesta. Corrige con otro
`echo` y repite hasta ver esto al final:

```text
✅ Lección 01: todo en verde.
```

Ya que tienes la hoja, mira su renglón:

```bash
ls -l ~/practica/respuestas
```

```text
total 4
-rw-r--r-- 1 alumno alumno 139 Nov 19 06:58 01.md
```

Es el detalle que quedó pendiente en el paso 4: tu hoja es reciente, y en lugar del año trae la hora. El tamaño y la
fecha de la tuya van a ser otros.

**Lo demuestra** (objetivos 1.2 y 1.3): `verifica 01` todo en verde.

---

## Una nota sobre el `ls` de esta terminal

En la lección 00 viste que los comandos básicos de esta terminal vienen de `rust-coreutils`, un proyecto que también
se llama uutils, y no del proyecto GNU, que es el clásico. El `ls` que usaste hoy es de esos: `ls --version`
contesta `ls (uutils coreutils) 0.10.0`. Si comparas con un tutorial o con un servidor que tenga el `ls` de GNU,
esto es lo que se midió:

- **En lo que enseña esta lección, la salida es idéntica.** Ubuntu deja instalado el `ls` de GNU con otro nombre,
  `gnuls`; lo dice su documentación. En la imagen de la prueba es la versión 9.7. Se compararon los dos en el
  laberinto con `ls`, `ls -a`, `ls -l`, `ls -la`, `ls -lah` y `ls -lhS`: las seis salidas coincidieron letra por
  letra. Puedes repetirlo tú: corre `gnuls -lah` en `bitacoras` y compáralo con tu `ls -lah`.
- **Lo que sí cambia es el texto de la ayuda.** `ls --help` y `man ls` describen las opciones con otras palabras.
  Para `-a`, el `ls` de esta terminal dice `Do not ignore hidden files (files with names that start with '.').` y el
  de GNU dice `do not ignore entries starting with .`
- **Y el mensaje cuando escribes una opción que no existe.** Con `ls -z`, el de esta terminal contesta
  `error: unexpected argument '-z' found`; el de GNU, `invalid option -- 'z'`.

La comparación vale para esas seis formas y para las versiones de la prueba, no para todas las opciones de `ls`. La
propia documentación de Ubuntu dice que `rust-coreutils` todavía no es compatible del todo con GNU, y por eso deja
los dos instalados.

---

## Tus ejercicios

El ejercicio 1.1 se hace **sin IA** y sin correr nada antes de predecir: solo tu papel y el dibujo del paso 1.

| # | Haces | Está hecho cuando |
|---|---|---|
| 1.1 | Predices por escrito en qué carpeta quedas después de cada uno de los diez `cd` de abajo, y luego los corres | Tienes tus diez predicciones y las diez salidas de `pwd`, y sabes explicar cada una que no coincidió |
| 1.2 | En `bitacoras`, encuentras el archivo más grande y el archivo oculto, y los anotas en tu hoja | `verifica 01` marca en verde `mas-grande` y `oculto` |
| 1.3 | Llegas a la carpeta `guardia` con una ruta absoluta y con una relativa que sale de `bitacoras`, y anotas las dos | `verifica 01` sale todo en verde |

**Los diez `cd` del 1.1.** Primero escribe las diez predicciones, todas, con la ruta completa que crees que va a
contestar `pwd`. No corras nada todavía. Si crees que un `cd` va a fallar, escríbelo así, y escribe dónde te quedas.

Empiezas parado en la entrada del laberinto, `~/practica/01`. Los comandos van en fila: cada uno sale de donde te
dejó el anterior.

| # | Comando |
|---|---|
| 1 | `cd pagos/config` |
| 2 | `cd plantillas` |
| 3 | `cd ../../bitacoras/viejas` |
| 4 | `cd ../..` |
| 5 | `cd /` |
| 6 | `cd pagos` |
| 7 | `cd home` |
| 8 | `cd ~` |
| 9 | `cd ../..` |
| 10 | `cd ..` |

Cuando tengas las diez, ve a la entrada con `cd ~/practica/01` y corre los comandos uno por uno, con un `pwd`
después de cada uno. Compara cada salida con tu papel. Las respuestas están en «Compara tus respuestas»; no las mires
antes de predecir.

No hay un número para pasar. Lo que cuenta es que, en cada una que fallaste, puedas decir qué leíste mal: si la ruta
era absoluta o relativa, o en qué carpeta estabas parado.

**Tiempo (estimado):** de 60 a 90 minutos, sin contar la reconstrucción de la imagen. Es una estimación: se ajusta
con el primer grupo que tome el curso.

---

## ¿No te corre?

| Ves | Causa probable | Qué haces |
|---|---|---|
| `bash: prepara: command not found` | Tu imagen es de antes de esta lección | Sal con `exit` y corre `./practicar reconstruir`, como en «Antes de empezar» |
| `Todavía no hay verifica para la lección «01».` | Lo mismo: la imagen es de antes | `./practicar reconstruir` |
| `❌ prepara corre dentro de la terminal de práctica…` | No estás como `alumno`, o no existe tu carpeta `~/practica` | Sal y entra con `./practicar` |
| `Ya existe /home/alumno/practica/01.` y una pregunta | Ya habías armado el laberinto, y armarlo de nuevo lo borra | Si quieres empezar de cero, escribe `si`. Si no, presiona Enter: no se borra nada |
| Después de `prepara 01`, `ls` no muestra nada | Estabas parado adentro del laberinto cuando se volvió a armar: esa carpeta se borró y tú sigues en la vieja | `cd ~/practica/01` |
| `bash: cd: …: No such file or directory` | La carpeta no existe con ese nombre en el lugar donde la pediste | `pwd` para saber dónde estás y `ls` para ver qué hay. Revisa las mayúsculas y si la ruta debía empezar con `/` |
| `bash: cd: too many arguments` | Escribiste un espacio dentro de la ruta | Una ruta va sin espacios: `cd pagos/config` |
| El indicador dice una carpeta que no esperabas | Un `cd` anterior te dejó en otro lado | `cd ~/practica/01` y empieza de ahí |
| `mkdir: /home/alumno/practica/respuestas: File exists` | Ya habías creado la carpeta de las respuestas | No pasa nada: sigue con el `echo` |
| `❌ No encontré tu respuesta «…»` | Falta ese renglón, la clave está mal escrita, o usaste `>` en lugar de `>>` y la hoja se reemplazó | `cat ~/practica/respuestas/01.md` para ver qué tiene, y agrega el renglón que falte |
| `❌ … no es una ruta absoluta` | Tu ruta no empieza con `/`. Si la escribiste con `~`, es la abreviatura | Escríbela desde la raíz: `pwd`, parado en la carpeta destino, te la da completa |
| `❌ Tu ruta relativa … no existe saliendo de …` | La probaste parado en otra carpeta | Ve a `bitacoras`, y desde ahí prueba tu ruta con `cd` y `pwd` |
| `❌ Tu laberinto no está completo…` | Borraste o cambiaste algo del laberinto | `prepara 01`. Tus respuestas no se pierden: viven en otra carpeta |
| Tu pantalla de `ls -l` no coincide con la de la lección | Si es el tamaño de una carpeta, es normal (nota de «Antes de empezar»). Si es otra cosa, el laberinto cambió | `prepara 01` y compara otra vez |

---

## Para cerrar

**Contesta sin volver a leer.**

**Cinco rutas: ¿absoluta o relativa?** Pasas con 4 de 5.

1. `config/plantillas`
2. `/home/alumno/practica`
3. `../respaldos`
4. `/`
5. `..`

**Y cinco preguntas:**

6. Estás en `/home/alumno/practica/01/respaldos/semana-1`. ¿Qué ruta relativa te lleva a `semana-2`?
7. Corres `ls` en una carpeta y no sale nada. ¿Ya puedes decir que está vacía? ¿Qué corres para estar seguro?
8. En `ls -lh` ves `812K` en un renglón y `3.1M` en otro. ¿Cuál archivo es más grande, y cómo lo sabes?
9. Un renglón de `ls -l` empieza con `d`. ¿Qué te dice esa letra? ¿Y qué dice de su contenido el número de la
   columna del tamaño?
10. Le vas a dictar a un compañero el camino a una carpeta, y no sabes en cuál está parado él. ¿Le dictas una ruta
    absoluta o una relativa? ¿Por qué?

**Lo que sigue:** ya sabes moverte por las carpetas y ver lo que tienen. En la lección 02 empiezas a cambiar lo que
hay: creas, copias, mueves y borras, en una terminal donde no hay papelera.

---

> **Qué hace Hábil**
>
> Operamos y modernizamos la infraestructura donde corren sistemas que no pueden fallar: servidores Linux,
> contenedores, Kubernetes y entrega continua, para banca, seguros y empresas reguladas. Si sus servidores dependen
> de una persona que «sabe dónde está todo», eso se puede ordenar sin detener la operación.
>
> [**CI/CD, DevSecOps y nube**](https://www.habil.mx/es/arquitectura-cloud-native/)
>
> Primera conversación, sin costo.

---

## Compara tus respuestas

**Ejercicio 1.1.** Lo que contesta `pwd` después de cada comando:

| # | Comando | Quedas en | Por qué |
|---|---|---|---|
| 1 | `cd pagos/config` | `/home/alumno/practica/01/pagos/config` | Relativa: baja dos niveles desde `01` |
| 2 | `cd plantillas` | `/home/alumno/practica/01/pagos/config/plantillas` | Relativa: baja uno |
| 3 | `cd ../../bitacoras/viejas` | `/home/alumno/practica/01/pagos/bitacoras/viejas` | Sube dos, hasta `pagos`, y baja dos por otra rama |
| 4 | `cd ../..` | `/home/alumno/practica/01/pagos` | Sube dos |
| 5 | `cd /` | `/` | Absoluta: la raíz, sin importar dónde estabas |
| 6 | `cd pagos` | `/` | Falla: `No such file or directory`. En la raíz no hay `pagos`, y cuando `cd` falla te quedas donde estabas |
| 7 | `cd home` | `/home` | Relativa: en la raíz sí hay una carpeta `home` |
| 8 | `cd ~` | `/home/alumno` | Tu carpeta personal, sin importar dónde estabas |
| 9 | `cd ../..` | `/` | Sube dos: de `alumno` a `home`, y de `home` a la raíz |
| 10 | `cd ..` | `/` | Arriba de la raíz no hay nada: `..` en la raíz es la misma raíz. No da error y no te mueve |

Las que más se fallan son la 6 y la 10. La 6, por dar por hecho que un comando que ya funcionó va a funcionar otra
vez: funcionó parado en otra carpeta. La 10, por esperar un error que no llega.

**Ejercicio 1.2.** `verifica 01` te dice si acertaste. El razonamiento que debe haber detrás:

- **El más grande** se lee en la columna del tamaño de `ls -lh`, primero la letra y después el número. El único
  renglón con `M` le gana a todos los que traen `K`, aunque uno de ellos tenga un número mayor. Sin `-h` se llega a
  lo mismo contando cifras: es el único tamaño de siete cifras.
- **El oculto** solo aparece con `-a`. Es el único nombre que empieza con punto y no es ni `.` ni `..`. En tu hoja
  va con todo y su punto: el punto es parte del nombre.

**Ejercicio 1.3.** Aquí tampoco va la respuesta escrita: el control es `verifica 01`. Para saber si vas bien:

- Tu ruta absoluta empieza con `/` y es, letra por letra, lo que contesta `pwd` cuando estás parado en `guardia`.
  Después de la raíz lleva siete nombres.
- Tu ruta relativa no empieza con `/` ni con `~`. La más corta sube un nivel y baja dos. Hay otras más largas que
  también llegan, y `verifica` las da por buenas: lo que mide es a dónde llevan, no cómo están escritas.

**Las preguntas del paso 4, con Claude Code.** `ls -lhS` ordena por el tamaño que cada renglón trae en su columna.
`viejas` queda en medio porque su renglón dice `4.0K`, que es el tamaño de la carpeta y no el de lo que lleva
adentro. El archivo oculto no aparece porque el comando no lleva `-a`: con `ls -lahS` sí sale.

**Para cerrar.**

1. Relativa.
2. Absoluta.
3. Relativa.
4. Absoluta: es la raíz.
5. Relativa.
6. `../semana-2`: subes a `respaldos` y bajas a `semana-2`.
7. Todavía no: `ls` a secas no lista lo oculto. Corres `ls -a`. Si solo salen `.` y `..`, ahora sí está vacía.
8. El de `3.1M`. Se lee primero la letra: una M vale más de mil K.
9. Que es una carpeta. Del contenido no dice nada: ese número es el tamaño de la carpeta misma, no de lo que lleva
   adentro.
10. Una absoluta. Una relativa solo sirve si los dos saben en qué carpeta está parado él; la absoluta sale de la
    raíz y funciona desde cualquier lugar.

---

## Fuentes de la lección

Abiertas el 19-nov-2026. Todas están en inglés.

- The Open Group. *POSIX.1-2024, Base Definitions, capítulo 3* (definiciones 3.2 «Absolute Pathname», 3.311 «Relative Pathname» y 3.111 «Dot-Dot»: una ruta absoluta empieza con diagonal; una relativa, no). https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap03.html
- The Open Group. *POSIX.1-2024, utilidad cd* (`cd` sin ruta usa la carpeta personal; `cd -` va a la carpeta anterior y la escribe). https://pubs.opengroup.org/onlinepubs/9799919799/utilities/cd.html
- The Open Group. *POSIX.1-2024, utilidad ls* (el orden de las columnas de `-l`; la fecha lleva hora si el archivo se modificó en los últimos seis meses y año si no). https://pubs.opengroup.org/onlinepubs/9799919799/utilities/ls.html
- GNU. *Bash Reference Manual, «Bourne Shell Builtins»* (`cd` y `pwd`; `pwd` escribe la ruta absoluta de la carpeta actual). https://www.gnu.org/software/bash/manual/html_node/Bourne-Shell-Builtins.html
- GNU. *Bash Reference Manual, «Tilde Expansion»* (`~` se cambia por la carpeta personal). https://www.gnu.org/software/bash/manual/html_node/Tilde-Expansion.html
- GNU. *Coreutils, «What information is listed»* (las columnas de `-l`, las letras del tipo, el renglón `total` y `-h` con potencias de 1024). https://www.gnu.org/software/coreutils/manual/html_node/What-information-is-listed.html
- GNU. *Coreutils, «Which files are listed»* (`ls` no lista los nombres que empiezan con punto; `-a` sí). https://www.gnu.org/software/coreutils/manual/html_node/Which-files-are-listed.html
- Ubuntu Manpages. *ls(1)*, de Ubuntu 26.04 (la página del `ls` de `rust-coreutils` 0.10.0: `-a`, `-l`, `-h`, `-S`). https://manpages.ubuntu.com/manpages/resolute/en/man1/ls.1.html
- Ubuntu Manpages. *hier(7)*, de Ubuntu 26.04 (las carpetas de la raíz de un sistema Linux; `/home`). https://manpages.ubuntu.com/manpages/resolute/en/man7/hier.7.html
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, sección «rust-coreutils» (los comandos de GNU siguen instalados con el prefijo `gnu`, como `gnuls`, porque `rust-coreutils` todavía no es compatible del todo). https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- Medición propia, 19-nov-2026, en una Mac con Apple silicon (Docker 29.2.1 con Colima, linux/arm64), con la imagen
  del curso sobre Ubuntu 26.04.1: `ls` de uutils coreutils 0.10.0, `gnuls` de GNU coreutils 9.7 y bash 5.3.9. De ahí
  salen las pantallas de la lección, la comparación de `ls` contra `gnuls`, el redondeo de `-h`, la tecla Tab, el
  tiempo de `prepara 01` y sus dos corridas con el mismo resultado. En Linux esta lección todavía no se prueba.
