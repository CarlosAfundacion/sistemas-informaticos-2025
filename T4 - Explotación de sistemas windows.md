# SI 04: ADMINISTRACIÓN BÁSICA DEL SISTEMA WINDOWS

---

## 1. El ordenador y Windows: qué ocurre cuando lo encendemos

Cuando encendemos un ordenador, no empezamos a trabajar directamente. Antes de que podamos usarlo, ocurre lo siguiente:

1. El hardware se enciende (procesador, memoria, disco…).
2. Se carga un programa muy especial llamado **sistema operativo**.
3. El sistema operativo toma el control del ordenador.

En nuestro caso, ese sistema operativo es **Windows**.

---

### 1.1 ¿Qué es un sistema operativo?

Un **sistema operativo** es el software más importante de un ordenador.
Sin él, el ordenador **no sirve para nada**.

Windows se encarga de:

* Mostrar la pantalla.
* Permitir usar el teclado y el ratón.
* Guardar archivos en el disco.
* Ejecutar programas.
* Proteger la información.

Todos los programas (navegador, editor de texto, juegos…) **dependen de Windows** para funcionar.

---

### 1.2 ¿Por qué hay que “administrar” Windows?

Windows está pensado para:

* Ser usado por varias personas.
* Guardar información importante.
* Funcionar durante mucho tiempo sin fallar.

Para que eso sea posible, alguien tiene que:

* Decidir quién puede usar el ordenador.
* Proteger los archivos.
* Evitar errores graves.
* Mantener el sistema ordenado.

Eso es **administrar Windows**.

---

## 2. Por qué Windows usa usuarios

Imagina un ordenador sin usuarios:

* Todo el mundo entra igual.
* Todos ven los mismos archivos.
* Cualquiera puede borrar cosas importantes.

Eso sería un caos.

Por eso Windows utiliza **usuarios**.

---

## 3. Qué es un usuario en Windows

Un **usuario** es una forma que tiene Windows de **diferenciar a las personas** que usan el mismo ordenador.

Cada usuario representa a **una persona concreta**.

---

### 3.1 Qué tiene un usuario

Cuando Windows crea un usuario, le asigna:

1. **Nombre de usuario**
   Es el identificador interno del sistema.

2. **Contraseña**
   Sirve para proteger el acceso.

3. **Perfil de usuario**
   Es su espacio personal dentro del ordenador.

4. **Permisos**
   Indican qué puede hacer y qué no.

---

### 3.2 El perfil de usuario explicado con un ejemplo

Piensa en un bloque de pisos:

* El edificio es el ordenador.
* Cada piso es un usuario.

Cada piso tiene:

* Sus muebles.
* Sus llaves.
* Su decoración.

En Windows:

* Cada usuario tiene su escritorio.
* Sus documentos.
* Sus configuraciones.

Normalmente se guardan en:

```
C:\Users\nombre_usuario
```

Un usuario **no ve ni toca** los archivos personales de otro, salvo que se le permita.

---

## 4. Iniciar sesión: autenticación

Cuando encendemos el ordenador y aparece una pantalla pidiendo usuario y contraseña, Windows está realizando un proceso llamado **autenticación**.

---

### 4.1 Qué es la autenticación

La **autenticación** es la forma que tiene Windows de comprobar:

> “¿Quién eres realmente?”

Para hacerlo, pide:

* Nombre de usuario.
* Contraseña.

Si coinciden:

* Te deja entrar.

Si no coinciden:

* No puedes usar el ordenador.

Sin autenticación **no hay acceso al sistema**.

---

## 5. Qué puedes hacer una vez dentro: autorización

Una vez que Windows sabe quién eres, tiene que decidir:

> “Ahora que sé quién eres… ¿qué te dejo hacer?”

Eso se llama **autorización**.

---

### 5.1 Autenticación ≠ autorización

Son cosas distintas:

* Autenticación: entrar al sistema.
* Autorización: acceder a recursos.

Ejemplo:

* Puedes entrar al edificio.
* Pero no puedes entrar a todas las viviendas.

En Windows:

* Puedes iniciar sesión.
* Pero no acceder a todos los archivos.

---

## 6. Tipos de usuarios en Windows

Windows no trata a todos los usuarios igual. Existen **roles**.

---

### 6.1 Usuario estándar

Es el usuario “normal”.

Sirve para:

* Trabajar.
* Estudiar.
* Usar programas.

Un usuario estándar puede:

* Crear archivos.
* Usar aplicaciones.
* Cambiar su fondo de pantalla.

Pero **no puede**:

* Instalar programas.
* Cambiar el sistema.
* Crear usuarios.
* Modificar configuraciones importantes.

Esto evita errores graves.

---

### 6.2 Usuario administrador

El administrador es el “encargado” del ordenador.

Puede:

* Cambiar cualquier cosa.
* Instalar software.
* Crear usuarios.
* Cambiar permisos.
* Acceder a todos los archivos.

Por eso:

* No se debe usar para trabajar a diario.
* Solo para administrar.

---

## 7. Por qué Windows pide confirmación (UAC)

Aunque seas administrador, Windows **no confía ciegamente**.

Cuando haces algo peligroso:

* Instalar programas.
* Cambiar permisos.
* Modificar el sistema.

Windows muestra una ventana preguntando:

> “¿Seguro que quieres hacer esto?”

Esto se llama **Control de Cuentas de Usuario (UAC)**.

Sirve para:

* Evitar errores.
* Evitar virus.
* Proteger el sistema.

---

## 8. Grupos: organizar usuarios

Imagina que tienes 20 usuarios y tienes que dar permisos uno a uno.
Sería lento y confuso.

Para eso existen los **grupos**.

---

### 8.1 Qué es un grupo

Un **grupo** es un conjunto de usuarios.

En lugar de dar permisos a personas, se dan a grupos.

Ejemplo:

* Grupo “ventas”.
* Grupo “administración”.

Si un usuario pertenece al grupo:

* Tiene todos los permisos del grupo.

---

### 8.2 Ventaja de los grupos

* Menos trabajo.
* Menos errores.
* Más orden.

Es el sistema que se usa en empresas reales.

---

## 9. Herramientas para administrar usuarios y grupos

Windows incluye herramientas especiales para administrar.

La más importante es **Administración de equipos**.

Desde ella se puede:

* Crear usuarios.
* Crear grupos.
* Añadir usuarios a grupos.
* Deshabilitar cuentas.

---

## 10. El disco duro y los sistemas de archivos

El disco duro no guarda archivos “al azar”.
Necesita reglas.

Esas reglas se llaman **sistema de archivos**.

---

### 10.1 FAT32 y NTFS

**FAT32**:

* Antiguo.
* Sin seguridad.
* Sin permisos.

**NTFS**:

* Moderno.
* Seguro.
* Permite permisos.
* Es obligatorio para Windows.

Sin NTFS **no hay seguridad**.

---

## 11. Permisos: proteger archivos y carpetas

Los **permisos** indican:

* Quién puede acceder.
* Qué puede hacer.

Se aplican a:

* Archivos.
* Carpetas.

---

### 11.1 Permisos básicos explicados

* **Lectura**: ver.
* **Escritura**: cambiar.
* **Modificar**: cambiar y borrar.
* **Control total**: hacer cualquier cosa.

Windows usa estos permisos para proteger la información.

---

### 11.2 Herencia: copiar permisos automáticamente

Por defecto:

* Las carpetas nuevas copian permisos de la carpeta superior.

Esto se llama **herencia**.

A veces es útil.
A veces hay que desactivarla para personalizar permisos.

---

### 11.3 Permisos finales (efectivos)

Si un usuario pertenece a varios grupos:

* Windows suma permisos.
* Si hay una denegación, gana la denegación.

Esto evita accesos indebidos.

---

## 12. Contraseñas y seguridad

Windows permite obligar a:

* Contraseñas largas.
* Contraseñas complejas.
* Cambios periódicos.

Esto se hace con **directivas de seguridad**.

Sirven para proteger el sistema incluso de usuarios descuidados.

---

## 13. Mantener Windows en buen estado

Administrar no es solo proteger, también es mantener.

Windows incluye herramientas para:

* Limitar espacio en disco.
* Optimizar discos.
* Comprobar errores.
* Automatizar tareas.

---

## 14. Restaurar el sistema

A veces algo falla:

* Un programa.
* Un controlador.
* Una configuración.

Windows permite volver atrás mediante **puntos de restauración**.

No recuperan archivos personales, pero sí el sistema.

---

## 15. Idea clave final

Todo lo que has leído sirve para entender una idea fundamental:

> **Un sistema operativo no es solo usar programas, es gestionar personas, datos y seguridad.**

Quien entiende:

* Usuarios.
* Grupos.
* Permisos.
* Seguridad.

Entiende cómo funcionan **todos los sistemas modernos**, no solo Windows.


