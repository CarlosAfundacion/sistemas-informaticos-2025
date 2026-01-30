# EXPLOTACIÓN DE WINDOWS 10. PARTICIONES, ARRANQUE Y ADMINISTRACIÓN BÁSICA

## 0. Objetivos de la unidad

Al finalizar estos apuntes el alumnado debe ser capaz de:

1. Identificar con precisión edición, versión y arquitectura de Windows 10.
2. Comprender el ciclo de vida y la importancia de las actualizaciones.
3. Explicar la estructura lógica del disco: particiones, volúmenes y sistemas de archivos.
4. Diferenciar MBR/BIOS frente a GPT/UEFI y sus implicaciones en el arranque.
5. Instalar un segundo Windows 10 en arranque dual y analizar el gestor de arranque.
6. Utilizar el Administrador de discos para crear particiones, asignar letras y reconocer particiones especiales.
7. Administrar el sistema desde CMD utilizando rutas, comodines, filtros, redirecciones y atributos.
8. Automatizar tareas con archivos por lotes (.bat), variables de entorno y programación de apagado.

Estos contenidos permiten resolver la tarea práctica completa (Partes A, B y C).

---

## 1. Windows 10: ediciones, arquitectura, versión y ciclo de vida

### 1.1 Ediciones principales de Windows 10

En Windows 10 existen varias ediciones. Las más relevantes son:

* Windows 10 Home

  * Enfoque doméstico.
  * No permite unirse a dominio (Active Directory).
  * No incluye Editor de directivas de grupo (gpedit.msc).
  * No incluye BitLocker (cifrado completo de unidad) en la edición estándar.
  * Escritorio remoto: permite usarlo como cliente, pero no como servidor en la configuración típica.

* Windows 10 Pro

  * Enfoque profesional.
  * Permite dominio, BitLocker, Escritorio remoto (como servidor) y directivas de grupo.
  * Es la edición habitual para aulas, empresas y prácticas.

* Windows 10 Enterprise

  * Similar a Pro, con características extra de seguridad y administración.
  * Se distribuye por volumen (licencias corporativas).

Nota importante: Windows 10 N es una edición (Home/Pro/Enterprise) sin determinados componentes multimedia por requisitos legales.

### 1.2 Arquitectura (32/64 bits)

* En entornos actuales, se trabaja prácticamente siempre con 64 bits (x64).
* 32 bits (x86) se considera obsoleto para nuevas instalaciones.

### 1.3 Cómo ver la versión, edición y arquitectura

Formas recomendadas:

1. Configuración → Sistema → Acerca de

   * Muestra edición (Home/Pro), versión y tipo de sistema.

2. Ejecutar winver

   * Muestra versión y compilación.

3. CMD:

   * systeminfo (muy completo)
   * ver (información básica)

### 1.4 Ciclo de vida y actualizaciones

* Un sistema Windows debe mantenerse actualizado por seguridad y estabilidad.
* Windows 10 terminó su soporte general en octubre de 2025. Lo usamos en clases por consumir menos recursos que el Windows 11.

Tipos de actualizaciones:

* Actualizaciones de seguridad (mensuales acumulativas).
* Actualizaciones de características (en los últimos años, generalmente anuales).

Consecuencias de no actualizar:

* Vulnerabilidades sin parchear.
* Incompatibilidades con software.
* Riesgos de malware y ransomware.

---

## 2. Entorno de trabajo: máquina virtual (VirtualBox)

### 2.1 Consideraciones prácticas

En máquinas virtuales, el rendimiento depende del host y de los recursos asignados.

Recomendación para una instalación fluida de Windows 10:

* CPU: 2 núcleos (o más si el host lo permite)
* RAM: 4 GB mínimo, 8 GB recomendado
* Disco por instalación: 50 GB mínimo (si va a haber actualizaciones y software)

Si se van a instalar dos Windows 10 en el mismo VDI, conviene:

* Disco total: 100–120 GB como base razonable si se quiere trabajar de manera cómoda. Para las prácticas nos conformaremos con unos 60 GB.

### 2.2 Permisos de administrador

Muchas operaciones requieren elevar privilegios:

* Administrador de discos.
* bcdedit.
* Cambios de fecha/hora.
* chkdsk /f.

Abrir CMD como administrador:

* Buscar cmd → clic derecho → Ejecutar como administrador.

---

## 3. Estructura lógica del disco: particiones, volúmenes y sistemas de archivos

### 3.1 Conceptos

* Disco (físico/virtual): unidad de almacenamiento.
* Partición: división lógica del disco.
* Volumen: partición formateada y montada.
* Letra de unidad: punto de montaje típico en Windows (C:, D:, E:...).

Importante:

* En “Este equipo” se ven volúmenes montados, no el disco entero.
* Una partición sin letra puede existir y ser crítica para el sistema.

### 3.2 Sectores y clústeres

* Sector: unidad física mínima (normalmente 512 bytes, o 4096 bytes en discos modernos).
* Clúster (unidad de asignación): unidad mínima lógica de almacenamiento.

Ejemplo de desperdicio por clúster:

* Archivo de 10 bytes en una partición con clúster de 4096 bytes:

  * Tamaño real: 10 bytes
  * Tamaño en disco: 4096 bytes

Equilibrio:

* Clúster pequeño: menos desperdicio, potencialmente menos rendimiento.
* Clúster grande: más rendimiento, más desperdicio.

---

## 4. Sistemas de archivos en Windows

### 4.1 Principales sistemas

* NTFS (sistema de archivos estándar en Windows)

  * Permisos ACL por usuario/grupo.
  * Archivos y particiones de gran tamaño.
  * Compresión y cifrado (EFS, según edición).
  * Registro (journaling): mejora integridad.

* FAT32

  * Muy compatible (Windows/Linux/macOS y dispositivos).
  * Límite de archivo: 4 GB.

* exFAT

  * Diseñado para dispositivos extraíbles.
  * Permite archivos grandes.
  * Menos funciones de seguridad que NTFS.

### 4.2 Formateo

Formatear implica:

* Crear estructuras internas del sistema de archivos.
* Preparar la partición para almacenar datos de forma organizada.

Herramientas:

* Administrador de discos (GUI).
* CMD: format (con cuidado; es destructivo).

---

## 5. Herramientas de particionado

### 5.1 Herramientas internas

* Administrador de discos (diskmgmt.msc)

  * Crear/Eliminar volúmenes.
  * Formatear.
  * Asignar letras.

* diskpart (línea de comandos)

  * Herramienta avanzada.
  * Requiere precisión; un error puede borrar discos.

### 5.2 Herramientas externas (concepto)

* GParted, EaseUS, etc.

En prácticas, normalmente se trabaja con:

* Administrador de discos + herramientas de Windows.

---

## 6. Esquemas de particionado y firmware: BIOS/MBR vs UEFI/GPT

### 6.1 BIOS y MBR (modelo tradicional)

* BIOS: firmware clásico.
* MBR: primer sector del disco (512 bytes) con:

  * Tabla de particiones.
  * Código de arranque.
  * Señalización de partición activa.

Tipos de particiones (MBR):

* Primaria: máximo 4.
* Extendida: una de ellas puede ser extendida.
* Lógicas: dentro de la extendida.

Conceptos clave:

* Partición activa: la marcada para arrancar.
* En MBR solo hay una activa por disco.

Limitaciones:

* Tamaño máximo del disco: 2 TB.
* Máximo 4 particiones primarias.

### 6.2 UEFI y GPT (modelo moderno)

* UEFI: firmware moderno.
* GPT: tabla de particiones moderna.

Ventajas:

* Muchas particiones (hasta 128 típicamente).
* Discos muy grandes.
* Mejor redundancia de tabla (copias).

Partición especial:

* ESP (EFI System Partition): contiene cargadores de arranque en sistemas UEFI.

Compatibilidad:

* Un Windows instalado en GPT normalmente requiere UEFI.
* En VirtualBox puede configurarse BIOS o UEFI.

---

## 7. Arranque de Windows 10 y gestor de arranque

### 7.1 Proceso de arranque (visión general)

1. Firmware (BIOS/UEFI) inicializa hardware.
2. Localiza el cargador/gestor de arranque.
3. Lee la configuración de arranque.
4. Carga el sistema operativo en memoria.

### 7.2 Particiones implicadas: activa, sistema y arranque

Estos conceptos son fundamentales para la Parte A.

* Partición de sistema:

  * Contiene el gestor de arranque de Windows y la configuración.
  * En BIOS/MBR: suele aparecer como “Reservado para el sistema”.
  * En UEFI/GPT: el equivalente práctico es la ESP.

* Partición activa (solo MBR):

  * Indicada como activa en la tabla MBR.
  * Es donde la BIOS busca el código de arranque.

* Partición de arranque:

  * Donde está instalado el Windows que está en ejecución.
  * Normalmente es la unidad C: del sistema arrancado.

Importante: “partición de sistema” y “partición de arranque” no son lo mismo.

### 7.3 BCD (Boot Configuration Data)

* Es la base de datos de configuración del arranque.
* Incluye entradas para cada Windows instalado.
* Se administra con:

  * Herramientas gráficas.
  * bcdedit (CMD como administrador).

---

## 8. Instalación de un segundo Windows 10 (arranque dual)

### 8.1 Reglas y recomendaciones

* Cada Windows debe estar en su propia partición.
* Es imprescindible dejar espacio suficiente.
* Tras instalar el segundo Windows, el gestor de arranque mostrará un menú.

En instalaciones entre versiones de Windows, la regla clásica es:

* Instalar de más antiguo a más nuevo.

En nuestro caso, ambos son Windows 10, por lo que no hay conflicto por antigüedad.

### 8.2 Preparación de la partición de 30 GB

Desde el instalador o desde el Administrador de discos:

* Liberar espacio no asignado.
* Crear una partición de 30 GB.

Buenas prácticas:

* Etiquetar volúmenes (por ejemplo: Win10_Tarde).
* Evitar letras que colisionen con unidades compartidas en VirtualBox.

### 8.3 Verificación del gestor de arranque

Al reiniciar:

* Debe aparecer un menú para elegir sistema.

Captura:

* Se debe capturar la pantalla del menú.

### 8.4 Renombrar entradas del gestor de arranque con bcdedit

Objetivo:

* Windows 10 – Turno Mañana
* Windows 10 – Turno Tarde

Procedimiento orientativo:

1. CMD como administrador.
2. bcdedit
3. Identificar los identificadores (GUID) de cada entrada.
4. Modificar la descripción con:

* bcdedit /set {IDENTIFICADOR} description "Windows 10 – Turno Mañana"

(igual para Turno Tarde)

Nota: En los ejercicios se debe justificar qué entrada corresponde a cada instalación. Una forma práctica es arrancar cada Windows y comprobar cuál es el “sistema actual” en bcdedit.

---

## 9. Administrador de discos: interpretación y casos típicos

### 9.1 Cómo abrir

* diskmgmt.msc
* O “Administración de equipos → Almacenamiento → Administración de discos”.

### 9.2 Qué observar

* Tamaño del disco.
* Volúmenes y letras.
* Particiones especiales:

  * Reservado para el sistema / EFI.
  * Particiones de recuperación.

### 9.3 Identificación de partición activa, de sistema y de arranque

Caso típico en BIOS/MBR:

* Partición pequeña “Reservado para el sistema”:

  * Sistema y activa.
* Partición donde está Windows arrancado:

  * Arranque.

Caso típico en UEFI/GPT:

* ESP:

  * Sistema (arranque UEFI).
* Windows arrancado:

  * Arranque.

---

## 10. CMD: fundamentos y buenas prácticas

### 10.1 Comandos, parámetros y modificadores

Estructura general:

* comando [parámetros] [opciones]

Ayuda:

* comando /?
* help comando

### 10.2 Directorios y rutas

* Directorio raíz: C:\
* Directorio actual: aparece en el prompt.

Rutas:

* Absoluta: C:\Users\Alumno\Documents
* Relativa: ..\Documents (depende del directorio actual)

Directorio actual/padre:

* . actual
* .. padre

### 10.3 Comodines

* \* cualquier cadena
* ? un carácter

Ejemplos:

* dir *.txt
* dir a????.docx

---

## 11. Comandos de administración y gestión (con ejemplos)

### 11.1 Fecha y hora

* Ver fecha: date /t
* Cambiar fecha: date y seguir instrucciones (requiere privilegios)

Verificar:

* date /t

### 11.2 Etiqueta de volumen

* Ver etiqueta al listar: dir c:
* Cambiar etiqueta: label c: NUEVA_ETIQUETA

### 11.3 Listados con dir

Opciones útiles:

* dir /a (incluye ocultos)
* dir /s (recursivo)
* dir /b (formato simple)
* dir /o (orden)
* dir /t:w (usar fecha de escritura)

Ejemplos:

* Listar ejecutables del sistema:

  * dir C:\Windows\System32*.exe /b

* Árbol de directorios (alternativas):

  * tree C:\ruta
  * dir C:\ruta /s

* Archivos modificados en una fecha concreta (enfoque práctico):

  * dir C:\ruta /s /t:w
  * Filtrar con find por la fecha según formato local.

Nota: La fecha en dir depende de la configuración regional. Conviene documentar el formato que aparece en el sistema.

### 11.4 Crear estructura de directorios

* mkdir o md

Ejemplo:

* mkdir C:\Practica\A\B\C

### 11.5 Copiar, mover, renombrar y borrar

* Copiar archivos: copy origen destino
* Copiar directorios: xcopy /e origen destino

Mover:

* move origen destino

Renombrar:

* ren ruta\archivo.txt nuevo.txt

Borrar:

* Archivos: del /q archivo
* Directorios: rmdir /s /q carpeta

“Una sola orden cuando sea posible”:

* Borrar todos los .tmp de una carpeta:

  * del /q C:\ruta*.tmp

* Borrar una carpeta completa sin preguntar:

  * rmdir /s /q C:\ruta\carpeta

### 11.6 Atributos con attrib

Atributos:

* R: solo lectura
* H: oculto
* S: sistema
* A: archivo (marca de archivado)

Ejemplos:

* attrib +r C:\ruta\a.txt
* attrib -r C:\ruta\a.txt
* attrib +h C:\ruta\a.txt

Comprobar:

* attrib C:\ruta\a.txt
* dir /a



### 11.7 Mostrar contenido de un archivo

El comando `type` nos permite mostrar lo que contiene un fichero

Ejemplo:

* type ejemplo.txt

---

## 12. Redirecciones, tuberías y concatenación

### 12.1 Redirección

* \> crea/sobrescribe.
* \> > añade al final.

Ejemplos:

* date /t > reporte.txt
* time /t >> reporte.txt

### 12.2 Tuberías

* \| envía la salida de un comando como entrada de otro.

Filtros:

* more (paginación)
* sort (ordenación)
* find (búsqueda)

Ejemplos:

* dir /s | more
* dir /s | find "informe"
* dir /b | sort

### 12.2 Concatenación

* & permite ejecutar dos comandos seguidos en la línea de comandos, independientemente de que el primero de ellos funcione
* && permite ejecutar dos comandos seguidos. El segundo de ellos solo se hará si el primero no da error

Ejemplos:

* mkdir C:\prueba & copy C:\Windows\*.exe C:\prueba
* mkdir C:\prueba && copy C:\Windows\*.log C:\prueba

  Si ejecutamos estos comandos seguidos los .log no se copiarían porque al tratar de crear prueba, como ya existe, daría error.


---

## 13. Archivos por lotes (.bat) y automatización

### 13.1 Concepto

Un archivo .bat es un script que contiene una lista de comandos CMD que se ejecutan secuencialmente.

Creación:

* Con Bloc de notas (guardar como .bat).

### 13.2 Variables de entorno

Windows expone variables accesibles como %NOMBRE%.

Ejemplos útiles:

* %DATE% fecha
* %TIME% hora
* %USERNAME% usuario
* %COMPUTERNAME% equipo
* %USERPROFILE% perfil de usuario

### 13.3 Control de eco y comentarios

* @echo off evita mostrar cada comando.
* rem comentario.

### 13.4 Guardar “ficheros modificados hoy”

Estrategia práctica (adaptable según formato de fecha local):

1. Generar un listado recursivo.
2. Filtrar por la cadena de fecha.
3. Guardar en un archivo con >>.

Ejemplo conceptual:

* dir C:\ruta /s /t:w | find "%DATE%" >> modificados_hoy.txt

Nota: La fecha en %DATE% y en la salida de dir puede no coincidir exactamente en formato; hay que verificar y ajustar el filtro si fuese necesario.

### 13.5 Apagado programado

* shutdown /s /t 20

Cancelar apagado:

* shutdown /a

---

## 14. Anexos prácticos orientados a la tarea

### 14.1 Parte A (arranque dual): checklist conceptual

1. Crear partición de 30 GB.
2. Instalar segundo Windows 10 Pro.
3. Crear usuario con segundo apellido.
4. Comprobar menú de arranque.
5. Capturar pantalla.
6. Renombrar entradas con bcdedit.
7. Arrancar en cada Windows y analizar discos.
8. Identificar:

   * Partición activa (si MBR).
   * Partición de sistema.
   * Partición de arranque.
9. Responder razonadamente.

### 14.2 Parte B (CMD): ejercicios guiados

* Búsquedas con comodines:

  * dir C:\ /s *.log
  * dir C:\ /s a????.txt

* Estructura con rutas absolutas y relativas:

  * mkdir C:\Practica\Datos\Entrada
  * cd C:\Practica
  * mkdir Salida

* Copiar y renombrar:

  * copy Datos\Entrada\a.txt Salida\a_copia.txt

* Atributos:

  * attrib +h Salida\a_copia.txt
  * dir Salida /a

* Redirección:

  * dir C:\Windows\System32*.exe /b > ejecutables.txt

### 14.3 Parte C (.bat): plantilla orientativa

Estructura típica de un .bat:

1. @echo off
2. Escribir cabecera con variables.
3. Generar listado y filtrarlo.
4. Añadir al archivo con >>.
5. Programar apagado.

Recordatorio:

* Probar primero comandos en CMD.
* Luego pegarlos en el .bat.

---

