
# Administración básica del sistema Linux con Debian

## 1. Introducción

Un sistema operativo GNU/Linux no solo debe saberse utilizar desde el punto de vista del usuario, sino también desde el punto de vista de la administración básica del sistema. En un entorno profesional es habitual tener que crear usuarios, gestionar grupos, controlar permisos, montar dispositivos de almacenamiento, administrar discos y particiones, supervisar procesos, consultar información del sistema y automatizar tareas.

Debian es una distribución GNU/Linux muy utilizada por su estabilidad, su organización y su importancia histórica dentro del ecosistema Linux. Muchas de las tareas de administración pueden realizarse desde entorno gráfico, pero en contextos técnicos y profesionales la terminal sigue siendo una herramienta fundamental. Por ello, en estos apuntes se estudian los conceptos y comandos básicos de administración del sistema utilizando Debian desde línea de comandos.

El objetivo no es memorizar órdenes aisladas, sino comprender qué hace el sistema, cómo se organiza y por qué se utilizan determinados comandos en cada situación.

---

# 2. Usuarios y grupos

## 2.1. El sistema multiusuario

Linux es un sistema operativo **multiusuario**. Esto significa que varias personas pueden disponer de cuentas diferentes dentro del mismo equipo, cada una con sus propios archivos, su propia configuración y sus propios permisos.

Cada usuario del sistema tiene asociados varios elementos importantes:

* Un **nombre de usuario**, que es el identificador textual con el que inicia sesión.
* Un **UID** (*User ID*), que es un identificador numérico único.
* Un **grupo principal**, identificado mediante un **GID** (*Group ID*).
* Un **directorio personal** o *home*, donde guarda sus archivos y configuración.
* Una **shell**, que es el intérprete de comandos que utilizará por defecto.

Además de usuarios, Linux utiliza **grupos** para organizar permisos y accesos. Un grupo permite reunir varios usuarios y asignar permisos comunes sobre archivos, carpetas o recursos del sistema.

Un usuario puede pertenecer a:

* un **grupo principal**, que es el que tiene asignado por defecto,
* y uno o varios **grupos secundarios** o suplementarios.

Esta organización permite gestionar el acceso a recursos de forma más eficiente.

---

## 2.2. Privilegios de administración

Las tareas relacionadas con la gestión de usuarios y grupos requieren permisos elevados. En Debian, estas acciones suelen realizarse mediante `sudo`, siempre que el usuario tenga autorización para ello.

Por ejemplo, para ejecutar una orden con privilegios administrativos:

```bash
sudo comando
```

En un entorno real, esto evita trabajar constantemente como administrador y mejora la seguridad del sistema.

---

## 2.3. Creación de usuarios

Para crear un usuario en Debian se utiliza habitualmente el comando:

```bash
sudo adduser nombre_usuario
```

Este comando no se limita a registrar una cuenta, sino que automatiza varias tareas:

* crea el usuario,
* crea un grupo con el mismo nombre,
* asigna ese grupo como grupo principal,
* crea el directorio personal dentro de `/home`,
* copia en él la configuración inicial desde `/etc/skel`,
* solicita una contraseña,
* y permite introducir datos descriptivos del usuario.

Ejemplo:

```bash
sudo adduser juan
```

Tras ejecutar esta orden, el sistema pedirá la contraseña del nuevo usuario y podrá solicitar información adicional como el nombre completo o el teléfono. Esa información es opcional.

### Directorio `/etc/skel`

El directorio `/etc/skel` contiene archivos y carpetas que se copian automáticamente al directorio personal de los nuevos usuarios. Sirve como plantilla inicial para que todas las cuentas partan de una configuración base común.

---

## 2.4. Creación de grupos

Para crear un grupo se utiliza:

```bash
sudo addgroup nombre_grupo
```

Ejemplo:

```bash
sudo addgroup alumnado
```

Este grupo podrá utilizarse posteriormente para organizar permisos o para añadir usuarios que compartan ciertas características o funciones.

---

## 2.5. Crear un usuario dentro de un grupo ya existente

Si se desea crear un usuario cuyo grupo principal sea uno que ya existe, puede utilizarse:

```bash
sudo adduser nombre_usuario --ingroup nombre_grupo
```

Ejemplo:

```bash
sudo adduser ana --ingroup alumnado
```

En este caso, el grupo principal de `ana` será `alumnado`.

---

## 2.6. Cambio de contraseña

La contraseña de un usuario puede modificarse mediante:

```bash
passwd nombre_usuario
```

Si un administrador quiere cambiar la contraseña de otro usuario, deberá hacerlo con privilegios elevados:

```bash
sudo passwd juan
```

El sistema solicitará la nueva contraseña dos veces para evitar errores de escritura.

Si un usuario ejecuta `passwd` sin indicar nombre, normalmente cambiará su propia contraseña.

---

## 2.7. Archivos de configuración de usuarios y grupos

La información sobre usuarios y grupos se almacena en varios archivos del sistema.

### `/etc/passwd`

Contiene una línea por cada usuario definido en el sistema. No solo aparecen usuarios humanos, sino también usuarios internos utilizados por servicios y procesos del sistema.

Puede consultarse con:

```bash
cat /etc/passwd
```

Cada línea contiene siete campos separados por `:`:

1. nombre de usuario,
2. marcador de contraseña,
3. UID,
4. GID,
5. descripción o información adicional,
6. directorio personal,
7. shell por defecto.

Ejemplo simplificado:

```text
juan:x:1001:1001:Juan Perez:/home/juan:/bin/bash
```

### Explicación de cada campo

* **juan**: nombre de usuario.
* **x**: indica que la contraseña cifrada no está aquí, sino en `/etc/shadow`.
* **1001**: UID del usuario.
* **1001**: GID del grupo principal.
* **Juan Perez**: descripción o nombre real.
* **/home/juan**: directorio personal.
* **/bin/bash**: shell por defecto.

### Observaciones importantes

* El usuario `root` tiene siempre **UID 0**.
* Los usuarios normales suelen empezar a partir del UID 1000.
* Algunos usuarios del sistema no están pensados para iniciar sesión de forma interactiva.
* Cuando una cuenta tiene una shell como `nologin`, se usa normalmente para servicios y no para acceso directo.

---

### `/etc/shadow`

Este archivo almacena las contraseñas cifradas de los usuarios y otra información relacionada con su validez o caducidad.

Solo puede ser leído por el administrador o por procesos autorizados.

Puede verse su existencia con:

```bash
ls -l /etc/shadow
```

---

### `/etc/group`

Contiene la definición de los grupos del sistema.

Se puede consultar con:

```bash
cat /etc/group
```

Cada línea suele contener cuatro campos:

1. nombre del grupo,
2. marcador interno,
3. GID,
4. usuarios que pertenecen al grupo como miembros secundarios.

Ejemplo:

```text
alumnado:x:1002:juan,ana,pablo
```

Esto indica que `juan`, `ana` y `pablo` pertenecen como miembros secundarios al grupo `alumnado`.

---

## 2.8. Eliminar usuarios y grupos

### Eliminar un usuario

Para eliminar una cuenta:

```bash
sudo userdel nombre_usuario
```

Ejemplo:

```bash
sudo userdel juan
```

Esto elimina la cuenta, pero puede dejar intacto el directorio personal.

Si además se quiere eliminar su *home*:

```bash
sudo userdel -r juan
```

La opción `-r` elimina también su directorio personal y otros archivos asociados.

### Eliminar un grupo

Para borrar un grupo:

```bash
sudo groupdel nombre_grupo
```

Ejemplo:

```bash
sudo groupdel alumnado
```

Un grupo no podrá eliminarse si sigue siendo necesario para algún usuario o proceso.

---

## 2.9. Modificación de usuarios

El comando `usermod` permite cambiar determinados parámetros de una cuenta ya existente.

Sintaxis general:

```bash
sudo usermod opciones nombre_usuario
```

Algunas opciones frecuentes son:

* `-d` cambia el directorio personal.
* `-g` cambia el grupo principal.
* `-m` mueve el contenido del directorio antiguo al nuevo cuando se usa junto con `-d`.

Ejemplo:

```bash
sudo usermod -d /home/juan_nuevo -m juan
```

Esto cambia el directorio personal de `juan` a `/home/juan_nuevo` y mueve allí su contenido.

---

## 2.10. Añadir usuarios a grupos secundarios

Un usuario puede añadirse a un grupo secundario mediante:

```bash
sudo adduser usuario grupo
```

Ejemplo:

```bash
sudo adduser juan alumnado
```

A partir de ese momento, `juan` seguirá teniendo su grupo principal, pero además pertenecerá al grupo suplementario `alumnado`.

---

## 2.11. Consultar los grupos de un usuario

Para ver a qué grupos pertenece el usuario actual:

```bash
groups
```

También puede consultarse un usuario concreto:

```bash
groups juan
```

Esta información es útil para comprobar permisos y acceso a recursos.

---

## 2.12. Propietario y grupo propietario de archivos

Cada archivo y cada directorio en Linux tiene:

* un **usuario propietario**,
* y un **grupo propietario**.

Esto puede verse con:

```bash
ls -l
```

Ejemplo de salida:

```text
-rw-r--r-- 1 juan alumnado 245 mar 12 10:30 ejemplo.txt
```

En este caso:

* el propietario es `juan`,
* el grupo propietario es `alumnado`.

### Cambiar propietario

Para cambiar el usuario propietario de un archivo:

```bash
sudo chown nuevo_usuario fichero
```

Ejemplo:

```bash
sudo chown ana ejemplo.txt
```

### Cambiar grupo propietario

Para cambiar el grupo propietario:

```bash
sudo chgrp nuevo_grupo fichero
```

Ejemplo:

```bash
sudo chgrp profesores ejemplo.txt
```

### Cambiar ambos a la vez

```bash
sudo chown usuario:grupo fichero
```

Ejemplo:

```bash
sudo chown ana:profesores ejemplo.txt
```

### Cambio recursivo

Para aplicar el cambio a un directorio y a todo su contenido:

```bash
sudo chown -R ana:profesores carpeta/
```

La opción `-R` significa **recursivo**.

---

## 2.13. `adduser` y `useradd`

En Debian es habitual utilizar `adduser` en lugar de `useradd` en un contexto didáctico o de administración básica, porque `adduser` es más cómodo y automatiza más tareas. `useradd` suele ser una orden de más bajo nivel y requiere más parámetros para lograr el mismo resultado.

Para los primeros pasos en administración de usuarios, lo más recomendable es trabajar con `adduser`.

---

# 3. Discos, particiones y montaje

## 3.1. Conceptos básicos

Para trabajar correctamente con dispositivos de almacenamiento en Linux conviene diferenciar tres conceptos:

* **disco**: dispositivo físico o virtual de almacenamiento,
* **partición**: división lógica del disco,
* **sistema de archivos**: estructura que organiza los datos dentro de una partición.

Además, para poder utilizar una partición en Linux es necesario **montarla**, es decir, asociarla a un directorio del sistema.

A diferencia de Windows, Linux no utiliza letras de unidad como `C:` o `D:`. Todo el sistema se organiza como un único árbol de directorios que parte de `/`.

---

## 3.2. Representación de dispositivos en `/dev`

Los dispositivos se representan mediante archivos especiales dentro del directorio `/dev`.

Ejemplos habituales:

* `/dev/sda`: primer disco.
* `/dev/sdb`: segundo disco.
* `/dev/sdc`: tercer disco.

Las particiones se identifican añadiendo un número:

* `/dev/sda1`
* `/dev/sda2`
* `/dev/sdb5`

De forma general:

* las particiones `1` a `4` suelen corresponder a particiones primarias,
* las particiones a partir de `5` suelen ser lógicas.

Otros ejemplos:

* `/dev/sr0`: unidad óptica CD/DVD.
* `/dev/fd0`: disquetera, en sistemas que la tengan.

Para listar discos y particiones detectados:

```bash
ls -l /dev/sd*
```

---

## 3.3. Puntos de montaje

Un **punto de montaje** es el directorio donde se “conecta” una partición o dispositivo para poder acceder a su contenido.

Los lugares más habituales son:

* `/media`: usado frecuentemente para montajes automáticos.
* `/mnt`: usado tradicionalmente para montajes manuales.

Sin embargo, técnicamente se puede montar en cualquier directorio vacío.

---

## 3.4. Sistemas de archivos comunes

Un sistema de archivos determina cómo se almacenan y organizan los datos dentro de una partición.

Algunos sistemas de archivos frecuentes son:

* `ext2`
* `ext3`
* `ext4`
* `vfat`
* `ntfs`
* `iso9660`

### Breve explicación

* **ext4**: sistema de archivos habitual en Linux.
* **vfat**: asociado normalmente a FAT32.
* **ntfs**: muy usado en Windows.
* **iso9660**: formato típico de CD-ROM.

---

## 3.5. Montar dispositivos

La sintaxis general es:

```bash
sudo mount dispositivo punto_de_montaje
```

Antes de montar, el directorio de destino debe existir.

Ejemplo:

```bash
sudo mkdir /mnt/datos
sudo mount /dev/sdb1 /mnt/datos
```

A partir de ese momento, el contenido de `/dev/sdb1` estará accesible desde `/mnt/datos`.

### Especificar el tipo de sistema de archivos

Si es necesario indicar el tipo:

```bash
sudo mount -t tipo dispositivo punto_de_montaje
```

Ejemplo:

```bash
sudo mount -t iso9660 /dev/sr0 /mnt/cdrom
```

---

## 3.6. Ver sistemas montados

El comando:

```bash
df -h
```

muestra los sistemas de archivos montados, el espacio total, el espacio usado, el libre y el punto de montaje.

La opción `-h` significa **human readable**, es decir, salida legible en KB, MB o GB.

---

## 3.7. Desmontar dispositivos

Antes de extraer un dispositivo es necesario desmontarlo correctamente. Esto garantiza que no queden datos pendientes de escritura y evita corrupción o pérdida de información.

Puede desmontarse por dispositivo:

```bash
sudo umount /dev/sdb1
```

o por punto de montaje:

```bash
sudo umount /mnt/datos
```

Es importante recordar que el comando es `umount`, no `unmount`.

---

## 3.8. Montaje automático con `/etc/fstab`

El archivo `/etc/fstab` define sistemas de archivos que deben montarse automáticamente al iniciar el sistema o cuando se soliciten.

Puede consultarse con:

```bash
cat /etc/fstab
```

La estructura general de cada línea es:

```text
dispositivo punto_de_montaje tipo opciones dump pass
```

Ejemplo:

```text
/dev/sdb1 /mnt/datos ext4 defaults 0 0
```

### Significado de los campos

* **dispositivo**: partición o sistema de archivos.
* **punto_de_montaje**: directorio donde se montará.
* **tipo**: sistema de archivos.
* **opciones**: modo de montaje.
* **dump**: parámetro relacionado con copias de seguridad.
* **pass**: parámetro relacionado con la comprobación del sistema de archivos al arrancar.

### Opciones habituales

* `rw`: lectura y escritura.
* `ro`: solo lectura.
* `user`: permite que un usuario normal monte ese dispositivo.
* `nouser`: solo el administrador puede montarlo.
* `defaults`: aplica un conjunto de opciones por defecto.

Para consultar más información:

```bash
man fstab
```

---

# 4. Administración de particiones

## 4.1. El comando `fdisk`

`fdisk` es una herramienta en modo texto para visualizar y administrar particiones. Aunque existen herramientas gráficas más cómodas, `fdisk` sigue siendo muy útil en servidores, sistemas sin entorno gráfico o sesiones remotas.

### Ver todas las particiones

```bash
sudo fdisk -l
```

### Ver las particiones de un disco concreto

```bash
sudo fdisk -l /dev/sda
```

### Entrar en modo interactivo

```bash
sudo fdisk /dev/sda
```

---

## 4.2. Órdenes básicas dentro de `fdisk`

Una vez dentro de `fdisk`, algunas órdenes frecuentes son:

* `m`: muestra ayuda.
* `p`: muestra la tabla de particiones.
* `n`: crea una nueva partición.
* `d`: elimina una partición.
* `w`: guarda los cambios y sale.
* `q`: sale sin guardar.

---

## 4.3. Formatear una partición

Después de crear una partición, normalmente es necesario formatearla para poder utilizarla.

Se utiliza `mkfs`:

```bash
sudo mkfs -t ext4 /dev/sda3
```

Esto crea un sistema de archivos `ext4` en la partición `/dev/sda3`.

En general, el proceso suele ser:

1. crear la partición,
2. formatearla,
3. crear el punto de montaje,
4. montarla,
5. y, si se desea, añadirla a `/etc/fstab`.

---

# 5. Permisos de archivos y directorios

## 5.1. Estructura de permisos

Al ejecutar:

```bash
ls -l
```

aparece información detallada de archivos y directorios. El primer bloque de caracteres representa el tipo y los permisos.

Ejemplo:

```text
-rwxr-xr--
```

El primer carácter indica el tipo:

* `-` archivo normal,
* `d` directorio,
* `l` enlace simbólico.

Los nueve caracteres siguientes se agrupan en tres bloques de tres:

* permisos del propietario,
* permisos del grupo,
* permisos del resto de usuarios.

---

## 5.2. Significado de los permisos

Los permisos básicos son:

* `r`: lectura,
* `w`: escritura,
* `x`: ejecución,
* `-`: ausencia de permiso.

### En archivos

* **lectura**: permite ver el contenido.
* **escritura**: permite modificarlo.
* **ejecución**: permite ejecutarlo como programa o script.

### En directorios

* **lectura**: permite listar su contenido.
* **escritura**: permite crear o borrar entradas dentro del directorio.
* **ejecución**: permite acceder al directorio y atravesarlo.

Este último punto es muy importante: en un directorio, el permiso `x` no significa “ejecutar” como un programa, sino “poder entrar o acceder”.

---

## 5.3. Cambio de permisos con `chmod`

Solo el propietario del archivo o el administrador pueden cambiar sus permisos.

### Sintaxis general

```bash
chmod permisos fichero
```

---

## 5.4. Notación octal

Cada permiso tiene un valor numérico:

* `r = 4`
* `w = 2`
* `x = 1`

Cada bloque se calcula sumando los valores presentes.

Por ejemplo:

* `rwx = 7`
* `rw- = 6`
* `r-x = 5`
* `r-- = 4`

Si un archivo tiene permisos:

```text
rw-r-x--x
```

su valor octal es:

* `rw- = 6`
* `r-x = 5`
* `--x = 1`

Resultado:

```bash
chmod 651 fichero
```

---

## 5.5. Notación simbólica

También es posible cambiar permisos con letras:

* `u`: usuario propietario.
* `g`: grupo.
* `o`: otros.
* `a`: todos.

Operadores:

* `+`: añade permisos.
* `-`: elimina permisos.
* `=`: establece exactamente esos permisos.

Ejemplos:

```bash
chmod o+w fichero
chmod go-w fichero
chmod u+x script.sh
```

### Explicación

* `o+w`: añade permiso de escritura a otros.
* `go-w`: elimina escritura a grupo y otros.
* `u+x`: añade ejecución al propietario.

---

# 6. Procesos del sistema

## 6.1. Qué es un proceso

Un **proceso** es un programa que está en ejecución. Cuando se abre una aplicación, se lanza un comando o se ejecuta un servicio, el sistema crea uno o varios procesos.

Cada proceso tiene, entre otros datos:

* un identificador numérico llamado **PID**,
* un proceso padre, identificado por **PPID**,
* un usuario propietario,
* un consumo de recursos,
* y un estado.

---

## 6.2. Arranque del sistema

Durante el arranque del equipo intervienen varios elementos:

1. El firmware del equipo detecta el hardware.
2. Se carga el gestor de arranque, habitualmente **GRUB**.
3. Se carga el **kernel** de Linux.
4. Se inicia el proceso principal del sistema.
5. A partir de él se van creando el resto de procesos y servicios.

Comprender esto ayuda a entender que el sistema está formado por numerosos procesos relacionados jerárquicamente.

---

## 6.3. Ver procesos con `ps`

Para mostrar procesos:

```bash
ps -ef
```

Esta orden ofrece información como:

* usuario,
* PID,
* PPID,
* terminal,
* tiempo de CPU,
* comando ejecutado.

Versión más detallada:

```bash
ps -efl
```

---

## 6.4. Finalizar procesos con `kill`

Para terminar un proceso se utiliza:

```bash
kill PID
```

o, si es necesario forzar su finalización:

```bash
kill -9 PID
```

### Diferencia entre ambas

* `kill PID` envía una señal de finalización estándar.
* `kill -9 PID` fuerza la finalización inmediata.

La señal `-9` debe usarse con prudencia, ya que no permite al proceso cerrarse de forma ordenada.

---

## 6.5. El comando `top`

Para monitorizar procesos en tiempo real:

```bash
top
```

Muestra información dinámica del sistema, como:

* carga del procesador,
* uso de memoria,
* procesos más activos,
* PID y usuario de cada proceso.

Es una herramienta muy útil para detectar procesos que consumen demasiados recursos.

---

## 6.6. Prioridades de procesos: `nice` y `renice`

Linux permite ajustar la prioridad con la que se ejecuta un proceso.

### Ejecutar un proceso con cierta prioridad

```bash
nice comando
```

o bien:

```bash
nice -n prioridad comando
```

Ejemplo:

```bash
nice -n 10 yes
```

Un valor más alto implica menor prioridad.

### Cambiar prioridad a un proceso ya iniciado

```bash
renice prioridad -p PID
```

Ejemplo:

```bash
renice 5 -p 2345
```

Los usuarios normales tienen limitaciones a la hora de asignar prioridades más favorables; el administrador puede hacer más cambios.

---

# 7. Información del sistema

## 7.1. Información del kernel con `uname`

Para ver la versión del kernel:

```bash
uname -r
```

Para mostrar más información:

```bash
uname -a
```

Esto puede incluir nombre del sistema, kernel, arquitectura y otros datos del entorno.

---

## 7.2. Memoria con `free`

Para consultar la memoria RAM y la memoria de intercambio:

```bash
free -h
```

La salida indica:

* memoria total,
* memoria usada,
* memoria libre,
* swap.

La opción `-h` muestra la información en formato legible.

---

## 7.3. Información del procesador con `lscpu`

```bash
lscpu
```

Permite consultar datos como:

* arquitectura,
* número de CPU lógicas,
* núcleos,
* hilos,
* frecuencia,
* virtualización,
* caché.

---

## 7.4. Uso del espacio con `df`

```bash
df -h
```

Muestra el tamaño y la ocupación de los sistemas de archivos montados.

---

## 7.5. Tamaño de directorios con `du`

Para consultar cuánto ocupa un directorio:

```bash
du -sh directorio
```

* `-s`: muestra solo el total.
* `-h`: lo hace legible.

Ejemplo:

```bash
du -sh /home/juan
```

---

## 7.6. El directorio `/proc`

`/proc` es un pseudo-sistema de archivos que proporciona información del sistema y de los procesos en tiempo real.

Ejemplos:

```bash
cat /proc/cpuinfo
cat /proc/meminfo
```

También existen subdirectorios asociados a cada proceso activo, identificados por su PID.

---

# 8. Registros del sistema

## 8.1. Logs del sistema

Linux almacena información sobre eventos y funcionamiento del sistema en archivos de registro, también llamados **logs**.

El directorio principal es:

```bash
/var/log
```

Dentro pueden encontrarse archivos y subdirectorios asociados a distintos servicios.

---

## 8.2. Consultar registros

Un archivo muy importante es el registro general del sistema, que puede revisarse con herramientas como `tail`.

Ejemplo:

```bash
tail -20 /var/log/syslog
```

Esto muestra las últimas 20 líneas del archivo. Es útil porque los archivos de log pueden crecer mucho y normalmente interesa revisar los eventos más recientes.

---

# 9. Automatización de tareas con cron

## 9.1. Qué es cron

`cron` es el sistema de programación de tareas de Linux. Permite ejecutar comandos o scripts automáticamente en momentos determinados.

Es muy útil para:

* copias de seguridad,
* limpieza de archivos temporales,
* generación de informes,
* apagado o reinicio programado,
* ejecución periódica de scripts.

---

## 9.2. Comprobar si cron está activo

Puede comprobarse con:

```bash
ps -ef | grep cron
```

Aquí aparece un concepto importante: la **tubería** (`|`).

Una tubería toma la salida de un comando y la envía como entrada al siguiente.

En este caso:

* `ps -ef` lista procesos,
* `grep cron` filtra solo las líneas que contienen la palabra `cron`.

---

## 9.3. Editar tareas programadas

### Tabla global del sistema

```bash
sudo nano /etc/crontab
```

### Tabla del usuario actual

```bash
crontab -e
```

Esta segunda opción permite que cada usuario mantenga sus propias tareas programadas.

---

## 9.4. Sintaxis de cron

Cada línea de cron tiene esta estructura:

```text
m h dom mon dow command
```

Donde:

* `m`: minuto.
* `h`: hora.
* `dom`: día del mes.
* `mon`: mes.
* `dow`: día de la semana.
* `command`: orden o script a ejecutar.

Ejemplo:

```text
30 8 * * 1 /home/juan/backup.sh
```

Esto ejecutaría el script `backup.sh` todos los lunes a las 08:30.

---

## 9.5. Comodines habituales

En cron se utilizan expresiones especiales:

* `*` → todos los valores.
* `3-6` → desde 3 hasta 6.
* `3,6` → valores concretos 3 y 6.
* `*/10` → cada 10 unidades.

Ejemplo:

```text
*/15 * * * * /home/juan/script.sh
```

Ejecuta el script cada 15 minutos.

---

## 9.6. Consultar o eliminar tareas programadas

Para listar las tareas del usuario:

```bash
crontab -l
```

Para eliminar la tabla de tareas del usuario:

```bash
crontab -r
```

---

## 9.7. Directorios de ejecución periódica

En algunos sistemas existen directorios específicos para tareas periódicas:

* `/etc/cron.hourly`
* `/etc/cron.daily`
* `/etc/cron.weekly`
* `/etc/cron.monthly`

Los scripts colocados en ellos se ejecutan con esa periodicidad.

---

# 10. Scripts de shell

## 10.1. Qué es un script

Un **script** es un archivo de texto que contiene varias órdenes que se ejecutan de forma secuencial. Permite automatizar tareas repetitivas y agrupar varios comandos en una sola ejecución.

En Linux, los scripts suelen usar la shell `bash`.

La primera línea habitual es:

```bash
#!/bin/bash
```

Esta línea indica qué intérprete debe utilizarse para ejecutar el script.

---

## 10.2. Crear un script sencillo

Ejemplo de creación con `nano`:

```bash
nano ejemplo.sh
```

Contenido posible:

```bash
#!/bin/bash
mkdir prueba
echo "Hola" > prueba/mensaje.txt
clear
ls -l prueba
cat prueba/mensaje.txt
```

### Explicación de los comandos

* `mkdir prueba` crea un directorio llamado `prueba`.
* `echo "Hola" > prueba/mensaje.txt` escribe el texto `Hola` en un archivo.
* `clear` limpia la pantalla.
* `ls -l prueba` lista el contenido del directorio.
* `cat prueba/mensaje.txt` muestra el contenido del archivo.

---

## 10.3. Dar permiso de ejecución

Para ejecutar directamente un script, debe tener permiso `x`.

```bash
chmod u+x ejemplo.sh
```

Esto añade permiso de ejecución al propietario.

---

## 10.4. Ejecutar el script

Si el script está en el directorio actual:

```bash
./ejemplo.sh
```

El prefijo `./` indica que el archivo está en el directorio actual.

Si no tiene permiso de ejecución, el sistema devolverá un error de **permiso denegado**.

---

