
# TAREA PRÁCTICA: SI-03 – Explotación de Windows 

## Objetivo general

Configurar un sistema Windows en máquina virtual con **doble arranque (dual boot)**, administrar particiones y realizar operaciones avanzadas desde **CMD** y mediante **archivo por lotes**.

## Entorno de trabajo

* **VirtualBox**
* Máquina virtual con **Windows 10** ya instalada (base)
* ISO de **Windows 10** disponible para instalar el segundo sistema
* Todas las operaciones de consola: **CMD como administrador** cuando se indique

## Normas de entrega

* Entregar un **PDF** con capturas y explicación breve de cada paso.
* Todas las capturas deben mostrar **lo necesario para verificar** que se ha hecho correctamente.

---

# PARTE A – Doble Windows + particiones + gestor de arranque (paso a paso)

## A.1 Comprobar si hay espacio suficiente

1. Arranca el Windows instalado.
2. Abre **Administración de discos**:

   * `Win + X` → **Administración de discos**
3. Comprueba si existe **espacio “Sin asignar”** suficiente para crear una nueva partición de **30 GB**.

   * Si **sí hay**: pasa al apartado **A.3**.
   * Si **no hay**: sigue el apartado **A.2 (ampliación del VDI)**.

---

## A.2 Ampliar el disco VDI en VirtualBox (si no hay espacio)

> Esto se hace **en el equipo anfitrión**, con la máquina virtual **apagada**.

1. Apaga el Windows correctamente (**Inicio → Apagar**).
2. En VirtualBox, verifica que la VM está **Apagada** (no “guardada”).
3. Abre el **Administrador de medios**:

   * VirtualBox → **Archivo → Herramientas → Administrador de medios**
4. Selecciona el disco `.vdi` de la máquina.
5. Aumenta el tamaño del disco (recomendación):

   * Si vas a instalar 2 Windows: deja el disco total en **60–70 GB**.
6. Guarda los cambios.
7. Arranca de nuevo la VM y vuelve a **Administración de discos**.
8. Verifica que ahora aparece espacio adicional como **“Sin asignar”** al final del disco.

 **Capturas obligatorias**

* VirtualBox mostrando el nuevo tamaño del disco.
* Administración de discos mostrando el espacio “Sin asignar”.

---

## A.3 Crear la partición para el segundo Windows (30 GB)

1. En **Administración de discos**, localiza el espacio **“Sin asignar”**.
2. Botón derecho → **Nuevo volumen simple**.
3. Indica el tamaño:

   * **30.000 MB aprox.** (o 30 GB).
4. Formato:

   * Sistema de archivos: **NTFS**
   * Etiqueta: `WIN_TARDE` (o similar)
5. Finaliza el asistente.

 **Captura obligatoria**

* Administración de discos mostrando claramente:

  * Partición principal (C:)
  * Nueva partición (30 GB)
  * Si queda más espacio sin asignar, también debe verse.

---

## A.4 Instalar el segundo Windows 10 en la partición creada

1. Apaga la máquina virtual.
2. En VirtualBox:

   * **Configuración → Almacenamiento**
   * Monta la **ISO de Windows 10** en la unidad óptica.
3. Arranca la VM y comienza la instalación.
4. En el asistente:

   * Selecciona **Instalación personalizada (avanzada)**.
5. Cuando te pida dónde instalar:

   * Selecciona la partición de **30 GB** creada anteriormente.
   * Si aparece la partición con nombre, mejor; si no, identifica por tamaño.
6. Completa la instalación.
7. Crea el usuario solicitado por el profesor (por ejemplo: segundo apellido).

 **Capturas obligatorias**

* Selección de “Instalación personalizada”.
* Pantalla de selección de partición (donde se vea el tamaño de 30 GB).
* Inicio de sesión del nuevo Windows ya instalado.

---

## A.5 Verificar gestor de arranque y renombrar entradas con BCDEDIT

1. Reinicia la máquina.
2. Comprueba que aparece el **gestor de arranque** con dos Windows.
3. Arranca el segundo Windows (el recién instalado).
4. Abre **CMD como administrador**:

   * Buscar “cmd” → botón derecho → **Ejecutar como administrador**
5. Ejecuta:

   * `bcdedit`
6. Localiza las entradas de arranque y cambia el nombre para que quede:

   * `Windows 10 Turno mañana` (primer Windows)
   * `Windows 10 Turno tarde` (segundo Windows)
7. Reinicia para comprobar que el gestor muestra los nombres correctos.

 **Capturas obligatorias**

* Gestor de arranque con las dos entradas renombradas.
* CMD mostrando `bcdedit` antes y después (o al menos después, con evidencias claras).

---

## A.6 Comprobación de particiones (activo/sistema/arranque)

1. Arranca **Windows 10 Turno tarde**.
2. Abre:

   * **Explorador → Este equipo** (para ver letras de unidades)
   * **Administración de discos**
3. Responde en el PDF:

   * ¿Cuál es la partición **activa**?
   * ¿Cuál es la partición de **sistema**?
   * ¿Cuál es la partición de **arranque**?
4. Justifica cada respuesta con captura de Administración de discos.

 **Capturas obligatorias**

* Administración de discos donde se vean claramente las etiquetas “Sistema”, “Arranque”, “Activo”, etc.

---

# PARTE B – CMD (guía paso a paso)

> **Todo lo de esta parte se hace en CMD como administrador**, salvo que el profesor indique lo contrario.

## B.1 Preparación

1. Abre CMD como administrador.
2. Ejecuta `whoami` para verificar usuario.
3. Ejecuta `cd \` para situarte en la raíz.

 Captura: CMD como administrador.

---

## B.2 Tareas (realiza y documenta)

Realiza cada apartado y tras completarlo **haz una captura** donde se vea el comando y el resultado.

1. Cambiar la fecha del sistema a una fecha indicada por el profesor.
2. Cambiar la etiqueta del volumen C: al formato:

   * `WIN_APELLIDO`
3. Listar (incluyendo subdirectorios de C:) todos los archivos/directorios cuyo nombre empiece por `w`.
4. Crear la siguiente estructura (directorios en mayúsculas, archivos en minúsculas) y escribir texto dentro de los ficheros:

   * `C:\SISTEMAS\DOS\MANUAL\manual.txt` (**ruta absoluta**)
   * `EJERCICIOS\hoja1.txt` y `EJERCICIOS\hoja2.txt` (**ruta relativa**, dentro de donde indique el profesor)
5. Desde `SISTEMAS`, listar con **ruta absoluta** los elementos cuyo **tercer carácter sea “s”** y con cualquier extensión.
6. Desde `SISTEMAS`, listar con **ruta relativa** los `.exe` en `WINDOWS\SYSTEM32`.
7. Copiar todos los ficheros del directorio `EJERCICIOS` a `C:\` con **una sola orden**.
8. Crear `C:\PRACTICA2` y copiar `LABEL.EXE` desde `SYSTEM32` a esa carpeta renombrándolo como `ETIQUETA.EXE` (en **dos comandos**).
9. Copiar todos los archivos de `EJERCICIOS` a `PRACTICA2` con **una sola orden**.
10. Copiar los `.txt` de `PRACTICA2` a `DOS` cambiando extensión a `.sol` con **una sola orden**.
11. Renombrar (ruta absoluta) `hoja1.sol` a `hoja.dat`.
12. Copiar el directorio `SISTEMAS` a `CopySist` (incluyendo subdirectorios) con una orden.
13. Cambiar a tu carpeta de usuario usando ruta absoluta.
14. Sin cambiar de carpeta, copiar (ruta relativa) solo los archivos de `C:\Windows` a `CopySist` (sin subcarpetas).
15. Sin cambiar de carpeta, borrar `CopySist` de forma recursiva.
16. Poner atributos **Oculto** y **Sistema** a `hoja.dat`, ejecutar `dir` y después dejarlo como estaba.
17. Crear desde `DOS` un archivo nuevo `hoja.txt` usando redirección.
18. Mostrar contenido de `hoja.dat` y `hoja.txt`.
19. Añadir el contenido de `hoja.txt` al final de `hoja.dat` sin borrar lo anterior.
20. Generar `comandos.txt` con la lista de ejecutables (`.exe` y `.com`) de:

* `C:\Windows`
* `C:\Windows\System32`
  (debe hacerse en **4 comandos**: dos para crear y dos para añadir/concatenar, según indique el profesor)

21. Crear:

* `listadoC.txt` con el árbol de directorios de C:
* `ordenadoC.txt` ordenado por nombre a partir del anterior
* `diaHoy.txt` con los archivos modificados “hoy” (según fecha de sistema)

 **Entrega de la parte B**

* Capturas por bloques (no hace falta una por comando si se agrupan, pero debe verse todo).
* Si un comando devuelve demasiadas líneas, usa paginación (`more`) o redirección a archivo y muestra el archivo.

---

# PARTE C – Archivo por lotes (.bat) (guía paso a paso)

## C.1 Crear el archivo lote.bat

1. Abre CMD como administrador.
2. Crea el archivo:

   * Debe llamarse `lote.bat` y guardarse en `C:\`
3. El contenido del `.bat` debe cumplir:

### Requisitos obligatorios del script

* Generar/actualizar `C:\diaHoy.txt`
* **No sobrescribir**: debe **añadir** cada ejecución al final del archivo.
* Debe escribir al inicio de cada ejecución una línea con:

  * fecha + hora + texto tipo “Ejecución: …”
* Debe listar todos los archivos del sistema (C: y subdirectorios) y filtrar los modificados el día de ejecución usando:

  * la variable de Windows **`%date%`**
* Debe incluir al final:

  * `shutdown /s /t 20`

 Captura obligatoria:

* Contenido del `lote.bat` (puede ser con `type C:\lote.bat`).

---

## C.2 Ejecutar y comprobar

1. Ejecuta `C:\lote.bat` como administrador.
2. Comprueba que:

   * `C:\diaHoy.txt` existe
   * Se añadió una nueva sección con la fecha y hora
   * Aparecen resultados coherentes
3. Realiza una segunda ejecución y comprueba que:

   * Se añade debajo (no se borra lo anterior)

 Capturas obligatorias

* Ejecución del lote.
* Visualización parcial del `diaHoy.txt` mostrando al menos dos ejecuciones.

 Nota importante:

* Si el apagado molesta para comprobar, comenta la línea `shutdown` durante pruebas y activarla al final.

---

