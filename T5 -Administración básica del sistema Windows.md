# SI 05: ADMINISTRACIÓN BÁSICA DEL SISTEMA WINDOWS

---

## 1. Introducción: el ordenador y el sistema operativo

Un ordenador está formado por componentes físicos (hardware) como el procesador, la memoria, el disco duro y los periféricos. Sin embargo, el hardware por sí solo no permite trabajar. Es necesario un software especial que actúe como intermediario entre el usuario y la máquina: el **sistema operativo**.

El sistema operativo es el primer software que se carga al encender el ordenador y es el encargado de tomar el control del sistema. En este módulo trabajamos con **Windows**, uno de los sistemas operativos más utilizados en entornos personales y profesionales.

### 1.1 Funciones del sistema operativo

Windows se encarga de:

* Gestionar el uso del procesador y la memoria.
* Controlar el acceso al disco duro y a los archivos.
* Permitir la interacción con el usuario mediante teclado, ratón y pantalla.
* Ejecutar aplicaciones.
* Proteger la información almacenada.

Todos los programas dependen del sistema operativo para funcionar correctamente.

---

## 2. Administración del sistema Windows

Administrar un sistema operativo no es lo mismo que utilizarlo. Mientras que un usuario normal se limita a ejecutar programas y trabajar con archivos, la **administración del sistema** consiste en configurar y mantener Windows para que funcione de forma correcta, segura y estable a lo largo del tiempo.

La administración del sistema incluye tareas como:

* Crear y gestionar usuarios.
* Definir contraseñas y políticas de seguridad.
* Controlar el acceso a archivos y carpetas.
* Proteger el sistema frente a errores y usos indebidos.
* Realizar tareas de mantenimiento.

En esta unidad se trabaja la **administración local de Windows**, es decir, sobre un único equipo, sin servidor ni dominio.

---

## 3. Usuarios en Windows

### 3.1 Concepto de usuario

Un **usuario** es una identidad que representa a una persona dentro del sistema operativo. Gracias a los usuarios, Windows puede diferenciar a las distintas personas que utilizan el mismo ordenador.

Cada usuario dispone de:

* Un nombre de usuario, que lo identifica internamente.
* Una contraseña, que protege el acceso.
* Un perfil de usuario.
* Un conjunto de permisos.

---

### 3.2 El perfil de usuario

El **perfil de usuario** es el espacio personal que Windows crea para cada usuario. En él se almacenan:

* El escritorio.
* Los documentos personales.
* Las descargas.
* Las imágenes y vídeos.
* La configuración del sistema y de las aplicaciones.

Normalmente, los perfiles de usuario se guardan en la ruta:

```
C:\Users\nombre_usuario
```

Cada usuario trabaja en su propio perfil, de manera independiente del resto.

---

## 4. Autenticación y autorización

### 4.1 Autenticación

La **autenticación** es el proceso mediante el cual Windows comprueba la identidad de un usuario. Para ello solicita un nombre de usuario y una contraseña.

Si los datos introducidos son correctos, el sistema permite iniciar sesión. En caso contrario, el acceso es denegado. Sin autenticación no es posible utilizar el sistema.

---

### 4.2 Autorización

Una vez que el usuario ha iniciado sesión, Windows debe decidir qué acciones puede realizar. Este proceso se denomina **autorización**.

La autorización determina:

* A qué archivos y carpetas puede acceder un usuario.
* Qué acciones puede realizar sobre ellos (leer, modificar, borrar, etc.).

Un usuario puede estar autenticado correctamente y, aun así, no estar autorizado a acceder a determinados recursos.

---

## 5. Tipos de cuentas de usuario

### 5.1 Usuario estándar

El usuario estándar es el tipo de cuenta recomendado para el uso diario del sistema. Permite trabajar con normalidad sin poner en riesgo la estabilidad del sistema.

Un usuario estándar puede:

* Usar aplicaciones instaladas.
* Crear y modificar sus propios archivos.
* Personalizar su entorno de trabajo.

No puede:

* Instalar software.
* Modificar configuraciones globales.
* Crear o eliminar usuarios.

---

### 5.2 Usuario administrador

El usuario administrador tiene control total sobre el sistema.

Puede:

* Instalar y desinstalar software.
* Cambiar configuraciones del sistema.
* Crear, modificar y eliminar usuarios.
* Cambiar permisos de archivos y carpetas.

Por motivos de seguridad, este tipo de cuenta debe utilizarse únicamente para tareas administrativas.

---

## 6. Control de cuentas de usuario (UAC)

Windows incorpora un mecanismo de seguridad llamado **Control de Cuentas de Usuario (UAC)**. Su función es solicitar confirmación cuando se intenta realizar una acción que puede afectar al sistema.

De este modo se evitan errores accidentales y se protege el sistema frente a software malicioso.

---

## 7. Grupos de usuarios

### 7.1 Concepto de grupo

Un **grupo** es un conjunto de usuarios que comparten permisos. En lugar de asignar permisos usuario por usuario, se asignan a grupos.

Ejemplos habituales de grupos son:

* Ventas.
* Administración.
* Contabilidad.

---

### 7.2 Ventajas del uso de grupos

El uso de grupos permite:

* Simplificar la administración del sistema.
* Reducir errores en la asignación de permisos.
* Facilitar la incorporación de nuevos usuarios.

Este modelo es el utilizado en entornos profesionales y empresariales.

---

## 8. Herramientas de administración

Windows incluye herramientas específicas para la administración del sistema. La más importante es **Administración de equipos**.

Desde esta herramienta se pueden:

* Crear y gestionar usuarios y grupos.
* Administrar discos y particiones.
* Configurar tareas automáticas.

---

## 9. Sistemas de archivos

### 9.1 Concepto de sistema de archivos

Un **sistema de archivos** define cómo se organizan y almacenan los datos en un disco.

---

### 9.2 FAT32 y NTFS

Los sistemas de archivos más habituales en Windows son:

* **FAT32**: sistema antiguo, sin seguridad ni permisos.
* **NTFS**: sistema moderno, seguro y con soporte de permisos.

Windows necesita NTFS para poder aplicar medidas de seguridad.

---

## 10. Permisos NTFS

Los permisos NTFS permiten controlar el acceso a archivos y carpetas.

### 10.1 Permisos estándar

Los permisos más habituales son:

* Lectura.
* Escritura.
* Modificar.
* Control total.

---

### 10.2 Herencia de permisos

Por defecto, las carpetas heredan los permisos de la carpeta superior. Esto se denomina **herencia** y facilita la administración.

En algunos casos es necesario desactivar la herencia para definir permisos específicos.

---

### 10.3 Permisos efectivos

Cuando un usuario pertenece a varios grupos, Windows calcula los **permisos efectivos** combinando todos los permisos aplicables. En caso de conflicto, siempre se aplica la opción más restrictiva.

---

## 11. Seguridad del sistema

### 11.1 Directivas de seguridad

Windows permite imponer reglas de seguridad mediante **directivas de seguridad local**, como:

* Longitud mínima de contraseña.
* Complejidad de contraseñas.
* Caducidad.
* Bloqueo de cuentas.

Estas directivas mejoran la seguridad del sistema.

---

## 12. Mantenimiento del sistema

Administrar un sistema también implica mantenerlo en buen estado.

### 12.1 Cuotas de disco

Las cuotas de disco permiten limitar el espacio que cada usuario puede utilizar, evitando un uso excesivo del disco.

---

### 12.2 Optimización y comprobación del disco

Windows incluye herramientas para:

* Optimizar el disco y mejorar el rendimiento.
* Detectar y corregir errores del sistema de archivos.

---

### 12.3 Programador de tareas

El **Programador de tareas** permite automatizar la ejecución de acciones, como scripts o tareas de mantenimiento, en un momento determinado.

---

## 13. Restauración del sistema

Windows permite crear **puntos de restauración**, que guardan el estado del sistema y permiten volver a una configuración anterior en caso de error.

Los puntos de restauración no sustituyen a las copias de seguridad de los archivos personales.

---

## 14. Conclusión

La administración básica de Windows proporciona los conocimientos necesarios para:

* Gestionar usuarios y grupos.
* Proteger la información mediante permisos.
* Aplicar políticas de seguridad.
* Mantener el sistema operativo estable y funcional.

Estos conceptos constituyen la base de la administración de sistemas y se aplican también en otros sistemas operativos como Linux o en entornos de servidor.


