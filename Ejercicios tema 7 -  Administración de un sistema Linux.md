# Tarea 07

## Administración básica del sistema Linux en Debian

---

# Introducción

En esta práctica vas a trabajar los bloques fundamentales de la administración básica de un sistema Linux. A lo largo de los ejercicios aprenderás a crear y administrar usuarios, organizar grupos, trabajar con permisos, identificar procesos, consultar información del sistema, acceder a dispositivos y automatizar tareas mediante `cron`.

El objetivo no es solo que ejecutes comandos, sino que comprendas:

* qué hace cada comando,
* en qué situaciones se utiliza,
* qué información devuelve,
* y cómo comprobar que el resultado es correcto.

Esta práctica está pensada para realizarse en **Debian**, utilizando principalmente la **terminal**.

Cuando una operación requiera permisos de administración, deberás usar:

```bash
sudo
```

## ¿Qué hace `sudo`?

`sudo` permite ejecutar un comando con privilegios de administrador.
Se utiliza en tareas como:

* crear usuarios o grupos,
* modificar archivos del sistema,
* consultar información protegida,
* cambiar propietarios o permisos,
* programar tareas del sistema.

Ejemplo:

```bash
sudo useradd juan
```

Ese comando crea el usuario `juan` usando privilegios administrativos.

---

# Ejercicio 1 — Usuarios y grupos

## Descripción general

Linux es un sistema **multiusuario**, lo que significa que distintas personas pueden tener su propia cuenta dentro del sistema. Cada usuario tiene asociado:

* un nombre de usuario,
* un identificador numérico llamado **UID**,
* un grupo principal, identificado por un **GID**,
* un directorio personal,
* y una shell por defecto.

Además, Linux utiliza **grupos** para organizar usuarios y simplificar la gestión de permisos.

En este ejercicio vas a:

* crear grupos,
* crear usuarios,
* consultar los archivos donde Linux guarda esta información,
* trabajar con archivos desde otro usuario,
* y realizar tareas administrativas básicas sobre cuentas y propiedades de archivos.

---

## 1. Crear los grupos necesarios

### Qué se va a hacer

Antes de crear los usuarios, vas a crear los grupos con los que se organizarán esas cuentas. Un grupo sirve para reunir usuarios que comparten una misma función o departamento.

### Qué se pide

Debes crear los grupos:

* `informatico`
* `vendedor`
* `juana`

### Comando que se va a usar: `groupadd`

`groupadd` sirve para **crear grupos nuevos** en el sistema.

Sintaxis general:

```bash
sudo groupadd nombre_grupo
```

### Pasos

1. Abre una terminal.
2. Ejecuta:

```bash
sudo groupadd informatico
sudo groupadd vendedor
sudo groupadd juana
```

3. Comprueba que los grupos se han creado correctamente con:

```bash
cat /etc/group | grep informatico
cat /etc/group | grep vendedor
cat /etc/group | grep juana
```

### Comandos utilizados

#### `cat`

Muestra el contenido de un archivo en pantalla.

#### `grep`

Busca una palabra o patrón dentro de un texto o dentro de la salida de otro comando.

En este caso, `cat /etc/group | grep informatico` muestra solo la línea del archivo `/etc/group` donde aparece el grupo `informatico`.

### Qué debes comprobar

Debes ver una línea para cada grupo creado dentro de `/etc/group`.

### Capturas obligatorias

1. Ejecución de los tres comandos `groupadd`.
2. Salida donde se vea que existen los grupos `informatico`, `vendedor` y `juana`.

---

## 2. Crear los usuarios

### Qué se va a hacer

Ahora vas a crear varios usuarios y asignarles su grupo principal. Cuando un usuario se crea correctamente, el sistema registra su información en los archivos internos de cuentas y, además, puede crear su directorio personal.

### Qué se pide

Debes crear estos usuarios:

| Usuario | Grupo principal |
| ------- | --------------- |
| juana   | juana           |
| luis    | informatico     |
| Lorena   | informatico     |
| maria   | vendedor        |
| angel   | vendedor        |

La contraseña inicial de cada usuario debe ser su propio nombre.

### Comando principal: `useradd`

`useradd` sirve para **crear usuarios**.

Opciones que vas a usar:

* `-m`: crea el directorio personal del usuario en `/home`
* `-g`: establece el grupo principal

### Pasos

1. Ejecuta:

```bash
sudo useradd -m -g juana juana
sudo useradd -m -g informatico luis
sudo useradd -m -g informatico Lorena
sudo useradd -m -g vendedor maria
sudo useradd -m -g vendedor angel
```

2. Asigna contraseñas con:

```bash
sudo passwd juana
sudo passwd luis
sudo passwd Lorena
sudo passwd maria
sudo passwd angel
```

### Comando utilizado: `passwd`

`passwd` sirve para **establecer o cambiar contraseñas** de usuarios.

Si lo ejecuta un administrador indicando un nombre de usuario, cambia la contraseña de esa cuenta.

### Comprobación

Muestra las líneas de los usuarios creados:

```bash
cat /etc/passwd | grep juana
cat /etc/passwd | grep luis
cat /etc/passwd | grep Lorena
cat /etc/passwd | grep maria
cat /etc/passwd | grep angel
```

### Qué debes comprobar

Debes ver que cada usuario existe y que tiene directorio personal en `/home/...`.

### Capturas obligatorias

1. Creación de los usuarios con `useradd`.
2. Asignación de contraseñas con `passwd`.
3. Verificación de las cuentas en `/etc/passwd`.

---

## 3. Consultar la información de usuarios y grupos

### Qué se va a hacer

Linux guarda la información de usuarios y grupos en varios archivos del sistema. En este apartado vas a consultar esos archivos para entender cómo se almacenan las cuentas.

### Qué se pide

Debes mostrar:

* el archivo de usuarios,
* el archivo de grupos,
* el archivo de contraseñas cifradas,

e identificar la información básica de los usuarios creados.

### Archivos importantes

#### `/etc/passwd`

Contiene la información general de las cuentas de usuario.

#### `/etc/group`

Contiene los grupos del sistema.

#### `/etc/shadow`

Contiene las contraseñas cifradas y datos relacionados con la autenticación.

### Pasos

1. Mostrar usuarios:

```bash
cat /etc/passwd
```

2. Mostrar grupos:

```bash
cat /etc/group
```

3. Mostrar contraseñas cifradas:

```bash
sudo cat /etc/shadow
```

4. Localiza las líneas de los usuarios que has creado y fíjate en:

   * nombre de usuario,
   * UID,
   * GID,
   * directorio personal,
   * shell.

### Qué debes comprobar

Debes ser capaz de identificar qué línea pertenece a cada usuario y a cada grupo.

### Capturas obligatorias

1. Parte de `/etc/passwd` donde aparezcan los usuarios creados.
2. Parte de `/etc/group` donde aparezcan los grupos creados.
3. Salida de `/etc/shadow`.

---

## 4. Crear archivos como el usuario juana

### Qué se va a hacer

Ahora vas a trabajar como un usuario normal del sistema. El objetivo es comprobar que cada usuario tiene su propio entorno de trabajo y su propio directorio personal.

### Qué se pide

Debes cambiar al usuario `juana`, situarte en su directorio personal y crear tres archivos vacíos:

* `factura1`
* `factura2`
* `carta`

### Comandos utilizados

#### `su - juana`

Permite cambiar a la cuenta `juana` cargando su entorno completo.

#### `pwd`

Muestra el directorio actual.

#### `touch`

Crea archivos vacíos.

#### `ls -l`

Lista archivos mostrando detalles, como permisos, propietario, grupo y tamaño.

### Pasos

1. Cambia al usuario `juana`:

```bash
su - juana
```

2. Comprueba que estás en su directorio personal:

```bash
pwd
```

Debería aparecer algo como:

```bash
/home/juana
```

3. Crea los archivos:

```bash
touch factura1
touch factura2
touch carta
```

4. Muestra el contenido del directorio:

```bash
ls -l
```

### Qué debes comprobar

Debes ver los tres archivos creados dentro de `/home/juana`.

### Capturas obligatorias

1. Cambio al usuario `juana`.
2. Resultado de `pwd`.
3. Creación de los tres archivos.
4. Resultado de `ls -l`.

---

## 5. Ejercicio de administración

### Qué se va a hacer

En este apartado vas a realizar tareas típicas de administración de usuarios y archivos:

* cambiar el grupo principal de un usuario,
* mover un archivo de un usuario a otro,
* cambiar el propietario de un archivo,
* cambiar el grupo propietario de un directorio,
* eliminar un grupo.

### Qué se pide

Debes hacer lo siguiente:

* `juana` pasará a tener como grupo principal `vendedor`
* el archivo `carta` pasará al directorio personal de `luis`
* `carta` deberá quedar con el propietario y grupo adecuados
* el grupo `juana` deberá eliminarse

### Comandos utilizados

#### `usermod`

Modifica propiedades de una cuenta de usuario.

#### `id`

Muestra información de una cuenta, como UID, GID y grupos.

#### `mv`

Mueve o renombra archivos.

#### `chown`

Cambia el propietario y/o grupo propietario de un archivo.

#### `groupdel`

Elimina un grupo.

### Pasos

1. Cambiar el grupo principal de `juana`:

```bash
sudo usermod -g vendedor juana
```

2. Comprobar el cambio:

```bash
id juana
```

3. Mover el archivo `carta` al directorio de `luis`:

```bash
sudo mv /home/juana/carta /home/luis/
```

4. Cambiar el propietario y grupo del archivo:

```bash
sudo chown luis:vendedor /home/luis/carta
```

5. Cambiar el grupo propietario del directorio de `juana`:

```bash
sudo chown :vendedor /home/juana
```

6. Eliminar el grupo `juana`:

```bash
sudo groupdel juana
```

7. Comprobar que ya no existe:

```bash
cat /etc/group | grep juana
```

### Qué debes comprobar

* que `juana` ya no pertenece al grupo principal `juana`,
* que `carta` está ahora dentro de `/home/luis`,
* que su propietario es `luis`,
* y que el grupo `juana` ya no existe.

### Capturas obligatorias

1. Resultado de `id juana`.
2. Movimiento del archivo `carta`.
3. Cambio de propietario con `chown`.
4. Eliminación del grupo `juana`.
5. Comprobación final de que ya no aparece en `/etc/group`.

---

## 6. Añadir luis al grupo sudo

### Qué se va a hacer

En Debian, los usuarios que pertenecen al grupo `sudo` pueden ejecutar comandos administrativos mediante `sudo`.

### Qué se pide

Debes añadir a `luis` al grupo `sudo` como grupo secundario y comprobar que el cambio se ha realizado.

### Comando utilizado: `usermod -aG`

* `-G` indica grupos suplementarios
* `-a` significa “añadir”, para no sustituir los grupos anteriores

### Pasos

1. Ejecuta:

```bash
sudo usermod -aG sudo luis
```

2. Comprueba los grupos de `luis`:

```bash
groups luis
```

3. Muestra la línea del grupo `sudo`:

```bash
cat /etc/group | grep sudo
```

### Qué debes comprobar

Debes ver que `luis` pertenece también al grupo `sudo`.

### Capturas obligatorias

1. Ejecución de `usermod -aG sudo luis`.
2. Resultado de `groups luis`.
3. Línea del grupo `sudo`.

---

# Ejercicio 2 — Dispositivos

## Descripción general

En Linux, muchos dispositivos hardware se representan como archivos dentro del directorio `/dev`. En este ejercicio vas a trabajar con una imagen ISO montada como unidad óptica virtual.

Aprenderás a:

* identificar dispositivos de almacenamiento,
* localizar el punto de montaje,
* consultar el contenido del dispositivo montado.

---

## 1. Montar una imagen ISO

### Qué se va a hacer

Vas a insertar una imagen ISO en la unidad óptica virtual de la máquina.

### Qué se pide

Montar una ISO desde VirtualBox y comprobar que Debian la reconoce.

### Pasos

1. En VirtualBox, ve a:

**Dispositivos → Unidad óptica → Elegir archivo de disco**

2. Selecciona una ISO, por ejemplo una de instalación de Windows.
3. Espera a que Debian detecte la unidad.

### Qué debes comprobar

Debes comprobar que el sistema detecta la unidad óptica y que aparece montada o accesible.

### Capturas obligatorias

1. ISO seleccionada en VirtualBox.
2. Aparición del dispositivo dentro de Debian.

---

## 2. Identificar el dispositivo y el punto de montaje

### Qué se va a hacer

Vas a averiguar qué archivo de dispositivo representa la unidad y dónde está montada en el sistema de archivos.

### Qué se pide

Identificar:

* el dispositivo, normalmente `/dev/sr0`
* el punto de montaje

### Comandos utilizados

#### `lsblk`

Muestra los dispositivos de bloques: discos, particiones y unidades ópticas.

#### `mount`

Muestra los sistemas de archivos montados.

### Pasos

1. Ejecuta:

```bash
lsblk
```

2. Después:

```bash
mount
```

3. Localiza la unidad óptica y su punto de montaje.

### Qué debes comprobar

Debes encontrar el nombre del dispositivo y la ruta donde está montado.

### Capturas obligatorias

1. Resultado de `lsblk`.
2. Resultado de `mount`.

---

## 3. Listar el contenido del CD

### Qué se va a hacer

Vas a acceder al contenido del dispositivo montado.

### Qué se pide

Listar los archivos y carpetas que contiene.

### Comando utilizado: `ls`

`ls` muestra el contenido de un directorio.

### Pasos

1. Usa el punto de montaje que hayas detectado.
2. Lista su contenido, por ejemplo:

```bash
sudo ls /media/cdrom
```

o la ruta que corresponda en tu sistema.

### Qué debes comprobar

Debes poder ver los archivos del CD.

### Capturas obligatorias

1. Listado del contenido del CD.

---

## 4. Mostrar un archivo de texto del CD

### Qué se va a hacer

Vas a abrir el contenido de un archivo de texto del dispositivo.

### Qué se pide

Mostrar en pantalla el contenido de algún archivo de texto del CD.

### Comando utilizado: `cat`

`cat` muestra el contenido de un archivo de texto.

### Pasos

1. Localiza un archivo de texto.
2. Muéstralo en pantalla:

```bash
cat ruta/al/archivo.txt
```

### Qué debes comprobar

Debes visualizar el contenido completo o parcial del archivo.

### Capturas obligatorias

1. Comando usado para mostrar el archivo.
2. Salida del contenido.

---

# Ejercicio 4 — Permisos

## Descripción general

Linux controla el acceso a los archivos mediante permisos para:

* propietario,
* grupo,
* otros usuarios.

Los permisos básicos son:

* `r`: lectura
* `w`: escritura
* `x`: ejecución

En este ejercicio vas a crear un script, consultar sus permisos, modificarlos y ejecutarlo.

---

## 1. Crear un script

### Qué se va a hacer

Vas a crear un pequeño script en Bash con varios comandos sencillos.

### Qué se pide

Crear un archivo llamado `archivo` con este contenido:

```bash
#!/bin/bash
clear
touch otroArchivo.txt
ls -l
```

### Comandos utilizados

#### `nano`

Editor de texto en terminal.

#### `touch`

Crea archivos vacíos.

#### `clear`

Limpia la pantalla.

### Pasos

1. Cambia al usuario `luis`:

```bash
su - luis
```

2. Crea el archivo:

```bash
nano archivo
```

3. Escribe el contenido indicado.
4. Guarda con:

   * `Ctrl + O`
   * `Enter`
   * `Ctrl + X`

### Qué debes comprobar

Debes tener un archivo llamado `archivo` con ese contenido.

### Capturas obligatorias

1. Archivo abierto en `nano`.
2. Contenido escrito.
3. Archivo guardado.

---

## 2. Consultar permisos

### Qué se va a hacer

Vas a observar los permisos iniciales del archivo creado.

### Qué se pide

Mostrar los permisos e identificar:

* el propietario,
* el grupo,
* los permisos de cada bloque.

### Comando utilizado: `ls -l`

Muestra información detallada de archivos.

### Pasos

```bash
ls -l archivo
```

### Qué debes comprobar

Debes interpretar correctamente la salida.

### Capturas obligatorias

1. Resultado de `ls -l archivo`.

---

## 3. Cambiar permisos con `chmod`

### Qué se va a hacer

Vas a modificar los permisos usando notación octal.

### Qué se pide

Dejar el archivo con estos permisos:

* propietario: `rwx`
* grupo: `rw-`
* otros: `r--`

Eso equivale a:

* `7` → `rwx`
* `6` → `rw-`
* `4` → `r--`

Resultado: `764`

### Comando utilizado: `chmod`

`chmod` cambia permisos.

### Pasos

```bash
chmod 764 archivo
```

Después comprueba:

```bash
ls -l archivo
```

### Qué debes comprobar

Debes ver que los permisos han cambiado a `rwxrw-r--`.

### Capturas obligatorias

1. Ejecución de `chmod 764 archivo`.
2. Resultado posterior de `ls -l archivo`.

---

## 4. Ejecutar el script

### Qué se va a hacer

Vas a ejecutar el script y comprobar su efecto.

### Qué se pide

Ejecutarlo correctamente desde el directorio actual y comprobar que crea el archivo `otroArchivo.txt`.

### Pasos

1. Asegúrate de que el propietario tiene permiso de ejecución.
2. Ejecuta:

```bash
./archivo
```

### Qué significa `./archivo`

Indica que el archivo está en el directorio actual.

### Qué debes comprobar

* que el script se ejecuta,
* que aparece el listado,
* y que se crea `otroArchivo.txt`.

### Capturas obligatorias

1. Ejecución del script.
2. Aparición de `otroArchivo.txt`.

---

## 5. Permitir ejecución a todos

### Qué se va a hacer

Vas a cambiar los permisos para que todos los usuarios puedan ejecutar el archivo.

### Qué se pide

Añadir permiso de ejecución a todos.

### Comando

```bash
chmod a+x archivo
```

### Explicación

* `a` significa “all” (todos)
* `+x` añade ejecución

### Comprobación

```bash
ls -l archivo
```

### Capturas obligatorias

1. Ejecución de `chmod a+x archivo`.
2. Resultado final de `ls -l archivo`.

---

# Ejercicio 5 — Procesos

## Descripción general

Un proceso es un programa que se está ejecutando. Linux permite:

* ver procesos,
* localizarlos por nombre,
* obtener su PID,
* finalizarlos,
* cambiar su prioridad,
* observar su consumo de CPU.

---

## 1. Ejecutar `sleep` y finalizarlo

### Qué se va a hacer

Vas a lanzar un proceso que se queda esperando un tiempo y lo finalizarás desde otra terminal.

### Qué se pide

Ejecutar `sleep 100`, localizar su PID y matarlo.

### Comandos utilizados

#### `sleep`

Detiene la ejecución durante un número de segundos.

#### `ps aux`

Muestra los procesos del sistema.

#### `kill`

Finaliza un proceso por su PID.

### Pasos

1. En una terminal:

```bash
sleep 100
```

2. En otra terminal:

```bash
ps aux | grep sleep
```

3. Anota el PID.
4. Finaliza el proceso:

```bash
kill PID
```

### Qué debes comprobar

Debes comprobar que el proceso desaparece.

### Capturas obligatorias

1. Ejecución de `sleep 100`.
2. Búsqueda con `ps aux | grep sleep`.
3. Finalización con `kill`.

---

## 2. Crear y ejecutar un script infinito

### Qué se va a hacer

Vas a crear un script que nunca termina por sí solo y analizar los procesos que genera.

### Qué se pide

Crear `infinito.sh`, ejecutarlo, identificar procesos relacionados y finalizarlo.

### Pasos

1. Crear el archivo:

```bash
nano infinito.sh
```

2. Escribir:

```bash
#!/bin/bash
while true
do
sleep 5
echo Han pasado 5 segundos
done
```

3. Dar permisos:

```bash
chmod +x infinito.sh
```

4. Ejecutarlo:

```bash
./infinito.sh
```

5. Desde otra terminal, localizar `sleep`:

```bash
ps aux | grep sleep
```

6. Finalizar el script o sus procesos asociados.

### Comandos utilizados

#### `while true`

Crea un bucle infinito.

#### `chmod +x`

Añade permiso de ejecución.

### Qué debes comprobar

Debes observar que el script sigue ejecutándose indefinidamente hasta que lo detengas.

### Capturas obligatorias

1. Contenido del script.
2. Script ejecutándose.
3. Proceso `sleep` localizado.
4. Finalización del proceso o script.

---

## 3. Proceso `yes`

### Qué se va a hacer

Vas a ejecutar un proceso que genera salida continua y consume recursos.

### Qué se pide

Lanzar `yes`, redirigir su salida a un archivo, comprobar consumo de CPU y tamaño del archivo.  **ACUÉRDATE DE FINALIZAR PRONTO EL PROCESO**

### Comandos utilizados

#### `yes`

Escribe repetidamente una cadena.

#### `top`

Muestra procesos en tiempo real y su consumo.

#### `ls -lh`

Muestra tamaños en formato legible.

### Pasos

1. Ejecuta:

```bash
yes hola
```

2. Deténlo con `Ctrl + C`.
3. Ejecuta ahora:

```bash
yes hola > archivo.txt
```

4. En otra terminal, abre:

```bash
top
```

5. Busca el proceso `yes` y observa su CPU.
6. Finalízalo.
7. Comprueba el tamaño del archivo:

```bash
ls -lh archivo.txt
```

8. Elimínalo:

```bash
rm archivo.txt
```

### Qué debes comprobar

Debes observar que `yes` genera muchísimo texto y puede consumir bastante CPU.

### Capturas obligatorias

1. Ejecución de `yes hola`.
2. Ejecución de `yes hola > archivo.txt`.
3. Proceso visible en `top`.
4. Tamaño de `archivo.txt`.

---

## 4. Ejecutar procesos con distinta prioridad

### Qué se va a hacer

Vas a lanzar procesos con prioridades distintas para entender cómo funciona `nice`.

### Qué se pide

Ejecutar un proceso con prioridad baja y otro con prioridad alta.

### Comando utilizado: `nice`

`nice` lanza un proceso con una prioridad determinada.

* valores altos: menos prioridad
* valores bajos o negativos: más prioridad

### Pasos

1. Ejecuta con prioridad baja:

```bash
nice -n 15 yes
```

2. Ejecuta con prioridad alta como administrador:

```bash
sudo nice -n -15 yes
```

3. Comprueba prioridades con:

```bash
ps -eo pid,ni,cmd
```

### Qué debes comprobar

Debes ver distintos valores de prioridad (`NI`).

### Capturas obligatorias

1. Proceso lanzado con `nice -n 15`.
2. Proceso lanzado con `sudo nice -n -15`.
3. Salida de `ps -eo pid,ni,cmd`.

---

## 5. Cambiar prioridad con `renice`

### Qué se va a hacer

Vas a modificar la prioridad de un proceso que ya se está ejecutando.

### Qué se pide

Localizar un PID y cambiar su prioridad.

### Comando utilizado: `renice`

Cambia la prioridad de un proceso existente.

### Pasos

1. Busca un proceso, por ejemplo `yes`:

```bash
ps aux | grep yes
```

2. Cambia su prioridad:

```bash
renice 10 PID
```

3. Comprueba el cambio con:

```bash
ps -eo pid,ni,cmd
```

### Qué debes comprobar

Debes observar el nuevo valor de prioridad.

### Capturas obligatorias

1. Búsqueda del proceso.
2. Ejecución de `renice`.
3. Comprobación final.

---

# Ejercicio 6 — Información del sistema

## Descripción general

Linux incluye muchos comandos para consultar el estado del sistema. En este ejercicio vas a obtener información sobre:

* kernel,
* CPU,
* espacio en disco,
* tamaño de directorios,
* registros del sistema.

---

## 1. Versión del kernel

### Qué se va a hacer

Consultar la versión del núcleo de Linux.

### Comando: `uname -r`

```bash
uname -r
```

### Explicación

* `uname` muestra información del sistema
* `-r` muestra la versión del kernel

### Capturas obligatorias

1. Resultado de `uname -r`.

---

## 2. Información de la CPU

### Qué se va a hacer

Consultar características del procesador.

### Comando: `lscpu`

```bash
lscpu
```

### Explicación

Muestra arquitectura, número de núcleos, hilos, fabricante y otros datos.

### Capturas obligatorias

1. Resultado de `lscpu`.

---

## 3. Registros del sistema

### Qué se va a hacer

Consultar mensajes recientes del sistema.

### Comando: `journalctl`

```bash
sudo journalctl -n 20
```

### Explicación

* `journalctl` consulta el registro del sistema
* `-n 20` muestra las 20 últimas líneas

### Capturas obligatorias

1. Resultado de `sudo journalctl -n 20`.

---

## 4. Espacio en disco

### Qué se va a hacer

Consultar el uso de los sistemas de archivos montados.

### Comando: `df -h`

```bash
df -h
```

### Explicación

* `df` muestra espacio usado y disponible
* `-h` usa formato legible

### Capturas obligatorias

1. Resultado de `df -h`.

---

## 5. Tamaño del directorio personal

### Qué se va a hacer

Calcular cuánto ocupa tu directorio personal.

### Comando: `du -sh ~`

```bash
du -sh ~
```

### Explicación

* `du` calcula uso de disco
* `-s` muestra solo el total
* `-h` lo hace legible
* `~` representa el directorio personal del usuario actual

### Capturas obligatorias

1. Resultado de `du -sh ~`.

---

# Ejercicio 7 — Tareas programadas con cron

## Descripción general

En Linux es muy habitual automatizar tareas repetitivas: copias de seguridad, generación de registros, mantenimiento del sistema, etc. Para ello se utiliza el servicio **cron**.

En este ejercicio vas a:

* crear un script que genere un informe sencillo del sistema,
* probarlo manualmente,
* comprobar que añade información a un archivo,
* y programarlo para que se ejecute automáticamente.

Este ejercicio es importante porque une varios contenidos:

* creación de scripts,
* permisos de ejecución,
* redirección de salida,
* y programación de tareas.

---

## 1. Crear el script `7.sh`

### Qué se va a hacer

Vas a crear un script que guarde en un archivo:

* la fecha y hora actuales,
* los sistemas montados,
* y los procesos en ejecución.

### Qué se pide

Crear un archivo llamado `7.sh` que añada esa información a `resultado7.txt` cada vez que se ejecute.

### Comando utilizado: `nano`

Se usará `nano` para editar el script.

### Contenido del script

```bash
#!/bin/bash

echo "============================" >> resultado7.txt
echo "Fecha y hora:" >> resultado7.txt
date >> resultado7.txt

echo "Sistemas montados:" >> resultado7.txt
mount >> resultado7.txt

echo "Procesos en ejecución:" >> resultado7.txt
ps aux >> resultado7.txt

echo "" >> resultado7.txt
```

### Explicación de los comandos usados dentro del script

#### `#!/bin/bash`

Indica que el script debe ejecutarse con el intérprete Bash.

#### `echo`

Escribe texto en pantalla o, si se redirige, dentro de un archivo.

#### `>>`

Redirección de salida en modo **añadir**.
No sobrescribe el archivo, sino que agrega contenido al final.

#### `date`

Muestra fecha y hora actuales.

#### `mount`

Muestra los sistemas de archivos montados.

#### `ps aux`

Muestra procesos en ejecución.

### Pasos

1. Crea el script:

```bash
nano 7.sh
```

2. Escribe el contenido indicado.
3. Guarda y sal.
4. Da permiso de ejecución:

```bash
chmod +x 7.sh
```

### Qué debes comprobar

Debes tener un archivo ejecutable llamado `7.sh`.

### Capturas obligatorias

1. Edición del archivo en `nano`.
2. Contenido completo del script.
3. Comando `chmod +x 7.sh`.

---

## 2. Probar el script manualmente

### Qué se va a hacer

Antes de programarlo, debes comprobar que funciona correctamente al ejecutarlo de forma manual.

### Qué se pide

Ejecutar el script y comprobar que se crea `resultado7.txt` con la información esperada.

### Pasos

1. Ejecuta:

```bash
./7.sh
```

2. Comprueba que se ha creado el archivo:

```bash
ls -l resultado7.txt
```

3. Muestra su contenido:

```bash
cat resultado7.txt
```

### Qué debes comprobar

Debes ver en el archivo:

* una cabecera,
* la fecha,
* la lista de sistemas montados,
* y la lista de procesos.

### Capturas obligatorias

1. Ejecución manual de `./7.sh`.
2. Aparición de `resultado7.txt`.
3. Contenido de `resultado7.txt`.

---

## 3. Comprobar que el script añade contenido y no sobrescribe

### Qué se va a hacer

Vas a ejecutar el script varias veces para comprobar que usa `>>` correctamente.

### Qué se pide

Ejecutarlo al menos dos veces y verificar que el contenido del archivo crece.

### Pasos

1. Ejecuta dos veces:

```bash
./7.sh
./7.sh
```

2. Comprueba el contenido:

```bash
cat resultado7.txt
```

3. Observa que aparecen varios bloques de información.

### Qué debes comprobar

Debes comprobar que el archivo no se reinicia en cada ejecución, sino que se amplía.

### Capturas obligatorias

1. Dos ejecuciones consecutivas del script.
2. Contenido del archivo con varios bloques.

---

## 4. Programar la tarea con `crontab`

### Qué se va a hacer

Ahora vas a indicar al sistema que ejecute ese script automáticamente cada cierto tiempo.

### Qué se pide

Programar el script para que se ejecute **todas las horas en punto, de lunes a viernes**.

### Comando utilizado: `crontab`

`crontab` permite editar la tabla de tareas programadas de un usuario.

### Sintaxis de cron

Cada línea de cron tiene 5 campos de tiempo y luego el comando:

```text
minuto hora día_del_mes mes día_de_la_semana comando
```

### Línea que debes añadir

```text
0 * * * 1-5 /home/usuario/7.sh
```

### Explicación de esa línea

* `0` → en el minuto 0
* `*` → a cualquier hora
* `*` → cualquier día del mes
* `*` → cualquier mes
* `1-5` → de lunes a viernes
* `/home/usuario/7.sh` → script que se ejecutará

### Importante

Debes sustituir `usuario` por el nombre real del usuario con el que estás trabajando.

### Pasos

1. Edita la tabla:

```bash
crontab -e
```

2. Añade la línea correspondiente.
3. Guarda y sal.
4. Comprueba la configuración:

```bash
crontab -l
```

### Qué debes comprobar

Debes ver la línea programada correctamente en la salida de `crontab -l`.

### Capturas obligatorias

1. Edición de `crontab -e`.
2. Línea añadida.
3. Resultado de `crontab -l`.

---

## 5. Explicar qué hace la tarea programada

### Qué se va a hacer

Además de programarla, debes comprender su funcionamiento.

### Qué se pide

Explicar brevemente:

* cuándo se ejecutará,
* qué archivo generará o modificará,
* y qué información guardará.

### Qué debe incluir tu explicación

* que se ejecuta cada hora en punto,
* que solo de lunes a viernes,
* que añade información a `resultado7.txt`,
* que guarda fecha, montajes y procesos.

### Capturas obligatorias

1. No requiere captura adicional, pero sí explicación escrita junto al ejercicio.

---

