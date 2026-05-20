# Tema 7 Resumen — Administración de un sistema Linux

# 1. Introducción a Linux

Linux es un sistema operativo libre y de código abierto ampliamente utilizado tanto en entornos domésticos como profesionales. Es especialmente importante en servidores, redes, ciberseguridad y administración de sistemas debido a su estabilidad, seguridad y flexibilidad.

A diferencia de otros sistemas operativos, Linux está diseñado para ser administrado de forma muy eficiente mediante terminal. Esto permite automatizar tareas, controlar el sistema con precisión y gestionar servidores remotos sin necesidad de entorno gráfico.

Linux es:

* **Multiusuario**: varios usuarios pueden trabajar simultáneamente sin interferirse.
* **Multitarea**: puede ejecutar muchos programas y servicios al mismo tiempo.
* **Seguro**: utiliza un sistema estricto de permisos y privilegios.
* **Modular**: se puede instalar únicamente lo necesario para cada sistema.

En el mundo profesional, Linux se utiliza en:

* Servidores web.
* Cloud computing.
* Redes y routers.
* Supercomputadores.
* Sistemas embebidos.
* Android.

---

# 2. Estructura del sistema de archivos

Linux organiza toda la información mediante una estructura jerárquica de directorios que comienza en la raíz (`/`).

A diferencia de Windows:

* No existen letras de unidad (`C:`, `D:`…).
* Todos los discos y dispositivos forman parte de un único árbol de directorios.
* Los dispositivos se “montan” dentro del sistema de archivos.

Esto hace que Linux tenga una organización más uniforme y flexible.

## Directorios más importantes

| Directorio | Función                         |
| ---------- | ------------------------------- |
| `/`        | Raíz del sistema                |
| `/home`    | Carpetas personales de usuarios |
| `/etc`     | Configuración del sistema       |
| `/var`     | Logs y datos variables          |
| `/bin`     | Comandos esenciales             |
| `/root`    | Carpeta del administrador       |
| `/media`   | Dispositivos externos           |
| `/mnt`     | Montajes manuales               |

## Conceptos importantes

### Ruta absoluta

Indica la ubicación completa desde la raíz.

Ejemplo:

```text
/home/carlos/documentos
```

### Ruta relativa

Depende del directorio en el que se encuentre el usuario.

### Directorio actual y directorio padre

* `.` representa el directorio actual.
* `..` representa el directorio superior.

Comprender bien la estructura de directorios es fundamental para:

* localizar archivos,
* administrar configuraciones,
* instalar servicios,
* gestionar usuarios y permisos.

---

# 3. Navegación y gestión de archivos

Una de las tareas básicas de cualquier administrador es moverse por el sistema y manipular archivos y carpetas.

Las operaciones más habituales son:

* crear directorios,
* copiar archivos,
* mover información,
* eliminar contenido,
* buscar archivos.

## Navegación

Linux utiliza comandos para desplazarse entre carpetas y consultar contenido.

### Comandos esenciales

```bash id="ll1"
ls
```

Muestra archivos y carpetas.

```bash id="ll2"
cd carpeta
```

Cambia de directorio.

## Gestión de archivos

Linux permite manipular archivos de forma muy eficiente desde terminal.

### Operaciones habituales

* Crear archivos y directorios.
* Copiar información.
* Renombrar elementos.
* Eliminar contenido.

### Comandos esenciales

```bash id="ll3"
cp
```

Copia archivos o carpetas.

```bash id="ll4"
mv
```

Mueve o renombra archivos.

```bash id="ll5"
rm
```

Elimina archivos.

## Aspectos importantes

En Linux es importante comprender:

* que eliminar un archivo puede ser irreversible,
* que los permisos afectan al acceso,
* que muchas configuraciones del sistema se almacenan en archivos de texto.

Por eso, la administración de archivos es una parte crítica del sistema.

---

# 4. Usuarios y grupos

Linux está diseñado para trabajar con múltiples usuarios manteniendo la seguridad y separación entre ellos.

Cada usuario tiene:

* su propia carpeta personal,
* sus permisos,
* sus configuraciones.

El sistema distingue entre:

* usuarios normales,
* administradores,
* el usuario `root`.

## El usuario root

El usuario `root` tiene control total sobre el sistema:

* puede modificar cualquier archivo,
* instalar software,
* eliminar configuraciones críticas.

Por seguridad, normalmente se trabaja con usuarios normales y solo se elevan privilegios cuando es necesario.

---

## Grupos

Los grupos permiten administrar permisos de forma colectiva.

En lugar de asignar permisos usuario por usuario, se puede:

* crear un grupo,
* añadir usuarios,
* asignar permisos al grupo completo.

Esto facilita enormemente la administración en redes y servidores.

---

## Operaciones habituales

* Crear usuarios.
* Cambiar contraseñas.
* Añadir usuarios a grupos.
* Bloquear cuentas.

### Comandos esenciales

```bash id="ll6"
useradd
```

Crea usuarios.

```bash id="ll7"
passwd
```

Cambia contraseñas.

---

# 5. Permisos en Linux

El sistema de permisos es una de las características más importantes de Linux.

Cada archivo y carpeta tiene permisos asociados para:

* propietario,
* grupo,
* otros usuarios.

Los permisos determinan:

* quién puede leer,
* quién puede modificar,
* quién puede ejecutar.

## Tipos de permisos

| Permiso | Significado |
| ------- | ----------- |
| `r`     | Lectura     |
| `w`     | Escritura   |
| `x`     | Ejecución   |

---

## Importancia de los permisos

Los permisos permiten:

* proteger información,
* evitar modificaciones no autorizadas,
* limitar el acceso a programas críticos,
* mejorar la seguridad del sistema.

Un permiso mal configurado puede:

* permitir acceso indebido,
* bloquear servicios,
* comprometer el sistema.

---

## Permisos octales

Linux también representa permisos mediante números:

| Valor | Significado |
| ----- | ----------- |
| 7     | rwx         |
| 6     | rw-         |
| 5     | r-x         |
| 4     | r--         |

Ejemplo:

```text
755
```

Significa:

* propietario → lectura, escritura y ejecución,
* grupo → lectura y ejecución,
* otros → lectura y ejecución.

---

## Comandos esenciales

```bash id="ll8"
chmod
```

Modifica permisos.

```bash id="ll9"
chown
```

Cambia propietario.

Lo realmente importante es entender:

* cómo afectan los permisos a la seguridad,
* qué usuarios pueden acceder,
* cómo proteger correctamente el sistema.

---

# 6. Procesos y administración del sistema

Un proceso es cualquier programa que se está ejecutando.

Linux puede ejecutar simultáneamente:

* aplicaciones de usuario,
* servicios de red,
* procesos internos del sistema.

Cada proceso tiene:

* un identificador (PID),
* prioridad,
* consumo de recursos.

---

## Servicios y demonios

Muchos procesos funcionan en segundo plano sin interacción del usuario.

Estos procesos se llaman:

* servicios,
* demonios (daemons).

Ejemplos:

* servidor web,
* SSH,
* bases de datos.

---

## Administración de procesos

Un administrador debe ser capaz de:

* identificar procesos problemáticos,
* controlar consumo de CPU y memoria,
* finalizar procesos bloqueados.

### Comandos esenciales

```bash id="ll10"
top
```

Monitoriza el sistema en tiempo real.

```bash id="ll11"
kill
```

Finaliza procesos.

---

# 7. Instalación y actualización de software

Linux utiliza gestores de paquetes para instalar programas de forma centralizada.

En Debian se utiliza APT.

Esto permite:

* descargar software desde repositorios oficiales,
* resolver dependencias automáticamente,
* mantener el sistema actualizado.

---

## Repositorios

Los repositorios son servidores que contienen paquetes de software.

Ventajas:

* seguridad,
* actualizaciones centralizadas,
* instalación sencilla.

---

## Dependencias

Muchos programas necesitan otros paquetes para funcionar.

APT gestiona automáticamente esas dependencias.

---

## Comandos esenciales

```bash id="ll12"
apt update
```

Actualiza la lista de paquetes.

```bash id="ll13"
apt install
```

Instala software.

Lo importante es comprender:

* cómo se instala software de forma segura,
* cómo mantener actualizado el sistema,
* cómo evitar paquetes no confiables.

---

# 8. Automatización mediante scripts

Linux permite automatizar tareas mediante scripts.

Un script es un archivo que contiene comandos ejecutados secuencialmente.

---

## Ventajas de la automatización

* ahorro de tiempo,
* reducción de errores,
* administración masiva,
* ejecución automática de tareas repetitivas.

---

## Usos habituales

* copias de seguridad,
* creación automática de usuarios,
* monitorización,
* mantenimiento del sistema.

La automatización es una de las grandes ventajas de Linux frente a otros sistemas.

---

# 9. Tareas programadas

Linux permite ejecutar tareas automáticamente en momentos concretos mediante `cron`.

Cada tarea programada puede indicar:

* minuto,
* hora,
* día,
* frecuencia,
* comando a ejecutar.

---

## Usos habituales

* backups automáticos,
* limpieza de logs,
* actualizaciones,
* reinicio de servicios.

La programación automática es fundamental en administración de servidores.

---

# 10. Seguridad básica en Linux

La seguridad es uno de los pilares de Linux.

Sin embargo, un sistema Linux mal administrado puede ser vulnerable.

---

## Buenas prácticas

### Actualizaciones

Mantener el sistema actualizado corrige fallos de seguridad.

### Permisos

Asignar únicamente los permisos necesarios reduce riesgos.

### Uso limitado de root

Trabajar continuamente como administrador aumenta el peligro de errores graves.

### Firewall

Permite controlar conexiones y proteger servicios.

### Contraseñas seguras

Evitan accesos no autorizados.

---

## Objetivo de la seguridad

La seguridad busca:

* proteger información,
* evitar accesos indebidos,
* garantizar disponibilidad del sistema,
* reducir riesgos de ataques o errores.
