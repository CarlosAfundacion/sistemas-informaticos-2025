# PRÁCTICA

# INSTALACIÓN Y ADMINISTRACIÓN BÁSICA DE DEBIAN EN MÁQUINA VIRTUAL

## Objetivo

Instalar Debian en una máquina virtual y realizar una serie de tareas de administración básica utilizando la terminal, el sistema de archivos, la gestión de usuarios y el gestor de paquetes APT.

La práctica deberá entregarse en un único documento PDF que incluya:

* Capturas de pantalla claras y completas.
* Los comandos utilizados.
* Breves explicaciones técnicas cuando se soliciten.

No se aceptarán capturas recortadas que oculten información relevante.

---

# 1. Creación e instalación del sistema

## 1.1 Creación de la máquina virtual

Crear una máquina virtual en VirtualBox con las siguientes características:

* Nombre: `Debian_Sistemas`
* Tipo: Linux
* Versión: Debian (64 bits)
* Memoria RAM: mínimo 1024 MB
* Disco duro: 25 GB, dinámico

No se debe añadir más de un disco ni modificar configuraciones avanzadas. Se debe de desmarcar la instalación desatendida.

### Captura obligatoria 1

Ventana de configuración final de la máquina virtual antes de iniciarla.

---

## 1.2 Instalación de Debian

Instalar Debian desde la imagen ISO oficial utilizando:

* Opción: Graphical Install
* Idioma: Español
* Zona horaria: correspondiente
* Teclado: Español
* Red automática (DHCP)
* Particionado:

  * Guiado
  * Usar todo el disco
  * Todos los archivos en una sola partición

No se realizará particionado manual.

Durante la instalación:

* Definir contraseña para el usuario root.
* Crear un usuario normal con vuestro nombre y apellidos en minúsculas con el formato cmrodriguez.

En la selección de software, marcar únicamente:

* Entorno de escritorio XFCE
* Utilidades estándar del sistema

### Capturas obligatorias 2, 3 y 4

2. Pantalla de creación del usuario.
3. Pantalla donde se ve el tipo de particionado seleccionado.
4. Escritorio funcionando tras el primer inicio de sesión.

---

# 2. Instalación de Guest Additions

Desde el menú de VirtualBox:

Dispositivos → Insertar imagen de CD de las Guest Additions.

En Debian:

1. Abrir una terminal.
2. Ejecutar:




* Escribe `su` en la terminal e introduce la contraseña de root.
* Escribe el comando `usermod -aG sudo nombre_usuario` 
* Usa el comando `nano /etc/apt/sources.list`
* Comenta la línea de cd con `#`
* Añade las siguientes líneas al archivo y después pulsa Ctrl + o, ENTER, Ctrl + x:

```
deb http://deb.debian.org/debian trixie main contrib non-free non-free-firmware
deb http://deb.debian.org/debian trixie-updates main contrib non-free non-free-firmware
deb http://security.debian.org/debian-security trixie-security main contrib non-free non-free-firmware
```
* Desde la consola ejecuta los siguientes  comandos:

 ```
sudo apt update
sudo apt install build-essential dkms linux-headers-amd64
```

3. Ejecutar el instalador `VBoxLinuxAdditions.run` como root.
4. Reiniciar el sistema.

### Capturas obligatorias 5 y 6

5. Instalación de dependencias en la terminal.
6. Escritorio a pantalla completa tras reiniciar (Pincha en Dispositivos o Devices en la ventana de la máquina virtual y tiene que aparecer seleccionable la opción de actualizar las GuestAdditions).

---

# 3. Exploración del sistema de archivos

Desde la terminal:

Ejecutar:

```
ls -l /
```

Después listar:

```
ls -l /home
ls -l /etc
ls -l /var
```

Explicar brevemente qué contiene cada uno de esos directorios.

### Capturas obligatorias 7, 8 y 9

7. Listado del directorio raíz.
8. Listado de /home.
9. Listado de /etc.

---

# 4. Listado avanzado del directorio personal

Mostrar vuestro directorio personal usando:

* Ruta absoluta
* Mostrando archivos ocultos
* Ordenado por fecha

Ejemplo de comando:

```
ls -la -t /home/tu_usuario
```

Explicar qué significan las opciones usadas y cómo se identifican los directorios.

### Captura obligatoria 10

Resultado completo del comando anterior.

---

# 5. Sesiones TTY y usuarios

Abrir una terminal en modo texto (Ctrl + Alt + F3).

1. Iniciar sesión con vuestro usuario.
2. Abrir otra TTY (Ctrl + Alt + F4).
3. Iniciar sesión como root.
4. Volver al entorno gráfico (Ctrl + Alt + F2).
5. Ejecutar:

```
who
```

Explicar qué información muestra.

### Capturas obligatorias 11 y 12

11. Inicio de sesión en TTY.
12. Resultado del comando who.

---

# 6. Visualización de archivos del sistema

Visualizar el archivo:

```
/etc/passwd
```

Utilizando:

```
cat
less
head
tail
```

Explicar brevemente la diferencia entre cada comando.

### Capturas obligatorias 13 y 14

13. Visualización con cat.
14. Visualización con less o head.

---

# 7. Archivo protegido

Intentar acceder a:

```
/etc/shadow
```

Realizar tres intentos:

* Como usuario normal.
* Como usuario normal usando sudo.
* Como root.

Explicar el resultado en cada caso.

### Captura obligatoria 15

Intento fallido como usuario normal y acceso correcto como root.

---

# 8. Creación y copia de archivos

En vuestro directorio personal:

1. Crear un directorio llamado `copia`.
2. Copiar dentro:

   * /etc/passwd
   * /etc/hosts
3. Mostrar propietario y grupo con:

```
ls -l copia
```

Explicar por qué cambia el propietario.

### Captura obligatoria 16

Contenido del directorio copia con permisos visibles.

---

# 9. Subdirectorios y redirecciones

Dentro de `copia`:

1. Crear un directorio llamado `subcarpeta`.
2. Dentro de él crear un archivo llamado `saludo.txt` cuyo contenido sea:

```
Hola desde Debian
```

Usar redirección.

3. Mostrar su contenido.

### Captura obligatoria 17

Creación y contenido del archivo saludo.txt.

---

# 10. Directorio oculto

Crear un directorio oculto llamado:

```
.privado
```

Ejecutar:

```
ls -l
ls -la
```

Explicar la diferencia.

### Captura obligatoria 18

Diferencia entre listado normal y listado con -a.

---

# 11. Copia recursiva y eliminación

1. Crear un directorio `practica`.
2. Copiar dentro el directorio `copia` completo usando `-r`.
3. Listar recursivamente usando `-R`.
4. Eliminar completamente `practica`.

Explicar qué hace la opción `-r`.

### Captura obligatoria 19

Listado recursivo antes de eliminar.

---

# 12. Redirecciones de salida y errores

Crear en vuestro directorio personal dos archivos:

## listado_etc.txt

Debe contener:

* Una línea inicial indicando:
  "Listado recursivo del directorio /etc"
* El listado completo y recursivo de /etc en formato largo.

## errores_etc.txt

Debe contener los errores generados durante el listado.

Utilizar redirección de salida estándar y de errores.

### Captura obligatoria 20

Contenido de ambos archivos.

---

# 13. Uso de tuberías

Realizar:

1. Listar /etc usando paginación.
2. Contar el número de líneas de /etc/passwd.
3. Mostrar solo las primeras 5 líneas.

Explicar qué es una tubería y qué símbolo se utiliza.

### Captura obligatoria 21

Uso correcto de tuberías.

---

# 14. Gestión de paquetes

Ejecutar:

```
sudo apt update
sudo apt upgrade
```

Instalar:

* gparted
* aptitude

Intentar instalar:

```
sudo apt install 7zip
```

Explicar el resultado.

Buscar el paquete correcto:

```
apt search 7zip
```

Instalar el paquete adecuado (p7zip).

Comprobar que gparted arranca correctamente (como root).

### Capturas obligatorias 22 y 23

22. Instalación de paquetes.
23. gparted ejecutándose correctamente.

---

# Entrega final

Documento PDF estructurado, con:

* Todas las capturas numeradas.
* Comandos visibles completos.
* Explicaciones técnicas claras.
* Sin copiar teoría literal de los apuntes.


