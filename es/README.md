# La terminal desde cero

**Por Dorian Chávez, fundador de Hábil y arquitecto de integración.**

**La página del curso:** [habil.mx/es/cursos/terminal/](https://www.habil.mx/es/cursos/terminal/), con el temario y las lecciones en cinco idiomas.

**Para quién es:** para quien programa y nunca ha vivido en una terminal. No se supone nada de Linux.

**Qué vas a terminar sabiendo:** entrar a una terminal de Linux sin pantalla y trabajar ahí sin miedo. Te mueves por
carpetas, creas, copias y borras sabiendo lo que haces, lees bitácoras, encuentras archivos y texto, encadenas
comandos, editas con vi, cambias texto con sed, lees y cambias permisos, y ves y detienes procesos con htop.

**Lo que necesitas antes de empezar:** una computadora con Docker. La [lección 00](00-tu-terminal-de-practica.md) te
dice cómo tenerlo en Linux, en Mac y en Windows, y te deja la terminal de práctica lista.

## Dónde se practica

En un **contenedor**: una computadora Linux chiquita que vive dentro de la tuya. Es Ubuntu 26.04 LTS, se levanta con
un solo comando (`./practicar`) y está separada de tus archivos: no se le conecta ninguna carpeta de tu computadora y
adentro no hay red. Si la descompones, la tiras y empiezas de nuevo. Todos practican ahí, también quienes ya tienen
Linux o Mac: así todo el grupo practica en el mismo entorno.

## Cómo se aprende aquí

- **Una idea nueva por lección.** Lo demás viene dado y explicado.
- **Predices, corres, comparas.** Antes de cada comando anotas qué crees que va a pasar; después lo compruebas y lo
  explicas con tus palabras.
- **Se comprueba solo.** Cada lección trae un `verifica` que revisa el estado de tu terminal de práctica y te dice
  qué falta. No califica lo que predices ni lo que explicas: eso lo comparas tú, al final de cada lección.
- **Con IA, en su lugar.** Un asistente de IA, como Claude Code, te puede explicar un comando. Pero en cada lección
  hay al menos un ejercicio que se hace sin IA, y todo comando que borra o cambia permisos lo escribes tú, a mano.

## Las lecciones

Se hacen en orden: cada una parte de lo que dejó la anterior. Empieza por la
[00](00-tu-terminal-de-practica.md), que deja lista tu terminal de práctica.

| # | Lección | La idea nueva |
|---|---|---|
| 00 | [Tu terminal de práctica](00-tu-terminal-de-practica.md) | En esta terminal no hay escritorio ni ventanas, solo texto: te dice quién eres y dónde estás en cada línea, y se le habla con comandos |
| 01 | [Moverte sin ver](01-moverte-sin-ver.md) | El árbol de carpetas, y la ruta absoluta contra la relativa |
| 02 | [Crear, copiar, mover y borrar sin miedo](02-crear-copiar-mover-borrar.md) | En la terminal no hay papelera: se piensa antes de borrar |
| 03 | [Ver lo que hay dentro](03-ver-lo-que-hay-dentro.md) | Casi todo en un servidor es texto, y se lee sin abrir un editor |
| 04 | Encontrar archivos: find | `find` recorre el árbol y filtra por lo que tú le digas |
| 05 | Buscar dentro: grep | `grep` busca texto dentro de los archivos; `find` busca los archivos |
| 06 | Tuberías y redirección | La salida de un comando puede ser la entrada del siguiente |
| 07 | Editar en el servidor: vi | vi tiene modos; la mitad de los problemas es no saber en cuál estás |
| 08 | Cambiar texto sin abrirlo: sed | Cambiar muchas líneas de un jalón, probando antes de escribir |
| 09 | Permisos básicos | Cada archivo dice quién puede leerlo, escribirlo y ejecutarlo |
| 10 | Procesos: ps, htop y kill | Un programa que corre es un proceso, con número y dueño, y se le puede pedir que pare |
| 11 | Junta todo: la bitácora del problema | Sin idea nueva: usas todo lo anterior para encontrar y arreglar una falla |

Las lecciones sin enlace están en preparación y se publican en orden.

## Lo que sigue después

Este curso es la puerta de los cursos de [Docker](https://www.habil.mx/es/cursos/docker/) y de
[Kubernetes](https://www.habil.mx/es/cursos/kubernetes/) de esta casa.
