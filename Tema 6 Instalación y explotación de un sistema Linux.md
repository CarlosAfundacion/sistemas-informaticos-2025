# Instalación y explotación de un sistema GNU/Linux con Debian

## 0. Objetivos y enfoque del tema

En este tema se trabaja con un sistema GNU/Linux real instalado en una **máquina virtual**. El objetivo no es “aprender una lista de comandos”, sino comprender **cómo está organizado Linux**, cómo se **instala**, cómo se **administra** (usuarios, permisos, software) y cómo se trabaja con soltura en **terminal**.

Debian se utiliza como referencia por ser una distribución **muy estable**, ampliamente usada en entornos profesionales, y porque su sistema de paquetes (APT) es estándar en multitud de sistemas derivados. 

---

## 1. Unix y GNU/Linux: historia y conceptos fundamentales

### 1.1. Contexto histórico (por qué nace Linux)

Durante los años 80, en universidades y centros de investigación era habitual trabajar con **UNIX** en grandes ordenadores. Los estudiantes programaban en C y aprendían comandos típicos de ese entorno. El problema era que, con la llegada del PC, no existía un sistema “doméstico” suficientemente parecido a UNIX para practicar en casa.

En 1991, Linus Torvalds desarrolla un núcleo (kernel) para PC con una interfaz y filosofía similares a UNIX, de manera que se pudiesen practicar herramientas y comandos de ese estilo en ordenadores personales. Ese núcleo se denominó **Linux**. La comunidad comenzó a colaborar, aportando mejoras, hasta llegar a versiones estables. 

### 1.2. ¿Qué significa GNU/Linux?

Aquí hay una idea clave:

* **Linux** = *kernel* (núcleo).
* **GNU** = conjunto de herramientas y utilidades (compilador, shell, comandos básicos, librerías, etc.) desarrolladas por el proyecto GNU.
* **GNU/Linux** = kernel Linux + herramientas GNU + software adicional que convierte todo eso en un sistema operativo utilizable.

Por eso se habla de GNU/Linux: porque el sistema completo no es solo el kernel.

### 1.3. Linux no es UNIX, aunque se parezcan

GNU/Linux usa en general **comandos y filosofía tipo UNIX**, pero **no es el mismo sistema**:

* UNIX tradicionalmente ha sido un sistema **propietario** (con variantes comerciales).
* GNU/Linux es **software libre** y su desarrollo se apoya en comunidad y empresas.

También es relevante entender que otros sistemas conocidos (por ejemplo macOS) están basados en tecnologías tipo UNIX, mientras que Android utiliza un kernel Linux (con una base distinta en el espacio de usuario). 

---

## 2. Distribuciones GNU/Linux

### 2.1. ¿Qué es una distribución?

Una distribución GNU/Linux es una “forma empaquetada” de entregar un sistema GNU/Linux listo para instalar. Consta de dos partes principales (tal como aparece en el material): 

1. **Kernel**: el núcleo que gestiona CPU, memoria, procesos, dispositivos y sistema de archivos.
2. **Software que acompaña**: instalador, gestor de paquetes, herramientas de administración, entorno gráfico, aplicaciones, etc.

Es importante remarcar que el kernel por sí solo no es un “sistema utilizable” para la mayoría de usuarios: hace falta todo el software alrededor.

### 2.2. ¿Por qué hay tantas distribuciones?

Porque, al ser software libre:

* cualquiera puede crear una distribución,
* elegir un instalador distinto,
* cambiar el escritorio,
* seleccionar paquetes por defecto,
* definir una política de actualizaciones (más estable o más “puntera”).

Por eso hay muchísimas distribuciones: cambian el objetivo y el público.

### 2.3. Familias principales de distribuciones

En el material se mencionan tres “raíces” históricas importantes: **Red Hat**, **Debian** y **Slackware**. 
En la práctica, muchas distribuciones actuales derivan de Debian o de Red Hat.

**Debian** es especialmente relevante porque:

* es conocida por su **estabilidad**,
* tiene un sistema de paquetes muy sólido (APT + paquetes `.deb`),
* es base de muchas derivadas (Ubuntu, Mint, etc.), aunque aquí trabajamos directamente con Debian.

---

## 3. Entorno gráfico en GNU/Linux: X11, servidor gráfico y escritorio

### 3.1. Qué es el “entorno gráfico”

En Linux, la interfaz de ventanas no es “el sistema operativo” en sí, sino una capa adicional. Tradicionalmente esta capa se ha basado en **X11** (X-Windows), desarrollado desde los años 80, y organizado con un modelo **cliente-servidor**: 

* **Servidor X**: gestiona el acceso a pantalla, teclado y ratón; se encarga de mostrar lo gráfico.
* **Clientes X**: son las aplicaciones gráficas (navegador, editor, terminal, etc.) que piden al servidor X que dibuje ventanas, textos, botones…

Esto explica por qué en Linux existen muchas combinaciones posibles de:

* servidor gráfico (X11 / X.Org, y también Wayland en sistemas actuales),
* gestor de ventanas,
* entorno de escritorio.

### 3.2. Entorno de escritorio vs gestor de ventanas

Conviene distinguir:

* **Gestor de ventanas**: controla colocación, tamaño, bordes, botones de minimizar/maximizar, etc.
* **Entorno de escritorio (Desktop Environment)**: incluye gestor de ventanas + paneles + menú + configuración + aplicaciones integradas.

Ejemplos típicos (mencionados en el material): **GNOME**, **KDE**, **XFCE**. 

### 3.3. Elección para una máquina virtual

Para máquinas virtuales interesa un entorno que consuma pocos recursos. Por eso se recomienda **XFCE**:

* es ligero,
* estable,
* suficientemente completo para uso diario,
* reduce consumo de RAM y CPU frente a entornos más pesados.

---

## 4. Virtualización y preparación de la máquina virtual

### 4.1. Qué es una máquina virtual y por qué se usa

Una máquina virtual es un ordenador “simulado” por software:

* tiene su “disco duro” (un archivo en el host),
* su RAM asignada,
* su tarjeta de red virtual,
* su CPU virtual.

Ventajas didácticas:

* permite instalar sin riesgo de romper el sistema real,
* facilita repetir instalaciones,
* estandariza el entorno de trabajo.

### 4.2. Recomendación de recursos para Debian en VirtualBox

Para Debian con XFCE (entorno fluido y ligero):

* **RAM**: 1 GB mínimo, 2 GB recomendado.
* **CPU**: 1 núcleo (mínimo), 2 (recomendado si el equipo lo permite).
* **Disco**: 20–30 GB dinámico.
* **Vídeo**: 64–128 MB; activar aceleración 3D si procede.

La idea es no “comerse” los recursos del equipo anfitrión para poder abrir simultáneamente otra VM cuando sea necesario.

---

## 5. Instalación de Debian (explicación completa del proceso)

### 5.1. ISO e instalador

La imagen recomendada es Debian **amd64** (64 bits).
En Debian se usan instaladores tipo “netinst” (ISO pequeña) o DVD completo. Para clase suele preferirse netinst por ligereza y porque permite seleccionar exactamente qué instalar.

### 5.2. Primeras pantallas

Normalmente se usa **Graphical install** (instalación gráfica).

Se configuran:

* **Idioma** (afecta al idioma del instalador y del sistema).
* **País / región** (afecta a zona horaria y opciones regionales).
* **Teclado** (importante para símbolos como `|`, `-`, `/`, `@`, etc.).

### 5.3. Red durante la instalación

El instalador detecta la tarjeta de red virtual:

* si la red está en modo NAT (por defecto), suele obtener IP por DHCP.
* si no hay red, algunas partes del instalador pueden limitarse (por ejemplo, elegir repositorios online).

En una instalación típica:

* **hostname**: nombre del equipo en red (ej. `debian-aula01`).
* **dominio**: se puede dejar vacío si no hay un dominio real (solo tiene sentido en redes gestionadas).

### 5.4. Usuarios: root y usuario normal (explicación detallada)

En Linux no se recomienda trabajar continuamente con permisos de administrador.

* **root**: es el superusuario. Puede hacer cualquier operación.
* **usuario normal**: trabaja en el día a día con permisos limitados.

Durante la instalación, Debian puede pedir:

1. contraseña de **root**
2. creación de un **usuario normal**

Una práctica habitual en clase:

* definir contraseña de root (para comprender la administración),
* crear un usuario normal (por ejemplo `alumno`) para el trabajo diario.

Esto permite explicar claramente:

* por qué root es potente pero peligroso,
* por qué se usan mecanismos como `sudo`.

### 5.5. Particionado del disco: qué significa realmente

El disco virtual se particiona igual que un disco físico. El instalador ofrece particionado guiado o manual.

**Opción recomendada para un nivel inicial**: particionado guiado “usar todo el disco” y “todos los ficheros en una partición”.

Aun así, hay conceptos que deben entenderse:

#### 5.5.1. Punto de montaje

En Linux no “entras en C:” o “D:”. En Linux:

* hay un árbol único que empieza en `/` (raíz),
* cada partición se “inserta” en un directorio: eso es **montar**.

Ejemplo:

* montar una partición en `/home` significa que el contenido de esa partición se verá dentro de `/home`.

#### 5.5.2. Partición raíz `/`

La partición montada en `/` contiene:

* el sistema,
* programas,
* configuraciones,
* y, si no se separa, todo lo demás.

#### 5.5.3. Swap (memoria de intercambio)

La **swap** es un área en disco que el sistema puede usar como extensión de la RAM cuando hay presión de memoria.

Ideas clave:

* Es mucho más lenta que la RAM.
* Evita algunos bloqueos cuando la RAM se llena.
* En equipos modernos a veces se usa swapfile en lugar de partición.
* En una VM con poca RAM puede ser útil.

En el material se sugiere a veces swap del doble de RAM; hoy no siempre es necesario, pero como concepto didáctico es correcto: “swap ayuda cuando falta RAM”. 

### 5.6. Selección de software (lo que significa realmente)

Aquí se elige qué “tipo de sistema” se instala:

* **Sistema base**: imprescindible.
* **Entorno de escritorio**: si quieres interfaz gráfica.
* **Utilidades estándar del sistema**: herramientas básicas.

Recomendación: instalar **XFCE** + utilidades estándar.
SSH solo si se prevé usar administración remota pronto.

### 5.7. Cargador de arranque: GRUB

GRUB es el programa que permite arrancar el sistema:

* se instala al final,
* normalmente en el disco principal,
* es el primer software que se ejecuta tras BIOS/UEFI para cargar el kernel.

Aunque en una VM suele ser transparente, conviene saber que si GRUB falla el sistema no arranca.

---

## 6. Guest Additions (VirtualBox): qué aportan y por qué se instalan

Las Guest Additions son un conjunto de controladores y utilidades que mejoran la integración entre host y VM:

* ajuste dinámico de resolución,
* integración del ratón,
* portapapeles compartido,
* carpetas compartidas (si se configuran),
* mejor rendimiento gráfico.

En Debian, suele requerir:

* herramientas de compilación (`build-essential`),
* `dkms` (para mantener módulos ante actualizaciones del kernel),
* cabeceras del kernel (`linux-headers-...`).

La razón técnica: algunas partes se integran como módulos del kernel, y necesitan compilarse/ajustarse al kernel instalado.

---

## 7. Primeros pasos: estructura del sistema de archivos

### 7.1. Diferencias clave con Windows (explicadas)

El material aporta comparativas que conviene entender bien: 

1. **Un único árbol de directorios**

   * Windows: cada unidad tiene su propia raíz (C:, D:\…).
   * Linux: todo parte de `/`. Cualquier dispositivo (USB, CD, otra partición) se monta dentro del árbol.

2. **Mayúsculas/minúsculas**

   * En Linux, `hoja.txt` y `Hoja.txt` son archivos distintos.
   * Esto afecta a programación, rutas y comandos.

3. **Extensiones**

   * En Linux un archivo ejecutable no “necesita” `.exe`.
   * Lo que determina si algo se puede ejecutar son los **permisos**.

4. **Separador de rutas**

   * Linux usa `/` (ej. `/home/alumno`).
   * Las opciones de comandos suelen ir con `-` (ej. `ls -l`), no con `/`.

### 7.2. Directorios principales (qué son y para qué sirven)

Listado esencial (muy parecido al del material, adaptado a Debian): 

* **/bin**: comandos esenciales para el arranque y funcionamiento mínimo (en sistemas modernos parte se integra con `/usr/bin`, pero el concepto “comandos básicos” se mantiene).
* **/usr/bin**: gran parte de comandos y programas de usuario.
* **/etc**: configuración del sistema. En general son archivos de texto plano (por eso es administrable).
* **/home**: carpetas personales de usuarios normales.
* **/root**: carpeta personal de root.
* **/usr**: software y recursos de usuario (aplicaciones, librerías, etc.).
* **/tmp**: ficheros temporales. Se vacía periódicamente; no es para datos importantes.
* **/boot**: kernel y ficheros de arranque.
* **/dev**: dispositivos como archivos (discos, terminales, etc.). Ej.: `/dev/sda` (disco), `/dev/tty1` (terminal). Conceptualmente, “todo es un archivo” en Unix ayuda a entenderlo.
* **/mnt** y **/media**: puntos de montaje. Automático suele ir a `/media`, manual a `/mnt`.
* **/var**: datos variables (logs, colas, cachés, bases de datos de servicios…). Muy importante en servidores.
* **/var/log**: registros del sistema y servicios.

---

## 8. Terminal, shell y sesiones (consolas)

### 8.1. Qué es el shell

El **shell** es el intérprete de comandos: recibe órdenes, las interpreta, ejecuta y muestra resultados. El shell permite trabajar con:

* ficheros de configuración,
* automatización,
* administración,
* y diagnóstico del sistema.

Aunque exista interfaz gráfica, en Linux la terminal sigue siendo fundamental. 

### 8.2. Terminal gráfica vs TTY

En Linux existen varias “consolas”:

* **Terminal gráfica**: una ventana dentro del escritorio.
* **TTY (terminales virtuales)**: sesiones de texto accesibles con combinaciones tipo `Ctrl+Alt+F3`, etc.

En máquinas virtuales, la combinación puede requerir la tecla anfitrión de VirtualBox. El concepto importante es:

* puedes tener varias sesiones abiertas en paralelo,
* incluso con usuarios distintos,
* y alternar entre ellas.

### 8.3. Prompt (lo que significa lo que ves)

Ejemplo típico:
`usuario@equipo:directorio$`

* `usuario`: identidad actual.
* `equipo`: hostname.
* `directorio`: ruta actual (a veces `~`).
* `$` indica usuario normal; `#` indica root.

`~` representa el HOME del usuario:

* si el usuario es `alumno`, `~` equivale a `/home/alumno`.

Esto permite orientarse rápidamente.

---

## 9. Sintaxis de comandos y ayuda del sistema

### 9.1. Sintaxis general

`comando [opciones] [parámetros]` 

* **Comando**: acción principal (`ls`, `cp`, `apt`, etc.).
* **Opciones**: modifican el comportamiento (`-l`, `-a`, `-R`…).
* **Parámetros**: sobre qué actúa (ruta, archivo, patrón…).

Ejemplo:
`ls -la /home/alumno`

### 9.2. Ayuda: `--help` y `man`

* `comando --help`: ayuda rápida.
* `man comando`: manual completo. Se navega con teclas de avance/retroceso y se sale con `q`. 

Diferencia didáctica importante:

* `--help` sirve para recordar opciones.
* `man` sirve para entender a fondo y ver ejemplos/explicaciones.

---

## 10. Usuarios, root, sudo y cambios de identidad

### 10.1. root (superusuario)

`root` tiene permiso absoluto. Puede:

* leer y modificar cualquier archivo,
* instalar software,
* cambiar configuración del sistema,
* gestionar usuarios y permisos.

Esto implica riesgo:

* un comando mal ejecutado como root puede dañar el sistema.

### 10.2. sudo (ejecutar un comando como administrador)

`sudo comando` ejecuta un comando con privilegios de root.

Características:

* pide la contraseña del usuario (no la de root, si está configurado así),
* registra acciones (según configuración),
* permite dar privilegios de forma controlada.

Ejemplo:
`sudo apt update`

### 10.3. su (cambiar de usuario)

`su usuario` cambia la identidad de la sesión actual a otro usuario.

* pide la contraseña del usuario destino.

`su -` (o `su root`) suele usarse para entrar como root (si root tiene contraseña).

### 10.4. Por qué no conviene trabajar siempre como root (propietario y permisos)

Cada archivo tiene:

* propietario (usuario),
* grupo,
* permisos.

Si creas archivos como root dentro de tu HOME, luego tu usuario normal puede no poder modificarlos, generando problemas de trabajo. Este ejemplo aparece en el material y es didácticamente muy bueno: crear carpetas como root hace que sean de root. 

---

## 11. Comandos básicos del sistema (con explicación)

### 11.1. Comandos generales

* `passwd usuario`: cambia contraseña del usuario.
* `exit`: cierra sesión o sale de shell actual.
* `who`: muestra usuarios conectados.
* `echo "texto"`: imprime texto (y se usa mucho para redirecciones).
* `clear`: limpia pantalla.
* `poweroff` / `reboot`: apagar / reiniciar (normalmente requieren privilegios). 

### 11.2. Directorios y rutas

**Rutas absolutas**: empiezan por `/` (ej. `/home/alumno`).
**Rutas relativas**: dependen del directorio actual (ej. `Documentos`, `../alumno`).

Comandos:

* `pwd`: muestra ruta actual absoluta.
* `cd ruta`: cambia de directorio.

  * `cd ..` sube un nivel
  * `cd /` va a la raíz
* `mkdir nombre`: crea directorio.
* `rmdir nombre`: elimina directorio vacío.

### 11.3. Listado con `ls` (entender de verdad el resultado)

`ls` lista contenido.
Opciones relevantes (del material): 

* `-l` (long): formato largo (permisos, propietario, tamaño, fecha…)
* `-a`: incluye ocultos
* `-R`: recursivo (árbol)
* `-t`: orden por fecha
* `-r`: invierte orden

Ejemplo:
`ls -la`

Formato largo: ejemplo de salida:
`drwxr-xr-x 2 alumno alumno 4096 2012-04-15 23:43 Descargas` 

Interpretación:

1. `d` → es directorio (si fuese `-` sería archivo regular; `l` enlace simbólico; `b/c` dispositivos).
2. `rwxr-xr-x` → permisos (usuario/grupo/otros).
3. `2` → número de enlaces (concepto avanzado; basta saber que existe).
4. `alumno` propietario.
5. `alumno` grupo propietario (por defecto se crea un grupo con el mismo nombre del usuario).
6. `4096` tamaño (en directorios suele ser 4096 bytes por estructura interna).
7. fecha/hora.
8. nombre.

### 11.4. Archivos: crear, editar, ver, copiar, mover, borrar

* `touch archivo`: crea archivo vacío o actualiza fecha de modificación.
* Editar en terminal:

  * `nano archivo.txt` (editor sencillo).
* Ver contenido:

  * `cat archivo` (muestra todo)
  * `less archivo` (navegable)
  * `head archivo` (primeras líneas)
  * `tail archivo` (últimas líneas) 
* `cp origen destino`: copia.

  * `cp -R carpeta1 carpeta2`: copia recursiva.
* `mv origen destino`: mueve o renombra.
* `rm archivo`: borra archivo.

  * `rm -r carpeta`: borra carpeta con contenido.
  * `rm -rf carpeta`: fuerza sin preguntar (debe explicarse como comando peligroso).

### 11.5. `type` en Linux (no es como Windows)

En Linux, `type comando` indica dónde está el ejecutable o qué es exactamente ese comando (si es alias, builtin del shell, etc.). Ejemplo del material: `type touch` muestra que está en `/usr/bin/touch`. 

---

## 12. Redirecciones: stdout, stderr y guardado de resultados

Cuando ejecutas un comando, se generan dos salidas:

* **stdout**: salida estándar (resultado “normal”).
* **stderr**: salida de errores.

Operadores:

* `>` redirige stdout sobrescribiendo archivo.
* `>>` redirige stdout añadiendo al final.
* `2>` redirige stderr sobrescribiendo.
* `2>>` redirige stderr añadiendo al final. 

Ejemplo completo:
`ls -laR / > listado.txt 2> errores.txt`

Esto:

* guarda el listado en `listado.txt`,
* guarda los “permiso denegado” y similares en `errores.txt`.

Entender esto es clave porque permite:

* generar informes,
* depurar comandos,
* automatizar tareas.

---

## 13. Instalación de software y gestión de paquetes

### 13.1. Gestor de paquetes: qué problema resuelve

En Linux, instalar software no suele consistir en bajar un instalador manual, sino en usar un sistema que:

* descarga el paquete,
* instala dependencias automáticamente,
* registra qué se ha instalado,
* facilita actualizaciones y desinstalación.

A esto se le llama **gestor de paquetes**. 

### 13.2. APT en Debian (comandos esenciales)

Debian usa paquetes `.deb` y el gestor APT:

* `sudo apt update`: actualiza lista de paquetes desde repositorios.
* `sudo apt upgrade`: actualiza paquetes instalados.
* `sudo apt install paquete`: instala.
* `sudo apt remove paquete`: desinstala.
* `apt search texto`: busca paquetes por nombre/descripcion. 

### 13.3. Repositorios: qué son y dónde se configuran

Los repositorios son servidores (normalmente HTTP/HTTPS) que ofrecen paquetes verificados.

APT sabe dónde buscar porque lo tiene definido en:

* `/etc/apt/sources.list`
* y, a veces, archivos en `/etc/apt/sources.list.d/`

En un nivel inicial es suficiente con:

* saber que existen,
* entender que `apt update` “descarga el índice” de esos repositorios,
* y que sin repositorios correctos no se puede instalar/actualizar bien. 

### 13.4. Instalación sin gestor (concepto)

Se puede instalar un `.deb` manualmente con `dpkg -i`, pero esto no resuelve dependencias de forma cómoda. Por eso se prefiere APT. El material lo explica como alternativa y conviene que el alumnado entienda “por qué se evita”. 

### 13.5. Instalación desde código fuente (solo como idea)

Compilar desde código fuente (`./configure`, `make`, `make install`) existe, pero es un nivel más avanzado. En un temario inicial se menciona como posibilidad, explicando que:

* requiere herramientas de compilación,
* puede complicar el mantenimiento,
* y no se integra tan bien con actualizaciones como APT. 

---

## 14. Entorno gráfico: configuración básica del sistema (en Debian)
En Debian con XFCE (u otro escritorio) existe un panel de configuración desde el que se ajusta:

* red,
* dispositivos (pantalla, teclado),
* usuarios (si se tiene permisos),
* actualizaciones.

Lo importante es comprender que:

* muchas configuraciones “de usuario” se guardan en el HOME (archivos ocultos),
* configuraciones “de sistema” se guardan en `/etc` y requieren permisos de administrador.

---


