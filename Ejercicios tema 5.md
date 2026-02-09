# BOLETÍN DE EJERCICIOS

## SI-05: Administración básica del sistema Windows

1. En el PDF, organiza por ejercicios (Ejercicio 1, 2, 3, 4 y 5).
2. Cada captura debe llevar debajo una línea: “Qué demuestra esta captura”.

---

## EJERCICIO 1. Partición y formato NTFS

### Objetivo

Crear una partición de 5 GB y formatearla en NTFS para poder gestionar permisos locales (NTFS). 

### Pasos

1. Abre el **Administrador de discos**:

   * Pulsa `Win + R`, escribe `diskmgmt.msc` y pulsa Enter.
2. Localiza **espacio sin asignar**:

   * Si no hay espacio sin asignar, selecciona la unidad, pulsa el botón derecho del ratón y dale a reducir volumen. En el recuadro de Tamaño del espacio que desea reducir, en MB escribe 5120. Pulsa en reducir.
3. Crea un volumen nuevo:

   * Clic derecho sobre el espacio sin asignar → **Nuevo volumen simple**.
   * Tamaño: **5120 MB** (aprox. 5 GB).
   * Asigna una letra (por ejemplo, `F:`).
4. Formatea:

   * Sistema de archivos: **NTFS**.
   * Etiqueta del volumen: `DATOS_TuNombre`.
   * Formato rápido: activado.

### Capturas obligatorias

* **C1.1** Administrador de discos con el volumen creado (se debe ver tamaño y estado).
* **C1.2** Explorador de archivos → clic derecho en la unidad creada → Propiedades (se debe ver: sistema de archivos NTFS y etiqueta `DATOS_TuNombre`).

---

## EJERCICIO 2. Usuarios y grupos locales

### Objetivo

Crear grupos y usuarios locales para después asignar permisos por departamentos. 

### Pasos

1. Abre “Usuarios y grupos locales”:

   * `Win + R` → `lusrmgr.msc` → Enter.
     
2. Crea estos grupos:

   * `administracion`
   * `ventas`
3. Crea 4 usuarios estándar (ninguno administrador):

   * Dos para Administración y dos para Ventas.
   * Nombre de usuario: inicial del nombre + apellido (ejemplo: `alopez`).
   * Contraseña: establece una contraseña sencilla para el aula, que no te vayas a olvidar.
4. Añade usuarios a su grupo correspondiente:

   * Cada usuario debe pertenecer al grupo de su departamento.

### Capturas obligatorias

* **C2.1** Ventana de “Grupos” mostrando `administracion` y `ventas`.
* **C2.2** Ventana de “Usuarios” mostrando los 4 usuarios creados.
* **C2.3** Propiedades del grupo `administracion` → pestaña Miembros (o lista de miembros) con sus 2 usuarios.
* **C2.4** Propiedades del grupo `ventas` → miembros con sus 2 usuarios.

---

## EJERCICIO 3. Carpetas y permisos NTFS básicos

### Objetivo

Crear estructura de carpetas y aplicar permisos NTFS: cada grupo con control en su carpeta, y una excepción de lectura.

### Pasos

#### A) Crear estructura y archivos

1. En `C:\` crea la carpeta:
   `C:\Empresa_TuApellido`
2. Dentro crea:

   * `C:\Empresa_TuApellido\Administracion`
   * `C:\Empresa_TuApellido\Ventas`
3. En cada carpeta mete al menos:

   * Un archivo de texto (por ejemplo `info.txt`)
   * Una imagen (puede ser cualquier imagen de prueba)

#### B) Configurar permisos

En las carpetas **Administracion** y **Ventas** configura permisos para cumplir:

* El grupo `administracion` puede **leer y escribir** en `Administracion`.
* El grupo `ventas` puede **leer y escribir** en `Ventas`.
* El grupo `administracion` puede **leer** `Ventas` pero **no modificar**.
* El grupo contrario no debe acceder a la carpeta del otro (ventas no entra en Administracion).

Recomendación de configuración (método simple y controlable):

1. Clic derecho en la carpeta `Administracion` → **Propiedades** → pestaña **Seguridad**.
2. En **Opciones avanzadas**:

   * Deshabilita herencia.
   * Elige “Convertir permisos heredados en permisos explícitos”
3. En la lista de permisos:

   * Elimina permisos que no correspondan al objetivo (no borres SYSTEM ni Administradores si aparecen).
4. Agrega el grupo `administracion` y marca **Modificar** (o “Lectura y ejecución + Escritura”, según interfaz).
5. Repite el proceso en la carpeta `Ventas`:

   * Grupo `ventas`: **Modificar**.
   * Grupo `administracion`: **Lectura y ejecución** (solo lectura).
6. Comprueba “Acceso efectivo” (si está disponible) para un usuario de cada grupo.

### Capturas obligatorias

Estructura:

* **C3.1** Explorador mostrando `C:\Empresa_TuApellido` y las dos subcarpetas.
* **C3.2** Contenido de `Administracion` (con el txt y la imagen visibles).
* **C3.3** Contenido de `Ventas` (con el txt y la imagen visibles).

Permisos:

* **C3.4** Propiedades → Seguridad de la carpeta `Administracion` (lista de grupos/usuarios y permisos visibles).
* **C3.5** Propiedades → Seguridad de la carpeta `Ventas`.
* **C3.6** Opciones avanzadas de `Administracion` donde se vea que la herencia está deshabilitada (o el estado equivalente).
* **C3.7** Opciones avanzadas de `Ventas` donde se vea que la herencia está deshabilitada.
* **C3.8** Acceso efectivo (o permisos efectivos) de un usuario de `administracion` sobre `Administracion`.
* **C3.9** Acceso efectivo de un usuario de `ventas` sobre `Administracion` (debe reflejar que no tiene acceso o no tiene permisos suficientes).
* **C3.10** Acceso efectivo de un usuario de `administracion` sobre `Ventas` (solo lectura).
* **C3.11** Acceso efectivo de un usuario de `ventas` sobre `Ventas` (modificar/escritura).

---

## EJERCICIO 4. Verificación iniciando sesión

### Objetivo

Demostrar con pruebas reales que los permisos funcionan. 

### Pasos

1. Cierra sesión e inicia con un usuario del grupo `ventas`.
2. Comprueba:

   * Puede crear un archivo dentro de `C:\Empresa_TuApellido\Ventas`.
   * No puede acceder a `C:\Empresa_TuApellido\Administracion` (o recibe acceso denegado).
3. Cierra sesión e inicia con un usuario del grupo `administracion`.
4. Comprueba:

   * Puede modificar archivos en `Administracion`.
   * Puede abrir un archivo de `Ventas` (lectura).
   * No puede borrar o guardar cambios en un archivo de `Ventas` (si has configurado solo lectura).

### Capturas obligatorias

* **C4.1** Sesión iniciada con usuario de ventas (se debe ver el nombre de usuario en Inicio o en la carpeta de usuario).
* **C4.2** Prueba de escritura en `Ventas` (por ejemplo archivo nuevo creado).
* **C4.3** Acceso denegado (o imposibilidad de entrar) al intentar abrir `Administracion` desde el usuario de ventas.
* **C4.4** Sesión iniciada con usuario de administración.
* **C4.5** Lectura correcta de un archivo de `Ventas` (abierto).
* **C4.6** Evidencia de que no puede modificar `Ventas` (por ejemplo, intento de guardar cambios fallido o mensaje de permisos).

---

## EJERCICIO 5. Seguridad local y restauración del sistema

### Objetivo

Aplicar una política básica de contraseñas y crear un punto de restauración.

### Pasos

#### A) Directiva de contraseñas (versión simplificada)

1. Abre la **Directiva de seguridad local**:

   * `Win + R` → `secpol.msc` → Enter.
2. Ve a:

   * Configuración de seguridad → Directivas de cuenta → Directiva de contraseñas.
3. Configura:

   * Longitud mínima: **8**
   * Complejidad: **Habilitada**

#### B) Punto de restauración

1. Abre “Protección del sistema”:

   * Busca “Crear un punto de restauración” en el menú Inicio y ábrelo.
2. Selecciona la unidad del sistema (normalmente `C:`) y pulsa **Configurar**:

   * Activa la protección del sistema.
   * Ajusta el uso máximo (por ejemplo 5%).
3. Pulsa **Crear**:

   * Nombre: `Antes_practica_SI05_TuNombre`.

### Capturas obligatorias

* **C5.1** Ventana de `secpol.msc` mostrando las directivas de contraseña (que se vean los valores aplicados).
* **C5.2** Ventana de Protección del sistema con la protección activada para `C:`.
* **C5.3** Confirmación o listado donde se vea el punto de restauración creado (nombre visible o mensaje de creación correcta).

---

