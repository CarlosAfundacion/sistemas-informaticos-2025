# TAREA 9 — Administración de Redes GNU/Linux

**Módulo:** Sistemas Informáticos · 1º DAW  
**Sistema:** Debian 12 (Bookworm)

---

# Prerrequisito — Instalación de Debian en modo consola en VirtualBox

## Descripción general

Antes de comenzar la práctica necesitas tener instalada una máquina Debian en VirtualBox configurada en **modo consola**, es decir, sin entorno gráfico. Este tipo de instalación es la habitual en servidores reales: consume menos RAM, menos CPU y tiene menos software instalado, lo que reduce posibles fallos y la superficie de ataque.

Esta guía cubre la instalación completa desde cero. Si ya tienes una máquina Debian instalada de una práctica anterior, puedes usarla directamente y pasar al Ejercicio 0, pero comprueba primero que cumple los requisitos de la tabla siguiente.

---

## Recursos mínimos necesarios

| Recurso | Mínimo recomendado | Observaciones |
|---|---|---|
| RAM asignada a la VM | 1024 MB | Con 512 MB funciona, pero puede ir lento durante la instalación de paquetes. |
| Disco duro virtual | 8 GB dinámico | El sistema base ocupa unos 2-3 GB. El resto queda libre para los servicios de la práctica. |
| Procesadores virtuales | 1 | No es necesario más de uno para esta práctica. |
| Adaptadores de red | 2 | Se configuran después de la instalación: uno NAT y uno de red interna. |
| ISO de Debian | Versión estable actual (Bookworm 12) | Descárgala de https://www.debian.org → Getting Debian → CD/DVD Images → netinst 64 bits (≈ 400 MB). |

> **Imagen netinst vs imagen completa:** la netinst descarga los paquetes durante la instalación y necesita conexión a Internet. Si el aula no tiene conexión fiable, usa la imagen DVD completa (≈ 4 GB), que incluye todos los paquetes sin necesidad de descargar nada durante el proceso.

---

## Parte 1. Crear la máquina virtual en VirtualBox

1. Abre VirtualBox y haz clic en **Nueva**.

2. En la pantalla de nombre y sistema operativo:
   - **Nombre:** `debian-servidor`
   - **Carpeta:** deja la predeterminada o elige una ubicación con espacio suficiente.
   - **Imagen ISO:** haz clic en el desplegable y selecciona **Otra…** para localizar la ISO de Debian que has descargado.
   - VirtualBox debería detectar automáticamente el tipo. Comprueba que aparece **Tipo: Linux** y **Versión: Debian (64-bit)**.

3. Marca la casilla **Saltar instalación desatendida**. Es imprescindible marcarla: la instalación desatendida de VirtualBox selecciona opciones automáticamente que no nos convienen para esta práctica.

4. Haz clic en **Siguiente**.

5. En la pantalla de hardware: **Memoria base (RAM):** `1024 MB` · **Procesadores:** `1`.

6. Haz clic en **Siguiente**.

7. En la pantalla de disco duro virtual:
   - Selecciona **Crear un disco duro virtual ahora**.
   - **Tamaño:** `20 GB` · **Tipo:** VDI · **Almacenamiento:** Reservado dinámicamente.

8. Haz clic en **Siguiente** y después en **Terminar**.

> **CP.1** — Pantalla de resumen final de la nueva máquina en VirtualBox, donde se vean el nombre, la RAM y el tamaño de disco.

---

## Parte 2. Iniciar la instalación de Debian

1. Selecciona `debian-servidor` y haz clic en **Iniciar**.

2. Aparece el menú de arranque del instalador. Selecciona:

   ```
   Install
   ```

   > No selecciones «Graphical install». Usaremos la instalación en modo texto, que es la habitual en servidores.

3. **Selecciona el idioma:** `Español`.
4. **Selecciona tu ubicación:** `España`.
5. **Configura el teclado:** `Español`.

> **CP.2** — Menú de arranque del instalador con la opción «Install» seleccionada.

---

## Parte 3. Configuración de red durante la instalación

1. El instalador intentará configurar la red automáticamente por DHCP. Deja que lo haga.

2. **Nombre de la máquina:** escribe exactamente:

   ```
   debian-servidor
   ```

3. **Nombre de dominio:** déjalo en blanco y pulsa **Continuar**.

> **CP.3** — Pantalla de nombre de máquina mostrando `debian-servidor`.

---

## Parte 4. Configurar contraseñas y usuario

1. **Contraseña del superusuario (root):** elige una contraseña que recuerdes.
2. **Vuelve a introducir la contraseña de root** para confirmarla.
3. **Nombre completo del nuevo usuario:** escribe tu nombre real.
4. **Nombre de usuario para la cuenta:** escribe tu usuario en minúsculas, por ejemplo `alumno`.
5. **Contraseña del nuevo usuario:** elige una contraseña.

> **CP.4** — Pantalla de creación del usuario normal mostrando el nombre de usuario elegido.

---

## Parte 5. Particionado del disco

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

> **Por qué no se hace particionado manual:** para esta práctica no es necesario. En producción, separar `/var` o `/srv` en particiones independientes evita que un servicio que llene el disco afecte al sistema completo. En el contexto de esta práctica, una sola partición es suficiente.

> **CP.5** — Pantalla de selección del método de particionado con «Guiado - utilizar todo el disco» seleccionado.
>
> **CP.6** — Resumen del particionado antes de confirmar la escritura en disco.

---

## Parte 6. Instalación del sistema base

1. El instalador copia el sistema base. Este proceso tarda varios minutos.
2. **Gestor de paquetes — configurar una réplica:** selecciona `España` y elige `ftp.es.debian.org` o cualquier espejo disponible. Si el aula no tiene Internet, selecciona «No usar una réplica de red».
3. **Participar en el estudio de uso de paquetes:** selecciona `No`.

---

## Parte 7. Selección de software — la pantalla más importante

Esta es la pantalla más importante de toda la instalación para esta práctica.

**Desmarca TODAS las opciones** excepto:

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
[ ] LXDE / LXQt
[ ] Servidor web
[ ] Servidor SSH
[ ] Impresora estándar
```

> ⚠️ **No instales ningún entorno de escritorio.** Esta máquina es un servidor y arrancará directamente en modo consola. Los servicios web y SSH los instalaremos manualmente durante la práctica para aprender el proceso completo.

> **CP.7** — Pantalla de selección de software mostrando únicamente «Utilidades estándar del sistema» marcado y sin ningún entorno de escritorio seleccionado.

---

## Parte 8. Instalar el cargador de arranque GRUB

1. Selecciona **Sí** para instalar GRUB en el disco principal.
2. Selecciona el dispositivo: normalmente `/dev/sda`.

---

## Parte 9. Finalizar la instalación y primer arranque

1. El instalador muestra «La instalación ha finalizado». Pulsa **Continuar**. La ISO se desmonta automáticamente en VirtualBox.

2. La máquina reinicia y arranca desde el disco duro por primera vez. Aparece:
   ```
   debian-servidor login:
   ```

3. Inicia sesión con el usuario que creaste.

4. Verifica que el sistema responde:
   ```bash
   hostname
   whoami
   ```
   `hostname` debe devolver `debian-servidor` y `whoami` debe devolver tu nombre de usuario.

> **CP.8** — Pantalla de login en modo texto con el prompt `debian-servidor login:`.
>
> **CP.9** — Resultado de `hostname` y `whoami` tras iniciar sesión.

---

## Parte 10. Configurar sudo para el usuario normal

1. Entra como root:
   ```bash
   su -
   ```

2. Añade tu usuario al grupo sudo (sustituye `alumno` por tu nombre de usuario):
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

> **CP.10** — Resultado de `sudo whoami` devolviendo `root`, confirmando que sudo está correctamente configurado.

---

## Parte 11. Actualizar el sistema antes de empezar

```bash
sudo apt update
sudo apt upgrade -y
```

Este proceso puede tardar varios minutos la primera vez.

> **CP.11** — Resultado de `sudo apt update` mostrando que los repositorios se han actualizado correctamente.

---

## Errores típicos durante la instalación

**La máquina arranca desde la ISO tras la instalación**  
La ISO sigue montada. En VirtualBox: apaga la máquina → Configuración → Almacenamiento → selecciona la ISO → icono del disco → «Eliminar disco de la unidad virtual». Vuelve a iniciar.

**El instalador dice que no detecta la red**  
Si usas imagen netinst y no hay Internet en el aula, la instalación puede fallar al descargar paquetes. Usa la imagen DVD completa.

**sudo no funciona después de añadir el usuario al grupo**  
Los cambios de grupo solo surten efecto al abrir una nueva sesión. Sal completamente con `exit` y vuelve a entrar.

**El sistema arranca en modo de emergencia o con errores de disco**  
El particionado no se completó correctamente. Reinstala la máquina desde el principio.

---

# Introducción

En esta práctica vas a trabajar los bloques fundamentales de la administración de redes en sistemas GNU/Linux. Usarás la máquina `debian-servidor` que acabas de instalar como servidor, y la máquina Debian con entorno gráfico de la práctica anterior como cliente.

Construirás una infraestructura cliente-servidor con ambos equipos conectados en red interna, compartirás recursos con Samba y NFS, administrarás el servidor de forma remota con SSH y publicarás una página web con Apache.

El objetivo no es solo que sigas una serie de pasos, sino que comprendas: qué hace cada herramienta o comando, en qué situaciones se utiliza, qué información devuelve, por qué una configuración funciona o falla, y cómo comprobar que el resultado es correcto.

---

## Coherencia con las prácticas anteriores

Esta práctica forma parte de una secuencia. Para que todo funcione correctamente, es fundamental que los usuarios y grupos de Linux coincidan con los que creaste en la **Tarea 5 de Windows**.

En la Tarea 5 creaste dos grupos (`administracion` y `ventas`) y cuatro usuarios (dos en cada grupo), con nombres en formato inicial del nombre + apellido (ejemplo: `alopez`).

En esta práctica debes crear en Linux **exactamente los mismos usuarios** que tienes en Windows, con **exactamente las mismas contraseñas**. Así, cuando una máquina Windows acceda a un recurso Samba en el servidor Linux, la autenticación funcionará de forma automática.

### Tabla de usuarios que usarás en esta práctica

Completa esta tabla con tus propios datos antes de empezar:

| Usuario Linux | Grupo Linux | Equivalente en Windows | Grupo Windows |
|---|---|---|---|
| (tu admin1) | administracion | (tu admin1 en cliente1) | administracion |
| (tu admin2) | administracion | (tu admin2 en cliente1) | administracion |
| (tu ventas1) | ventas | (tu ventas1 en cliente1) | ventas |
| (tu ventas2) | ventas | (tu ventas2 en cliente1) | ventas |

> **Ejemplo:** si en Windows creaste `alopez` y `mgarcia` para administración y `jperez` y `lruiz` para ventas, esos son exactamente los nombres que crearás en Linux en el Ejercicio 0.

---

## Qué vas a aprender en esta práctica

- Configurar dos máquinas Debian en una red interna de VirtualBox.
- Entender la diferencia entre una red NAT y una red interna.
- Asignar direcciones IP estáticas en Linux.
- Hacer que Linux funcione como router para otros equipos de la red.
- Comprobar conectividad entre equipos con herramientas de diagnóstico.
- Compartir carpetas con Samba controlando el acceso por grupos.
- Acceder a recursos Samba desde un cliente Linux y desde Windows.
- Compartir carpetas entre sistemas Linux con NFS.
- Administrar el servidor de forma remota con SSH.
- Transferir archivos entre equipos con `scp`.
- Publicar una página web sencilla con Apache.
- Montar recursos de red de forma permanente con `/etc/fstab`.

---

## Entrega

La práctica deberá entregarse en un único documento PDF que incluya:

- Capturas de pantalla claras y completas.
- Los comandos utilizados cuando se soliciten.
- Explicaciones breves y técnicas cuando se indiquen.
- Debajo de cada captura una línea con el texto: **Qué demuestra esta captura**.

> ⚠️ No se aceptarán capturas recortadas que oculten información importante. En las capturas de terminal deben verse siempre el comando ejecutado y su resultado completo.

---

## Estimación de tiempo

Esta práctica está diseñada para completarse en **3 semanas de 6 sesiones de 50 minutos**:

| Semana | Sesiones | Contenido |
|---|---|---|
| 1 | 1-2 | Prerrequisito (instalación de Debian) + Ejercicio 0 (usuarios) |
| 1 | 3-4 | Ejercicio 1 (red) + Ejercicio 2 (enrutamiento) |
| 1 | 5-6 | Ejercicio 3 (Samba) |
| 2 | 1-2 | Ejercicio 4 (acceso a Samba desde Linux y Windows) |
| 2 | 3-4 | Ejercicio 5 (NFS) |
| 2 | 5-6 | Ejercicio 6 (SSH y SCP) |
| 3 | 1-2 | Ejercicio 7 (Apache) |
| 3 | 3-4 | Ejercicio 8 (diagnóstico de red) |
| 3 | 5-6 | Corrección de errores y preparación de la entrega |

---

# Conceptos previos necesarios

## 1. Qué es una dirección IP

Una dirección IP identifica a un equipo dentro de una red. En esta práctica usarás:

```
Servidor Debian (debian-servidor): 192.168.1.10
Cliente Debian  (práctica anterior): 192.168.1.11
```

Estos valores están en la misma red que usaste en la Tarea 8 de Windows (`192.168.1.X`) para que todas las máquinas puedan verse entre sí cuando estén conectadas a la misma red interna.

## 2. Qué es la máscara de subred

La máscara indica qué parte de la dirección IP identifica la red. En esta práctica usarás `255.255.255.0`, igual que en Windows.

## 3. Qué es una red NAT y qué es una red interna en VirtualBox

**NAT** permite a la máquina virtual salir a Internet. Es necesaria para instalar paquetes con `apt`.

**Red interna** permite que varias máquinas virtuales se comuniquen entre sí en una red aislada. Es la misma red `intranet` que usaste en la Tarea 8 de Windows.

Cada máquina Debian tendrá dos tarjetas:

- **Adaptador 1:** NAT — para salir a Internet e instalar paquetes.
- **Adaptador 2:** Red interna `intranet` — para comunicarse con el resto de máquinas.

## 4. Qué son los servicios en Linux

Un servicio se gestiona con el comando `service`:

```bash
service nombre start    # iniciar
service nombre stop     # parar
service nombre restart  # reiniciar
service nombre status   # consultar estado
```

En esta práctica trabajarás con `smbd`, `nmbd`, `nfs-kernel-server`, `ssh` y `apache2`.

## 5. Qué es Samba y para qué sirve

Samba permite que un equipo Linux comparta carpetas usando el protocolo SMB, el mismo que usa Windows. La configuración se guarda en `/etc/samba/smb.conf`. Para que un usuario pueda autenticarse, debe existir como usuario Linux **y** estar dado de alta en Samba con `smbpasswd`.

## 6. Qué es NFS y para qué sirve

NFS comparte directorios entre equipos Linux de forma nativa. Es más sencillo que Samba pero no es compatible con Windows. La configuración se guarda en `/etc/exports`.

## 7. Qué es SSH y para qué sirve

SSH permite conectarse de forma remota a otro equipo Linux y trabajar en su terminal. Usa el puerto 22 y cifra toda la comunicación. Incluye el comando `scp` para copiar archivos entre equipos de forma segura.

## 8. Qué es Apache y para qué sirve

Apache es el servidor web más utilizado en Linux. Publica por defecto los archivos de `/var/www/html`. Escucha por el puerto 80. Es el equivalente Linux de IIS, que usaste en la Tarea 8 de Windows.

---

# Ejercicio 0 — Preparar usuarios y grupos en el servidor Linux

## Descripción general

Antes de configurar ningún servicio, debes tener en `debian-servidor` los mismos usuarios y grupos que creaste en la Tarea 5 de Windows. Este ejercicio es el paso más importante de toda la práctica: sin él, la autenticación cruzada entre Linux y Windows no funcionará.

## Teoría necesaria

Cuando un equipo Windows accede a un recurso Samba en Linux, envía automáticamente el nombre de usuario y la contraseña de la sesión activa. Si el usuario existe en Linux con la misma contraseña, el acceso es transparente. Si no coincide, Windows pedirá credenciales o denegará el acceso.

---

## Parte 1. Crear los grupos

```bash
sudo groupadd administracion
sudo groupadd ventas
```

Verifica:

```bash
cat /etc/group | grep administracion
cat /etc/group | grep ventas
```

> **C0.1** — Resultado de la creación o verificación de los grupos `administracion` y `ventas`.

---

## Parte 2. Crear los usuarios

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

> **C0.2** — Creación de los cuatro usuarios con `useradd`.
>
> **C0.3** — Asignación de contraseñas con `passwd`.
>
> **C0.4** — Verificación de los usuarios en `/etc/passwd`.

---

## Parte 3. Verificar la pertenencia a grupos

```bash
id admin1
id admin2
id ventas1
id ventas2
```

> **C0.5** — Resultado de `id` para los cuatro usuarios mostrando el grupo correcto.

---

## Errores típicos del ejercicio 0

**`useradd` dice que el usuario ya existe**  
Comprueba con `id nombre_usuario`. Si existe con grupo incorrecto, usa `sudo usermod -g nombre_grupo nombre_usuario`.

**El nombre del usuario no coincide con el de Windows**  
Linux distingue mayúsculas y minúsculas. `Admin1` y `admin1` son usuarios distintos.

---

# Ejercicio 1 — Configuración de la red en VirtualBox

## Descripción general

En este ejercicio configurarás las tarjetas de red de `debian-servidor` y del cliente Debian de la práctica anterior, y asignarás las direcciones IP en el rango `192.168.1.X`, el mismo que usaste en la Tarea 8 de Windows.

## Teoría necesaria

En Debian 12, las interfaces de red tienen nombres como `enp0s3` o `enp0s8`. Comprueba siempre el nombre real con `ip a` antes de editar ningún archivo de configuración.

> ℹ️ Debian 12 usa el sistema de configuración de red `ifupdown`. El archivo `/etc/network/interfaces` sigue siendo el método estándar para configurar IPs estáticas en instalaciones sin entorno gráfico.

---

## Parte 1. Configurar los adaptadores en VirtualBox

En **ambas máquinas**, con cada una apagada, configura en VirtualBox:

- **Adaptador 1** → NAT
- **Adaptador 2** → Red interna, nombre: `intranet`

> ⚠️ El nombre `intranet` debe ser exactamente el mismo que usaste en la Tarea 8 de Windows. Si no coincide, las máquinas estarán en redes aisladas distintas y no podrán verse.

> **C1.1** — Adaptador 2 de `debian-servidor` configurado como red interna `intranet`.
>
> **C1.2** — Adaptador 2 del cliente Debian configurado como red interna `intranet`.

---

## Parte 2. Identificar las interfaces de red

Enciende ambas máquinas y ejecuta en cada una:

```bash
ip a
```

Identifica la interfaz NAT (IP tipo `10.0.2.X`) y la interfaz de red interna (sin IP todavía). Anota el nombre exacto de la segunda antes de continuar.

> **C1.3** — Resultado de `ip a` en `debian-servidor`.
>
> **C1.4** — Resultado de `ip a` en el cliente Debian.

---

## Parte 3. Asignar IP estática en el servidor

```bash
sudo nano /etc/network/interfaces
```

Añade al final (sustituye `enp0s8` por el nombre real de tu interfaz si es diferente):

```
auto enp0s8
iface enp0s8 inet static
    address 192.168.1.10
    netmask 255.255.255.0
```

Guarda con `Ctrl + O`, `Enter`, `Ctrl + X`. Después aplica los cambios:

```bash
sudo ifdown enp0s8 2>/dev/null; sudo ifup enp0s8
ip a
```

> ℹ️ Se recomienda usar `ifdown`/`ifup` sobre la interfaz concreta en lugar de reiniciar el servicio `networking` completo, ya que el reinicio completo puede interrumpir la interfaz NAT temporalmente. Si `ifdown` da un error porque la interfaz no estaba activa, ignóralo y ejecuta solo `ifup`.

Debes ver `192.168.1.10` asignada a la interfaz de red interna.

> **C1.5** — Contenido del archivo `/etc/network/interfaces` del servidor.
>
> **C1.6** — Resultado de `ip a` en el servidor mostrando `192.168.1.10`.

---

## Parte 4. Asignar IP estática en el cliente

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
sudo ifdown enp0s8 2>/dev/null; sudo ifup enp0s8
ip a
```

> **C1.7** — Archivo `/etc/network/interfaces` del cliente.
>
> **C1.8** — Resultado de `ip a` en el cliente mostrando `192.168.1.11`.

---

## Parte 5. Comprobar conectividad

Desde el cliente Debian:

```bash
ping -c 4 192.168.1.10
```

Desde `debian-servidor`:

```bash
ping -c 4 192.168.1.11
```

**Qué debes explicar en el PDF:**
- Qué comprueba el comando `ping`.
- Por qué es importante que funcione en los dos sentidos antes de continuar.

> **C1.9** — Ping correcto del cliente Debian al servidor.
>
> **C1.10** — Ping correcto del servidor al cliente Debian.

---

## Errores típicos del ejercicio 1

**La IP no aparece tras aplicar la configuración**  
Comprueba que el nombre de la interfaz en `/etc/network/interfaces` coincide exactamente con el que muestra `ip a`. Una diferencia de un carácter impide que funcione.

**El ping entre Linux y Windows no funciona**  
Comprueba que el firewall de Windows tiene activada la regla «Permitir ping» que creaste en la Tarea 8.

---

# Ejercicio 2 — Enrutamiento y diagnóstico de red

## Descripción general

Configurarás `debian-servidor` para que actúe como router, permitiendo que el cliente Debian salga a Internet. Después aprenderás a diagnosticar problemas de red de forma ordenada y configurarás la resolución de nombres local.

## Teoría necesaria

Por defecto, Linux no reenvía paquetes entre interfaces aunque tenga dos tarjetas. Para que actúe como router hay que activar el **reenvío IP** y añadir una regla en `iptables` para que el tráfico de la red interna salga enmascarado a través de la interfaz NAT.

---

## Parte 1. Activar el enrutamiento en el servidor

```bash
cat /proc/sys/net/ipv4/ip_forward
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
sudo iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -d 0/0 -j MASQUERADE
cat /proc/sys/net/ipv4/ip_forward
```

El último comando debe mostrar `1`.

> **C2.1** — Valor de `ip_forward` antes (0) y después (1) de activarlo.
>
> **C2.2** — Ejecución del comando `iptables`.

---

## Parte 2. Comprobar la salida a Internet desde el cliente

```bash
ping -c 4 8.8.8.8
ping -c 4 google.com
```

Si el ping a `8.8.8.8` funciona pero el de `google.com` falla, el cliente no tiene DNS configurado. Edita `/etc/resolv.conf` y añade:

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

> **C2.3** — Ping correcto a `8.8.8.8` desde el cliente.
>
> **C2.4** — Ping correcto a `google.com` desde el cliente.

---

## Parte 3. Hacer el enrutamiento permanente

En Debian 12, el archivo `/etc/rc.local` no existe ni está habilitado por defecto. El método correcto para ejecutar comandos al arranque es crear un **servicio systemd** propio. Sigue estos pasos exactamente:

**Paso 1.** Crea el script de enrutamiento:

```bash
sudo nano /usr/local/sbin/enrutamiento.sh
```

Contenido exacto del script:

```bash
#!/bin/bash
echo 1 > /proc/sys/net/ipv4/ip_forward
iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -d 0/0 -j MASQUERADE
```

Dale permisos de ejecución:

```bash
sudo chmod +x /usr/local/sbin/enrutamiento.sh
```

**Paso 2.** Crea el servicio systemd que ejecutará ese script al arrancar:

```bash
sudo nano /etc/systemd/system/enrutamiento.service
```

Contenido exacto del archivo:

```ini
[Unit]
Description=Activar enrutamiento IP y NAT
After=network.target

[Service]
Type=oneshot
ExecStart=/usr/local/sbin/enrutamiento.sh
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```

**Paso 3.** Activa el servicio para que se ejecute en cada arranque:

```bash
sudo systemctl daemon-reload
sudo systemctl enable enrutamiento.service
sudo systemctl start enrutamiento.service
sudo systemctl status enrutamiento.service
```

**Paso 4.** Reinicia el servidor y comprueba desde el cliente:

```bash
ping -c 4 google.com
```

> ℹ️ **Por qué usamos systemd y no `rc.local`:** en Debian 12, `rc.local` no existe por defecto y requiere pasos adicionales para habilitarlo. Un servicio systemd es la forma estándar y robusta de ejecutar comandos al arranque en los sistemas Linux actuales. El servicio que hemos creado se comporta exactamente igual que lo haría `rc.local` pero de forma más fiable.

> **C2.5** — Contenido del script `/usr/local/sbin/enrutamiento.sh`.
>
> **C2.6** — Contenido del archivo `/etc/systemd/system/enrutamiento.service`.
>
> **C2.7** — Resultado de `systemctl status enrutamiento.service` mostrando activo.
>
> **C2.8** — Ping correcto a `google.com` desde el cliente después de reiniciar el servidor.

---

## Parte 4. Diagnóstico ordenado de red

Desde el cliente Debian, ejecuta estos cinco ping en orden:

```bash
ping -c 4 127.0.0.1          # nivel 1: el propio sistema
ping -c 4 192.168.1.11        # nivel 2: la propia IP
ping -c 4 192.168.1.10        # nivel 3: el servidor
ping -c 4 8.8.8.8             # nivel 4: Internet por IP
ping -c 4 google.com          # nivel 5: Internet por nombre
```

**Qué debes explicar en el PDF:**

1. ¿Qué significa que el nivel 3 falle pero el nivel 2 funcione?
2. ¿Qué diferencia hay entre hacer ping a `8.8.8.8` y a `google.com`?
3. Si el nivel 4 funciona pero el nivel 5 falla, ¿qué parte no está funcionando?
4. ¿Por qué es útil diagnosticar por niveles en lugar de ir directamente al servicio que falla?

> **C2.9** — Resultado de los cinco ping por niveles desde el cliente.
>
> **C2.10** — Respuestas escritas a las cuatro preguntas de diagnóstico.

---

## Parte 5. Resolución de nombres local con `/etc/hosts`

En el cliente Debian y en `debian-servidor`, edita `/etc/hosts` en **ambas máquinas** y añade al final:

```
192.168.1.10 debian-servidor
192.168.1.11 debian-cliente
```

Comprueba:

```bash
ping -c 4 debian-servidor    # desde el cliente
ping -c 4 debian-cliente     # desde el servidor
```

**Qué debes explicar en el PDF:**
- Qué diferencia hay entre `/etc/hosts` y un servidor DNS.
- Por qué en una red grande no sería suficiente con `/etc/hosts`.

> ℹ️ **Nota importante para el Ejercicio 8:** en ese ejercicio utilizarás `nslookup` para comprobar resolución de nombres. Ten en cuenta que `nslookup` consulta únicamente los servidores DNS externos, **no** el archivo `/etc/hosts`. Por eso, `nslookup debian-servidor` devolverá un error aunque `ping debian-servidor` funcione correctamente. Esto es normal y esperado, no un error de configuración.

> **C2.11** — Archivo `/etc/hosts` del cliente con las líneas añadidas.
>
> **C2.12** — Ping por nombre desde el cliente a `debian-servidor`.

---

## Errores típicos del ejercicio 2

**`ping google.com` falla con «Temporary failure in name resolution»**  
El cliente no tiene DNS. Añade `nameserver 8.8.8.8` en `/etc/resolv.conf`.

**El enrutamiento deja de funcionar tras reiniciar**  
Comprueba que el servicio está activo con `sudo systemctl status enrutamiento.service`. Si está inactivo, revisa que el script tiene permiso de ejecución con `ls -l /usr/local/sbin/enrutamiento.sh`.

**`systemctl enable` da error «Failed to enable unit»**  
Comprueba que el archivo `.service` está en `/etc/systemd/system/` y que no tiene errores de sintaxis. Ejecuta `sudo systemctl daemon-reload` antes de volver a intentarlo.

---

# Ejercicio 3 — Samba: recurso público y recursos privados

## Descripción general

Instalarás Samba en el servidor y crearás tres recursos compartidos que replican la estructura de permisos de la Tarea 8 de Windows: una carpeta pública, una carpeta de administración solo para ese grupo y una carpeta de ventas donde `ventas` puede escribir pero `administracion` solo puede leer.

## Teoría necesaria

Samba trabaja con dos capas de control de acceso:

- **Permisos del sistema de archivos Linux** (`chmod`, `chgrp`): controlan el acceso real al disco.
- **Configuración en `smb.conf`**: controla el acceso a través de la red.

Si alguna de las dos capas deniega el acceso, el resultado final es denegado. Es el mismo principio que en Windows con los permisos de compartición y los permisos NTFS.

---

## Parte 1. Instalar Samba

```bash
sudo apt update
sudo apt install samba samba-common-bin
service smbd status
service nmbd status
```

> **C3.1** — Instalación de Samba completada.
>
> **C3.2** — Estado del servicio `smbd` mostrando que está activo.

---

## Parte 2. Crear las carpetas y configurar permisos

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

> **C3.3** — Creación de las tres carpetas y sus archivos de prueba.
>
> **C3.4** — Permisos de las tres carpetas con `ls -ld`.

---

## Parte 3. Configurar el archivo `smb.conf`

```bash
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.original
sudo nano /etc/samba/smb.conf
```

Localiza la línea de `workgroup` y escribe el mismo nombre de grupo de trabajo que usaste en Windows:

```
workgroup = EMPRESA
```

Añade al final del archivo:

```ini
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

| Opción | Significado |
|---|---|
| `workgroup = EMPRESA` | El servidor Samba aparece en el mismo grupo de trabajo que los equipos Windows de la Tarea 8. |
| `guest ok = yes` | Permite el acceso sin contraseña (solo en `[publica]`). |
| `valid users = @administracion` | El `@` indica que es un grupo. Solo los usuarios de ese grupo pueden entrar. |
| `write list = @ventas` | Solo el grupo `ventas` puede escribir. Los de `administracion` entran pero no pueden escribir. |

Valida la configuración y reinicia los servicios:

```bash
testparm
sudo service smbd restart
sudo service nmbd restart
```

> **C3.5** — Copia de seguridad del archivo original.
>
> **C3.6** — Los tres bloques de recursos añadidos al final de `smb.conf`.
>
> **C3.7** — Resultado de `testparm` sin errores.
>
> **C3.8** — Reinicio de `smbd` y `nmbd`.

---

## Parte 4. Dar de alta los usuarios en Samba

Para que un usuario pueda autenticarse en Samba, además de existir como usuario Linux, debe estar registrado en la base de datos de contraseñas de Samba. Son dos sistemas de contraseñas independientes.

```bash
sudo smbpasswd -a admin1
sudo smbpasswd -a admin2
sudo smbpasswd -a ventas1
sudo smbpasswd -a ventas2
```

Introduce la misma contraseña que en Windows para que la autenticación sea transparente.

> **C3.9** — Alta de los cuatro usuarios en Samba con `smbpasswd`.

---

## Errores típicos del ejercicio 3

**`testparm` muestra «Unknown parameter»**  
Hay un error de escritura. `testparm` indica el número de línea exacto.

**Un usuario de `administracion` puede escribir en `ventas`**  
Verifica que `write list = @ventas` está en el bloque `[ventas]` y que has reiniciado Samba.

---

# Ejercicio 4 — Acceso a Samba desde cliente Linux y desde Windows

## Descripción general

Accederás a los recursos Samba desde el cliente Debian usando la terminal, y desde las máquinas Windows de la Tarea 8. Comprobarás que los permisos funcionan de forma equivalente a la configuración NTFS de Windows.

---

## Parte 1. Acceso desde el cliente Linux por terminal

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
smb: \> put leeme.txt prueba_escritura.txt
smb: \> quit
```

Accede al recurso `administracion` con `admin1`:

```bash
smbclient //192.168.1.10/administracion -U admin1
smb: \> ls
smb: \> put leeme.txt desde_linux.txt
smb: \> quit
```

Accede al recurso `ventas` con `admin1` e intenta escribir (debe fallar):

```bash
smbclient //192.168.1.10/ventas -U admin1
smb: \> put leeme.txt intento_escritura.txt
smb: \> quit
```

> **C4.1** — Listado de recursos con `smbclient -L`.
>
> **C4.2** — Acceso al recurso público y descarga de `leeme.txt`.
>
> **C4.3** — Intento fallido de escritura en el recurso público.
>
> **C4.4** — Acceso de `admin1` a `administracion` y subida correcta de un archivo.
>
> **C4.5** — Intento fallido de escritura de `admin1` en `ventas`.

---

## Parte 2. Montaje del recurso Samba en el cliente Linux

```bash
sudo mkdir -p /mnt/administracion
sudo mount -t cifs //192.168.1.10/administracion /mnt/administracion -o user=admin1
ls -l /mnt/administracion
echo "Creado desde montaje CIFS" | sudo tee /mnt/administracion/desde_montaje.txt
ls -l /samba/administracion    # verificar en el servidor
sudo umount /mnt/administracion
```

> **C4.6** — Montaje con `mount` y listado del contenido.
>
> **C4.7** — Archivo creado desde el cliente visible en el servidor.
>
> **C4.8** — Desmontaje correcto con `umount`.

---

## Parte 3. Montaje automático con `/etc/fstab`

Crea un archivo con las credenciales para no tener que escribirlas en `/etc/fstab` en texto plano visible:

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

> **C4.9** — Archivo de credenciales y sus permisos con `ls -l`.
>
> **C4.10** — Línea añadida en `/etc/fstab`.
>
> **C4.11** — Resultado de `mount -a` y contenido del recurso montado.

---

## Parte 4. Acceso desde las máquinas Windows de la Tarea 8

Desde `cliente1` o `cliente2` de la Tarea 8 (en la red interna `intranet`):

1. Abre el Explorador de Windows.
2. En la barra de direcciones escribe `\\192.168.1.10` o `\\debian-servidor`.
3. Deben aparecer los tres recursos: `publica`, `administracion` y `ventas`.
4. Accede a `publica` sin contraseña y comprueba que puedes leer pero no crear archivos.
5. Accede a `administracion` con `admin1`. Si la sesión activa de Windows usa ese usuario y la contraseña coincide, el acceso será automático y transparente.
6. Crea un archivo en `administracion` y verifica que aparece en el servidor Linux.
7. Intenta crear un archivo en `ventas` con `admin1`. Debe fallar.
8. Limpia las credenciales con `net use * /delete` y accede a `ventas` con `ventas1`. Debe poder crear archivos.

> ℹ️ **Por qué el acceso desde Windows es transparente:** cuando Windows accede a Samba con un usuario cuyo nombre y contraseña coinciden en Linux, la autenticación es silenciosa. Windows envía automáticamente las credenciales de la sesión activa y Samba las acepta. Por eso era tan importante el Ejercicio 0.

> ⚠️ Si no tienes las máquinas Windows disponibles, indica en el PDF la razón.

> **C4.12** — Recursos Samba visibles desde Windows al acceder a `\\192.168.1.10`.
>
> **C4.13** — Archivo creado en `administracion` desde Windows, visible en el servidor Linux.
>
> **C4.14** — Intento fallido de escritura en `ventas` con usuario de `administracion`.
>
> **C4.15** — Acceso correcto de `ventas1` a `ventas` y creación de un archivo.

---

## Errores típicos del ejercicio 4

**Windows pide contraseña aunque debería coincidir**  
La contraseña de `smbpasswd` no coincide con la de Windows. Restablécela con `sudo smbpasswd admin1`.

**`mount -t cifs` da «unknown filesystem type 'cifs'»**  
Falta `cifs-utils`. Instálalo con `sudo apt install cifs-utils`.

**`admin1` puede escribir en `ventas`**  
Verifica que `write list = @ventas` está en el bloque `[ventas]` y que has reiniciado Samba.

---

# Ejercicio 5 — NFS: instalación y montaje

## Descripción general

Instalarás NFS para compartir carpetas directamente entre los dos equipos Linux, sin autenticación por usuario. NFS es más sencillo que Samba para redes de solo Linux, pero no es compatible con Windows.

## Teoría necesaria

NFS controla el acceso por dirección IP, no por usuario y contraseña. En `/etc/exports` defines qué directorio se comparte, a qué equipos y con qué permisos.

---

## Parte 1. Instalar el servidor NFS

```bash
sudo apt install nfs-kernel-server
service nfs-kernel-server status
```

> **C5.1** — Instalación de `nfs-kernel-server`.
>
> **C5.2** — Estado del servicio NFS activo.

---

## Parte 2. Crear carpetas y configurar `/etc/exports`

```bash
sudo mkdir -p /nfs/lectura
sudo mkdir -p /nfs/escritura
echo "Archivo NFS de solo lectura" | sudo tee /nfs/lectura/saludo.txt
sudo chown -R nobody:nogroup /nfs
sudo chmod -R 755 /nfs
```

> ℹ️ Se usa `755` en lugar de `770` para que el acceso NFS desde el cliente funcione correctamente. Con NFS el acceso se produce como `nobody` (usuario anónimo del sistema), y con `770` nobody no tendría permisos de lectura si no es miembro del grupo propietario. Con `755` cualquier usuario puede leer, y la escritura queda controlada por la configuración `ro`/`rw` del propio `/etc/exports`.

Edita `/etc/exports`:

```bash
sudo nano /etc/exports
```

Añade al final:

```
/nfs/lectura   192.168.1.0/24(ro,sync,no_subtree_check)
/nfs/escritura 192.168.1.11(rw,sync,no_subtree_check)
```

```bash
sudo exportfs -a
sudo service nfs-kernel-server restart
```

> **C5.3** — Creación de carpetas y archivo de prueba.
>
> **C5.4** — Contenido de `/etc/exports`.
>
> **C5.5** — `exportfs -a` y reinicio del servicio.

---

## Parte 3. Instalar el cliente NFS y montar los recursos

```bash
sudo apt install nfs-common
sudo mkdir -p /mnt/nfs/lectura
sudo mkdir -p /mnt/nfs/escritura
```

Monta el recurso de solo lectura y comprueba que puedes leer pero no escribir:

```bash
sudo mount -t nfs 192.168.1.10:/nfs/lectura /mnt/nfs/lectura
cat /mnt/nfs/lectura/saludo.txt
echo "intento" | sudo tee /mnt/nfs/lectura/test.txt    # debe fallar
```

Monta el recurso de escritura y comprueba que puedes escribir:

```bash
sudo mount -t nfs 192.168.1.10:/nfs/escritura /mnt/nfs/escritura
echo "Desde el cliente" | sudo tee /mnt/nfs/escritura/desde_cliente.txt
ls -l /nfs/escritura    # verificar en el servidor
```

> **C5.6** — Instalación de `nfs-common` y puntos de montaje.
>
> **C5.7** — Lectura correcta de `saludo.txt`.
>
> **C5.8** — Error al intentar escribir en el recurso de solo lectura.
>
> **C5.9** — Escritura correcta en el recurso de escritura.
>
> **C5.10** — Archivo visible en el servidor.

---

## Parte 4. Montaje automático con `/etc/fstab`

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

**Qué debes explicar en el PDF:**
- Diferencia práctica entre Samba y NFS.
- En qué situaciones usarías Samba y en cuáles NFS.

> **C5.11** — Líneas NFS en `/etc/fstab`.
>
> **C5.12** — `mount -a` y los dos recursos NFS montados.

---

## Errores típicos del ejercicio 5

**«Connection refused» al montar NFS**  
El servicio NFS no está activo. Ejecuta `service nfs-kernel-server status`.

**El cliente monta pero no puede escribir en `/escritura`**  
Comprueba que la IP del cliente (`192.168.1.11`) coincide con la que aparece en `/etc/exports` para ese directorio. NFS deniega el acceso si la IP no coincide exactamente.

---

# Ejercicio 6 — SSH y transferencia de archivos con SCP

## Descripción general

Instalarás el servidor SSH en `debian-servidor` y lo administrarás de forma remota. Usarás `scp` para copiar archivos entre los dos equipos, de forma equivalente al cliente FTP de la Tarea 8 pero de forma segura y sin servidor FTP separado.

## Teoría necesaria

SSH cifra toda la comunicación. `scp` usa el canal SSH como medio de transporte: si SSH funciona, `scp` funciona sin configuración adicional.

La primera vez que te conectas a un servidor SSH, el sistema muestra su huella digital (fingerprint) y pregunta si confías en él. Debes responder `yes`.

---

## Parte 1. Instalar el servidor SSH

```bash
sudo apt install ssh
service ssh status
ss -tlnp | grep :22
```

> **C6.1** — Instalación del paquete SSH.
>
> **C6.2** — Estado del servicio SSH activo.
>
> **C6.3** — Puerto 22 en escucha con `ss -tlnp`.

---

## Parte 2. Conectarse al servidor desde el cliente

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

> **C6.4** — Mensaje de fingerprint y confirmación con `yes`.
>
> **C6.5** — Sesión SSH con `hostname` y `whoami` mostrando valores correctos.
>
> **C6.6** — Comandos ejecutados remotamente en el servidor.
>
> **C6.7** — `who` mostrando la sesión SSH activa con la IP del cliente.

---

## Parte 3. Transferir archivos con `scp`

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

**Qué debes explicar en el PDF:**
- Por qué SSH es más seguro que Telnet.
- Qué ventaja tiene `scp` frente al servidor FTP que configuraste en la Tarea 8 con IIS.
- Qué significa el mensaje de fingerprint que aparece la primera vez.

> **C6.8** — Envío de archivo al servidor y verificación.
>
> **C6.9** — Descarga de archivo desde el servidor.
>
> **C6.10** — Copia recursiva de directorio con `scp -r`.

---

## Errores típicos del ejercicio 6

**«Connection refused»**  
El servicio SSH no está activo. Ejecuta `service ssh status` en el servidor.

**«Permission denied» al introducir la contraseña**  
La contraseña es incorrecta. Cámbiala con `sudo passwd admin1` en el servidor.

---

# Ejercicio 7 — Apache: servidor web básico

## Descripción general

Instalarás Apache en `debian-servidor` y publicarás una página web accesible desde el cliente Debian y desde los equipos Windows de la Tarea 8. Es el equivalente Linux del servidor web IIS que configuraste en Windows.

## Teoría necesaria

Apache publica por defecto los archivos de `/var/www/html`. Cuando un navegador accede a `http://192.168.1.10`, Apache busca `index.html` en esa carpeta y lo envía. Escucha por el puerto 80, igual que IIS en Windows.

---

## Parte 1. Instalar Apache

```bash
sudo apt update
sudo apt install apache2
service apache2 status
ss -tlnp | grep :80
```

> **C7.1** — Instalación de Apache.
>
> **C7.2** — Estado del servicio `apache2` activo.
>
> **C7.3** — Puerto 80 en escucha.

---

## Parte 2. Probar la página por defecto

Desde el servidor:

```bash
curl http://localhost
```

Desde el cliente Debian, abre el navegador y accede a `http://192.168.1.10`.

> **C7.4** — Resultado de `curl http://localhost` en el servidor.
>
> **C7.5** — Página de bienvenida de Apache cargada en el navegador del cliente.

---

## Parte 3. Crear la página principal personalizada

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

> **C7.6** — Copia de seguridad de `index.html`.
>
> **C7.7** — Edición del nuevo `index.html`.
>
> **C7.8** — Página personalizada cargada desde el cliente mostrando tu nombre.

---

## Parte 4. Crear una segunda página

```bash
sudo nano /var/www/html/servicios.html
```

Contenido (sustituye las IPs de Windows por las reales de tu práctica):

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
        <li>cliente1 Windows (Tarea 8): (tu IP de cliente1)</li>
        <li>cliente2 Windows (Tarea 8): (tu IP de cliente2)</li>
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

> ℹ️ En `servicios.html` debes sustituir las IPs de los equipos Windows por las que realmente usaste en la Tarea 8. No copies las IPs de los equipos Linux en los campos de Windows.

Accede desde el cliente a `http://192.168.1.10/servicios.html`.

> **C7.9** — Edición de `servicios.html`.
>
> **C7.10** — Página `servicios.html` cargada desde el cliente.

---

## Parte 5. Acceso desde Windows

Si tienes las máquinas Windows de la Tarea 8 activas, abre el navegador en `cliente2` y accede a `http://192.168.1.10`.

> **C7.11** — Página personalizada cargada desde un navegador Windows (si está disponible).

---

## Errores típicos del ejercicio 7

**El cliente sigue viendo la página de bienvenida de Apache**  
Pulsa `Ctrl + F5` para forzar la recarga sin caché.

**Error 403**  
Apache no puede leer el archivo. Ejecuta `sudo chmod 755 /var/www/html/` y `sudo chown -R www-data:www-data /var/www/html/`.

---

# Ejercicio 8 — Diagnóstico de red con herramientas TCP/IP

## Descripción general

Usarás las herramientas de diagnóstico de Linux y las compararás con las equivalentes de Windows que usaste en la Tarea 8.

## Tabla comparativa Linux / Windows

| Herramienta Linux | Equivalente Windows | Función |
|---|---|---|
| `ip a` | `ipconfig` | Ver interfaces y direcciones IP |
| `ip route` | `route print` | Ver tabla de rutas |
| `ping` | `ping` | Comprobar conectividad |
| `traceroute` | `tracert` | Ver saltos hasta el destino |
| `ss -tlnp` | `netstat -an` | Ver puertos en escucha |
| `nslookup` | `nslookup` | Consultar resolución DNS |
| `journalctl` | Visor de eventos | Consultar registros del sistema |

---

## Parte 1. Configuración de red completa

En `debian-servidor`:

```bash
ip a
ip route
```

Identifica las dos interfaces, las IPs y la tabla de rutas. Compara el resultado con el de `ipconfig /all` de la Tarea 8.

> **C8.1** — Resultado de `ip a` en el servidor mostrando las dos interfaces.
>
> **C8.2** — Resultado de `ip route` en el servidor.

---

## Parte 2. Comprobación de conectividad por niveles

Desde el cliente Debian:

```bash
ping -c 4 127.0.0.1
ping -c 4 192.168.1.11
ping -c 4 192.168.1.10
ping -c 4 8.8.8.8
ping -c 4 google.com
ping -c 4 debian-servidor
```

> **C8.3** — Resultado de los ping por niveles desde el cliente.

---

## Parte 3. `traceroute`

```bash
sudo apt install traceroute
traceroute 192.168.1.10
```

Compara con el `tracert` de la Tarea 8. En ambos casos verás un único salto al estar en la misma red local.

> **C8.4** — Resultado de `traceroute` mostrando el único salto al servidor.

---

## Parte 4. Puertos en escucha con `ss`

En `debian-servidor`:

```bash
ss -tlnp
```

Identifica los puertos de los servicios activos:

| Puerto | Servicio |
|---|---|
| 22 | SSH |
| 80 | Apache |
| 139 | Samba (NetBIOS) |
| 445 | Samba (SMB) |
| 2049 | NFS |

Compara con el `netstat -an` de la Tarea 8, donde veías los puertos 21 (FTP) y 80 (HTTP) de IIS.

> **C8.5** — Resultado de `ss -tlnp` con los puertos de los servicios activos.
>
> **C8.6** — Detalle de al menos dos puertos identificados.

---

## Parte 5. Consulta DNS con `nslookup`

```bash
nslookup google.com
nslookup debian-servidor
```

> ℹ️ Es normal que `nslookup debian-servidor` devuelva un error. `nslookup` consulta los servidores DNS externos (como `8.8.8.8`) y el nombre `debian-servidor` solo existe en tu archivo `/etc/hosts` local. El `ping` por nombre funciona porque el sistema operativo sí consulta `/etc/hosts`, pero `nslookup` no lo hace. Esto no es un error de configuración: es una diferencia de comportamiento entre ambos mecanismos de resolución de nombres.

> **C8.7** — Resultado de ambas consultas `nslookup` (incluyendo el error esperado para `debian-servidor`).

---

## Parte 6. Registros del sistema con `journalctl`

En `debian-servidor`:

```bash
sudo journalctl -u ssh -n 10
sudo journalctl -u apache2 -n 10
sudo journalctl -u smbd -n 10
```

Localiza eventos relacionados con las conexiones realizadas durante la práctica. Es el equivalente Linux del Visor de eventos de Windows.

> **C8.8** — Resultado de `journalctl -u ssh -n 10` con eventos de conexión.
>
> **C8.9** — Resultado de `journalctl -u apache2 -n 10` con peticiones web.

---

## Parte 7. Explicación comparativa final

Escribe en el PDF una breve comparativa entre las herramientas de Windows de la Tarea 8 y sus equivalentes en Linux. Para cada par indica qué información obtienes y cuándo fue útil:

- **`ipconfig` / `ip a`:** qué muestra cada uno y diferencias en el formato.
- **`ping`:** mismo comando en ambos sistemas; ¿alguna diferencia de comportamiento?
- **`tracert` / `traceroute`:** resultado con un único salto; qué significa en ambos casos.
- **`netstat -an` / `ss -tlnp`:** puertos en Windows (21, 80 de IIS) vs. puertos en Linux (22, 80, 139, 445, 2049).
- **Visor de eventos / `journalctl`:** qué tipo de información ofrece cada uno y cuál te parece más útil.

Esta parte no requiere capturas adicionales, solo texto en el PDF.

---

# Conclusión final de la práctica

Al terminar esta práctica habrás construido una infraestructura cliente-servidor Linux completamente funcional integrada con los equipos Windows de la Tarea 8 en la misma red interna.

Los bloques que has trabajado son:

- Instalación de Debian 12 en modo consola en VirtualBox.
- Red local en el mismo rango IP `192.168.1.X` que Windows con dos adaptadores y enrutamiento persistente con systemd.
- Usuarios y grupos de Linux alineados con los de Windows para autenticación transparente en Samba.
- Samba con tres recursos y permisos equivalentes a los NTFS de la Tarea 8.
- NFS para compartición nativa entre sistemas Linux.
- SSH como alternativa segura al acceso remoto y al FTP de IIS.
- Apache como alternativa Linux al servidor web IIS.
- Diagnóstico de red con las herramientas Linux equivalentes a las de Windows.

La diferencia más importante entre Windows y Linux en administración de redes no es qué se puede hacer, sino cómo se hace: en Linux todo se configura mediante archivos de texto editables desde la terminal, lo que lo hace más predecible, automatizable y reproducible que las interfaces gráficas de Windows.
