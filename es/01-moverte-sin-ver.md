# Lección 01 — Moverte sin ver

**Por Dorian Chávez, fundador de Hábil y arquitecto de integración.**

**Antes de leer, cinco palabras.** Una línea cada una; las demás llegan en el paso donde se usan.

- **Carpeta:** un lugar que guarda archivos y otras carpetas. En la terminal y en los manuales la vas a ver como
  *directory*, «directorio». Es lo mismo.
- **Árbol:** la forma en que están acomodadas las carpetas: una adentro de otra, como las ramas de un árbol.
- **Raíz:** la carpeta de hasta arriba, la que contiene a las demás. Se escribe con una sola diagonal: `/`.
- **Carpeta actual:** la carpeta donde estás parado en este momento. Es la que contesta `pwd`.
- **Ruta:** las indicaciones para llegar a una carpeta o a un archivo: nombres de carpetas separados por diagonales.
  Por ejemplo, `pagos/bitacoras`.

**Qué te llevas:** te mueves por las carpetas sin verlas, con `cd`; sabes en cuál vas a quedar antes de presionar
Enter; ves lo que hay en una carpeta con `ls`, también lo oculto; lees cada columna de `ls -l`; y escribes el camino
a una carpeta de dos maneras.

**Idea nueva de esta lección (la única que se evalúa):** las carpetas forman un árbol, y a un lugar de ese árbol se
llega con dos tipos de ruta: la **absoluta**, que sale de la raíz, y la **relativa**, que sale de donde estás
parado.

**En qué orden va:** armas un laberinto de carpetas (paso 1), caminas por él con rutas relativas (paso 2) y con
rutas absolutas (paso 3), miras lo que hay en una carpeta y lees sus columnas (paso 4), y al final anotas cuatro
respuestas que `verifica` revisa (paso 5).

---

## La escena

*Escenario inventado; las pantallas que aparecen en la lección sí son reales.*

Martes, 23:47. Lucía ya entró al servidor. Ya leyó el indicador: sabe quién es ahí adentro y en qué carpeta está
parada. Del otro lado del teléfono le piden lo siguiente:

—Ve a la carpeta de las bitácoras de pagos y dime cuál archivo pesa más.

«Ve.» En su computadora, ir a una carpeta es dar doble clic hasta llegar. Aquí no hay nada a qué darle clic. Está
la misma línea de antes, esperando:

```text
operaciones@pagos-03:~$
```

Lucía tiene que llegar a una carpeta que no ve, por un camino que no ve, y decir cuál archivo es el más grande sin
equivocarse, porque del otro lado van a actuar con lo que ella diga.

Esta lección es ese recorrido, en un laberinto hecho para perderse sin consecuencias.

---

## Antes de empezar

**Qué necesitas:** la lección 00 hecha, con `verifica 00` en verde; el material del curso actualizado; y papel y
lápiz para tus predicciones.

**Actualiza tu terminal de práctica.** La imagen que construiste en la lección 00 no trae lo de esta lección. Desde
tu computadora, en la carpeta del curso:

```bash
git pull
./practicar reconstruir
```

`git pull` trae del repositorio los archivos nuevos. `./practicar reconstruir` los mete a la imagen y normalmente
reutiliza Ubuntu y los paquetes que ya estaban construidos; necesita red solo si le falta alguno. En la Mac de la
prueba, con todo lo anterior ya construido, tardó menos de dos segundos. Termina con `✅ Imagen lista.` Si no usas
`git`, vuelve a descargar el ZIP del curso y corre `./practicar reconstruir` en la carpeta nueva.

Reconstruir no borra tu trabajo: tu carpeta `~/practica` vive aparte, en el volumen de la lección 00. Después entra
como ya sabes, con `./practicar`.

**Dónde se probó esta lección, y dónde falta:**

| Sistema | Estado |
|---|---|
| Mac con Apple silicon, Docker 29.2.1 corriendo con Colima | Probado el 19-nov-2026: `./practicar reconstruir`, `prepara 01` dos veces, cada comando de los cinco pasos, `verifica 01` en rojo y en verde, y las pantallas que ves aquí |
| Linux Mint 22.3, Ubuntu, Mac con Docker Desktop, Windows con Docker Desktop y WSL 2 | Sin probar todavía con esta lección |

**Qué va a coincidir con tu pantalla y qué no.** Coinciden los nombres, los tamaños de los archivos, las fechas y
las rutas. Pueden variar el acomodo en columnas, los colores, el renglón `total` y el tamaño de las carpetas, que
dependen de tu ventana y de dónde guarda Docker el volumen.

---

## Paso 1 · Arma el laberinto y míralo desde arriba

Adentro de tu terminal de práctica, corre:

```bash
prepara 01
```

```text
✅ Laberinto listo en /home/alumno/practica/01 (medí: 10 carpetas y 12 archivos, cada archivo con su tamaño).
   Te dejé la hoja de respuestas, con sus claves vacías: /home/alumno/practica/respuestas/01.md
   Empieza por ahí:  cd ~/practica/01
```

`prepara` es un comando del curso: arma el material de práctica de una lección. Tres cosas sobre él:

- **No tiene azar.** Los nombres, los tamaños y las fechas vienen de una lista fija. En la prueba se armó varias
  veces y quedó idéntico.
- **Toca dos cosas y nada más:** la carpeta `~/practica/01`, y tu hoja de respuestas, que crea solo si no existe.
- **Se puede volver a correr.** Si te pierdes o descompones algo, `prepara 01` deja el laberinto como el primer
  día. Para eso borra el que ya está, así que te pide que escribas `si`. Por eso va aparte de `verifica`, que solo
  mira.

**El laberinto, visto desde arriba.** Este dibujo no es la salida de ningún comando: es lo que vas a tener que
imaginar mientras caminas. En la terminal solo vas a ver la carpeta donde estás parado.

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
`home`, una carpeta `alumno`; y así. Cada nivel hacia la derecha es «adentro de». Subir es ir hacia la raíz; bajar
es entrar a una carpeta que está adentro de la tuya. Los archivos son de relleno: importan su nombre, su tamaño y
su fecha.

Ahora párate en la entrada del laberinto. `cd` es el comando para cambiar de carpeta, y lo ves con calma en el
siguiente paso. Hoy esta línea es una receta dada:

```bash
cd ~/practica/01
```

```text
alumno@practica:~/practica/01$
```

`cd` no contestó nada: como en la lección 00, el silencio quiere decir que salió bien. Lo que cambió es el
indicador: antes decía `~` y ahora dice `~/practica/01`. **El indicador te va diciendo en qué carpeta estás
parado.** Compruébalo con `pwd`:

```text
/home/alumno/practica/01
```

El indicador usa la forma corta, con `~`. `pwd` contesta la forma completa.

**Lo demuestra:** todavía nada. Este paso deja listo el material de los tres ejercicios.

---

## Paso 2 · Camina: `cd` con rutas relativas

`cd` viene de *change directory*, «cambiar de carpeta». Se escribe `cd`, un espacio y la ruta a donde quieres ir.

Para saber a dónde puedes ir necesitas ver qué hay. `ls`, a secas, lista lo que hay en la carpeta actual; lo ves a
fondo en el paso 4.

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
carpeta `pagos` que está aquí, donde estoy parado». Funcionó porque en `01` hay una carpeta `pagos`.

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

**Varios niveles en una sola ruta.** Las carpetas de una ruta se separan con una diagonal. Puedes bajar dos niveles
de un jalón, y subir dos con dos veces `..`:

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

**Un error a propósito.** Estás en `pagos`. Pide una carpeta que no existe ahí:

```bash
cd nada
```

```text
bash: cd: nada: No such file or directory
```

«No existe ese archivo o carpeta.» El mensaje te regresó el indicador, así que no te atrapó. Y el indicador sigue
diciendo `pagos`: **cuando `cd` falla, te quedas donde estabas.** Pasa lo mismo con `cd Bitacoras`: la carpeta se
llama `bitacoras`, y la terminal distingue mayúsculas de minúsculas en los nombres igual que en los comandos.

Todas las rutas de este paso son relativas: **su resultado depende de dónde estás parado.** El mismo `cd pagos` que
funcionó en `01` falla en una carpeta que no tenga una `pagos` adentro.

**Lo demuestra** (objetivo 1.1, primera mitad): tus predicciones de este paso, comparadas con el indicador y con
`pwd`.

---

## Paso 3 · La otra manera: rutas absolutas

Una ruta relativa sirve mientras sepas dónde estás. Hay otra manera de escribir un camino, que no depende de eso.

**Sube hasta arriba.** La raíz se escribe con una sola diagonal:

```bash
cd /
ls
```

```text
bin   dev  home  media  opt   root  sbin  sys  usr
boot  etc  lib   mnt    proc  run   srv   tmp  var
```

El indicador ahora dice `alumno@practica:/$`. Estás en la raíz, y esas son las carpetas de un sistema Linux. Hoy te
importa una, `home`, porque adentro está tu carpeta personal.

**Predice:** estás en la raíz. ¿Qué pasa si corres `cd pagos`, el mismo comando que funcionó en el paso 2?

```text
bash: cd: pagos: No such file or directory
```

Falló, y con razón: en la raíz no hay ninguna carpeta `pagos`.

**Una ruta absoluta empieza con `/`.** Dice el camino completo desde la raíz, carpeta por carpeta. Por eso funciona
igual sin importar dónde estés parado. Esta lleva a las bitácoras:

```bash
cd /home/alumno/practica/01/pagos/bitacoras
```

```text
alumno@practica:~/practica/01/pagos/bitacoras$
```

Léela junto con el dibujo del paso 1: de la raíz a `home`, de `home` a `alumno`, de ahí a `practica`, a `01`, a
`pagos` y a `bitacoras`. Es lo mismo que contesta `pwd` cuando estás ahí: **`pwd` contesta la ruta absoluta de la
carpeta actual.**

**La diagonal del principio cambia todo.** `cd pagos` pide la carpeta `pagos` que está adentro de donde estoy.
`cd /pagos` pide la carpeta `pagos` que está en la raíz. Compruébalo donde sí hay una `pagos`:

```bash
cd ~/practica/01
cd /pagos
```

```text
bash: cd: /pagos: No such file or directory
```

Hay una `pagos` a un paso de ti, pero pediste otra: una que estaría en la raíz, y esa no existe.

**Tu carpeta personal tiene abreviatura: `~`.** Al principio de una ruta, la terminal cambia `~` por
`/home/alumno` antes de correr el comando. Por eso `cd ~/practica/01` y `cd /home/alumno/practica/01` llevan al
mismo lugar, y `cd` a secas, sin ruta, te lleva a tu carpeta personal. Pero `~` es una abreviatura de la terminal y
no parte del árbol: en otra cuenta, `~` es otra carpeta. Cuando te pidan una ruta absoluta, escríbela desde la
raíz, con `/`.

**Los cuatro puntos de partida,** juntos:

| La ruta empieza con | Sale de | Ejemplo | Depende de dónde estás |
|---|---|---|---|
| `/` | La raíz | `/home/alumno/practica/01` | No |
| `~` | Tu carpeta personal | `~/practica/01` | No |
| `..` | La carpeta de arriba de la actual | `../config` | Sí |
| Un nombre | La carpeta actual | `pagos/bitacoras` | Sí |

**¿Cuál conviene?** La relativa es corta: conviene cuando el lugar está cerca. La absoluta no deja dudas: conviene
cuando le dictas un camino a alguien que no sabe en qué carpeta estás parado, como a Lucía por teléfono.

**Lo demuestra** (objetivo 1.1 completo): ejercicio 1.1, abajo. Y es la mitad del objetivo 1.3.

---

## Paso 4 · Mira lo que hay: `ls` y sus columnas

Ve a la carpeta de las bitácoras, con la ruta que quieras, y lista:

```bash
cd ~/practica/01/pagos/bitacoras
ls
```

```text
abril.log  enero.log  febrero.log  marzo.log  mayo.log  viejas
```

`ls` viene de *list*, «listar». A secas da solo los nombres, en orden alfabético. No da tamaños ni fechas: para eso
están las opciones, las palabras con guion que cambian lo que hace un comando. Hoy son tres.

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
oculto**: `ls` a secas no lo lista. No está protegido ni es secreto; solo no estorba en la lista de diario. `-a`
viene de *all*, «todo». `.pendiente` es un archivo oculto del laberinto. `..` ya lo conoces: es la carpeta de
arriba. Y `.` es la carpeta actual.

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

`-l` viene de *long*, «larga»: un renglón por archivo o carpeta, con sus datos en columnas. Lee el renglón de
`marzo.log` de izquierda a derecha:

| Columna | En `marzo.log` | Qué dice |
|---|---|---|
| **Tipo** | `-` | La primera letra del renglón. Un guion es un archivo normal. Una `d`, de *directory*, es una carpeta: mira el renglón de `viejas`. Hay otros tipos; en este laberinto solo hay estos dos |
| **Permisos** | `rw-r--r--` | Las nueve letras que siguen. Dicen quién puede leer, escribir y ejecutar el archivo. Hoy solo les pones nombre: se leen y se cambian en la lección 09 |
| **Enlaces** | `1` | Cuántos nombres apuntan a ese mismo archivo. En este curso no lo usas |
| **Dueño** | `alumno` | El usuario al que pertenece el archivo. Aquí eres tú |
| **Grupo** | `alumno` | El grupo de usuarios al que pertenece. Vuelve a salir en la lección 09 |
| **Tamaño** | `1200000` | Cuánto mide el archivo, en bytes |
| **Fecha** | `Mar 31  2025` | Cuándo se modificó por última vez. Los meses salen en inglés: `Mar` es marzo y `Apr` es abril |
| **Nombre** | `marzo.log` | Cómo se llama |

Tres detalles:

- **El `total` de arriba no es la suma de la columna de tamaño.** Es el espacio que lo listado ocupa en disco, en
  bloques de 1024 bytes. En este curso no lo usas.
- **El tamaño de una carpeta no es lo que lleva adentro.** `viejas` dice `4096` y adentro tiene un archivo de
  20,000 bytes. Para saber cuánto hay adentro, entras y miras.
- **La fecha cambia de forma según la edad.** Si el archivo se modificó hace más de seis meses, ves el año, como
  aquí. Si es reciente, en lugar del año ves la hora, en hora universal (UTC) y no la de tu reloj. Lo vas a notar
  en el paso 5.

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

Sin letra son bytes. `K` son kilobytes: 1,024 bytes. `M` son megabytes: 1,024 K. `G` son gigabytes: 1,024 M.

**La trampa de esta columna: se lee primero la letra y después el número.** `975K` tiene un número más grande que
`1.2M`, y aun así es el archivo más chico de los dos, porque una M vale más de mil K. Y un límite: `-h` redondea.
`marzo.log` mide 1,200,000 bytes, que son 1.14 M, y `ls` escribe `1.2M`. Cuando necesites el número exacto, quita
la `h`.

### Las opciones se juntan, y `ls` acepta una ruta

Varias opciones de una letra se pueden escribir pegadas: `ls -lah` es lo mismo que `ls -l -a -h`. Córrelo: es la
lista larga, con lo oculto y con tamaños legibles. Ahí `.pendiente` ya tiene su renglón.

Y no hace falta ir a una carpeta para ver qué tiene: `ls` acepta una ruta, relativa o absoluta, igual que `cd`.
`ls -lh ..` te muestra la carpeta de arriba y sigues parado en `bitacoras`. Mirar no te mueve.

### Con Claude Code, en este orden

En este curso un asistente de IA entra de una sola manera: **predices → le pides → lees → corres → explicas.**

1. **Predices.** Con tu `ls -lah` enfrente, anota en tu papel cuál es el archivo más grande de `bitacoras`.
2. **Le pides.** En tu computadora, a Claude Code: «Dame un comando de `ls` que ordene una carpeta por tamaño, del
   más grande al más chico, y explícame cada opción». Claude Code no ve tu laberinto: corre afuera.
3. **Lees.** Antes de correr lo que te dio, busca cada opción en `ls --help`, como en la lección 00.
4. **Corres.** El comando lo escribes tú, en tu terminal de práctica.
5. **Explicas.** Con tus palabras: ¿por qué quedó en ese orden? ¿Coincide con tu predicción?

**Lo demuestra** (objetivo 1.2): ejercicio 1.2, abajo.

---

## Paso 5 · Anota tus respuestas y compruébalas

Hasta aquí todo lo comprobaste tú, con el indicador y con `pwd`. Ahora le toca a `verifica`. Para que pueda revisar
lo que encontraste, lo anotas en tu **hoja de respuestas**, `~/practica/respuestas/01.md`, que `prepara` ya te
dejó. Es texto simple, con cuatro renglones: una clave, dos puntos y tu respuesta.

| Clave | Qué va |
|---|---|
| `mas-grande` | El nombre del archivo más grande del laberinto. Solo el nombre, como lo escribe `ls`, sin su carpeta |
| `oculto` | El nombre del archivo oculto del laberinto. Solo el nombre, con su punto |
| `ruta-absoluta` | La ruta absoluta de la carpeta destino |
| `ruta-relativa` | La ruta relativa a la carpeta destino, saliendo de `bitacoras` |

Corre `verifica 01` antes de anotar nada, para ver de dónde partes. Los dos primeros renglones salen así:

```text
✅ Tu laberinto está completo (medí: las 22 carpetas y archivos de la lista existen en ~/practica/01, ninguno es un enlace y cada archivo tiene su tamaño)
❌ Tu renglón «mas-grande:» está vacío (medí: lo que hay después de los dos puntos). Anótalo con  anota 01 mas-grande  y el nombre del archivo
```

Siguen tres rojos iguales, uno por cada clave que falta. Es normal: la hoja está vacía.

**Cómo se anota.** Todavía no has visto un editor de texto; llega en la lección 07. Por eso el curso trae un
comando, `anota`: escribes el número de la lección, la clave y tu respuesta. Supón que crees que el más grande es
`enero.log`:

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

`anota` no sabe si acertaste: solo escribe. Eso lo dice `verifica 01`, que ahora cambia su segundo renglón:

```text
❌ «enero.log» no es el archivo más grande del laberinto (medí: comparé sus bytes contra los de los demás archivos de la lista; hay uno con más). Vuelve a leer la columna del tamaño con  ls -lh : fíjate en la letra, no solo en el número
```

Así se lee un rojo: dice qué anotaste, qué se midió y con qué comando volver a mirar. No dice la respuesta. Para
corregir, anotas otra vez la misma clave: `anota` cambia ese renglón. Y `anota 01`, sin nada más, te muestra la
hoja.

**Las dos rutas del ejercicio 1.3.** La carpeta destino es `guardia`: está adentro de `entregas`, que está adentro
de `pagos`. Búscala en el dibujo del paso 1. Tienes que llegar a ella de dos maneras: con una **ruta absoluta**, y
con una **ruta relativa** que salga de la carpeta `bitacoras`.

Antes de anotar cada ruta, recórrela: párate donde corresponde, entra con `cd` y pregunta con `pwd` si llegaste a
`guardia`. Cuando una ruta te lleve, anótala tal como la escribiste. `verifica` hace la misma prueba que tú.

Cuando tengas los cuatro renglones, corre `verifica 01`. Corrige y repite hasta ver esto al final:

```text
✅ Lección 01: todo en verde.
```

Ya que la hoja tiene tus respuestas, mira su renglón con `ls -l ~/practica/respuestas`:

```text
total 4
-rw-r--r-- 1 alumno alumno 139 Nov 19 07:35 01.md
```

Es el detalle que quedó pendiente en el paso 4: tu hoja es reciente, y en lugar del año trae la hora. El tamaño y la
fecha de la tuya van a ser otros.

**Lo demuestra** (objetivos 1.2 y 1.3): `verifica 01` todo en verde.

---

## Tus ejercicios

El ejercicio 1.1 se hace **sin IA** y sin correr nada antes de predecir: solo tu papel y el dibujo del paso 1.

| # | Haces | Está hecho cuando |
|---|---|---|
| 1.1 | Predices por escrito en qué carpeta quedas después de cada uno de los diez `cd` de abajo, y luego los corres | Tienes tus diez predicciones y las diez salidas de `pwd`, y sabes explicar cada una que no coincidió |
| 1.2 | En `bitacoras`: encuentras el archivo más grande y el oculto y los anotas en tu hoja; y llenas en tu papel la tabla de columnas de abajo | `verifica 01` marca en verde `mas-grande` y `oculto`, y tu tabla coincide con «Compara tus respuestas» |
| 1.3 | Llegas a la carpeta `guardia` con una ruta absoluta y con una relativa que sale de `bitacoras`, y anotas las dos | `verifica 01` sale todo en verde |

**Los diez `cd` del 1.1.** Primero escribe las diez predicciones, con la ruta completa que crees que va a contestar
`pwd`. No corras nada todavía. Si crees que un `cd` va a fallar, escríbelo así, y escribe dónde te quedas. Empiezas
parado en `~/practica/01`, y cada comando sale de donde te dejó el anterior.

| # | Comando | # | Comando |
|---|---|---|---|
| 1 | `cd pagos/config` | 6 | `cd pagos` |
| 2 | `cd plantillas` | 7 | `cd home` |
| 3 | `cd ../../bitacoras/viejas` | 8 | `cd ~` |
| 4 | `cd ../..` | 9 | `cd ../..` |
| 5 | `cd /` | 10 | `cd ..` |

Cuando tengas las diez, ve a la entrada con `cd ~/practica/01` y corre los comandos uno por uno, con un `pwd`
después de cada uno. No hay un número para pasar: lo que cuenta es que, en cada una que fallaste, puedas decir qué
leíste mal: si la ruta era absoluta o relativa, o en qué carpeta estabas parado.

**La tabla de columnas del 1.2.** Corre `ls -la` en `bitacoras` y llena en tu papel estos dos renglones. Esta parte
no la revisa `verifica`: la comparas tú.

| Nombre | ¿Archivo o carpeta? | Permisos (las nueve letras) | Dueño | Tamaño en bytes | Fecha |
|---|---|---|---|---|---|
| `.pendiente` | | | | | |
| `viejas` | | | | | |

**Tiempo (estimado):** de 60 a 90 minutos. Es una estimación: se ajusta con el primer grupo que tome el curso.

---

## Si quieres más

*Opcional. Nada de este recuadro se evalúa ni hace falta para los ejercicios.*

- **`cd -` te regresa a la carpeta anterior.** Es un guion después de `cd`. Sirve para ir y venir entre dos
  lugares, y contesta a dónde te regresó.
- **La tecla Tab completa nombres.** En `pagos`, escribe `cd bit` y presiona Tab: la terminal completa
  `bitacoras/`, porque es la única carpeta de ahí que empieza así. Menos letras, menos errores de dedo.
- **`ls -lhS` ordena por tamaño.** Es una respuesta posible a lo que le pediste a Claude Code. En `ls --help`, la
  `S` dice `Sort by file size, largest first.` Así salió en `bitacoras`:

```text
total 2.2M
-rw-r--r-- 1 alumno alumno 1.2M Mar 31  2025 marzo.log
-rw-r--r-- 1 alumno alumno 975K Apr 30  2025 abril.log
-rw-r--r-- 1 alumno alumno 9.6K Feb 28  2025 febrero.log
drwxr-xr-x 2 alumno alumno 4.0K Jan  2  2025 viejas
-rw-r--r-- 1 alumno alumno  980 Jan 31  2025 enero.log
-rw-r--r-- 1 alumno alumno    0 May 31  2025 mayo.log
```

  `viejas` queda en medio porque su renglón dice `4.0K`, el tamaño de la carpeta y no de lo que lleva adentro. Y el
  archivo oculto no aparece, porque el comando no lleva `-a`.
- **El `ls` de esta terminal y el de otros lados.** Aquí `ls` viene de `rust-coreutils` y no del proyecto GNU,
  como viste en la lección 00. En las formas de esta lección la salida fue idéntica a la del `ls` de GNU, que
  Ubuntu deja instalado como `gnuls`: compara `gnuls -lah` con tu `ls -lah`. Lo que cambia es el texto de
  `ls --help` y de `man ls`.

---

## ¿No te corre?

| Ves | Causa probable | Qué haces |
|---|---|---|
| `bash: prepara: command not found`, o `Todavía no hay verifica para la lección «01».` | Tu imagen es de antes de esta lección | Sal con `exit` y corre `./practicar reconstruir` |
| `Ya existe /home/alumno/practica/01.` y una pregunta | Ya habías armado el laberinto, y armarlo de nuevo lo borra | Para empezar de cero, escribe `si`. Si no, presiona Enter: no se borra nada |
| Después de `prepara 01`, `ls` no muestra nada, o sale `error retrieving current directory` | Estabas parado adentro del laberinto cuando se volvió a armar: esa carpeta ya no existe | `cd ~/practica/01` |
| `❌ No preparo nada: … es un enlace…` | En tu carpeta de práctica hay un enlace donde debería ir una carpeta tuya | Desde tu computadora, `./practicar desde-cero` |
| `bash: cd: …: No such file or directory` | La carpeta no existe con ese nombre en el lugar donde la pediste | `pwd` para saber dónde estás y `ls` para ver qué hay. Revisa las mayúsculas y si la ruta debía empezar con `/` |
| `❌ Tu ruta relativa … no existe saliendo de …` | La probaste parado en otra carpeta | Ve a `bitacoras` y prueba ahí tu ruta con `cd` y `pwd` |
| `❌ Tu laberinto no está completo…` | Borraste o cambiaste algo del laberinto | `prepara 01`. Tu hoja de respuestas no se pierde |

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

**Ejercicio 1.2, el más grande y el oculto.** `verifica 01` te dice si acertaste. El razonamiento que debe haber
detrás:

- **El más grande** se lee en la columna del tamaño de `ls -lh`, primero la letra y después el número: el único
  renglón con `M` les gana a los que traen `K`. Sin `-h` se llega a lo mismo contando cifras.
- **El oculto** solo aparece con `-a`. Es el único nombre que empieza con punto y no es ni `.` ni `..`. En tu hoja
  va con todo y su punto: el punto es parte del nombre.

**Ejercicio 1.2, la tabla de columnas.**

| Nombre | ¿Archivo o carpeta? | Permisos | Dueño | Tamaño en bytes | Fecha |
|---|---|---|---|---|---|
| `.pendiente` | Archivo: el renglón empieza con un guion | `rw-r--r--` | `alumno` | `48000` | `May 12  2025`: 12 de mayo de 2025 |
| `viejas` | Carpeta: el renglón empieza con `d` | `rwxr-xr-x` | `alumno` | `4096` en la prueba; en tu computadora puede ser otro | `Jan  2  2025`: 2 de enero de 2025 |

Si en el tamaño de `.pendiente` escribiste `47K`, corriste `ls -lah`: es el mismo archivo, con el tamaño abreviado.
Si en dueño dudaste entre las dos columnas que dicen `alumno`: la primera es el dueño y la segunda es el grupo.

**Ejercicio 1.3.** Aquí no va la respuesta escrita: el control es `verifica 01`. Para saber si vas bien:

- Tu ruta absoluta empieza con `/` y es, letra por letra, lo que contesta `pwd` cuando estás parado en `guardia`.
  Después de la raíz lleva siete nombres.
- Tu ruta relativa no empieza con `/` ni con `~`. La más corta sube un nivel y baja dos. Hay otras más largas que
  también llegan, y `verifica` las da por buenas: lo que mide es que el camino escrito, carpeta por carpeta, lleve a
  `guardia`.

**Para cerrar.**

1. Relativa.
2. Absoluta.
3. Relativa.
4. Absoluta: es la raíz.
5. Relativa.
6. `../semana-2`: subes a `respaldos` y bajas a `semana-2`.
7. Todavía no: `ls` a secas no lista lo oculto. Corres `ls -a`. Si solo salen `.` y `..`, ahora sí está vacía.
8. El de `3.1M`. Se lee primero la letra: una M vale más de mil K.
9. Que es una carpeta. Del contenido no dice nada: ese número es el tamaño de la carpeta misma.
10. Una absoluta. Una relativa solo sirve si los dos saben en qué carpeta está parado él; la absoluta sale de la
    raíz.

---

## Fuentes de la lección

Abiertas el 19-nov-2026. Todas están en inglés.

- The Open Group. *POSIX.1-2024, Base Definitions, capítulo 3* (3.2 «Absolute Pathname», 3.311 «Relative Pathname» y 3.111 «Dot-Dot»). https://pubs.opengroup.org/onlinepubs/9799919799/basedefs/V1_chap03.html
- The Open Group. *POSIX.1-2024, utilidad cd* (`cd` sin ruta usa la carpeta personal; `cd -` va a la carpeta anterior y la escribe). https://pubs.opengroup.org/onlinepubs/9799919799/utilities/cd.html
- The Open Group. *POSIX.1-2024, utilidad ls* (el orden de las columnas de `-l`; hora o año según los últimos seis meses). https://pubs.opengroup.org/onlinepubs/9799919799/utilities/ls.html
- GNU. *Bash Reference Manual, «Bourne Shell Builtins»* (`cd` y `pwd`; `pwd` escribe la ruta absoluta de la carpeta actual). https://www.gnu.org/software/bash/manual/html_node/Bourne-Shell-Builtins.html
- GNU. *Bash Reference Manual, «Tilde Expansion»* (`~` se cambia por la carpeta personal). https://www.gnu.org/software/bash/manual/html_node/Tilde-Expansion.html
- GNU. *Coreutils, «What information is listed»* (las columnas de `-l`, las letras del tipo, el renglón `total` y `-h` con potencias de 1024). https://www.gnu.org/software/coreutils/manual/html_node/What-information-is-listed.html
- GNU. *Coreutils, «Which files are listed»* (`ls` no lista los nombres que empiezan con punto; `-a` sí). https://www.gnu.org/software/coreutils/manual/html_node/Which-files-are-listed.html
- Ubuntu Manpages. *ls(1)*, de Ubuntu 26.04 (el `ls` de `rust-coreutils` 0.10.0: `-a`, `-l`, `-h`, `-S`). https://manpages.ubuntu.com/manpages/resolute/en/man1/ls.1.html
- Ubuntu Manpages. *hier(7)*, de Ubuntu 26.04 (las carpetas de la raíz de un sistema Linux; `/home`). https://manpages.ubuntu.com/manpages/resolute/en/man7/hier.7.html
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, sección «rust-coreutils» (los comandos de GNU siguen instalados con el prefijo `gnu`, como `gnuls`). https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- Medición propia, 19-nov-2026, en una Mac con Apple silicon (Docker 29.2.1 con Colima, linux/arm64), con la imagen
  del curso sobre Ubuntu 26.04.1: `ls` de uutils coreutils 0.10.0, `gnuls` de GNU coreutils 9.7 y bash 5.3.9. De ahí
  salen las pantallas, la comparación de `ls` contra `gnuls`, el redondeo de `-h`, la tecla Tab y el tiempo de
  `./practicar reconstruir`. En Linux esta lección todavía no se prueba.
