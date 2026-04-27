# TAREA 9

# ADMINISTRACIÓN DE REDES GNU-LINUX

---

# Prerrequisito — Instalación de Debian en modo consola en VirtualBox

## Descripción general

Antes de comenzar la práctica necesitas tener instalada una máquina Debian en VirtualBox configurada en **modo consola**, es decir, sin entorno gráfico. Este tipo de instalación es la habitual en servidores reales: consume menos RAM, menos CPU y tiene menos software instalado, lo que reduce posibles fallos y la superficie de ataque.

Esta guía cubre la instalación completa desde cero. Si ya tienes una máquina Debian instalada de una práctica anterior, puedes usarla directamente y pasar al Ejercicio 0, pero comprueba primero que cumple los requisitos de la tabla siguiente.

---

## Recursos mínimos necesarios

Antes de crear la máquina virtual, comprueba que tu equipo anfitrión tiene recursos suficientes. Esta máquina actúa como servidor sin entorno gráfico, por lo que los requisitos son modestos:

| Recurso | Mínimo recomendado | Observaciones |
|---|---|---|
| RAM asignada a la VM | 1024 MB | Con 512 MB funciona, pero puede ir lento durante la instalación de paquetes. |
| Disco duro virtual | 20 GB dinámico | El sistema base ocupa unos 2-3 GB. El resto queda libre para los servicios de la práctica. |
| Procesadores virtuales | 1 | No es necesario más de uno para esta práctica. |
| Adaptadores de red | 2 | Se configuran después de la instalación: uno NAT y uno de red interna. Durante la instalación solo se usa el NAT por defecto. |
| ISO de Debian | Versión estable actual | Descárgala de `https://www.debian.org` → «Getting Debian» → «CD/DVD Images» → imagen **netinst de 64 bits**. Pesa unos 400 MB. |

> **Imagen netinst vs imagen completa:** la netinst descarga los paquetes durante la instalación y necesita conexión a Internet. Si el aula no tiene conexión fiable, usa la imagen DVD completa (unos 4 GB), que incluye todos los paquetes sin necesidad de descargar nada durante el proceso.

---

## Parte 1. Crear la máquina virtual en VirtualBox

### Pasos

1. Abre VirtualBox y haz clic en **Nueva**.

2. En la pantalla de nombre y sistema operativo:
   * **Nombre:** `debian-servidor`
   * **Carpeta:** deja la predeterminada o elige una ubicación con espacio suficiente.
   * **Imagen ISO:** haz clic en el desplegable y selecciona **Otra…** para localizar la ISO de Debian que has descargado.
   * VirtualBox debería detectar automáticamente el tipo. Comprueba que aparece **Tipo: Linux** y **Versión: Debian (64-bit)**.

3. Marca la casilla **Saltar instalación desatendida**. Es imprescindible marcarla: la instalación desatendida de VirtualBox selecciona opciones automáticamente que no nos convienen para esta práctica.

4. Haz clic en **Siguiente**.

5. En la pantalla de hardware:
   * **Memoria base (RAM):** `1024 MB`
   * **Procesadores:** `1`

6. Haz clic en **Siguiente**.

7. En la pantalla de disco duro virtual:
   * Selecciona **Crear un disco duro virtual ahora**.
   * **Tamaño:** `20 GB`
   * **Tipo de archivo:** VDI (predeterminado)
   * **Almacenamiento:** **Reservado dinámicamente** (el archivo crece según se necesite, no ocupa los 20 GB desde el principio).

8. Haz clic en **Siguiente** y después en **Terminar**.

### Qué debes comprobar

La máquina aparece en la lista de VirtualBox con el nombre `debian-servidor`, tipo Linux, y con la ISO montada en la unidad óptica.

### Capturas obligatorias

**CP.1** Pantalla de resumen final de la nueva máquina en VirtualBox, donde se vean el nombre, la RAM y el tamaño de disco.

---

## Parte 2. Iniciar la instalación de Debian

### Pasos

1. Selecciona `debian-servidor` y haz clic en **Iniciar**.

2. Aparece el menú de arranque del instalador de Debian. Con las teclas de cursor, selecciona:

   ```
   Install
   ```

   > No selecciones «Graphical install». Usaremos la instalación en modo texto, que es la habitual en servidores.

3. **Selecciona el idioma:** `Español`.

4. **Selecciona tu ubicación:** `España`.

5. **Configura el teclado:** `Español`.

### Capturas obligatorias

**CP.2** Menú de arranque del instalador con la opción «Install» seleccionada.

---

## Parte 3. Configuración de red durante la instalación

### Pasos

1. El instalador intentará configurar la red automáticamente por DHCP. Deja que lo haga; es necesario para descargar paquetes si usas la imagen netinst.

2. **Nombre de la máquina:** escribe exactamente:

   ```
   debian-servidor
   ```

3. **Nombre de dominio:** déjalo en blanco y pulsa **Continuar**.

### Capturas obligatorias

**CP.3** Pantalla de nombre de máquina mostrando `debian-servidor`.

---

## Parte 4. Configurar contraseñas y usuario

### Pasos

1. **Contraseña del superusuario (root):** elige una contraseña que recuerdes. La necesitarás para administrar el servidor.

2. **Volver a introducir la contraseña de root:** confírmala.

3. **Nombre completo del nuevo usuario:** escribe tu nombre real.

4. **Nombre de usuario para la cuenta:** escribe tu usuario en minúsculas, por ejemplo `alumno`.

5. **Contraseña del nuevo usuario:** elige una contraseña.

### Capturas obligatorias

**CP.4** Pantalla de creación del usuario normal mostrando el nombre de usuario elegido.

---

## Parte 5. Particionado del disco

### Pasos

1. Selecciona:

   ```
   Guiado - utilizar todo el disco
   ```

2. Selecciona el único disco disponible (normalmente `sda`).

3. Selecciona:

   ```
   Todos los ficheros en una partición (recomendado para nuevos usuarios)
   ```

4. Selecciona **Finalizar el particionado y escribir los cambios en el disco**.

5. En la confirmación «¿Desea escribir los cambios en los discos?» selecciona **Sí**.

### Por qué no se hace particionado manual

Para esta práctica no es necesario. El particionado manual sería útil en producción para separar `/var` o `/srv` en particiones independientes y evitar que un servicio que llene el disco afecte al sistema completo. En el contexto de esta práctica, una sola partición es suficiente.

### Capturas obligatorias

**CP.5** Pantalla de selección del método de particionado con «Guiado - utilizar todo el disco» seleccionado.

**CP.6** Resumen del particionado antes de confirmar la escritura en disco.

---

## Parte 6. Instalación del sistema base

### Pasos

1. El instalador copia el sistema base. Este proceso tarda varios minutos.

2. **Gestor de paquetes — configurar una réplica:** selecciona `España` y elige `ftp.es.debian.org` o cualquier espejo disponible. Si el aula no tiene Internet, selecciona «No usar una réplica de red».

3. **Participar en el estudio de uso de paquetes:** selecciona `No`.

---

## Parte 7. Selección de software — la pantalla más importante

### Pasos

1. Aparece la pantalla **Selección de programas**. Esta es la pantalla más importante de toda la instalación para esta práctica.

2. **Desmarca todas las opciones** que estén marcadas excepto:

   ```
   [*] Utilidades estándar del sistema
   ```

   Asegúrate de que las siguientes opciones **no están marcadas**:

   ```
   [ ] Entorno de escritorio Debian
   [ ] GNOME
   [ ] Xfce
   [ ] KDE Plasma
   [ ] Cinnamon
   [ ] MATE
   [ ] LXDE
   [ ] LXQt
   [ ] Servidor web
   [ ] Servidor SSH
   [ ] Impresora estándar
   ```

   > No instales ningún entorno de escritorio. Esta máquina es un servidor y arrancará directamente en modo consola. Si instalas un entorno gráfico, consumirá RAM y CPU innecesariamente. Los servicios web y SSH los instalaremos manualmente durante la práctica para aprender el proceso completo.

3. Con solo «Utilidades estándar del sistema» marcado, pulsa **Continuar**.

### Capturas obligatorias

**CP.7** Pantalla de selección de software mostrando únicamente «Utilidades estándar del sistema» marcado y sin ningún entorno de escritorio seleccionado.

---

## Parte 8. Instalar el cargador de arranque GRUB

### Pasos

1. Selecciona **Sí** para instalar GRUB en el disco principal.

2. Selecciona el dispositivo: normalmente `/dev/sda`.

---

## Parte 9. Finalizar la instalación y primer arranque

### Pasos

1. El instalador muestra «La instalación ha finalizado». Pulsa **Continuar**. La ISO se desmonta automáticamente en VirtualBox.

2. La máquina reinicia y arranca desde el disco duro por primera vez. Aparece la pantalla de login en modo texto:

   ```
   debian-servidor login:
   ```

3. Inicia sesión con el usuario que creaste. Escribe el nombre de usuario, pulsa Enter, después la contraseña (no se muestra mientras escribes) y pulsa Enter.

4. Verifica que el sistema responde:

   ```bash
   hostname
   whoami
   ```

   `hostname` debe devolver `debian-servidor` y `whoami` debe devolver tu nombre de usuario.

### Capturas obligatorias

**CP.8** Pantalla de login en modo texto con el prompt `debian-servidor login:`.

**CP.9** Resultado de `hostname` y `whoami` tras iniciar sesión.

---

## Parte 10. Configurar sudo para el usuario normal

Durante la práctica usarás `sudo` para ejecutar comandos con privilegios de administrador. Si el instalador no añadió tu usuario al grupo sudo, hazlo ahora:

### Pasos

1. Entra como root:

   ```bash
   su -
   ```

2. Añade tu usuario al grupo sudo (sustituye `alumno` por tu nombre real):

   ```bash
   usermod -aG sudo alumno
   ```

3. Sal de root:

   ```bash
   exit
   ```

4. Cierra tu sesión de usuario normal también con `exit` y vuelve a iniciar sesión. Los cambios de grupo no surten efecto hasta abrir una nueva sesión.

5. Comprueba que sudo funciona:

   ```bash
   sudo whoami
   ```

   Debe responder `root`.

### Capturas obligatorias

**CP.10** Resultado de `sudo whoami` devolviendo `root`, confirmando que sudo está correctamente configurado.

---

## Parte 11. Actualizar el sistema antes de empezar

```bash
sudo apt update
sudo apt upgrade -y
```

Este proceso puede tardar varios minutos la primera vez.

### Capturas obligatorias

**CP.11** Resultado de `sudo apt update` mostrando que los repositorios se han actualizado correctamente.

---

## Errores típicos durante la instalación

**Error: la máquina arranca desde la ISO tras la instalación en lugar del disco duro**
La ISO sigue montada. En VirtualBox, apaga la máquina → Configuración → Almacenamiento → selecciona la ISO → icono del disco → «Eliminar disco de la unidad virtual». Vuelve a iniciar.

**Error: el instalador dice que no detecta la red**
Si usas imagen netinst y no hay Internet en el aula, la instalación puede fallar al descargar paquetes. Usa la imagen DVD completa.

**Error: sudo no funciona después de añadir el usuario al grupo**
Los cambios de grupo solo surten efecto al abrir una nueva sesión. Sal completamente con `exit` y vuelve a entrar.

**Error: el sistema arranca en modo de emergencia o con errores de disco**
El particionado no se completó correctamente. Reinstala la máquina desde el principio.

---

# Introducción

En esta práctica vas a trabajar los bloques fundamentales de la administración de redes en sistemas GNU/Linux. Usarás la máquina `debian-servidor` que acabas de instalar como servidor, y la máquina Debian con entorno gráfico de la práctica anterior como cliente.

Construirás una infraestructura cliente-servidor con ambos equipos conectados en red interna, compartirás recursos con Samba y NFS, administrarás el servidor de forma remota con SSH y publicarás una página web con Apache.

El objetivo no es solo que sigas una serie de pasos, sino que comprendas:

* qué hace cada herramienta o comando,
* en qué situaciones se utiliza,
* qué información devuelve,
* por qué una configuración funciona o falla,
* y cómo comprobar que el resultado es correcto.

---

## Coherencia con las prácticas anteriores

Esta práctica forma parte de una secuencia. Para que todo funcione correctamente, es fundamental que los usuarios y grupos de Linux coincidan con los que creaste en la **Tarea 5 de Windows**.

### Grupos y usuarios que debes tener en Linux

En la **Tarea 5 de Windows** creaste:

* dos grupos: `administracion` y `ventas`
* cuatro usuarios: dos pertenecientes a `administracion` y dos a `ventas`
* el nombre de cada usuario sigue el formato inicial del nombre + apellido (ejemplo: `alopez`)

En la **Tarea 7 de Linux** creaste grupos y usuarios distintos (`informatico`, `vendedor`, `luis`, `Lorena`, etc.) que se usaron para aprender administración local del sistema. Esos usuarios **no son válidos aquí** porque no coinciden con los de Windows.

En esta práctica debes crear en Linux **exactamente los mismos usuarios** que tienes en Windows, con **exactamente las mismas contraseñas**. Así, cuando una máquina Windows acceda a un recurso Samba en el servidor Linux, la autenticación funcionará de forma automática.

### Tabla de usuarios que usarás en esta práctica

Completa esta tabla con tus propios datos antes de empezar:

| Usuario Linux | Grupo Linux    | Equivalente en Windows   | Grupo Windows  |
|---------------|----------------|--------------------------|----------------|
| (tu admin1)   | administracion | (tu admin1 en cliente1)  | administracion |
| (tu admin2)   | administracion | (tu admin2 en cliente1)  | administracion |
| (tu ventas1)  | ventas         | (tu ventas1 en cliente1) | ventas         |
| (tu ventas2)  | ventas         | (tu ventas2 en cliente1) | ventas         |

> **Ejemplo:** si en Windows creaste `alopez` y `mgarcia` para administración y `jperez` y `lruiz` para ventas, esos son exactamente los nombres que crearás en Linux en el Ejercicio 0.

### Por qué esto es necesario

Cuando Windows accede a un recurso compartido Samba, envía automáticamente el nombre de usuario y contraseña de la sesión activa. Si ese usuario existe en Linux con la misma contraseña, el acceso es transparente. Si no coinciden, Windows pedirá credenciales o denegará el acceso.

---

## Qué vas a aprender en esta práctica

Al finalizar, deberías ser capaz de:

* configurar dos máquinas Debian en una red interna de VirtualBox,
* entender la diferencia entre una red NAT y una red interna,
* asignar direcciones IP estáticas en Linux,
* hacer que Linux funcione como router para otros equipos de la red,
* comprobar conectividad entre equipos con herramientas de diagnóstico,
* compartir carpetas con Samba controlando el acceso por grupos,
* acceder a recursos Samba desde un cliente Linux y desde Windows,
* compartir carpetas entre sistemas Linux con NFS,
* administrar el servidor de forma remota con SSH,
* transferir archivos entre equipos con `scp`,
* publicar una página web sencilla con Apache,
* y montar recursos de red de forma permanente con `/etc/fstab`.

---

## Entrega

La práctica deberá entregarse en un único documento PDF que incluya:

* capturas de pantalla claras y completas,
* los comandos utilizados cuando se soliciten,
* explicaciones breves y técnicas cuando se indiquen,
* y debajo de cada captura una línea con el texto:

**Qué demuestra esta captura**

No se aceptarán capturas recortadas que oculten información importante. En las capturas de terminal deben verse siempre el comando ejecutado y su resultado completo.

---

## Estimación de tiempo

Esta práctica está diseñada para completarse en **3 semanas de 6 sesiones de 50 minutos**:

**Semana 1**

* Sesiones 1 y 2: Prerrequisito (instalación de Debian) + Ejercicio 0 (usuarios)
* Sesiones 3 y 4: Ejercicio 1 (red) + Ejercicio 2 (enrutamiento)
* Sesiones 5 y 6: Ejercicio 3 (Samba)

**Semana 2**

* Sesiones 1 y 2: Ejercicio 4 (acceso a Samba desde Linux y Windows)
* Sesiones 3 y 4: Ejercicio 5 (NFS)
* Sesiones 5 y 6: Ejercicio 6 (SSH y SCP)

**Semana 3**

* Sesiones 1 y 2: Ejercicio 7 (Apache)
* Sesiones 3 y 4: Ejercicio 8 (diagnóstico de red)
* Sesiones 5 y 6: Corrección de errores y preparación de la entrega

---

# Conceptos previos necesarios

---

## 1. Qué es una dirección IP

Una dirección IP identifica a un equipo dentro de una red. En esta práctica usarás:

```
Servidor Debian (debian-servidor): 192.168.1.10
Cliente Debian  (práctica anterior): 192.168.1.11
```

Estos valores están en la misma red que usaste en la Tarea 8 de Windows (`192.168.1.X`) para que todas las máquinas puedan verse entre sí cuando estén conectadas a la misma red interna.

---

## 2. Qué es la máscara de subred

La máscara indica qué parte de la dirección IP identifica la red. En esta práctica usarás `255.255.255.0`, igual que en Windows.

---

## 3. Qué es una red NAT y qué es una red interna en VirtualBox

**NAT** permite a la máquina virtual salir a Internet. Es necesaria para instalar paquetes con `apt`.

**Red interna** permite que varias máquinas virtuales se comuniquen entre sí en una red aislada. Es la misma red `intranet` que usaste en la Tarea 8 de Windows, por lo que si conectas las máquinas Linux a esa misma red, todas podrán verse entre sí.

Cada máquina Debian tendrá dos tarjetas:

* **Adaptador 1**: NAT — para salir a Internet e instalar paquetes
* **Adaptador 2**: Red interna `intranet` — para comunicarse con el resto de máquinas

---

## 4. Qué son los servicios en Linux

Un servicio se gestiona con el comando `service`:

```bash
service nombre start    # iniciar
service nombre stop     # parar
service nombre restart  # reiniciar
service nombre status   # consultar estado
```

En esta práctica trabajarás con `smbd`, `nmbd`, `nfs-kernel-server`, `ssh` y `apache2`.

---

## 5. Qué es Samba y para qué sirve

Samba permite que un equipo Linux comparta carpetas usando el protocolo SMB, el mismo que usa Windows. La configuración se guarda en `/etc/samba/smb.conf`. Para que un usuario pueda autenticarse, debe existir como usuario Linux **y** estar dado de alta en Samba con `smbpasswd`.

---

## 6. Qué es NFS y para qué sirve

NFS comparte directorios entre equipos Linux de forma nativa. Es más sencillo que Samba pero no es compatible con Windows. La configuración se guarda en `/etc/exports`.

---

## 7. Qué es SSH y para qué sirve

SSH permite conectarse de forma remota a otro equipo Linux y trabajar en su terminal. Usa el puerto 22 y cifra toda la comunicación. Incluye el comando `scp` para copiar archivos entre equipos de forma segura.

---

## 8. Qué es Apache y para qué sirve

Apache es el servidor web más utilizado en Linux. Publica por defecto los archivos de `/var/www/html`. Escucha por el puerto 80. Es el equivalente Linux de IIS, que usaste en la Tarea 8 de Windows.

---

# Ejercicio 0 — Preparar usuarios y grupos en el servidor Linux

## Descripción general

Antes de configurar ningún servicio, debes tener en `debian-servidor` los mismos usuarios y grupos que creaste en la Tarea 5 de Windows. Este ejercicio es el paso más importante de toda la práctica: sin él, la autenticación cruzada entre Linux y Windows no funcionará.

---

## Teoría necesaria

Cuando un equipo Windows accede a un recurso Samba en Linux, envía automáticamente el nombre de usuario y la contraseña de la sesión activa. Si el usuario existe en Linux con la misma contraseña, el acceso es transparente. Si no coincide, Windows pedirá credenciales o denegará el acceso.

---

## Parte 1. Crear los grupos

### Qué se pide

En `debian-servidor`:

```bash
sudo groupadd administracion
sudo groupadd ventas
```

Verifica:

```bash
cat /etc/group | grep administracion
cat /etc/group | grep ventas
```

### Capturas obligatorias

**C0.1** Resultado de la creación o verificación de los grupos `administracion` y `ventas`.

---

## Parte 2. Crear los usuarios

### Qué se pide

Crea los cuatro usuarios con los nombres exactos de la Tarea 5 de Windows (los comandos usan nombres genéricos; sustitúyelos por los tuyos):

```bash
# Usuarios del grupo administracion
sudo useradd -m -g administracion admin1
sudo useradd -m -g administracion admin2

# Usuarios del grupo ventas
sudo useradd -m -g ventas ventas1
sudo useradd -m -g ventas ventas2
```

Asigna la misma contraseña que en Windows:

```bash
sudo passwd admin1
sudo passwd admin2
sudo passwd ventas1
sudo passwd ventas2
```

Comprueba:

```bash
cat /etc/passwd | grep admin1
cat /etc/passwd | grep ventas1
```

### Capturas obligatorias

**C0.2** Creación de los cuatro usuarios con `useradd`.

**C0.3** Asignación de contraseñas con `passwd`.

**C0.4** Verificación de los usuarios en `/etc/passwd`.

---

## Parte 3. Verificar la pertenencia a grupos

### Qué se pide

```bash
id admin1
id admin2
id ventas1
id ventas2
```

### Capturas obligatorias

**C0.5** Resultado de `id` para los cuatro usuarios mostrando el grupo correcto.

---

## Errores típicos del ejercicio 0

**Error: `useradd` dice que el usuario ya existe**
Comprueba con `id nombre_usuario`. Si existe con grupo incorrecto, usa `sudo usermod -g nombre_grupo nombre_usuario`.

**Error: el nombre del usuario no coincide con el de Windows**
Linux distingue mayúsculas y minúsculas. `Admin1` y `admin1` son usuarios distintos.

---

# Ejercicio 1 — Configuración de la red en VirtualBox

## Descripción general

En este ejercicio configurarás las tarjetas de red de `debian-servidor` y del cliente Debian de la práctica anterior, y asignarás las direcciones IP en el rango `192.168.1.X`, el mismo que usaste en la Tarea 8 de Windows.

---

## Teoría necesaria

En Linux, las interfaces de red tienen nombres como `enp0s3` o `enp0s8`. Comprueba siempre el nombre real con `ip a` antes de editar ningún archivo de configuración.

---

## Parte 1. Configurar los adaptadores en VirtualBox

### Qué se pide

En **ambas máquinas**, con cada una apagada, configura en VirtualBox:

* **Adaptador 1** → NAT
* **Adaptador 2** → Red interna, nombre: `intranet`

El nombre `intranet` debe ser exactamente el mismo que usaste en la Tarea 8 de Windows.

### Capturas obligatorias

**C1.1** Adaptador 2 de `debian-servidor` configurado como red interna `intranet`.

**C1.2** Adaptador 2 del cliente Debian configurado como red interna `intranet`.

---

## Parte 2. Identificar las interfaces de red

### Qué se pide

Enciende ambas máquinas y ejecuta en cada una:

```bash
ip a
```

Identifica la interfaz NAT (IP tipo `10.0.2.X`) y la interfaz de red interna (sin IP). Anota el nombre exacto de la segunda antes de continuar.

### Capturas obligatorias

**C1.3** Resultado de `ip a` en `debian-servidor`.

**C1.4** Resultado de `ip a` en el cliente Debian.

---

## Parte 3. Asignar IP estática en el servidor

### Qué se pide

En `debian-servidor`:

```bash
sudo nano /etc/network/interfaces
```

Añade al final:

```
auto enp0s8
iface enp0s8 inet static
address 192.168.1.10
netmask 255.255.255.0
```

> Sustituye `enp0s8` por el nombre real de tu interfaz si es diferente.

Guarda con `Ctrl + O`, `Enter`, `Ctrl + X`. Reinicia el servicio de red:

```bash
sudo service networking restart
ip a
```

Debes ver `192.168.1.10` en la interfaz de red interna.

### Capturas obligatorias

**C1.5** Contenido del archivo `/etc/network/interfaces` del servidor.

**C1.6** Resultado de `ip a` en el servidor mostrando `192.168.1.10`.

---

## Parte 4. Asignar IP estática en el cliente

### Qué se pide

En el cliente Debian de la práctica anterior:

```bash
sudo nano /etc/network/interfaces
```

Añade al final:

```
auto enp0s8
iface enp0s8 inet static
address 192.168.1.11
netmask 255.255.255.0
gateway 192.168.1.10
```

El cliente usará como puerta de enlace la IP del servidor, porque en el siguiente ejercicio el servidor actuará como router.

```bash
sudo service networking restart
ip a
```

### Capturas obligatorias

**C1.7** Archivo `/etc/network/interfaces` del cliente.

**C1.8** Resultado de `ip a` en el cliente mostrando `192.168.1.11`.

---

## Parte 5. Comprobar conectividad

### Qué se pide

Desde el cliente Debian:

```bash
ping -c 4 192.168.1.10
```

Desde `debian-servidor`:

```bash
ping -c 4 192.168.1.11
```

Si tienes las máquinas Windows de la Tarea 8 activas en la misma red interna, prueba también:

```bash
ping -c 4 192.168.1.10    # cliente1 Windows, si está encendido
```

### Qué debes explicar en el PDF

* Qué comprueba el comando `ping`.
* Por qué es importante que funcione en los dos sentidos antes de continuar.

### Capturas obligatorias

**C1.9** Ping correcto del cliente Debian al servidor.

**C1.10** Ping correcto del servidor al cliente Debian.

---

## Errores típicos del ejercicio 1

**Error: la IP no aparece tras reiniciar el servicio de red**
Comprueba que el nombre de la interfaz en `/etc/network/interfaces` coincide exactamente con el que muestra `ip a`.

**Error: el ping entre Linux y Windows no funciona**
Comprueba que el firewall de Windows tiene activada la regla «Permitir ping» que creaste en la Tarea 8.

---

# Ejercicio 2 — Enrutamiento y diagnóstico de red

## Descripción general

Configurarás `debian-servidor` para que actúe como router, permitiendo que el cliente Debian salga a Internet. Después aprenderás a diagnosticar problemas de red de forma ordenada y configurarás la resolución de nombres local.

---

## Teoría necesaria

Por defecto, Linux no reenvía paquetes entre interfaces aunque tenga dos tarjetas. Para que actúe como router hay que activar el **reenvío IP** y añadir una regla en `iptables` para que el tráfico de la red interna salga enmascarado a través de la interfaz NAT.

---

## Parte 1. Activar el enrutamiento en el servidor

### Qué se pide

En `debian-servidor`:

```bash
cat /proc/sys/net/ipv4/ip_forward
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
sudo iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -d 0/0 -j MASQUERADE
cat /proc/sys/net/ipv4/ip_forward
```

El último comando debe mostrar `1`.

### Capturas obligatorias

**C2.1** Valor de `ip_forward` antes (0) y después (1) de activarlo.

**C2.2** Ejecución del comando `iptables`.

---

## Parte 2. Comprobar la salida a Internet desde el cliente

### Qué se pide

Desde el cliente Debian:

```bash
ping -c 4 8.8.8.8
ping -c 4 google.com
```

Si el ping a `8.8.8.8` funciona pero el de `google.com` falla, el cliente no tiene DNS. Edita `/etc/resolv.conf` y añade:

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

### Capturas obligatorias

**C2.3** Ping correcto a `8.8.8.8` desde el cliente.

**C2.4** Ping correcto a `google.com` desde el cliente.

---

## Parte 3. Hacer el enrutamiento permanente

### Qué se pide

```bash
sudo nano /etc/rc.local
```

Contenido exacto:

```bash
#!/bin/bash
echo 1 > /proc/sys/net/ipv4/ip_forward
iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -d 0/0 -j MASQUERADE
exit 0
```

```bash
sudo chmod +x /etc/rc.local
```

Reinicia el servidor y comprueba desde el cliente:

```bash
ping -c 4 google.com
```

### Capturas obligatorias

**C2.5** Contenido del archivo `/etc/rc.local`.

**C2.6** Ping correcto a `google.com` desde el cliente después de reiniciar el servidor.

---

## Parte 4. Diagnóstico ordenado de red

### Qué se pide

Desde el cliente Debian:

```bash
ping -c 4 127.0.0.1          # nivel 1: el propio sistema
ping -c 4 192.168.1.11        # nivel 2: la propia IP
ping -c 4 192.168.1.10        # nivel 3: el servidor
ping -c 4 8.8.8.8             # nivel 4: Internet por IP
ping -c 4 google.com          # nivel 5: Internet por nombre
```

### Qué debes explicar en el PDF

1. ¿Qué significa que el nivel 3 falle pero el nivel 2 funcione?
2. ¿Qué diferencia hay entre hacer ping a `8.8.8.8` y a `google.com`?
3. Si el nivel 4 funciona pero el nivel 5 falla, ¿qué parte no está funcionando?
4. ¿Por qué es útil diagnosticar por niveles en lugar de ir directamente al servicio que falla?

### Capturas obligatorias

**C2.7** Resultado de los cinco ping por niveles desde el cliente.

**C2.8** Respuestas escritas a las cuatro preguntas de diagnóstico.

---

## Parte 5. Resolución de nombres local con `/etc/hosts`

### Qué se pide

En el cliente Debian y en `debian-servidor`, edita `/etc/hosts` en ambas máquinas y añade:

```
192.168.1.10 debian-servidor
192.168.1.11 debian-cliente
```

Comprueba:

```bash
ping -c 4 debian-servidor    # desde el cliente
ping -c 4 debian-cliente     # desde el servidor
```

### Qué debes explicar en el PDF

* Qué diferencia hay entre `/etc/hosts` y un servidor DNS.
* Por qué en una red grande no sería suficiente con `/etc/hosts`.

### Capturas obligatorias

**C2.9** Archivo `/etc/hosts` del cliente con las líneas añadidas.

**C2.10** Ping por nombre desde el cliente a `debian-servidor`.

---

## Errores típicos del ejercicio 2

**Error: `ping google.com` falla con «Temporary failure in name resolution»**
El cliente no tiene DNS. Añade `nameserver 8.8.8.8` en `/etc/resolv.conf`.

**Error: el enrutamiento deja de funcionar tras reiniciar**
Comprueba que `/etc/rc.local` tiene permiso de ejecución con `ls -l /etc/rc.local`.

---

# Ejercicio 3 — Samba: recurso público y recursos privados

## Descripción general

Instalarás Samba en el servidor y crearás tres recursos compartidos que replican la estructura de permisos de la Tarea 8 de Windows: una carpeta pública, una carpeta de administración solo para ese grupo y una carpeta de ventas donde `ventas` puede escribir pero `administracion` solo puede leer.

---

## Teoría necesaria

Samba trabaja con dos capas de control de acceso:

* **Permisos del sistema de archivos Linux** (`chmod`, `chgrp`): controlan el acceso real al disco.
* **Configuración en `smb.conf`**: controla el acceso a través de la red.

Si alguna de las dos capas deniega el acceso, el resultado final es denegado. Es el mismo principio que en Windows con los permisos de compartición y los permisos NTFS.

---

## Parte 1. Instalar Samba

### Qué se pide

En `debian-servidor`:

```bash
sudo apt update
sudo apt install samba samba-common-bin
service smbd status
service nmbd status
```

### Capturas obligatorias

**C3.1** Instalación de Samba completada.

**C3.2** Estado del servicio `smbd` mostrando que está activo.

---

## Parte 2. Crear las carpetas y configurar permisos

### Qué se pide

```bash
sudo mkdir -p /samba/publica
sudo mkdir -p /samba/administracion
sudo mkdir -p /samba/ventas

echo "Carpeta pública - acceso libre de lectura" | sudo tee /samba/publica/leeme.txt
echo "Carpeta de administración - solo grupo administracion" | sudo tee /samba/administracion/info_admin.txt
echo "Carpeta de ventas - ventas escribe, administracion solo lee" | sudo tee /samba/ventas/info_ventas.txt

sudo chmod 755 /samba/publica
sudo chgrp -R administracion /samba/administracion
sudo chmod 770 /samba/administracion
sudo chgrp -R ventas /samba/ventas
sudo chmod 775 /samba/ventas

ls -ld /samba/publica /samba/administracion /samba/ventas
```

### Capturas obligatorias

**C3.3** Creación de las tres carpetas y sus archivos de prueba.

**C3.4** Permisos de las tres carpetas con `ls -ld`.

---

## Parte 3. Configurar el archivo `smb.conf`

### Qué se pide

```bash
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.original
sudo nano /etc/samba/smb.conf
```

Localiza la línea de `workgroup` y escribe el mismo nombre de grupo de trabajo que usaste en Windows:

```
workgroup = EMPRESA
```

Añade al final del archivo:

```
[publica]
path = /samba/publica
browseable = yes
guest ok = yes
read only = yes

[administracion]
path = /samba/administracion
browseable = yes
guest ok = no
read only = no
valid users = @administracion

[ventas]
path = /samba/ventas
browseable = yes
guest ok = no
read only = no
valid users = @ventas, @administracion
write list = @ventas
```

### Explicación de las opciones clave

`workgroup = EMPRESA` hace que el servidor Samba aparezca en el mismo grupo de trabajo que los equipos Windows de la Tarea 8.

`guest ok = yes` en `publica` permite el acceso sin contraseña.

`valid users = @administracion` restringe el acceso a los usuarios del grupo `administracion`. El `@` indica que es un grupo, no un usuario individual.

`write list = @ventas` restringe la escritura solo al grupo `ventas`. Los usuarios de `administracion` podrán entrar pero no escribir, replicando exactamente la configuración NTFS de la Tarea 8 donde `administracion` tenía solo «Lectura y ejecución» en la carpeta Ventas.

```bash
testparm
sudo service smbd restart
sudo service nmbd restart
```

### Capturas obligatorias

**C3.5** Copia de seguridad del archivo original.

**C3.6** Los tres bloques de recursos añadidos al final de `smb.conf`.

**C3.7** Resultado de `testparm` sin errores.

**C3.8** Reinicio de `smbd` y `nmbd`.

---

## Parte 4. Dar de alta los usuarios en Samba

### Qué se pide

```bash
sudo smbpasswd -a admin1
sudo smbpasswd -a admin2
sudo smbpasswd -a ventas1
sudo smbpasswd -a ventas2
```

Introduce la misma contraseña que en Windows para que la autenticación sea transparente.

### Capturas obligatorias

**C3.9** Alta de los cuatro usuarios en Samba con `smbpasswd`.

---

## Errores típicos del ejercicio 3

**Error: `testparm` muestra «Unknown parameter»**
Hay un error de escritura. `testparm` indica el número de línea exacto.

**Error: un usuario de `administracion` puede escribir en `ventas`**
Verifica que `write list = @ventas` está en el bloque `[ventas]` y que has reiniciado Samba.

---

# Ejercicio 4 — Acceso a Samba desde cliente Linux y desde Windows

## Descripción general

Accederás a los recursos Samba desde el cliente Debian usando la terminal, y desde las máquinas Windows de la Tarea 8. Comprobarás que los permisos funcionan de forma equivalente a la configuración NTFS de Windows.

---

## Parte 1. Acceso desde el cliente Linux por terminal

### Qué se pide

En el cliente Debian:

```bash
sudo apt install samba-common-bin cifs-utils smbclient
smbclient -L //192.168.1.10 -N
```

Accede al recurso público sin contraseña:

```bash
smbclient //192.168.1.10/publica -N
```

Dentro: `ls`, `get leeme.txt`, `quit`. Comprueba que el archivo se descargó.

Intenta escribir en el recurso público (debe fallar):

```bash
smbclient //192.168.1.10/publica -N
put leeme.txt prueba_escritura.txt
quit
```

Accede al recurso `administracion` con `admin1`:

```bash
smbclient //192.168.1.10/administracion -U admin1
```

Dentro: `ls`, `put leeme.txt desde_linux.txt`, `quit`.

Accede al recurso `ventas` con `admin1` e intenta escribir (debe fallar):

```bash
smbclient //192.168.1.10/ventas -U admin1
put leeme.txt intento_escritura.txt
quit
```

### Capturas obligatorias

**C4.1** Listado de recursos con `smbclient -L`.

**C4.2** Acceso al recurso público y descarga de `leeme.txt`.

**C4.3** Intento fallido de escritura en el recurso público.

**C4.4** Acceso de `admin1` a `administracion` y subida correcta de un archivo.

**C4.5** Intento fallido de escritura de `admin1` en `ventas`.

---

## Parte 2. Montaje del recurso Samba en el cliente Linux

### Qué se pide

```bash
sudo mkdir -p /mnt/administracion
sudo mount -t cifs //192.168.1.10/administracion /mnt/administracion -o user=admin1
ls -l /mnt/administracion
echo "Creado desde montaje CIFS" | sudo tee /mnt/administracion/desde_montaje.txt
ls -l /samba/administracion    # verificar en el servidor
sudo umount /mnt/administracion
```

### Capturas obligatorias

**C4.6** Montaje con `mount` y listado del contenido.

**C4.7** Archivo creado desde el cliente visible en el servidor.

**C4.8** Desmontaje correcto con `umount`.

---

## Parte 3. Montaje automático con `/etc/fstab`

### Qué se pide

```bash
sudo nano /root/.credenciales_samba
```

Contenido:

```
username=admin1
password=contraseña_de_admin1
```

```bash
sudo chmod 600 /root/.credenciales_samba
```

Añade en `/etc/fstab` del cliente:

```
//192.168.1.10/administracion /mnt/administracion cifs credentials=/root/.credenciales_samba,iocharset=utf8 0 0
```

```bash
sudo mount -a
ls -l /mnt/administracion
```

### Capturas obligatorias

**C4.9** Archivo de credenciales y sus permisos con `ls -l`.

**C4.10** Línea añadida en `/etc/fstab`.

**C4.11** Resultado de `mount -a` y contenido del recurso montado.

---

## Parte 4. Acceso desde las máquinas Windows de la Tarea 8

### Qué se pide

Desde `cliente1` o `cliente2` de la Tarea 8 (en la red interna `intranet`):

1. Abre el Explorador de Windows.
2. En la barra de direcciones escribe `\\192.168.1.10` o `\\debian-servidor`.
3. Deben aparecer los tres recursos: `publica`, `administracion` y `ventas`.
4. Accede a `publica` sin contraseña y comprueba que puedes leer pero no crear archivos.
5. Accede a `administracion` con `admin1`. Si la sesión activa de Windows usa ese usuario y la contraseña coincide, el acceso será automático y transparente.
6. Crea un archivo en `administracion` y verifica que aparece en el servidor Linux.
7. Intenta crear un archivo en `ventas` con `admin1`. Debe fallar.
8. Limpia las credenciales con `net use * /delete` y accede a `ventas` con `ventas1`. Debe poder crear archivos.

### Por qué el acceso desde Windows es transparente

Cuando Windows accede a Samba con un usuario cuyo nombre y contraseña coinciden en Linux, la autenticación es silenciosa. Windows envía automáticamente las credenciales de la sesión activa y Samba las acepta. Por eso era tan importante el Ejercicio 0.

### Capturas obligatorias

**C4.12** Recursos Samba visibles desde Windows al acceder a `\\192.168.1.10`.

**C4.13** Archivo creado en `administracion` desde Windows, visible en el servidor Linux.

**C4.14** Intento fallido de escritura en `ventas` con usuario de `administracion`.

**C4.15** Acceso correcto de `ventas1` a `ventas` y creación de un archivo.

> Si no tienes las máquinas Windows disponibles, indica en el PDF la razón.

---

## Errores típicos del ejercicio 4

**Error: Windows pide contraseña aunque debería coincidir**
La contraseña de `smbpasswd` no coincide con la de Windows. Restablécela con `sudo smbpasswd admin1`.

**Error: `mount -t cifs` da «unknown filesystem type 'cifs'»**
Falta `cifs-utils`. Instálalo con `sudo apt install cifs-utils`.

**Error: `admin1` puede escribir en `ventas`**
Verifica que `write list = @ventas` está en el bloque `[ventas]` y que has reiniciado Samba.

---

# Ejercicio 5 — NFS: instalación y montaje

## Descripción general

Instalarás NFS para compartir carpetas directamente entre los dos equipos Linux, sin autenticación por usuario. NFS es más sencillo que Samba para redes de solo Linux pero no es compatible con Windows.

---

## Teoría necesaria

NFS controla el acceso por dirección IP, no por usuario y contraseña. En `/etc/exports` defines qué directorio se comparte, a qué equipos y con qué permisos.

---

## Parte 1. Instalar el servidor NFS

### Qué se pide

En `debian-servidor`:

```bash
sudo apt install nfs-kernel-server
service nfs-kernel-server status
```

### Capturas obligatorias

**C5.1** Instalación de `nfs-kernel-server`.

**C5.2** Estado del servicio NFS activo.

---

## Parte 2. Crear carpetas y configurar `/etc/exports`

### Qué se pide

```bash
sudo mkdir -p /nfs/lectura
sudo mkdir -p /nfs/escritura
echo "Archivo NFS de solo lectura" | sudo tee /nfs/lectura/saludo.txt
sudo chown -R nobody:nogroup /nfs
sudo chmod -R 770 /nfs
```

Edita `/etc/exports`:

```bash
sudo nano /etc/exports
```

Añade al final:

```
/nfs/lectura  192.168.1.0/24(ro,sync,no_subtree_check)
/nfs/escritura 192.168.1.11(rw,sync,no_subtree_check)
```

```bash
sudo exportfs -a
sudo service nfs-kernel-server restart
```

### Capturas obligatorias

**C5.3** Creación de carpetas y archivo de prueba.

**C5.4** Contenido de `/etc/exports`.

**C5.5** `exportfs -a` y reinicio del servicio.

---

## Parte 3. Instalar el cliente NFS y montar los recursos

### Qué se pide

En el cliente Debian:

```bash
sudo apt install nfs-common
sudo mkdir -p /mnt/nfs/lectura
sudo mkdir -p /mnt/nfs/escritura
sudo mount -t nfs 192.168.1.10:/nfs/lectura /mnt/nfs/lectura
cat /mnt/nfs/lectura/saludo.txt
echo "intento" | sudo tee /mnt/nfs/lectura/test.txt    # debe fallar
sudo mount -t nfs 192.168.1.10:/nfs/escritura /mnt/nfs/escritura
echo "Desde el cliente" | sudo tee /mnt/nfs/escritura/desde_cliente.txt
ls -l /nfs/escritura    # verificar en el servidor
```

### Capturas obligatorias

**C5.6** Instalación de `nfs-common` y puntos de montaje.

**C5.7** Lectura correcta de `saludo.txt`.

**C5.8** Error al intentar escribir en el recurso de solo lectura.

**C5.9** Escritura correcta en el recurso de escritura.

**C5.10** Archivo visible en el servidor.

---

## Parte 4. Montaje automático con `/etc/fstab`

### Qué se pide

En el cliente Debian, añade en `/etc/fstab`:

```
192.168.1.10:/nfs/lectura   /mnt/nfs/lectura   nfs ro,intr,x-gvfs-show 0 0
192.168.1.10:/nfs/escritura /mnt/nfs/escritura nfs rw,intr,x-gvfs-show 0 0
```

```bash
sudo umount /mnt/nfs/lectura
sudo umount /mnt/nfs/escritura
sudo mount -a
ls -l /mnt/nfs/lectura
ls -l /mnt/nfs/escritura
```

### Qué debes explicar en el PDF

* Diferencia práctica entre Samba y NFS.
* En qué situaciones usarías Samba y en cuáles NFS.

### Capturas obligatorias

**C5.11** Líneas NFS en `/etc/fstab`.

**C5.12** `mount -a` y los dos recursos NFS montados.

---

## Errores típicos del ejercicio 5

**Error: «Connection refused» al montar NFS**
El servicio NFS no está activo. Ejecuta `service nfs-kernel-server status`.

**Error: el cliente monta pero no puede escribir**
Prueba `sudo chmod 777 /nfs/escritura` en el servidor para descartar problema de permisos Linux.

---

# Ejercicio 6 — SSH y transferencia de archivos con SCP

## Descripción general

Instalarás el servidor SSH en `debian-servidor` y lo administrarás de forma remota. Usarás `scp` para copiar archivos entre los dos equipos, de forma equivalente al cliente FTP de la Tarea 8 pero de forma segura y sin servidor FTP separado.

---

## Teoría necesaria

SSH cifra toda la comunicación. `scp` usa el canal SSH como medio de transporte: si SSH funciona, `scp` funciona sin configuración adicional.

La primera vez que te conectas a un servidor SSH, el sistema muestra su huella digital (fingerprint) y pregunta si confías en él. Debes responder `yes`.

---

## Parte 1. Instalar el servidor SSH

### Qué se pide

En `debian-servidor`:

```bash
sudo apt install ssh
service ssh status
ss -tlnp | grep :22
```

### Capturas obligatorias

**C6.1** Instalación del paquete SSH.

**C6.2** Estado del servicio SSH activo.

**C6.3** Puerto 22 en escucha con `ss -tlnp`.

---

## Parte 2. Conectarse al servidor desde el cliente

### Qué se pide

En el cliente Debian:

```bash
ssh admin1@192.168.1.10
```

Escribe `yes` ante el mensaje del fingerprint. Una vez conectado:

```bash
hostname
whoami
ls /samba/administracion
ls /samba/ventas
exit
```

Conecta también con `ventas1`:

```bash
ssh ventas1@192.168.1.10
who
exit
```

El resultado de `who` mostrará las sesiones activas con la IP de origen, igual que el Visor de eventos de Windows registraba los inicios de sesión de red en la Tarea 8.

### Capturas obligatorias

**C6.4** Mensaje de fingerprint y confirmación con `yes`.

**C6.5** Sesión SSH con `hostname` y `whoami` mostrando valores correctos.

**C6.6** Comandos ejecutados remotamente en el servidor.

**C6.7** `who` mostrando la sesión SSH activa con la IP del cliente.

---

## Parte 3. Transferir archivos con `scp`

### Qué se pide

**Enviar un archivo del cliente al servidor:**

```bash
echo "Archivo enviado con scp" > prueba_scp.txt
scp prueba_scp.txt admin1@192.168.1.10:/home/admin1/
ssh admin1@192.168.1.10 "ls -l /home/admin1/"
```

**Recibir un archivo del servidor:**

```bash
ssh admin1@192.168.1.10 "echo 'Creado en el servidor' > /home/admin1/desde_servidor.txt"
scp admin1@192.168.1.10:/home/admin1/desde_servidor.txt .
cat desde_servidor.txt
```

**Copiar un directorio completo:**

```bash
scp -r admin1@192.168.1.10:/home/admin1 /tmp/copia_admin1
ls -l /tmp/copia_admin1
```

### Qué debes explicar en el PDF

* Por qué SSH es más seguro que Telnet.
* Qué ventaja tiene `scp` frente al servidor FTP que configuraste en la Tarea 8 con IIS.
* Qué significa el mensaje de fingerprint que aparece la primera vez.

### Capturas obligatorias

**C6.8** Envío de archivo al servidor y verificación.

**C6.9** Descarga de archivo desde el servidor.

**C6.10** Copia recursiva de directorio con `scp -r`.

---

## Errores típicos del ejercicio 6

**Error: «Connection refused»**
El servicio SSH no está activo. Ejecuta `service ssh status` en el servidor.

**Error: «Permission denied» al introducir la contraseña**
La contraseña es incorrecta. Cámbiala con `sudo passwd admin1` en el servidor.

---

# Ejercicio 7 — Apache: servidor web básico

## Descripción general

Instalarás Apache en `debian-servidor` y publicarás una página web accesible desde el cliente Debian y desde los equipos Windows de la Tarea 8. Es el equivalente Linux del servidor web IIS que configuraste en Windows.

---

## Teoría necesaria

Apache publica por defecto los archivos de `/var/www/html`. Cuando un navegador accede a `http://192.168.1.10`, Apache busca `index.html` en esa carpeta y lo envía. Escucha por el puerto 80, igual que IIS en Windows.

---

## Parte 1. Instalar Apache

### Qué se pide

En `debian-servidor`:

```bash
sudo apt update
sudo apt install apache2
service apache2 status
ss -tlnp | grep :80
```

### Capturas obligatorias

**C7.1** Instalación de Apache.

**C7.2** Estado del servicio `apache2` activo.

**C7.3** Puerto 80 en escucha.

---

## Parte 2. Probar la página por defecto

### Qué se pide

Desde el servidor:

```bash
curl http://localhost
```

Desde el cliente Debian, abre el navegador y accede a `http://192.168.1.10`. Si tienes un equipo Windows de la Tarea 8 activo en la red, puedes acceder también desde él con la misma URL.

### Capturas obligatorias

**C7.4** Resultado de `curl http://localhost` en el servidor.

**C7.5** Página de bienvenida de Apache cargada en el navegador del cliente.

---

## Parte 3. Crear la página principal personalizada

### Qué se pide

```bash
sudo cp /var/www/html/index.html /var/www/html/index.html.copia
sudo nano /var/www/html/index.html
```

Contenido (sustituye los datos por los tuyos):

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Servidor Linux - Sistemas Informáticos</title>
</head>
<body>
    <h1>Servidor web en Debian</h1>
    <p><strong>Alumno:</strong> Nombre y apellidos</p>
    <p><strong>Módulo:</strong> Sistemas Informáticos – 1º DAW</p>
    <h2>Servicios activos en este servidor</h2>
    <table border="1" cellpadding="5">
        <tr><th>Servicio</th><th>Puerto</th><th>Función</th></tr>
        <tr><td>SSH</td><td>22</td><td>Acceso remoto seguro</td></tr>
        <tr><td>Samba</td><td>139 / 445</td><td>Recursos compartidos (compatible Windows)</td></tr>
        <tr><td>NFS</td><td>2049</td><td>Recursos compartidos entre Linux</td></tr>
        <tr><td>Apache</td><td>80</td><td>Servidor web HTTP</td></tr>
    </table>
    <p><a href="servicios.html">Ver detalles de la red</a></p>
</body>
</html>
```

Accede desde el cliente y actualiza con `Ctrl + F5`.

### Capturas obligatorias

**C7.6** Copia de seguridad de `index.html`.

**C7.7** Edición del nuevo `index.html`.

**C7.8** Página personalizada cargada desde el cliente mostrando tu nombre.

---

## Parte 4. Crear una segunda página

### Qué se pide

```bash
sudo nano /var/www/html/servicios.html
```

Contenido:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Detalles de la red</title>
</head>
<body>
    <h1>Configuración de la red</h1>
    <h2>Equipos</h2>
    <ul>
        <li>debian-servidor (Linux): 192.168.1.10</li>
        <li>debian-cliente (Linux): 192.168.1.11</li>
        <li>cliente1 Windows (Tarea 8): 192.168.1.10</li>
        <li>cliente2 Windows (Tarea 8): 192.168.1.11</li>
    </ul>
    <h2>Grupos y permisos en Samba</h2>
    <ul>
        <li>Grupo administracion: escritura en /administracion, lectura en /ventas</li>
        <li>Grupo ventas: escritura en /ventas</li>
    </ul>
    <p><a href="index.html">Volver al inicio</a></p>
</body>
</html>
```

Accede desde el cliente a `http://192.168.1.10/servicios.html`.

### Capturas obligatorias

**C7.9** Edición de `servicios.html`.

**C7.10** Página `servicios.html` cargada desde el cliente.

---

## Parte 5. Acceso desde Windows

### Qué se pide

Si tienes las máquinas Windows de la Tarea 8 activas, abre el navegador en `cliente2` y accede a `http://192.168.1.10`.

### Capturas obligatorias

**C7.11** Página personalizada cargada desde un navegador Windows (si está disponible).

---

## Errores típicos del ejercicio 7

**Error: el cliente sigue viendo la página de bienvenida de Apache**
Pulsa `Ctrl + F5` para forzar la recarga sin caché.

**Error: error 403**
Apache no puede leer el archivo. Ejecuta `sudo chmod 755 /var/www/html/` y `sudo chown -R www-data:www-data /var/www/html/`.

---

# Ejercicio 8 — Diagnóstico de red con herramientas TCP/IP

## Descripción general

Usarás las herramientas de diagnóstico de Linux y las compararás con las equivalentes de Windows que usaste en la Tarea 8.

---

## Teoría necesaria

| Herramienta Linux | Equivalente Windows | Función                             |
|-------------------|---------------------|-------------------------------------|
| `ip a`            | `ipconfig`          | Ver interfaces y direcciones IP     |
| `ip route`        | `route print`       | Ver tabla de rutas                  |
| `ping`            | `ping`              | Comprobar conectividad              |
| `traceroute`      | `tracert`           | Ver saltos hasta el destino         |
| `ss -tlnp`        | `netstat -an`       | Ver puertos en escucha              |
| `nslookup`        | `nslookup`          | Consultar resolución DNS            |
| `journalctl`      | Visor de eventos    | Consultar registros del sistema     |

---

## Parte 1. Configuración de red completa

### Qué se pide

En `debian-servidor`:

```bash
ip a
ip route
```

Identifica las dos interfaces, las IPs y la tabla de rutas. Compara el resultado con el de `ipconfig /all` de la Tarea 8.

### Capturas obligatorias

**C8.1** Resultado de `ip a` en el servidor mostrando las dos interfaces.

**C8.2** Resultado de `ip route` en el servidor.

---

## Parte 2. Comprobación de conectividad por niveles

### Qué se pide

Desde el cliente Debian:

```bash
ping -c 4 127.0.0.1
ping -c 4 192.168.1.11
ping -c 4 192.168.1.10
ping -c 4 8.8.8.8
ping -c 4 google.com
ping -c 4 debian-servidor
```

Si tienes máquinas Windows activas en la red, añade: `ping -c 4 192.168.1.10` (cliente1 Windows).

### Capturas obligatorias

**C8.3** Resultado de los ping por niveles desde el cliente.

---

## Parte 3. `traceroute`

### Qué se pide

```bash
sudo apt install traceroute
traceroute 192.168.1.10
```

Compara con el `tracert` de la Tarea 8. En ambos casos verás un único salto al estar en la misma red local.

### Capturas obligatorias

**C8.4** Resultado de `traceroute` mostrando el único salto al servidor.

---

## Parte 4. Puertos en escucha con `ss`

### Qué se pide

En `debian-servidor`:

```bash
ss -tlnp
```

Identifica los puertos:

| Puerto | Servicio |
|--------|---------|
| 22     | SSH |
| 80     | Apache |
| 139    | Samba (NetBIOS) |
| 445    | Samba (SMB) |
| 2049   | NFS |

Compara con el `netstat -an` de la Tarea 8, donde veías los puertos 21 (FTP) y 80 (HTTP) de IIS.

### Capturas obligatorias

**C8.5** Resultado de `ss -tlnp` con los puertos de los servicios activos.

**C8.6** Detalle de al menos dos puertos identificados.

---

## Parte 5. Consulta DNS con `nslookup`

### Qué se pide

En el cliente Debian:

```bash
nslookup google.com
nslookup debian-servidor
```

### Capturas obligatorias

**C8.7** Resultado de ambas consultas `nslookup`.

---

## Parte 6. Registros del sistema con `journalctl`

### Qué se pide

En `debian-servidor`:

```bash
sudo journalctl -u ssh -n 10
sudo journalctl -u apache2 -n 10
sudo journalctl -u smbd -n 10
```

Localiza eventos relacionados con las conexiones realizadas durante la práctica. Es el equivalente Linux del Visor de eventos de Windows.

### Capturas obligatorias

**C8.8** Resultado de `journalctl -u ssh -n 10` con eventos de conexión.

**C8.9** Resultado de `journalctl -u apache2 -n 10` con peticiones web.

---

## Parte 7. Explicación comparativa final

### Qué debes incluir en el PDF

Escribe una breve comparativa entre las herramientas de Windows de la Tarea 8 y sus equivalentes en Linux. Para cada par indica qué información obtienes y cuándo fue útil:

* **`ipconfig` / `ip a`**: qué muestra cada uno y diferencias en el formato.
* **`ping`**: mismo comando en ambos sistemas; ¿alguna diferencia de comportamiento?
* **`tracert` / `traceroute`**: resultado con un único salto; qué significa en ambos casos.
* **`netstat -an` / `ss -tlnp`**: puertos en Windows (21, 80 de IIS) vs. puertos en Linux (22, 80, 139, 445, 2049).
* **Visor de eventos / `journalctl`**: qué tipo de información ofrece cada uno y cuál te parece más útil.

Esta parte no requiere capturas adicionales, solo texto en el PDF.

---

# Conclusión final de la práctica

Al terminar esta práctica habrás construido una infraestructura cliente-servidor Linux completamente funcional integrada con los equipos Windows de la Tarea 8 en la misma red interna.

Los bloques que has trabajado son:

* instalación de Debian en modo consola en VirtualBox,
* red local en el mismo rango IP `192.168.1.X` que Windows con dos adaptadores y enrutamiento,
* usuarios y grupos de Linux alineados con los de Windows para autenticación transparente en Samba,
* Samba con tres recursos y permisos equivalentes a los NTFS de la Tarea 8,
* NFS para compartición nativa entre sistemas Linux,
* SSH como alternativa segura al acceso remoto y al FTP de IIS,
* Apache como alternativa Linux al servidor web IIS,
* y diagnóstico de red con las herramientas Linux equivalentes a las de Windows.

La diferencia más importante entre Windows y Linux en administración de redes no es qué se puede hacer, sino cómo se hace: en Linux todo se configura mediante archivos de texto editables desde la terminal, lo que lo hace más predecible, automatizable y reproducible que las interfaces gráficas de Windows.

---
