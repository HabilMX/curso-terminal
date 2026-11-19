# Lección 02 — Crear, copiar, mover y borrar sin miedo

**Por Dorian Chávez, fundador de Hábil y arquitecto de integración.**

**Antes de leer, cuatro palabras.** Una línea cada una; las demás llegan en el paso donde se usan.

- **Origen y destino:** al copiar o mover, el origen es lo que ya tienes y el destino es a dónde va. En un comando,
  el origen se escribe primero y el destino al final.
- **Sobrescribir:** escribir encima de un archivo que ya existía. Lo que tenía antes se pierde.
- **Papelera:** el lugar donde tu computadora guarda lo que borraste, por si te arrepientes. En la terminal no hay.
- **Patrón:** un nombre con comodines, como `*.log`, que atrapa varios nombres de un jalón. El `*` quiere decir
  «cualquier cosa».

**Qué te llevas:** creas carpetas y archivos con `mkdir` y `touch`; copias, mueves y renombras con `cp` y `mv`, y
sabes qué pasa cuando el destino ya existe; borras con `rm` y `rmdir`, viendo antes qué se va a ir; y puedes
explicar por qué una variable vacía convierte un borrado chico en uno enorme.

**Idea nueva de esta lección (la única que se evalúa):** en la terminal no hay papelera. Lo que `rm` borra no se
recupera, y `cp` y `mv` sobrescriben sin preguntar. Por eso **se piensa antes de borrar**: primero se mira qué se va
a ir, y después se borra.

**En qué orden va:** armas un taller de práctica (paso 1), creas (paso 2), copias, mueves y renombras (paso 3),
borras con cuidado (paso 4), ves el peligro de la variable vacía sin correrlo (paso 5) y al final compruebas todo con
`verifica` (paso 6).

---

## La escena

*Escenario inventado; las pantallas que aparecen en la lección sí son reales.*

Miércoles, 00:07. Lucía encontró la bitácora de pagos. La última línea dice:

```text
2025-06-02 00:09 pagos: disco lleno, no puedo escribir
```

Del otro lado del teléfono le dan tres órdenes:

—Antes de tocar nada, respalda la configuración. Luego deja el reporte de junio en su carpeta. Y borra los
temporales de la caché, que son los que llenaron el disco.

En la caché hay archivos que terminan en `.tmp`, y también uno, `cache-indice.txt`, sin el cual el servicio no
vuelve a arrancar. Lucía sabe una cosa del comando para borrar: no pregunta, y no hay papelera de donde sacar lo que
se lleve.

Esta lección es esa media hora, en un taller hecho para equivocarse sin consecuencias.

---

## Antes de empezar

**Qué necesitas:** la lección 01 hecha; el material del curso actualizado; y papel y lápiz para tus predicciones.

**Actualiza tu terminal de práctica.** Igual que en la lección 01, desde tu computadora, en la carpeta del curso:

```bash
git pull
./practicar reconstruir
```

Con eso basta para recibir esta lección: `./practicar reconstruir` reutiliza Ubuntu y los paquetes ya construidos y
solo mete los archivos nuevos. En la Mac de la prueba tardó menos de dos segundos y terminó con `✅ Imagen lista.`
Tu carpeta `~/practica` no se toca. Después entra con `./practicar`.

**Dónde se probó esta lección:**

| Sistema | Estado |
|---|---|
| Mac con Apple silicon, Docker 29.2.1 corriendo con Colima | Probado el 19-nov-2026: `./practicar reconstruir`, `prepara 02` dos veces con el mismo resultado, cada comando de los pasos 1 a 6, y `verifica 02` en rojo y en verde |
| Linux Mint 22.3 (x86_64), Docker Engine 29.10.0 | Probado el 19-nov-2026: `./practicar reconstruir`, `prepara 02` dos veces con el mismo resultado que en la Mac, los mensajes de `mkdir`, `touch` y `rmdir`, las preguntas de `cp -i`, `mv -i`, `rm -i` y `rm -I`, la lección completa como alumno, y `verifica 02` en verde y en rojo con un enlace duro |
| Ubuntu, Mac con Docker Desktop, Windows con Docker Desktop y WSL 2 | Sin probar todavía con esta lección |

**Qué va a coincidir con tu pantalla y qué no.** Coinciden los nombres, los contenidos, las fechas del taller y los
mensajes de los comandos. Cambian la hora de lo que tú crees o toques, el acomodo en columnas y los colores.

---

## Paso 1 · Arma el taller

Adentro de tu terminal de práctica:

```bash
prepara 02
```

```text
✅ Taller listo en /home/alumno/practica/02 (medí: 12 carpetas y 22 archivos, cada archivo con su contenido).
   Te dejé la hoja de respuestas, con su clave vacía: /home/alumno/practica/respuestas/02.md
   Empieza por ahí:  cd ~/practica/02
```

`prepara 02` funciona como el de la 01: no tiene azar, toca solo `~/practica/02` y tu hoja de respuestas, y para
volver a armar el taller lo borra, así que te pide escribir `si`. Cada archivo trae un renglón de texto distinto:
por el contenido se nota si una copia es la buena o si algo se sobrescribió.

**El taller, visto desde arriba:**

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

Las demostraciones van en `ensayo`; los ejercicios, en lo demás.

**Lo demuestra:** todavía nada. Este paso deja listo el material.

---

## Paso 2 · Crear: `mkdir` y `touch`

Entra al ensayo:

```bash
cd ~/practica/02/ensayo
ls
```

```text
caja  dos.txt  tres.log  uno.txt
```

### `mkdir`: una carpeta nueva

`mkdir` viene de *make directory*, «crear carpeta». Se escribe `mkdir` y el nombre:

```bash
mkdir nueva
```

Silencio: salió bien. **Predice:** ¿qué pasa si corres la misma línea otra vez?

```text
mkdir: nueva: File exists
```

«Ya existe.» `mkdir` no sobrescribe una carpeta: se niega, y nada se pierde.

**Varias carpetas, una adentro de otra.** **Predice:** `uno` no existe. ¿Qué pasa con `mkdir uno/dos/tres`?

```text
mkdir: No such file or directory
```

Falló, porque `mkdir` crea la última carpeta de la ruta y espera que las de antes ya existan. Para crear todo el
camino de un jalón está la opción `-p`, de *parents*, «las de arriba también»:

```bash
mkdir -p uno/dos/tres
ls uno/dos
```

```text
tres
```

Con `-p`, si la carpeta ya existía, no se queja.

### `touch`: un archivo vacío

`touch` quiere decir «tocar». Si el archivo no existe, lo crea vacío:

```bash
touch nota.txt
ls -l nota.txt
```

```text
-rw-r--r-- 1 alumno alumno 0 Nov 19 09:28 nota.txt
```

Cero bytes, y la hora de ahora. **Predice:** `uno.txt` ya existe y dice `uno`. Si lo tocas, ¿pierde su contenido?

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

No lo pierde: sobre un archivo que existe, `touch` solo le cambia la fecha a la de ahora. `cat` muestra lo que tiene
un archivo; lo ves a fondo en la lección 03.

### Lo que verías distinto en otro Linux

En esta imagen, `mkdir`, `touch` y `rmdir` vienen de `rust-coreutils`, como el `ls` de la lección 01; `cp`, `mv` y
`rm` siguen siendo de GNU. Lo dicen `--version` y la documentación de Ubuntu. Lo que se nota es el texto de dos
errores de `mkdir`. El de GNU sigue instalado como `gnumkdir`, y así contestó:

| Comando | `mkdir` de esta imagen | `gnumkdir`, el de GNU |
|---|---|---|
| Crear `nueva`, que ya existe | `mkdir: nueva: File exists` | `gnumkdir: cannot create directory ‘nueva’: File exists` |
| Crear `uno/dos/tres`, sin `-p` | `mkdir: No such file or directory` | `gnumkdir: cannot create directory ‘uno/dos/tres’: No such file or directory` |

En un Linux con GNU verías el segundo texto, empezando con `mkdir:`. Lo que importa: aquí el error **no dice qué
ruta falló**. Si te pasa con varias carpetas en la misma línea, revisa cada una con `ls`. Los errores de `rmdir` y
de `touch` salieron iguales a los de GNU.

### Tu turno: el árbol de la guardia

Vuelve al taller con `cd ~/practica/02` y crea, tal cual, la carpeta de la guardia de Lucía:

```text
~/practica/02
└── guardia
    └── 2025-06-02
        ├── bitacoras/
        ├── reportes/
        │   └── resumen.txt
        └── pendientes.txt
```

Las que terminan en `/` son carpetas; las otras dos, archivos vacíos. Pista: `mkdir -p` y `touch` aceptan varias
rutas en la misma línea. Al final, `ls guardia/2025-06-02` debe mostrar `bitacoras  pendientes.txt  reportes`.

**Lo demuestra** (objetivo 2.1): la primera marca de `verifica 02`, que revisa el árbol **tal cual**: cada carpeta y
cada archivo, de su tipo, y nada de más. Un error de dedo como `reprotes` sale en rojo.

---

## Paso 3 · Copiar, mover y renombrar: `cp` y `mv`

Vuelve al ensayo con `cd ~/practica/02/ensayo`.

### `cp`: copiar

`cp` viene de *copy*. Se escribe `cp`, el origen y el destino. Mira primero qué dice `dos.txt`:

```bash
cat dos.txt
```

```text
dos
```

**Predice antes de correr la siguiente línea.** `dos.txt` ya existe. Si copias `uno.txt` encima, ¿te pregunta algo?
¿Qué dice `dos.txt` después?

```bash
cp uno.txt dos.txt
cat dos.txt
```

```text
uno
```

No preguntó nada. **Sobrescribió**, en silencio. El `dos` que había adentro ya no existe en ningún lado.

**Para que pregunte: `-i`,** de *interactive*. Pregunta antes de sobrescribir, y si contestas `n`, no copia:

```bash
cp -i tres.log uno.txt
```

```text
cp: overwrite 'uno.txt'? n
```

«¿Sobrescribo `uno.txt`?» Con `n`, `uno.txt` sigue diciendo `uno`. Con `y`, de *yes*, copia.

**Copiar a una carpeta.** Si el destino es una carpeta que ya existe, `cp` deja adentro la copia, con el mismo
nombre:

```bash
cp caja/adentro.txt nueva
ls nueva
```

```text
adentro.txt
```

### `cp -r`: copiar una carpeta entera

**Predice:** ¿`cp` copia una carpeta igual que un archivo?

```bash
cp caja otra
```

```text
cp: -r not specified; omitting directory 'caja'
```

«No pusiste `-r`; me salto `caja`.» Para copiar una carpeta con todo lo que tiene va `-r`, de *recursive*, «hasta el
fondo»:

```bash
cp -r caja otra
ls otra
```

```text
adentro.txt
```

**La trampa de `cp -r`.** **Predice:** corres exactamente la misma línea otra vez. `otra` ya existe. ¿Qué queda?

```bash
cp -r caja otra
ls otra
```

```text
adentro.txt  caja
```

La segunda vez el destino ya era una carpeta, así que `cp` metió la copia **adentro**: hay una `caja` dentro de
`otra`. La misma línea hace dos cosas según exista o no el destino. Antes de copiar una carpeta, mira con `ls` si el
destino ya está.

### `mv`: mover y renombrar

`mv` viene de *move*. Funciona como `cp`, con una diferencia: **el origen desaparece**.

**Renombrar es mover en el mismo lugar:**

```bash
mv uno.txt primero.txt
ls
```

```text
caja  dos.txt  nota.txt  nueva  otra  primero.txt  tres.log  uno
```

Ya no hay `uno.txt`. Es el mismo archivo, con otro nombre.

**Predice:** `dos.txt` existe. ¿Qué pasa si mueves `tres.log` encima?

```bash
mv tres.log dos.txt
cat dos.txt
```

```text
tres
```

Igual que `cp`: **sobrescribió sin preguntar**, y `tres.log` ya no existe. `mv -i` pregunta antes:

```bash
mv -i primero.txt dos.txt
```

```text
mv: overwrite 'dos.txt'? n
```

Y si el destino es una carpeta, `mv` mete el archivo adentro, como `cp`:

```bash
mv nota.txt nueva
ls nueva
```

```text
adentro.txt  nota.txt
```

**En resumen:**

| Comando | El origen | Si el destino es un archivo que existe | Si el destino es una carpeta |
|---|---|---|---|
| `cp` | Se queda | Lo sobrescribe sin preguntar | Copia adentro |
| `cp -r` | Se queda | Igual que `cp` | Copia adentro, también la segunda vez |
| `mv` | Desaparece | Lo sobrescribe sin preguntar | Mueve adentro |
| `cp -i`, `mv -i` | Igual | Pregunta antes | Igual |

### Tu turno: el respaldo y el reporte

Vuelve a `~/practica/02`. Son cuatro tareas. **Antes de cada una, escribe en tu papel qué esperas que pase**; las
tres predicciones marcadas se comparan al final, en «Compara tus respuestas».

1. **Copia** `config/pagos.conf` encima de `respaldo/pagos.conf`. Mira antes qué dice el respaldo con `cat`.
   *Predicción A:* ¿te va a preguntar? ¿Qué va a decir `respaldo/pagos.conf` al final?
2. **Copia** la carpeta `config/plantillas` entera a `respaldo/plantillas`, **una sola vez**.
   *Predicción B:* si la copias dos veces con la misma línea, ¿qué queda en `respaldo/plantillas`?
3. **Renombra** `bitacoras/hoy.log` a `bitacoras/2025-06-02.log`.
4. **Mueve** `entrada/reporte-junio.txt` a `reportes`, donde ya hay un `reporte-junio.txt`.
   *Predicción C:* ¿qué le pasa al borrador viejo de `reportes`? ¿Y a la carpeta `entrada`?

Los comandos los armas tú, con lo que viste en el ensayo.

**Con Claude Code.** En este curso un asistente de IA entra de una sola manera: predices, le pides, lees, corres y
explicas. Para borrar hay una regla más: nunca le pides el comando; le pides que te explique el tuyo. El ejercicio
con Claude Code de esta lección, sobre `mv -n`, está en «Si quieres más».

**Lo demuestra** (objetivo 2.2): las marcas dos a cinco de `verifica 02`, y tus predicciones A, B y C, que comparas
tú. `verifica` compara **contenidos**, no solo nombres: sabe si el respaldo trae la versión buena o la vieja, y si la
copia de `plantillas` tiene lo mismo que el original.

---

## Paso 4 · Borrar: `rm` y `rmdir`

Antes de escribir un solo `rm`, lo que hace distinta a esta parte:

- **No hay papelera.** `rm` no manda a ningún lado: borra.
- **En este taller, `rm` normalmente no pregunta:** los archivos son tuyos y se pueden escribir. En otros casos el
  `rm` de GNU puede preguntar, pero no dependas de esa pregunta: mira primero con `ls`, y usa `rm -i` cuando quieras
  que confirme.
- **Todo comando que borra lo escribes tú, a mano**, también los ejemplos de este paso: ni copiado de la lección ni
  pedido a una IA.

Sigue en el ensayo, `cd ~/practica/02/ensayo`.

### `rm`: borrar un archivo

`rm` viene de *remove*, «quitar»:

```bash
rm primero.txt
ls
```

```text
caja  dos.txt  nueva  otra  uno
```

Sin preguntar y sin mensaje. `primero.txt` ya no existe.

**Predice:** ¿`rm` borra una carpeta?

```bash
rm caja
```

```text
rm: cannot remove 'caja': Is a directory
```

«No puedo: es una carpeta.» Así viene por omisión: `rm` solo borra archivos.

### `rmdir` y `rm -r`: borrar carpetas

`rmdir` viene de *remove directory*. **Predice:** `caja` tiene un archivo adentro. ¿La borra?

```bash
rmdir caja
```

```text
rmdir: failed to remove 'caja': Directory not empty
```

No. `rmdir` **solo borra carpetas vacías**, y eso lo hace seguro: si tenía algo adentro, se niega. Con una vacía sí
funciona: `rmdir uno/dos/tres` borra la `tres` que creaste en el paso 2, en silencio.

Para borrar una carpeta **con todo lo que tiene adentro** está `rm -r`, la misma `-r` de `cp`:

```bash
rm -r caja
```

Sin preguntar, `caja` y lo que tenía se fueron. `rm -r` es el comando más peligroso de esta lección: no se fija
cuánto hay adentro.

### `rm -i`: que pregunte

```bash
rm -i dos.txt
```

```text
rm: remove regular file 'dos.txt'? n
```

«¿Borro el archivo normal `dos.txt`?» Con `n`, se queda. Con un archivo vacío dice `regular empty file`.

### Los patrones: ver antes qué se va a ir

Crea tres archivos de prueba:

```bash
touch a.bak b.bak c.bak.txt
```

**Predice:** ¿cuáles atrapa el patrón `*.bak`? El `*` es «cualquier cosa», así que `*.bak` es «cualquier nombre que
termine en `.bak`».

```bash
ls *.bak
```

```text
a.bak  b.bak
```

`c.bak.txt` no cayó: termina en `.txt`. **Un patrón es exacto**, y por eso hay que mirarlo antes de usarlo.

**El patrón no lo lee `ls`: lo lee la terminal.** Antes de correr el comando, bash cambia `*.bak` por la lista de
nombres que atrapa, y `ls` recibe esa lista. `echo`, que repite lo que le escribas, lo deja ver:

```bash
echo *.bak
```

```text
a.bak b.bak
```

Eso es exactamente lo que recibiría `rm *.bak`: `rm a.bak b.bak`. **De ahí sale el hábito de esta lección:**

1. Escribe el patrón con `ls` y lee la lista.
2. Si es justo lo que quieres borrar, escribe `rm` con **el mismo patrón**, letra por letra.
3. Si la lista trae algo de más, cambia el patrón y vuelve al 1.

Y cuando el patrón no atrapa nada, bash lo deja tal cual:

```bash
ls *.zip
```

```text
ls: cannot access '*.zip': No such file or directory
```

`echo *.zip` contesta `*.zip`. No hay archivos `.zip`: `ls` buscó uno que se llamara, literal, `*.zip`.

**Con `-i`, el patrón pregunta uno por uno.** Escríbelo tú: `rm -i *.bak`. Contesta `y` al primero y `n` al
segundo:

```text
rm: remove regular empty file 'a.bak'? y
rm: remove regular empty file 'b.bak'? n
```

Con trescientos archivos, el hábito de arriba vale más que `-i`.

### Tu turno, sin IA: la limpieza

Este ejercicio se hace **sin IA y sin copiar comandos**: ni de esta lección ni de ningún otro lado. Vas a
`~/practica/02/limpieza` y haces lo que le pidieron a Lucía:

1. **Borra los archivos cuyo nombre termina en `.tmp`, y nada más.** Primero escribe un patrón con `ls` y lee lo
   que atrapa. Ojo con los vecinos: `cache-indice.txt` y `tmp-notas.txt` se quedan.
2. **Anota tu patrón** con `anota`, **entre comillas simples**. Para un patrón `ejemplo*` sería
   `anota 02 patron 'ejemplo*'`. Sin comillas, la terminal cambiaría tu patrón por los nombres que atrapa antes de
   que `anota` lo viera.
3. **Borra los `.tmp`** con un `rm` que escribes tú, con el mismo patrón.
4. **Borra la carpeta `sesiones-viejas`**, con todo lo que tiene, y **la carpeta `vacia`**. Empieza con `rmdir` en
   las dos y lee lo que contesta cada una: te dice cuál necesita otra cosa. `sesiones-activas` se queda.
5. Corre `verifica 02`.

Si borras algo de más **en este taller**, no pasa nada: `prepara 02` lo restaura. Úsalo para practicar la pausa de
mirar antes de borrar.

**Lo demuestra** (objetivo 2.3, primera parte): las marcas seis a ocho de `verifica 02`. La seis prueba tu patrón
contra los ocho nombres que `prepara` puso en `limpieza`. La siete revisa que lo pedido ya no exista. La ocho, que
**todo lo demás siga ahí, con su contenido**. Esas dos importan igual: borrar de menos se arregla; borrar de más, no.

---

## Paso 5 · La variable vacía, vista solo con `echo`

⛔ **En este paso ningún `rm` se corre.** Todo se ve con `echo` adelante, que solo escribe el comando en la pantalla.
Si tecleas una de estas líneas sin el `echo`, no estás haciendo el ejercicio.

### El borrado que crece solo

Los guiones de mantenimiento guardan la carpeta que limpian en una **variable**, `CARPETA` (lo mínimo, en la caja
al final de este paso), y borran con una línea así: `rm -rf "$CARPETA/"`. `-f`, de *force*, le dice a `rm` que no
pregunte y que no se queje si algo no existe.

**Predice:** si `CARPETA` está vacía, ¿qué pide borrar esa línea? Escribe esto, con el `echo`:

```bash
CARPETA=
echo rm -rf "$CARPETA/"
```

```text
rm -rf /
```

La variable vacía desapareció, y la diagonal que iba después quedó sola. **`/` es la raíz: todo el sistema.** Un
guion que debía borrar una carpeta de práctica pide borrar todo. Si al final de la línea hay un `*`, es peor:

```bash
echo rm -rf "$CARPETA"/*
```

```text
rm -rf /bin /boot /dev /etc /home /lib /media /mnt /opt /proc /root /run /sbin /srv /sys /tmp /usr /var
```

Cada carpeta de la raíz. Según su manual, el `rm` de GNU se niega a borrar `/` tal cual
(`--preserve-root`); con el `*` ya no recibe `/`, sino esta lista. No lo probamos ni lo vas a probar.

**Un caso reportado.** En enero de 2015, el issue público #3671 de Steam para Linux describió una pérdida de
archivos tras ejecutar `steam.sh`. Comentarios del hilo señalaron `rm -rf "$STEAMROOT/"*` y el riesgo si `STEAMROOT`
quedaba vacío. Es una razón para validar variables; no hace falta que pruebes el incidente para aprender la regla.

### Por qué no se prueba, ni en el contenedor

**El contenedor es desechable. Tu carpeta `~/practica` no:** vive en un volumen que sobrevive al contenedor. Mira de
quién es cada cosa:

```bash
ls -ld / /usr /etc ~/practica
```

```text
drwxr-xr-x 1 root   root   4096 Nov 19 09:28 /
drwxr-xr-x 1 root   root   4096 Nov 19 09:28 /etc
drwxr-xr-x 4 alumno alumno 4096 Nov 19 09:28 /home/alumno/practica
drwxr-xr-x 1 root   root   4096 Nov  6 10:27 /usr
```

Las carpetas del sistema son de `root`, el administrador: `touch /usr/prueba` contesta `Permission denied`. Pero
`~/practica` es tuya, y lo tuyo sí lo puedes borrar. Tus respuestas no vuelven con `./practicar`.

### El freno: `${CARPETA:?}`

Los guiones se protegen con un freno que tiene nombre: `${CARPETA:?}`. Si la variable está vacía, la terminal se
niega a correr la línea. Con `echo` adelante, como siempre en este paso:

```bash
CARPETA=
echo rm -rf "${CARPETA:?}/"
```

```text
bash: CARPETA: parameter null or not set
```

«La variable está vacía o no existe.» Y el `echo` **no se corrió**: según el manual de bash, la terminal cancela el
comando, y con un `rm` tampoco correría. Para ti basta con esto: **si un `rm` lleva una variable,
la variable va como `${NOMBRE:?}`**.

> **Lo mínimo de una variable.** `CARPETA=ensayo` guarda `ensayo` con el nombre `CARPETA`: todo pegado, sin
> espacios. Con `$` adelante, `$CARPETA`, la terminal cambia el nombre por su valor antes de correr el comando, igual
> que cambia un patrón por la lista. Una variable vacía, o que nunca guardó nada, vale nada, **sin dar error**. Este
> curso no va más allá de eso.

**Lo demuestra** (objetivo 2.3, segunda parte): tu explicación de las preguntas 9 y 10 de «Para cerrar», comparada
con «Compara tus respuestas».

---

## Paso 6 · Comprueba con `verifica 02`

Corre `verifica 02` cuando quieras, desde cualquier carpeta. Recién armado el taller, siete marcas salen en rojo y
una en verde: la última, porque todavía no has borrado nada que debía quedarse.

Así se lee un rojo de la marca del patrón, con el `ejemplo*` que se usó arriba como muestra:

```text
❌ 2.3 · Tu patrón «ejemplo*» deja fuera «cache-01.tmp», que sí se borra (medí: probé tu patrón contra los 8 nombres que prepara puso en limpieza)
```

Dice qué anotaste, qué midió y qué falló, pero no cuál es el patrón bueno.

**Lo que no lo engaña:** un enlace, el atajo de la lección 01. Un enlace a `config/pagos.conf` «dice» lo mismo que
una copia, pero no es un respaldo: si el original se pierde, el enlace no tiene nada. `verifica 02` revisa la ruta
real de cada cosa, y un enlace en lugar de un archivo o de una carpeta sale en rojo. Tampoco lo engaña un **enlace
duro** (`ln`): un segundo nombre para el mismo archivo, que parece un archivo normal. Si cambias uno, cambia el
otro. `verifica` lo nota porque los dos nombres comparten el *inode*, el número con el que el disco conoce al
archivo.

Corrige y repite hasta ver esto al final:

```text
✅ Lección 02: todo en verde.
```

---

## Tus ejercicios

El ejercicio 2.3 se hace **sin IA**, y todos sus comandos de borrar los escribes tú.

| # | Haces | Está hecho cuando |
|---|---|---|
| 2.1 | Creas el árbol de la guardia del paso 2 | `verifica 02` marca en verde el árbol |
| 2.2 | Las cuatro tareas del paso 3, con tus predicciones A, B y C escritas antes | `verifica 02` marca en verde las cuatro, y comparaste tus predicciones con «Compara tus respuestas» |
| 2.3 | La limpieza del paso 4: miras con tu patrón, lo anotas y borras a mano | `verifica 02` sale todo en verde, y contestaste las preguntas 9 y 10 de «Para cerrar» |

**Tiempo (estimado):** de 60 a 90 minutos. Es una estimación: se ajusta con el primer grupo que tome el curso.

---

## Si quieres más

*Opcional. Nada de este recuadro se evalúa ni hace falta para los ejercicios.*

- **`-v` cuenta lo que hizo.** Con `cp -v`, `mv -v`, `rm -v` y `mkdir -v`, cada comando escribe un renglón por cosa
  que tocó. `rm -v` contestó, por ejemplo, `removed 'viaje/vuelta/dos.txt'`.
- **Llaves para varias carpetas.** `mkdir -p viaje/{ida,vuelta}` crea `viaje/ida` y `viaje/vuelta`. Las llaves las
  expande bash, igual que el `*`: compruébalo con `echo viaje/{ida,vuelta}`.
- **Con Claude Code: `mv -n`.** Predices en tu papel qué hace `mv -n`. Le pides a Claude Code, en tu computadora:
  «¿Qué hace `mv -n` y en qué se distingue de `mv -i`?». Lees `-n` en `mv --help` y lo comparas. Corres, en el
  ensayo: `touch a.txt b.txt`, `mv -n a.txt b.txt` y `ls`. Explicas con tus palabras si coincidió. Lo que pasó en la
  prueba está en «Compara tus respuestas».
- **`rm -I` pregunta una sola vez.** Con más de tres archivos, o con `-r`, pregunta antes de empezar: `rm -I x*`
  sobre cuatro archivos contestó `rm: remove 4 arguments?`. Con `n` no borró ninguno.

---

## ¿No te corre?

| Ves | Causa probable | Qué haces |
|---|---|---|
| `bash: prepara: …` o `Todavía no hay prepara para la lección «02».` | Tu imagen es de antes de esta lección | Sal con `exit` y corre `./practicar reconstruir` |
| `Ya existe /home/alumno/practica/02.` y una pregunta | Ya habías armado el taller, y armarlo de nuevo lo borra | Para empezar de cero, escribe `si`. Si no, presiona Enter: no se borra nada |
| `mkdir: No such file or directory` | Falta una carpeta de en medio, y este `mkdir` no dice cuál | Usa `mkdir -p`, o revisa cada tramo de la ruta con `ls` |
| `rmdir: failed to remove …: Directory not empty` | La carpeta tiene algo adentro | Mira qué tiene con `ls -a` antes de decidir |
| `❌ … son nombres, no un patrón` | Anotaste el patrón sin comillas, y la terminal lo cambió por los nombres | `anota 02 patron` y tu patrón entre comillas simples |
| `❌ … Copiaste dos veces` | Corriste el mismo `cp -r` dos veces | Borra `respaldo/plantillas` con cuidado y cópiala una vez, o `prepara 02` |
| `❌ … ya no está, y no se pedía borrarlo` | Tu patrón o tu `rm -r` se llevaron de más | En este taller no pasa nada: `prepara 02` lo restaura, y repites |
| `❌ … es un enlace…` | Hay un atajo donde debía ir un archivo o una carpeta de verdad | Hazlo con `cp` o `mv`, o `prepara 02` |
| `❌ … es un enlace duro…` | Ese archivo es el mismo que el original, con otro nombre | Bórralo con cuidado y cópialo con `cp` |

---

## Para cerrar

**Contesta sin volver a leer.**

**Cinco situaciones: ¿qué pasa?** Pasas con 4 de 5.

1. `cp a.txt b.txt`, y `b.txt` ya existía con otro texto.
2. `mv a.txt docs`, y `docs` es una carpeta que existe.
3. `mkdir x/y`, y `x` no existe.
4. `rmdir fotos`, y `fotos` tiene doce fotos adentro.
5. `touch notas.txt`, y `notas.txt` ya tenía tres páginas de notas.

**Y cinco preguntas:**

6. ¿Qué diferencia hay entre `cp` y `mv` en lo que le pasa al origen?
7. Vas a borrar con `rm *.log`. ¿Qué corres antes, y qué buscas en lo que contesta?
8. Escribiste `anota 02 patron *.tmp`, sin comillas, parado en `limpieza`. ¿Qué se anotó, y por qué?
9. Explica con tus palabras, en tres renglones, por qué `rm -rf "$CARPETA/"` es peligroso si `CARPETA` está vacía.
10. ¿Cómo ves, sin correr nada peligroso, qué borraría esa línea? ¿Y cómo la proteges?

**Lo que sigue:** ya sabes crear, copiar, mover y borrar sin perder lo que no querías. En la lección 03 abres lo
que hay dentro de los archivos: lees bitácoras largas sin abrir un editor.

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

**Predicción A (copiar sobre el respaldo).** No pregunta. Al final `respaldo/pagos.conf` dice
`puerto=8443 version=2025-06-02 (la buena, la de hoy)`, igual que `config/pagos.conf`. La versión del puerto 8080 se
perdió: aquí era lo que se quería, pero con otro archivo podría ser lo único que no querías perder.

**Predicción B (`cp -r` dos veces).** La primera vez crea `respaldo/plantillas` con sus tres archivos. La segunda,
como `respaldo/plantillas` ya existe, mete otra copia adentro: queda `respaldo/plantillas/plantillas`. Así lo
contestó `ls` en la prueba:

```text
'aviso de corte.txt'   aviso.txt   plantillas   recibo.txt
```

Las comillas no son parte del nombre: `ls` las pone porque el nombre lleva espacios.

**Con Claude Code: `mv -n`.** En la prueba, `mv -n a.txt b.txt` no movió nada y no dijo nada: `ls` siguió
mostrando `a.txt  b.txt`. `-n` es «nunca sobrescribas», en silencio; `-i` pregunta. Si Claude Code te dijo otra
cosa, gana lo que viste en tu terminal.

**Predicción C (mover sobre el borrador).** `mv` no pregunta: el borrador viejo de `reportes` se sobrescribe y se
pierde, y en `reportes/reporte-junio.txt` queda la versión nueva. El archivo desaparece de `entrada`, pero la
carpeta `entrada` se queda, vacía: `mv` mueve lo que le pides y nada más.

**Ejercicio 2.3.** El control es `verifica 02`; aquí no va el comando. Para saber si vas bien:

- Tu patrón atrapa los tres `.tmp` y a ningún vecino. `cache*` se lleva `cache-indice.txt`; `*tmp*` se lleva
  `tmp-notas.txt`. El bueno se fija en **cómo termina** el nombre.
- `rmdir` borró `vacia` y se negó con `sesiones-viejas`. Esa necesitaba `rm -r`, y antes la miraste con `ls`.

**Para cerrar.**

1. `b.txt` queda con el texto de `a.txt`, sin preguntar. Lo que tenía antes se pierde.
2. `a.txt` queda dentro de la carpeta: `docs/a.txt`. Ya no está donde estaba.
3. Falla con `No such file or directory`, y en esta imagen sin decir qué ruta. Con `mkdir -p x/y` se crean las dos.
4. Se niega: `Directory not empty`. `rmdir` solo borra carpetas vacías, y nada se pierde.
5. Las notas siguen ahí. `touch` solo le cambia la fecha a la de ahora.
6. Con `cp` el origen se queda y aparece una copia. Con `mv` el origen desaparece: el archivo cambia de lugar o de
   nombre.
7. `ls *.log`, con el mismo patrón. Buscas que traiga solo lo que quieres borrar; si trae de más, cambias el patrón.
8. Los nombres de los tres `.tmp`. Bash cambió `*.tmp` por la lista antes de correr `anota`, igual que con `rm` y
   con `ls`. `verifica` lo detecta y te pide las comillas.
9. Algo así: si `CARPETA` está vacía, la terminal la cambia por nada y la línea queda `rm -rf /`, que pide borrar la
   raíz, todo el sistema. `-r` entra a todas las carpetas y `-f` no pregunta. Con un `*` al final, recibe una por una
   las carpetas de la raíz, y `--preserve-root` ya no lo detiene.
10. Lo ves con `echo` adelante: `echo rm -rf "$CARPETA/"` escribe el comando sin correrlo. Lo proteges con
    `"${CARPETA:?}/"`: si la variable está vacía, bash cancela el comando antes de correrlo.

---

## Fuentes de la lección

Abiertas el 19-nov-2026. Todas están en inglés.

- Ubuntu Manpages. *rm(1)*, Ubuntu 26.04, GNU coreutils 9.7 (sin `-r` no borra carpetas; `-i`, `-I`, `-r`, `-f`;
  `--preserve-root` por omisión; la pregunta ante archivos que no se pueden escribir).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/rm.1.html
- Ubuntu Manpages. *cp(1)*, Ubuntu 26.04, GNU coreutils 9.7 (`-i`, `-r`).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/cp.1.html
- Ubuntu Manpages. *mv(1)*, Ubuntu 26.04, GNU coreutils 9.7 (renombra o mueve a una carpeta; `-i`, `-n`).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/mv.1.html
- Ubuntu Manpages. *mkdir(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (`-p`).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/mkdir.1.html
- Ubuntu Manpages. *touch(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (cambia la fecha; crea el archivo si no
  existe). https://manpages.ubuntu.com/manpages/resolute/en/man1/touch.1.html
- Ubuntu Manpages. *rmdir(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (solo carpetas vacías).
  https://manpages.ubuntu.com/manpages/resolute/en/man1/rmdir.1.html
- Ubuntu Manpages. *bash(1)*, Ubuntu 26.04, bash 5.3, el manual de bash: `${parameter:?word}` (con la variable
  vacía, una terminal interactiva no corre el comando), *Pathname Expansion* (bash cambia el patrón por la lista de
  nombres, o lo deja tal cual si no atrapa nada). https://manpages.ubuntu.com/manpages/resolute/en/man1/bash.1.html
- Ubuntu. *Ubuntu 26.04 LTS: summary for LTS users*, sección «rust-coreutils» (`cp`, `mv` y `rm` siguen siendo de
  GNU). https://documentation.ubuntu.com/release-notes/26.04/summary-for-lts-users/
- GitHub, repositorio ValveSoftware/steam-for-linux, issue 3671, abierto el 14 de enero de 2015: el reporte de un
  usuario y los comentarios del hilo, que señalan `rm -rf "$STEAMROOT/"*`. https://github.com/ValveSoftware/steam-for-linux/issues/3671
- Ubuntu Manpages. *ln(1)*, Ubuntu 26.04, `rust-coreutils` 0.10.0 (crea enlaces duros por omisión; con `-s`,
  simbólicos). https://manpages.ubuntu.com/manpages/resolute/man1/ln.1.html
- Medición propia, 19-nov-2026, en una Mac con Apple silicon (Docker 29.2.1 con Colima, linux/arm64), imagen del
  curso sobre Ubuntu 26.04.1: `mkdir`, `touch` y `rmdir` de uutils coreutils 0.10.0, `cp`, `mv` y `rm` de GNU
  coreutils 9.7, y bash 5.3.9. De ahí salen las pantallas, la tabla de `mkdir` y el tiempo de `./practicar reconstruir`.
