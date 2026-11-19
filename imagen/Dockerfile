# La imagen del curso «La terminal desde cero».
#
# Un Dockerfile es una receta: cada instrucción (FROM, RUN, COPY…) agrega un paso, y
# Docker los sigue en orden para armar la imagen. De la imagen sale tu terminal de
# práctica. No necesitas entender este archivo para tomar el curso: está comentado
# para quien tenga curiosidad. Lo construye el guion ./practicar, desde la raíz.

# FROM: de dónde se parte. La imagen oficial de Ubuntu 26.04 LTS, una versión
# común en servidores. Ojo: «26.04» es una etiqueta que se mueve.
# Canonical la vuelve a publicar con las correcciones de seguridad, así que una
# imagen construida otro día puede traer paquetes un poco más nuevos. Lo que no
# cambia es que es Ubuntu 26.04. El día de la prueba (18-nov-2026) era la 26.04.1.
# Por eso las salidas de las lecciones pueden variar un poco tras una actualización.
FROM ubuntu:26.04

# La imagen oficial viene «minimizada»: para pesar poco, no trae las páginas del
# manual (man), ni vi, ni htop. Este RUN las pone. Es un solo RUN, con los comandos
# unidos por «&&»: si uno falla, la construcción se detiene ahí.
#
# 1. Se borra la regla que le dice al instalador de paquetes «no instales manuales».
# 2. apt-get update: se baja la lista de paquetes disponibles.
# 3. Se instala lo que el curso usa:
#      man-db    el comando man          manpages   páginas generales del manual
#      vim-tiny  el editor vi            htop       el visor de procesos
#      less      el paginador            procps     ps, kill y compañía
# 4. Se reinstalan los paquetes que ya venían. Hace falta: se instalaron ANTES de
#    quitar la regla del paso 1, así que llegaron sin su página de manual. Sin este
#    paso, «man ls» contestaba «No manual entry for ls» (medido el 18-nov-2026).
# 5. En la imagen mínima, «man» está cambiado por un aviso que dice «This system
#    has been minimized». Se quita el aviso y se regresa el man de verdad.
# 6. Se borra la lista de paquetes del paso 2: ya no se usa y ocupa espacio.
RUN rm -f /etc/dpkg/dpkg.cfg.d/excludes \
 && apt-get update \
 && apt-get install -y --no-install-recommends man-db manpages vim-tiny htop less procps \
 && apt-get install -y --reinstall $(dpkg-query -W -f='${Package} ') \
 && rm -f /usr/bin/man && dpkg-divert --quiet --remove --rename /usr/bin/man \
 && rm -rf /var/lib/apt/lists/*

# ENV: una variable que queda puesta para todo lo que corra adentro. Esta le dice a
# la terminal que el texto va en UTF-8, para que la ñ y los acentos se vean y se
# escriban bien.
ENV LANG=C.UTF-8

# COPY: mete un archivo de esta carpeta a la imagen. «verifica» es el comando que
# revisa tus ejercicios; queda en /usr/local/bin, donde la terminal lo encuentra
# desde cualquier carpeta, y con permiso de ejecución (755).
COPY --chmod=755 verifica /usr/local/bin/verifica

# Desde la lección 01 hay un segundo comando, «prepara», que arma el escenario de
# cada lección dentro de tu carpeta de práctica (para la 01, un laberinto de
# carpetas). Va aparte de verifica porque verifica solo mira y prepara crea y
# borra. La lista del laberinto queda en /usr/local/share/curso-terminal: la leen
# los dos comandos, y así el laberinto tiene una sola definición. Este COPY no
# lleva --chmod a propósito: se lo pondría también a la carpeta nueva, y una
# carpeta con permiso 644 no deja entrar (medido el 19-nov-2026).
COPY --chmod=755 prepara /usr/local/bin/prepara
COPY escenarios/01-laberinto.txt /usr/local/share/curso-terminal/01-laberinto.txt

# La imagen de Ubuntu entra como root, el usuario que puede todo. En el curso
# trabajas como un usuario normal, «alumno», igual que en un servidor bien cuidado.
#   useradd -m -s /bin/bash alumno   crea al usuario, con su carpeta /home/alumno y
#                                    con bash como su terminal
#   install -d -o alumno -g alumno   crea la carpeta de práctica y se la da a alumno
RUN useradd -m -s /bin/bash alumno && install -d -o alumno -g alumno /home/alumno/practica

# Esta imagen NO declara un volumen (no hay instrucción VOLUME), a propósito: si la
# declarara, correr la imagen a mano dejaría un volumen sin nombre en cada corrida.
# Que tu carpeta ~/practica se guarde entre una sesión y otra es trabajo de
# ./practicar, que le conecta un volumen con nombre y etiqueta. Si corres la imagen
# sin ese guion, lo que hagas en ~/practica se pierde al salir.

# USER: de aquí en adelante, todo corre como «alumno», no como root.
USER alumno

# WORKDIR: la carpeta donde apareces al entrar.
WORKDIR /home/alumno

# CMD: el programa que arranca al entrar. «bash -l» es la terminal bash, abierta
# como sesión de entrada. Cuando sales de ella, el contenedor termina.
CMD ["bash", "-l"]
