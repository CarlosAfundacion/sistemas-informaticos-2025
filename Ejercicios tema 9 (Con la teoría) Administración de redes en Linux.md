# TAREA 9 — Administración de Redes GNU/Linux

**Módulo:** Sistemas Informáticos · 1º DAW  
**Sistema:** Debian 12/13 (válido para ambas versiones)

---

## Leyenda de máquinas

A lo largo de toda la práctica, cada bloque de comandos indica en qué máquina debes ejecutarlo:

| Icono | Máquina |
|---|---|
| 🖥️ `debian-servidor` | Máquina Debian sin entorno gráfico (la que instalas en esta práctica) |
| 💻 `debian-cliente` | Máquina Debian con entorno gráfico (de la práctica anterior) |
| 🪟 `Windows` | Cualquiera de los equipos Windows de la Tarea 8 |

> ⚠️ Cuando el texto no indique ningún icono en la cabecera del bloque de comandos, vuelve a leer el párrafo anterior. Nunca ejecutes un comando en una máquina sin estar seguro de en cuál estás.

---

# Prerrequisito — Instalación de Debian en modo consola en VirtualBox

> ⚠️ **No es necesario hacer las capturas de esta parte** si ya tienes la máquina instalada correctamente. Si la tienes, pasa directamente al Ejercicio 0. Tener la máquina ya instalada y operativa suma 0,5 puntos adicionales.

## Descripción general

Antes de comenzar la práctica necesitas tener instalada una máquina Debian en VirtualBox configurada en **modo consola**, es decir, sin entorno gráfico. Este tipo de instalación es la habitual en servidores reales: consume menos RAM, menos CPU y tiene menos software instalado, lo que reduce posibles fallos y la superficie de ataque.

---

## Recursos mínimos necesarios

| Recurso | Mínimo recomendado | Observaciones |
|---|---|---|
| RAM asignada a la VM | 1024 MB | Con 512 MB funciona, pero puede ir lento durante la instalación de paquetes. |
| Disco duro virtual | 8 GB dinámico | El sistema base ocupa unos 2–3 GB. El resto queda libre para los servicios de la práctica. |
| Procesadores virtuales | 1 | No es necesario más de uno para esta práctica. |
| Adaptadores de red | 2 | Se configuran después de la instalación: uno NAT y uno de red interna. |
| ISO de Debian | Versión estable actual | Descárgala de https://www.debian.org → Getting Debian → CD/DVD Images → netinst 64 bits (≈ 650 MB). |

> **Imagen netinst vs imagen completa:** la netinst descarga los paquetes durante la instalación y necesita conexión a Internet. Si el aula no tiene conexión fiable, usa la imagen DVD completa (≈ 3–4 GB), que incluye todos los paquetes sin necesidad de descargar nada durante el proceso.

---

## Parte 1. Crear la máquina virtual en VirtualBox

> 🖥️ Todo este prerrequisito se realiza en el **equipo anfitrión** (tu ordenador físico), dentro de VirtualBox.

1. Abre VirtualBox y haz clic en **Nueva**.

2. En la pantalla de nombre y sistema operativo:
   - **Nombre:** `debian-servidor`
   - **Carpeta:** deja la predeterminada o elige una ubicación con espacio suficiente.
   - **Imagen ISO:** haz clic en el desplegable y selecciona **Otra…** para localizar la ISO de Debian que has descargado.
   - VirtualBox debería detectar automáticamente el tipo. Comprueba que aparece **Tipo: Linux** y **Versión: Debian (64-bit)**.

3. ⚠️ **IMPORTANTE — Instalación desatendida:** en la versión actual de VirtualBox, cuando detecta la ISO de Debian, activa automáticamente una instalación desatendida que configura el sistema por su cuenta con opciones que no nos convienen (instala entorno gráfico, configura mal la red, etc.). Debes **desmarcar** la casilla **«Instalación desatendida»** o **«Skip Unattended Installation»** antes de continuar. Si no lo haces, tendrás que eliminar la máquina y empezar de nuevo.

4. Haz clic en **Siguiente**.

5. En la pantalla de hardware: **Memoria base (RAM):** `1024 MB` · **Procesadores:** `1`.

6. Haz clic en **Siguiente**.

7. En la pantalla de disco duro virtual:
   - Selecciona **Crear un disco duro virtual ahora**.
   - **Tamaño:** `8 GB` · **Tipo:** VDI · **Almacenamiento:** Reservado dinámicamente.

8. Haz clic en **Siguiente** y después en **Terminar**.

> **CP.1** — Pantalla de resumen final de la nueva máquina en VirtualBox, donde se vean el nombre, la RAM y el tamaño de disco.

---

## Parte 2. Iniciar la instalación de Debian

1. Selecciona `debian-servidor` y haz clic en **Iniciar**.

2. Aparece el menú de arranque del instalador de Debian. Con las teclas de cursor, selecciona:

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

1. El instalador intentará configurar la red automáticamente por DHCP. Deja que lo haga; puede tardar un momento.

2. **Nombre de la máquina:** escribe exactamente:

   ```
   debian-servidor
   ```

3. **Nombre de dominio:** déjalo en blanco y pulsa **Continuar**.

> **CP.3** — Pantalla de nombre de máquina mostrando `debian-servidor`.

---

## Parte 4. Configurar contraseñas y usuario

1. **Contraseña del superusuario (root):** elige una contraseña que recuerdes y no sea trivial.
2. **Vuelve a introducir la contraseña de root** para confirmarla.
3. **Nombre completo del nuevo usuario:** escribe tu nombre real.
4. **Nombre de usuario para la cuenta:** escribe tu usuario en minúsculas, sin espacios, por ejemplo `alumno`.
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

> **Por qué no se hace particionado manual:** para esta práctica no es necesario. En producción real, separar `/var` o `/srv` en particiones independientes evita que un servicio que llene el disco afecte al sistema completo. Una sola partición es suficiente aquí.

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

Esta es la pantalla más crítica de toda la instalación.

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

> ⚠️ **No instales ningún entorno de escritorio.** Esta máquina es un servidor y arrancará directamente en modo consola. Los servicios web y SSH los instalaremos manualmente durante la práctica para aprender el proceso completo paso a paso.

> **CP.7** — Pantalla de selección de software mostrando únicamente «Utilidades estándar del sistema» marcado.

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

3. Inicia sesión con el usuario que creaste (no con root).

4. 🖥️ **`debian-servidor`** — Verifica que el sistema responde:
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

> 🖥️ **`debian-servidor`** — Todo este apartado se realiza en el servidor.

En Debian la instalación mínima no incluye `sudo` por defecto. Hay que instalarlo y añadir el usuario al grupo correspondiente.

1. Entra como root:
   ```bash
   su -
   ```
   > El guion después de `su` es importante: carga completamente el entorno de root, incluidas las variables de entorno y el PATH. Sin él, algunos comandos pueden no encontrarse.

2. Instala sudo:
   ```bash
   apt-get install sudo
   ```

3. Añade tu usuario al grupo sudo (sustituye `alumno` por tu nombre de usuario real):
   ```bash
   usermod -aG sudo alumno
   ```

4. Sal de root:
   ```bash
   exit
   ```

5. Cierra tu sesión de usuario normal también con `exit` y vuelve a iniciar sesión. Los cambios de grupo no surten efecto hasta abrir una nueva sesión.

6. Comprueba que sudo funciona:
   ```bash
   sudo whoami
   ```
   Debe responder `root`.

> **CP.10** — Resultado de `sudo whoami` devolviendo `root`, confirmando que sudo está correctamente configurado.

---

## Parte 11. Actualizar el sistema antes de empezar

> 🖥️ **`debian-servidor`**

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
Si usas imagen netinst y no hay Internet en el aula, la instalación puede fallar al descargar paquetes. Usa la imagen DVD completa o continúa sin réplica de red.

**sudo no funciona después de añadir el usuario al grupo**  
Los cambios de grupo solo surten efecto al abrir una nueva sesión. Sal completamente con `exit` y vuelve a entrar.

**El sistema arranca en modo de emergencia o con errores de disco**  
El particionado no se completó correctamente. Reinstala la máquina desde el principio.

**VirtualBox instaló el sistema automáticamente sin pedir nada**  
La instalación desatendida estaba activada. La máquina resultante tendrá configuración incorrecta. Elimínala y empieza de nuevo desmarcando la casilla de instalación desatendida.

---

# Introducción

En esta práctica vas a trabajar los bloques fundamentales de la administración de redes en sistemas GNU/Linux. Usarás la máquina `debian-servidor` que acabas de instalar como servidor, y la máquina Debian con entorno gráfico de la práctica anterior como cliente.

Construirás una infraestructura cliente-servidor con ambos equipos conectados en red interna, compartirás recursos con Samba y NFS, administrarás el servidor de forma remota con SSH y publicarás una página web con Apache.

El objetivo no es solo que sigas una serie de pasos, sino que comprendas: qué hace cada herramienta o comando, en qué situaciones se utiliza, qué información devuelve, por qué una configuración funciona o falla, y cómo comprobar que el resultado es correcto.

---

## Esquema de red de la práctica

```
Internet
    │
    │ (NAT VirtualBox)
    │
┌───┴──────────────────┐
│   debian-servidor     │  10.0.2.X      ← enp0s3 (NAT, IP dinámica)
│   (sin escritorio)    │  192.168.1.10  ← enp0s8 (red interna, IP estática)
└───────────┬──────────┘
            │
            │ Red interna "intranet"
            │
   ┌─────────┴──────────┬──────────────────┐
   │                    │                  │
┌──┴──────────┐  ┌──────┴──────┐  ┌────────┴────────┐
│debian-cliente│  │  cliente1   │  │    cliente2      │
│192.168.1.11 │  │192.168.1.20 │  │  192.168.1.21   │
│(con escritorio)│  │  (Windows) │  │   (Windows)     │
└─────────────┘  └─────────────┘  └─────────────────┘
```

---

## Direcciones IP utilizadas en esta práctica

| Equipo | Sistema | IP |
|---|---|---|
| `debian-servidor` | Debian Linux (servidor, sin escritorio) | `192.168.1.10` |
| `debian-cliente` | Debian Linux (cliente gráfico, práctica anterior) | `192.168.1.11` |
| `cliente1` | Windows (Tarea 8) | `192.168.1.20` |
| `cliente2` | Windows (Tarea 8) | `192.168.1.21` |

> ⚠️ Las máquinas Windows **deben usar IPs a partir de `192.168.1.20`** para no entrar en conflicto con los equipos Debian. Si en la Tarea 8 usaste IPs distintas, actualiza la configuración de esas máquinas antes de continuar con esta práctica.

---

## Coherencia con las prácticas anteriores

En la Tarea 5 creaste dos grupos (`administracion` y `ventas`) y cuatro usuarios (dos en cada grupo), con nombres en formato inicial del nombre + apellido (ejemplo: `alopez`).

En esta práctica debes crear en Linux **exactamente los mismos usuarios** que tienes en Windows, con **exactamente las mismas contraseñas**. Así, cuando una máquina Windows acceda a un recurso Samba en el servidor Linux, la autenticación funcionará de forma automática.

### Tabla de usuarios — complétala antes de empezar

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
| 1 | 1–2 | Prerrequisito (instalación de Debian) + Ejercicio 0 (usuarios) |
| 1 | 3–4 | Ejercicio 1 (red) + Ejercicio 2 (enrutamiento) |
| 1 | 5–6 | Ejercicio 3 (Samba) |
| 2 | 1–2 | Ejercicio 4 (acceso a Samba desde Linux y Windows) |
| 2 | 3–4 | Ejercicio 5 (NFS) |
| 2 | 5–6 | Ejercicio 6 (SSH y SCP) |
| 3 | 1–2 | Ejercicio 7 (Apache) |
| 3 | 3–4 | Ejercicio 8 (diagnóstico de red) |
| 3 | 5–6 | Corrección de errores y preparación de la entrega |

---

# Conceptos previos necesarios

## 1. Qué es una dirección IP

Una dirección IP identifica a un equipo dentro de una red. En esta práctica usarás el rango `192.168.1.X`, que es el mismo que el de la Tarea 8 de Windows, para que todos los equipos puedan verse entre sí en la misma red interna.

## 2. Qué es la máscara de subred

La máscara indica qué parte de la dirección IP identifica la red y qué parte identifica al equipo concreto dentro de ella. En esta práctica usarás `255.255.255.0`, igual que en Windows.

## 3. Qué es una red NAT y qué es una red interna en VirtualBox

**NAT** permite a la máquina virtual salir a Internet a través del sistema anfitrión. Es necesaria para instalar paquetes con `apt`. La VM tiene acceso a la red exterior, pero desde el exterior no se puede acceder directamente a la VM.

**Red interna** permite que varias máquinas virtuales se comuniquen entre sí en una red aislada y privada. Es la misma red `intranet` que usaste en la Tarea 8 de Windows.

Cada máquina Debian tendrá dos tarjetas de red:

- **Adaptador 1:** NAT — para salir a Internet e instalar paquetes.
- **Adaptador 2:** Red interna `intranet` — para comunicarse con el resto de máquinas.

## 4. Cómo se gestionan los servicios en Linux con systemctl

En Debian actual, los servicios se gestionan con `systemctl`. Este es el método correcto y estándar:

```bash
sudo systemctl start nombre_servicio      # iniciar
sudo systemctl stop nombre_servicio       # parar
sudo systemctl restart nombre_servicio    # reiniciar
sudo systemctl status nombre_servicio     # consultar estado
sudo systemctl enable nombre_servicio     # activar al arranque automático
sudo systemctl disable nombre_servicio    # desactivar el arranque automático
```

> ⚠️ El comando antiguo `service nombre start` puede funcionar en algunos sistemas como alias de `systemctl`, pero **en esta práctica usaremos siempre `systemctl`**, que es el comando estándar en todos los sistemas Linux modernos.

En esta práctica trabajarás con los servicios `smbd`, `nmbd`, `nfs-kernel-server`, `ssh` y `apache2`.

## 5. Qué es Samba y para qué sirve

Samba permite que un equipo Linux comparta carpetas usando el protocolo SMB, el mismo que usa Windows para sus recursos compartidos. La configuración se guarda en `/etc/samba/smb.conf`. Para que un usuario pueda autenticarse, debe existir como usuario Linux **y** estar dado de alta en Samba con `smbpasswd`.

## 6. Qué es NFS y para qué sirve

NFS (Network File System) comparte directorios entre equipos Linux de forma nativa y eficiente. Es más sencillo que Samba pero no es compatible con Windows. La configuración se guarda en `/etc/exports`.

## 7. Qué es SSH y para qué sirve

SSH permite conectarse de forma remota y segura a otro equipo Linux y trabajar en su terminal como si estuvieras físicamente delante de él. Usa el puerto 22 y cifra toda la comunicación. Incluye el comando `scp` para copiar archivos entre equipos de forma segura.

## 8. Qué es Apache y para qué sirve

Apache es el servidor web más utilizado en Linux. Publica por defecto los archivos de `/var/www/html`. Escucha por el puerto 80. Es el equivalente Linux de IIS, que usaste en la Tarea 8 de Windows.

---

# Ejercicio 0 — Preparar usuarios y grupos en el servidor Linux

> 🖥️ **Todo este ejercicio se realiza en `debian-servidor`.**

## Descripción general

Antes de configurar ningún servicio, debes tener en `debian-servidor` los mismos usuarios y grupos que creaste en la Tarea 5 de Windows. Este ejercicio es el más importante de toda la práctica: sin él, la autenticación cruzada entre Linux y Windows no funcionará.

## Teoría necesaria

Cuando un equipo Windows accede a un recurso Samba en Linux, envía automáticamente el nombre de usuario y la contraseña de la sesión activa. Si el usuario existe en Linux con la misma contraseña, el acceso es transparente. Si no coincide, Windows pedirá credenciales o denegará el acceso.

---

## Parte 1. Crear los grupos

🖥️ **`debian-servidor`**

```bash
sudo groupadd administracion
sudo groupadd ventas
```

Verifica que se han creado correctamente:

```bash
grep administracion /etc/group
grep ventas /etc/group
```

> El archivo `/etc/group` almacena la lista de grupos del sistema. Cada línea tiene el formato `nombre_grupo:x:GID:miembros`.

> **C0.1** — Resultado de la verificación de los grupos `administracion` y `ventas` en `/etc/group`.

---

## Parte 2. Crear los usuarios

🖥️ **`debian-servidor`**

Crea los cuatro usuarios con los nombres exactos de la Tarea 5 de Windows. Los comandos usan nombres genéricos: **sustitúyelos por los tuyos**.

```bash
# Usuarios del grupo administracion
sudo useradd -m -g administracion admin1
sudo useradd -m -g administracion admin2

# Usuarios del grupo ventas
sudo useradd -m -g ventas ventas1
sudo useradd -m -g ventas ventas2
```

- `-m` crea el directorio personal del usuario en `/home/`.
- `-g nombre_grupo` asigna el grupo principal del usuario.

Asigna la misma contraseña que en Windows. El comando `passwd` pedirá la contraseña dos veces:

```bash
sudo passwd admin1
sudo passwd admin2
sudo passwd ventas1
sudo passwd ventas2
```

Comprueba que los usuarios existen en el sistema:

```bash
grep admin1 /etc/passwd
grep ventas1 /etc/passwd
```

> El archivo `/etc/passwd` almacena la lista de usuarios. Cada línea tiene el formato `usuario:x:UID:GID:nombre_completo:directorio_home:shell`.

> **C0.2** — Creación de los cuatro usuarios con `useradd`.
>
> **C0.3** — Asignación de contraseñas con `passwd` para los cuatro usuarios.
>
> **C0.4** — Verificación de los usuarios en `/etc/passwd`.

---

## Parte 3. Verificar la pertenencia a grupos

🖥️ **`debian-servidor`**

```bash
id admin1
id admin2
id ventas1
id ventas2
```

El comando `id` muestra el UID, el GID principal y todos los grupos a los que pertenece el usuario. Comprueba que cada usuario aparece en el grupo correcto.

> **C0.5** — Resultado de `id` para los cuatro usuarios mostrando el grupo correcto.

---

## Errores típicos del ejercicio 0

**`useradd` dice que el usuario ya existe**  
Comprueba con `id nombre_usuario`. Si existe con grupo incorrecto, usa `sudo usermod -g nombre_grupo nombre_usuario`.

**El nombre del usuario no coincide con el de Windows**  
Linux distingue mayúsculas y minúsculas. `Admin1` y `admin1` son usuarios completamente distintos. Usa siempre minúsculas.

---

# Ejercicio 1 — Configuración de la red en VirtualBox

## Descripción general

En este ejercicio configurarás las tarjetas de red de `debian-servidor` y del cliente Debian, y asignarás las direcciones IP estáticas.

## Teoría necesaria

En Debian, las interfaces de red tienen nombres generados automáticamente según el hardware virtual detectado. Los nombres más habituales en VirtualBox son `enp0s3` para el primer adaptador y `enp0s8` para el segundo, pero **el nombre real puede variar**. Siempre debes comprobar el nombre real con `ip a` antes de editar ningún archivo de configuración.

Debian usa el sistema de configuración de red `ifupdown`. El archivo `/etc/network/interfaces` es el método estándar para configurar IPs estáticas en instalaciones sin entorno gráfico.

---

## Parte 1. Configurar los adaptadores en VirtualBox

> 🖥️ / 💻 Esta parte se realiza en el **anfitrión** (VirtualBox), con **ambas máquinas apagadas**.

En **cada máquina**, ve a Configuración → Red y configura:

- **Adaptador 1** → Conectado a: **NAT**
- **Adaptador 2** → Conectado a: **Red interna**, Nombre: `intranet`

> ⚠️ El nombre `intranet` debe ser exactamente el mismo que usaste en la Tarea 8 de Windows. Si no coincide, las máquinas estarán en redes virtuales distintas y no podrán verse entre sí.

> **C1.1** — Configuración del Adaptador 2 de `debian-servidor` en VirtualBox (red interna `intranet`).
>
> **C1.2** — Configuración del Adaptador 2 de `debian-cliente` en VirtualBox (red interna `intranet`).

---

## Parte 2. Identificar las interfaces de red

> Enciende ambas máquinas. Ejecuta este comando **en cada una por separado**.

🖥️ **`debian-servidor`** — y también 💻 **`debian-cliente`**:

```bash
ip a
```

La salida mostrará todas las interfaces de red. Identifica:

- La interfaz NAT: tendrá una IP del tipo `10.0.2.X` asignada automáticamente.
- La interfaz de red interna: aparecerá sin dirección IP (estado `DOWN` o `UP` sin `inet`).

**Anota el nombre exacto de cada interfaz antes de continuar.** Una diferencia de un solo carácter en el nombre hará que la configuración no funcione.

> **C1.3** — Resultado completo de `ip a` en `debian-servidor`.
>
> **C1.4** — Resultado completo de `ip a` en `debian-cliente`.

---

## Parte 3. Asignar IP estática en el servidor

🖥️ **`debian-servidor`**

```bash
sudo nano /etc/network/interfaces
```

Añade las siguientes líneas al final del archivo. Sustituye `enp0s8` por el nombre real de tu interfaz de red interna si es diferente:

```
auto enp0s8
iface enp0s8 inet static
    address 192.168.1.10
    netmask 255.255.255.0
```

- `auto enp0s8` → la interfaz se levanta automáticamente al arrancar.
- `inet static` → IP configurada de forma estática (no por DHCP).
- El servidor no necesita `gateway` en esta interfaz porque la puerta de enlace a Internet la gestiona el adaptador NAT.

Guarda con `Ctrl + O`, `Enter`, `Ctrl + X`. Aplica los cambios:

```bash
sudo ifdown enp0s8 2>/dev/null; sudo ifup enp0s8
```

> `ifdown` desactiva la interfaz (`2>/dev/null` descarta el posible error si ya estaba inactiva). `ifup` la vuelve a activar con la nueva configuración.

Comprueba el resultado:

```bash
ip a
```

Debes ver `192.168.1.10` asignada a la interfaz de red interna.

> **C1.5** — Contenido completo del archivo `/etc/network/interfaces` del servidor.
>
> **C1.6** — Resultado de `ip a` en `debian-servidor` mostrando `192.168.1.10`.

---

## Parte 4. Asignar IP estática en el cliente

💻 **`debian-cliente`**

> ⚠️ **Importante:** `debian-cliente` tiene entorno gráfico, lo que significa que **NetworkManager** gestiona las interfaces de red automáticamente. En este caso, editar `/etc/network/interfaces` directamente puede entrar en conflicto con NetworkManager y provocar comportamientos inesperados. Usa el método que corresponda según tu instalación:

**Opción A — Si el cliente tiene GNOME o KDE (NetworkManager activo):**

Usa el entorno gráfico. Ve a **Configuración → Red → (tu interfaz de red interna) → icono del engranaje → IPv4**:

- Método: **Manual**
- Dirección: `192.168.1.11`
- Máscara de red: `255.255.255.0`
- Puerta de enlace: `192.168.1.10`
- DNS: `8.8.8.8`

Haz clic en **Aplicar** y desactiva/activa la interfaz para que tome la nueva configuración.

**Opción B — Si el cliente no tiene NetworkManager activo (instalación mínima con escritorio ligero):**

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

```bash
sudo ifdown enp0s8 2>/dev/null; sudo ifup enp0s8
```

> ℹ️ Para comprobar si NetworkManager está activo en el cliente: `systemctl is-active NetworkManager`. Si devuelve `active`, usa la Opción A.

Comprueba en cualquier caso:

```bash
ip a
```

> **C1.7** — Configuración de red aplicada en `debian-cliente` (captura del entorno gráfico o del archivo `/etc/network/interfaces`, según el método usado).
>
> **C1.8** — Resultado de `ip a` en `debian-cliente` mostrando `192.168.1.11`.

---

## Parte 5. Comprobar conectividad

💻 **`debian-cliente`** → comprueba que llega al servidor:

```bash
ping -c 4 192.168.1.10
```

🖥️ **`debian-servidor`** → comprueba que llega al cliente:

```bash
ping -c 4 192.168.1.11
```

**Qué debes explicar en el PDF:**
- Qué comprueba el comando `ping`.
- Por qué es importante verificar la conectividad en los dos sentidos antes de continuar.

> **C1.9** — Ping correcto de `debian-cliente` al servidor (4 paquetes enviados y recibidos).
>
> **C1.10** — Ping correcto de `debian-servidor` al cliente.

---

## Errores típicos del ejercicio 1

**La IP no aparece tras aplicar la configuración en el servidor**  
Comprueba con `ip a` que el nombre de la interfaz en `/etc/network/interfaces` coincide exactamente.

**`ifup` dice que la interfaz no está configurada**  
El nombre de la interfaz no coincide. Usa `ip a` para ver el nombre real.

**La IP no se aplica en el cliente aunque editaste `/etc/network/interfaces`**  
El cliente tiene NetworkManager activo, que ignora los cambios en `/etc/network/interfaces`. Usa la Opción A (configuración gráfica) o detén NetworkManager antes: `sudo systemctl stop NetworkManager`. Recuerda que si detienes NetworkManager en una máquina gráfica puedes perder conectividad temporal.

**El ping entre Linux y Windows no funciona**  
Comprueba: (1) que las máquinas Windows tienen IP en el rango `.20`/`.21`, (2) que el Adaptador 2 de Windows está configurado como red interna `intranet`, (3) que el firewall de Windows permite ping (regla creada en la Tarea 8).

---

# Ejercicio 2 — Enrutamiento y diagnóstico de red

## Descripción general

Configurarás `debian-servidor` para que actúe como router, permitiendo que el cliente Debian salga a Internet. Después aprenderás a diagnosticar problemas de red de forma ordenada y configurarás la resolución de nombres local.

## Teoría necesaria

Por defecto, Linux no reenvía paquetes entre sus interfaces aunque tenga dos tarjetas. Para que actúe como router hay que activar el **reenvío IP** (`ip_forward`) y añadir una regla en `iptables` para que el tráfico de la red interna salga enmascarado a través de la interfaz NAT (técnica llamada NAT o masquerade).

---

## Parte 1. Activar el enrutamiento en el servidor

🖥️ **`debian-servidor`**

Primero comprueba si `iptables` está disponible. En instalaciones mínimas de Debian puede no estar instalado:

```bash
which iptables
```

Si el comando no devuelve ninguna ruta, instálalo:

```bash
sudo apt install iptables
```

Comprueba el estado actual del reenvío IP (normalmente `0`, desactivado):

```bash
cat /proc/sys/net/ipv4/ip_forward
```

Actívalo:

```bash
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
```

Añade la regla de NAT. Sustituye `enp0s3` por el nombre real de tu interfaz NAT (la que tiene la IP `10.0.2.X`):

```bash
sudo iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o enp0s3 -j MASQUERADE
```

> ⚠️ Es fundamental usar `-o enp0s3` (interfaz de salida) y no `-d 0/0` (destino), que es una sintaxis incorrecta. Si el nombre de tu interfaz NAT es diferente, corrígelo aquí.

Verifica que el reenvío está activado:

```bash
cat /proc/sys/net/ipv4/ip_forward
```

Debe mostrar `1`.

> **C2.1** — Valor de `ip_forward` antes (`0`) y después (`1`) de activarlo.
>
> **C2.2** — Ejecución del comando `iptables` con la regla de NAT.

---

## Parte 2. Comprobar la salida a Internet desde el cliente

> ⚠️ Cambia a `debian-cliente` para este apartado.

💻 **`debian-cliente`**

```bash
ping -c 4 8.8.8.8
```

Si este ping funciona, el enrutamiento es correcto. Ahora comprueba la resolución de nombres:

```bash
ping -c 4 google.com
```

### Si `ping google.com` falla con error de resolución de nombres

En Debian actual, `/etc/resolv.conf` puede estar gestionado por `systemd-resolved` y sobrescribirse automáticamente. El método correcto para configurar DNS de forma permanente es editar la configuración de `systemd-resolved`:

```bash
sudo nano /etc/systemd/resolved.conf
```

Localiza la línea `#DNS=` (está comentada) y reemplázala por:

```
DNS=8.8.8.8 8.8.4.4
```

Guarda y reinicia el servicio:

```bash
sudo systemctl restart systemd-resolved
```

Si en tu instalación `systemd-resolved` no está activo (puede ocurrir en instalaciones mínimas), el método alternativo es editar directamente `/etc/resolv.conf`:

```bash
sudo nano /etc/resolv.conf
```

Añade:

```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

> ℹ️ En instalaciones sin entorno gráfico es frecuente que `/etc/resolv.conf` no esté gestionado por `systemd-resolved`. En ese caso, editarlo directamente es perfectamente válido. Si tras reiniciar el equipo el archivo vuelve a estar vacío, es señal de que `systemd-resolved` lo está sobrescribiendo y debes usar el método de `resolved.conf`.

Vuelve a probar:

```bash
ping -c 4 google.com
```

> **C2.3** — Ping correcto a `8.8.8.8` desde `debian-cliente`.
>
> **C2.4** — Ping correcto a `google.com` desde `debian-cliente`.

---

## Parte 3. Hacer el enrutamiento permanente con systemd

> 🖥️ Vuelve a `debian-servidor` para este apartado.

🖥️ **`debian-servidor`**

Los cambios hechos en la Parte 1 son temporales: desaparecen al reiniciar el servidor. El método correcto en Debian actual es crear un **servicio systemd** que ejecute el script de enrutamiento en cada arranque.

> ℹ️ **Por qué no usamos `/etc/rc.local`:** en Debian actual, `rc.local` no está habilitado por defecto y requiere pasos adicionales para activarlo. Un servicio systemd es la forma estándar, robusta y recomendada.

**Paso 1.** Crea el script de enrutamiento. Sustituye `enp0s3` por tu interfaz NAT real:

```bash
sudo nano /usr/local/sbin/enrutamiento.sh
```

```bash
#!/bin/bash
echo 1 > /proc/sys/net/ipv4/ip_forward
iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o enp0s3 -j MASQUERADE
```

Dale permisos de ejecución y comprueba:

```bash
sudo chmod +x /usr/local/sbin/enrutamiento.sh
ls -l /usr/local/sbin/enrutamiento.sh
```

Debe aparecer `-rwxr-xr-x` (la `x` de ejecución es imprescindible).

**Paso 2.** Crea el archivo del servicio systemd:

```bash
sudo nano /etc/systemd/system/enrutamiento.service
```

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

- `After=network.target` → garantiza que las interfaces están listas antes de ejecutar el script.
- `Type=oneshot` → ejecuta un comando y termina (no es un proceso continuo).
- `RemainAfterExit=yes` → systemd considera el servicio como «activo» aunque el proceso haya terminado.

**Paso 3.** Registra y activa el servicio:

```bash
sudo systemctl daemon-reload
sudo systemctl enable enrutamiento.service
sudo systemctl start enrutamiento.service
sudo systemctl status enrutamiento.service
```

El estado debe mostrar `active (exited)`, que es el comportamiento correcto para un servicio `oneshot`.

**Paso 4.** Reinicia el servidor y comprueba desde el cliente:

```bash
# En el servidor:
sudo reboot
```

> ⚠️ Espera a que el servidor arranque completamente antes de continuar.

💻 **`debian-cliente`** — después de que el servidor haya arrancado:

```bash
ping -c 4 google.com
```

> **C2.5** — Contenido del script `/usr/local/sbin/enrutamiento.sh`.
>
> **C2.6** — Contenido del archivo `/etc/systemd/system/enrutamiento.service`.
>
> **C2.7** — Resultado de `systemctl status enrutamiento.service` mostrando `active (exited)`.
>
> **C2.8** — Ping correcto a `google.com` desde `debian-cliente` **después de reiniciar** el servidor, demostrando que el enrutamiento es permanente.

---

## Parte 4. Diagnóstico ordenado de red

💻 **`debian-cliente`**

Una de las habilidades más importantes de un administrador de sistemas es diagnosticar problemas de red de forma sistemática, empezando por el nivel más básico y subiendo progresivamente. Ejecuta estos ping en orden:

```bash
ping -c 4 127.0.0.1          # nivel 1: la pila TCP/IP local
ping -c 4 192.168.1.11        # nivel 2: la propia IP del cliente
ping -c 4 192.168.1.10        # nivel 3: el servidor (otro equipo)
ping -c 4 8.8.8.8             # nivel 4: Internet por dirección IP
ping -c 4 google.com          # nivel 5: Internet por nombre de dominio
```

**Qué debes explicar en el PDF:**

1. ¿Qué significa que el nivel 3 falle pero el nivel 2 funcione?
2. ¿Qué diferencia hay entre hacer ping a `8.8.8.8` y a `google.com`?
3. Si el nivel 4 funciona pero el nivel 5 falla, ¿qué parte no está funcionando?
4. ¿Por qué es útil diagnosticar por niveles en lugar de ir directamente al servicio que falla?

> **C2.9** — Resultado de los cinco ping por niveles desde `debian-cliente`, todos correctos.
>
> **C2.10** — Respuestas escritas a las cuatro preguntas de diagnóstico.

---

## Parte 5. Resolución de nombres local con `/etc/hosts`

El archivo `/etc/hosts` permite asociar nombres de host a direcciones IP directamente en el equipo local, sin necesidad de un servidor DNS.

> Realiza este paso en **ambas máquinas por separado**.

🖥️ **`debian-servidor`** — y también 💻 **`debian-cliente`** (en cada una):

```bash
sudo nano /etc/hosts
```

Añade al final de cada archivo:

```
192.168.1.10 debian-servidor
192.168.1.11 debian-cliente
```

Comprueba que funciona:

💻 **`debian-cliente`**:
```bash
ping -c 4 debian-servidor
```

🖥️ **`debian-servidor`**:
```bash
ping -c 4 debian-cliente
```

**Qué debes explicar en el PDF:**
- Qué diferencia hay entre `/etc/hosts` y un servidor DNS.
- Por qué en una red de 500 equipos no sería suficiente con `/etc/hosts`.

> ℹ️ **Nota importante para el Ejercicio 8:** `nslookup` consulta únicamente los servidores DNS externos, **no** el archivo `/etc/hosts`. Por eso, `nslookup debian-servidor` devolverá un error aunque `ping debian-servidor` funcione perfectamente. Esto es normal y esperado, no un error de configuración.

> **C2.11** — Contenido de `/etc/hosts` del cliente con las líneas añadidas.
>
> **C2.12** — Ping por nombre desde `debian-cliente` a `debian-servidor` funcionando correctamente.

---

## Errores típicos del ejercicio 2

**`ping google.com` falla con «Temporary failure in name resolution»**  
El cliente no tiene DNS configurado. Consulta la Parte 2 de este ejercicio para el método correcto según tu instalación.

**El enrutamiento deja de funcionar tras reiniciar**  
Comprueba con `sudo systemctl status enrutamiento.service`. Si está en error, revisa que el script tiene permisos de ejecución (`ls -l /usr/local/sbin/enrutamiento.sh`) y que el nombre de la interfaz NAT en el script es correcto.

**`systemctl enable` da error «Failed to enable unit»**  
El archivo `.service` tiene errores de sintaxis. Ejecuta `sudo systemctl daemon-reload` y después `sudo journalctl -xe` para ver el error detallado.

---

# Ejercicio 3 — Samba: recurso público y recursos privados

> 🖥️ **Todo este ejercicio se realiza en `debian-servidor`**, salvo que se indique lo contrario.

## Descripción general

Instalarás Samba en el servidor y crearás tres recursos compartidos: una carpeta pública accesible sin contraseña, una carpeta de administración solo para ese grupo, y una carpeta de ventas donde `ventas` puede escribir pero `administracion` solo puede leer.

## Teoría necesaria

Samba trabaja con dos capas independientes de control de acceso:

- **Permisos del sistema de archivos Linux** (`chmod`, `chgrp`): controlan quién puede leer o escribir en el disco.
- **Configuración en `smb.conf`**: controla el acceso a través de la red Samba.

Si alguna de las dos capas deniega el acceso, el resultado final es **acceso denegado**. Es el mismo principio que permisos de compartición + permisos NTFS en Windows.

---

## Parte 1. Instalar Samba

🖥️ **`debian-servidor`**

```bash
sudo apt update
sudo apt install samba samba-common-bin
```

Verifica que los servicios están en ejecución:

```bash
sudo systemctl status smbd
sudo systemctl status nmbd
```

- `smbd` gestiona los recursos compartidos y la autenticación.
- `nmbd` gestiona la resolución de nombres NetBIOS (permite que los equipos se encuentren por nombre).

> **C3.1** — Instalación de Samba completada sin errores.
>
> **C3.2** — Estado del servicio `smbd` con `systemctl status` mostrando `active (running)`.

---

## Parte 2. Crear las carpetas y configurar permisos

🖥️ **`debian-servidor`**

```bash
sudo mkdir -p /samba/publica
sudo mkdir -p /samba/administracion
sudo mkdir -p /samba/ventas
```

Crea un archivo de prueba en cada carpeta:

```bash
echo "Carpeta pública - acceso libre de lectura" | sudo tee /samba/publica/leeme.txt
echo "Carpeta de administración - solo grupo administracion" | sudo tee /samba/administracion/info_admin.txt
echo "Carpeta de ventas - ventas escribe, administracion solo lee" | sudo tee /samba/ventas/info_ventas.txt
```

Configura los permisos:

```bash
# Pública: todos pueden leer (755)
sudo chmod 755 /samba/publica

# Administración: solo el grupo administracion (770)
sudo chgrp -R administracion /samba/administracion
sudo chmod 770 /samba/administracion

# Ventas: el grupo ventas tiene acceso completo (775)
sudo chgrp -R ventas /samba/ventas
sudo chmod 775 /samba/ventas
```

Verifica los permisos:

```bash
ls -ld /samba/publica /samba/administracion /samba/ventas
```

> **C3.3** — Creación de las tres carpetas y sus archivos de prueba.
>
> **C3.4** — Resultado de `ls -ld` mostrando los permisos correctos de las tres carpetas.

---

## Parte 3. Configurar el archivo `smb.conf`

🖥️ **`debian-servidor`**

Haz primero una copia de seguridad:

```bash
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.original
```

Abre el archivo:

```bash
sudo nano /etc/samba/smb.conf
```

Localiza la línea `workgroup =` en la sección `[global]` y asegúrate de que coincide con el nombre de grupo de trabajo que usaste en Windows:

```
workgroup = EMPRESA
```

En esa misma sección `[global]`, añade también esta línea, que es **imprescindible** para que el acceso sin contraseña al recurso público funcione en las versiones actuales de Samba:

```
map to guest = bad user
```

> ℹ️ Sin `map to guest = bad user`, Samba rechaza los accesos anónimos aunque el recurso tenga `guest ok = yes`. Esta directiva indica a Samba que los usuarios que no existen en su base de datos deben tratarse como invitados (`guest`), en lugar de rechazar la conexión.

Añade los tres bloques al final del archivo:

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
| `workgroup = EMPRESA` | El servidor Samba aparece en el mismo grupo de trabajo que los equipos Windows. |
| `browseable = yes` | El recurso es visible al explorar la red. |
| `guest ok = yes` | Permite el acceso sin contraseña (solo en `[publica]`). |
| `read only = yes` | Nadie puede escribir, aunque tenga permisos de sistema de archivos. |
| `valid users = @administracion` | El `@` indica grupo. Solo los usuarios de ese grupo pueden conectarse. |
| `write list = @ventas` | Solo `ventas` puede escribir. `@administracion` puede leer, pero no escribir. |

Valida la sintaxis:

```bash
testparm
```

`testparm` analiza `smb.conf` y muestra los parámetros activos. Si hay errores, indica el número de línea.

Reinicia los servicios:

```bash
sudo systemctl restart smbd
sudo systemctl restart nmbd
```

> **C3.5** — Copia de seguridad del archivo original con `cp`.
>
> **C3.6** — Los tres bloques de recursos añadidos al final de `smb.conf`.
>
> **C3.7** — Resultado de `testparm` sin errores.
>
> **C3.8** — Reinicio de `smbd` y `nmbd` con `systemctl restart` y verificación de estado con `systemctl status`.

---

## Parte 4. Dar de alta los usuarios en Samba

🖥️ **`debian-servidor`**

Samba mantiene su propia base de datos de contraseñas, independiente del sistema Linux. Para que un usuario pueda autenticarse en Samba debe estar registrado en **ambos** sistemas.

```bash
sudo smbpasswd -a admin1
sudo smbpasswd -a admin2
sudo smbpasswd -a ventas1
sudo smbpasswd -a ventas2
```

Introduce la misma contraseña que en Windows para que la autenticación sea transparente.

> **C3.9** — Alta de los cuatro usuarios en Samba con `smbpasswd -a`.

---

## Errores típicos del ejercicio 3

**`testparm` muestra «Unknown parameter encounter»**  
Hay un error tipográfico en alguna opción. `testparm` indica el número de línea. Abre `smb.conf` y corrígelo.

**`systemctl restart smbd` falla**  
Hay un error grave de sintaxis en `smb.conf`. Ejecuta `testparm` primero.

**Un usuario de `administracion` puede escribir en `ventas`**  
Verifica que `write list = @ventas` está dentro del bloque `[ventas]` y que has reiniciado `smbd`.

---

# Ejercicio 4 — Acceso a Samba desde cliente Linux y desde Windows

## Descripción general

Accederás a los recursos Samba desde `debian-cliente` usando la terminal, y desde las máquinas Windows de la Tarea 8. Comprobarás que los permisos funcionan correctamente.

---

## Parte 1. Acceso desde el cliente Linux por terminal

💻 **`debian-cliente`**

Instala las herramientas necesarias:

```bash
sudo apt install samba-common-bin cifs-utils smbclient
```

Lista los recursos disponibles en el servidor (sin contraseña):

```bash
smbclient -L //192.168.1.10 -N
```

Deben aparecer los tres recursos: `publica`, `administracion` y `ventas`.

**Acceso al recurso público** (sin contraseña):

```bash
smbclient //192.168.1.10/publica -N
```

Una vez dentro, el prompt cambia a `smb: \>`. Ejecuta estos comandos **dentro de la sesión smbclient**:

```
smb: \> ls
smb: \> get leeme.txt
smb: \> quit
```

De vuelta en la terminal normal, comprueba que el archivo se descargó:

```bash
ls -l leeme.txt
```

**Intento de escritura en el recurso público** (debe fallar):

```bash
smbclient //192.168.1.10/publica -N
```

Dentro de smbclient:

```
smb: \> put leeme.txt prueba_escritura.txt
smb: \> quit
```

Debe aparecer un error de acceso denegado.

**Acceso al recurso `administracion`** con `admin1`:

```bash
smbclient //192.168.1.10/administracion -U admin1
```

Introduce la contraseña cuando se pida. Dentro de smbclient:

```
smb: \> ls
smb: \> put leeme.txt desde_linux.txt
smb: \> quit
```

**Intento de escritura de `admin1` en `ventas`** (debe fallar):

```bash
smbclient //192.168.1.10/ventas -U admin1
```

Dentro de smbclient:

```
smb: \> put leeme.txt intento_escritura.txt
smb: \> quit
```

> **C4.1** — Listado de recursos con `smbclient -L`.
>
> **C4.2** — Acceso al recurso público y descarga de `leeme.txt`.
>
> **C4.3** — Error al intentar escribir en el recurso público.
>
> **C4.4** — Acceso de `admin1` a `administracion` y subida correcta de un archivo.
>
> **C4.5** — Error al intentar que `admin1` escriba en `ventas`.

---

## Parte 2. Montaje del recurso Samba en el cliente Linux

💻 **`debian-cliente`**

```bash
sudo mkdir -p /mnt/administracion
sudo mount -t cifs //192.168.1.10/administracion /mnt/administracion -o user=admin1
```

El sistema pedirá la contraseña de `admin1`. Una vez montado:

```bash
ls -l /mnt/administracion
echo "Creado desde montaje CIFS" | sudo tee /mnt/administracion/desde_montaje.txt
```

Verifica en el servidor:

🖥️ **`debian-servidor`**:
```bash
ls -l /samba/administracion
```

Desmonta el recurso:

💻 **`debian-cliente`**:
```bash
sudo umount /mnt/administracion
```

> **C4.6** — Montaje con `mount -t cifs` y listado del contenido.
>
> **C4.7** — Archivo `desde_montaje.txt` visible en `/samba/administracion` del servidor.
>
> **C4.8** — Desmontaje correcto con `umount`.

---

## Parte 3. Montaje automático con `/etc/fstab`

💻 **`debian-cliente`**

Crea el archivo de credenciales con permisos restrictivos:

```bash
sudo nano /root/.credenciales_samba
```

Contenido (sustituye la contraseña real):

```
username=admin1
password=contraseña_de_admin1
```

```bash
sudo chmod 600 /root/.credenciales_samba
```

Con `600`, solo `root` puede leer y escribir este archivo.

Añade la línea en `/etc/fstab`:

```bash
sudo nano /etc/fstab
```

```
//192.168.1.10/administracion /mnt/administracion cifs credentials=/root/.credenciales_samba,iocharset=utf8 0 0
```

Prueba el montaje:

```bash
sudo mount -a
ls -l /mnt/administracion
```

`mount -a` monta todo lo que está en `/etc/fstab` y no está montado todavía.

> **C4.9** — Archivo `/root/.credenciales_samba` y sus permisos con `ls -l`.
>
> **C4.10** — Línea añadida en `/etc/fstab` del cliente.
>
> **C4.11** — Resultado de `mount -a` sin errores y contenido del recurso montado.

---

## Parte 4. Acceso desde las máquinas Windows de la Tarea 8

> 🪟 **`Windows` (`cliente1` o `cliente2`)**

Las máquinas Windows deben tener IP en el rango `192.168.1.20`–`192.168.1.21` y el Adaptador 2 configurado como red interna `intranet`.

1. Abre el Explorador de Windows.
2. En la barra de direcciones escribe `\\192.168.1.10` y pulsa Enter.
3. Deben aparecer los tres recursos: `publica`, `administracion` y `ventas`.
4. Accede a `publica` sin contraseña. Comprueba que puedes leer `leeme.txt` pero no crear archivos nuevos.
5. Accede a `administracion` con `admin1`. Si la sesión activa de Windows usa ese mismo usuario y la contraseña coincide con la de Samba, el acceso es automático y transparente.
6. Crea un archivo en `administracion` desde Windows.

🖥️ **`debian-servidor`** — Verifica que el archivo aparece:
```bash
ls -l /samba/administracion
```

🪟 **Windows** — continúa:

7. Intenta crear un archivo en `ventas` con `admin1`. Debe fallar.
8. Abre una ventana de comandos y ejecuta `net use * /delete` para borrar las credenciales en caché. Luego accede a `ventas` e introduce las credenciales de `ventas1`. Ahora sí debes poder crear archivos.

> ℹ️ **Por qué el acceso desde Windows puede ser transparente:** Windows envía automáticamente las credenciales de la sesión activa al conectarse a Samba. Si el nombre y la contraseña coinciden, el acceso es silencioso. Por eso era tan importante el Ejercicio 0.

> ⚠️ Si no tienes las máquinas Windows disponibles, indica en el PDF la razón.

> **C4.12** — Recursos Samba visibles desde el Explorador de Windows al acceder a `\\192.168.1.10`.
>
> **C4.13** — Archivo creado en `administracion` desde Windows, visible en el servidor Linux.
>
> **C4.14** — Error de acceso denegado al intentar escribir en `ventas` con un usuario de `administracion`.
>
> **C4.15** — Acceso correcto de `ventas1` a `ventas` y creación de un archivo.

---

## Errores típicos del ejercicio 4

**Windows pide contraseña aunque el usuario debería coincidir**  
La contraseña de `smbpasswd` no coincide con la de Windows. Restablécela: `sudo smbpasswd admin1`.

**`mount -t cifs` da «unknown filesystem type 'cifs'»**  
Falta el paquete `cifs-utils`. Instálalo: `sudo apt install cifs-utils`.

**`admin1` puede escribir en `ventas`**  
Verifica en `smb.conf` que `write list = @ventas` está dentro del bloque `[ventas]` y que has ejecutado `sudo systemctl restart smbd`.

**Windows no encuentra el servidor al escribir `\\192.168.1.10`**  
Comprueba: (1) que la IP del adaptador de red interna de la máquina Windows es `192.168.1.20` o `.21`, (2) que el firewall de Windows permite la conexión SMB.

---

# Ejercicio 5 — NFS: instalación y montaje

## Descripción general

Instalarás NFS para compartir carpetas directamente entre los dos equipos Linux. NFS es más eficiente que Samba para redes de solo Linux, pero no es compatible con Windows.

## Teoría necesaria

NFS controla el acceso por **dirección IP**, no por usuario y contraseña. En `/etc/exports` defines qué directorio se comparte, a qué equipos y con qué permisos.

---

## Parte 1. Instalar el servidor NFS

🖥️ **`debian-servidor`**

```bash
sudo apt install nfs-kernel-server
sudo systemctl status nfs-kernel-server
```

> **C5.1** — Instalación de `nfs-kernel-server` completada.
>
> **C5.2** — Estado del servicio NFS con `systemctl status` mostrando `active (running)`.

---

## Parte 2. Crear carpetas y configurar `/etc/exports`

🖥️ **`debian-servidor`**

```bash
sudo mkdir -p /nfs/lectura
sudo mkdir -p /nfs/escritura
echo "Archivo NFS de solo lectura" | sudo tee /nfs/lectura/saludo.txt
```

Asigna el propietario `nobody:nogroup`:

```bash
sudo chown -R nobody:nogroup /nfs
sudo chmod -R 755 /nfs
```

> ℹ️ Antes de ejecutar el `chown`, verifica que el grupo `nogroup` existe en tu sistema: `grep nogroup /etc/group`. En Debian debe aparecer. Si no existiera, el comando correcto sería `nobody:nobody`.

> ℹ️ Se usa `755` (y no `770`) porque NFS accede al sistema de archivos como el usuario `nobody`, que no pertenece a ningún grupo concreto. Con `770`, `nobody` no tendría permisos de lectura. Con `755`, cualquier usuario puede leer, y el control de escritura lo ejerce la configuración `ro`/`rw` del propio `/etc/exports`.

Edita el archivo de exportaciones:

```bash
sudo nano /etc/exports
```

Añade al final:

```
/nfs/lectura   192.168.1.0/24(ro,sync,no_subtree_check)
/nfs/escritura 192.168.1.11(rw,sync,no_subtree_check)
```

- `ro` → solo lectura para toda la red `192.168.1.0/24`.
- `rw` → lectura y escritura, pero solo para `192.168.1.11`.
- `sync` → escribe en disco antes de confirmar la operación (más seguro).
- `no_subtree_check` → mejora el rendimiento evitando comprobaciones de subdirectorios.

Aplica la configuración y reinicia:

```bash
sudo exportfs -a
sudo systemctl restart nfs-kernel-server
```

`exportfs -a` hace efectivos los cambios en `/etc/exports` sin necesidad de reiniciar el servicio completo. El reinicio posterior garantiza que el servicio arranca desde un estado limpio.

> **C5.3** — Creación de carpetas y archivo de prueba, con `ls -l` mostrando los permisos.
>
> **C5.4** — Contenido completo de `/etc/exports`.
>
> **C5.5** — Ejecución de `exportfs -a` y reinicio del servicio NFS.

---

## Parte 3. Instalar el cliente NFS y montar los recursos

💻 **`debian-cliente`**

```bash
sudo apt install nfs-common
sudo mkdir -p /mnt/nfs/lectura
sudo mkdir -p /mnt/nfs/escritura
```

**Monta el recurso de solo lectura:**

```bash
sudo mount -t nfs 192.168.1.10:/nfs/lectura /mnt/nfs/lectura
cat /mnt/nfs/lectura/saludo.txt
echo "intento" | sudo tee /mnt/nfs/lectura/test.txt
```

La lectura debe funcionar. El intento de escritura debe fallar.

**Monta el recurso de escritura:**

```bash
sudo mount -t nfs 192.168.1.10:/nfs/escritura /mnt/nfs/escritura
echo "Desde el cliente Linux" | sudo tee /mnt/nfs/escritura/desde_cliente.txt
```

Verifica en el servidor:

🖥️ **`debian-servidor`**:
```bash
ls -l /nfs/escritura
```

> **C5.6** — Instalación de `nfs-common` y creación de los puntos de montaje.
>
> **C5.7** — Lectura correcta de `saludo.txt` desde `debian-cliente`.
>
> **C5.8** — Error al intentar escribir en el recurso de solo lectura.
>
> **C5.9** — Escritura correcta en el recurso de escritura.
>
> **C5.10** — Archivo `desde_cliente.txt` visible en el servidor en `/nfs/escritura`.

---

## Parte 4. Montaje automático con `/etc/fstab`

💻 **`debian-cliente`**

```bash
sudo nano /etc/fstab
```

Añade estas líneas:

```
192.168.1.10:/nfs/lectura   /mnt/nfs/lectura   nfs ro,intr,x-gvfs-show 0 0
192.168.1.10:/nfs/escritura /mnt/nfs/escritura nfs rw,intr,x-gvfs-show 0 0
```

Desmonta y vuelve a montar con `fstab`:

```bash
sudo umount /mnt/nfs/lectura
sudo umount /mnt/nfs/escritura
sudo mount -a
ls -l /mnt/nfs/lectura
ls -l /mnt/nfs/escritura
```

**Qué debes explicar en el PDF:**
- Diferencia práctica entre Samba y NFS: mecanismo de acceso, autenticación y compatibilidad.
- En qué situaciones usarías Samba y en cuáles NFS.

> **C5.11** — Líneas NFS añadidas en `/etc/fstab` del cliente.
>
> **C5.12** — Resultado de `mount -a` y listado de los dos recursos NFS montados.

---

## Errores típicos del ejercicio 5

**«Connection refused» al montar NFS**  
El servicio NFS no está activo. Ejecuta `sudo systemctl status nfs-kernel-server` en el servidor.

**«Permission denied» al montar desde el cliente**  
La IP del cliente no coincide con la de `/etc/exports`. Comprueba con `ip a` la IP real del cliente.

**El cliente monta pero no puede escribir en `/escritura`**  
Comprueba que `chown -R nobody:nogroup /nfs` se ejecutó correctamente. Si el propietario es `root`, el cliente no tendrá permisos de escritura.

---

# Ejercicio 6 — SSH y transferencia de archivos con SCP

## Descripción general

Instalarás el servidor SSH en `debian-servidor` y lo administrarás de forma remota. Usarás `scp` para copiar archivos entre los dos equipos de forma segura.

## Teoría necesaria

SSH cifra toda la comunicación entre cliente y servidor. `scp` usa el canal SSH como medio de transporte: si SSH funciona, `scp` también funciona sin configuración adicional.

La primera vez que te conectas a un servidor SSH, el cliente muestra la **huella digital** (fingerprint) del servidor y pregunta si confías en él. Al aceptar, el cliente guarda esa huella y la compara en conexiones futuras.

---

## Parte 1. Instalar el servidor SSH

🖥️ **`debian-servidor`**

```bash
sudo apt install openssh-server
sudo systemctl status ssh
ss -tlnp | grep :22
```

> ℹ️ En Debian, el paquete se llama `openssh-server` pero el servicio del sistema se llama `ssh`. Tras la instalación, el servicio arranca automáticamente y se activa para los próximos reinicios. Puedes comprobarlo con `systemctl is-enabled ssh`, que debe devolver `enabled`.

> **C6.1** — Instalación del paquete `openssh-server` completada.
>
> **C6.2** — Estado del servicio SSH con `systemctl status ssh` mostrando `active (running)`.
>
> **C6.3** — Puerto 22 en escucha con `ss -tlnp`.

---

## Parte 2. Conectarse al servidor desde el cliente

💻 **`debian-cliente`**

```bash
ssh admin1@192.168.1.10
```

La primera vez aparecerá un mensaje como este:

```
The authenticity of host '192.168.1.10 (192.168.1.10)' can't be established.
ED25519 key fingerprint is SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Escribe `yes` y pulsa Enter. Introduce la contraseña de `admin1`.

> ⚠️ Una vez ejecutado `ssh`, estás trabajando **dentro del servidor** a través de la red. Los comandos que ejecutes a continuación se ejecutan en `debian-servidor`, aunque los escribas en la pantalla de `debian-cliente`.

🖥️ **`debian-servidor` (vía SSH desde `debian-cliente`)** — ejecuta dentro de la sesión SSH:

```bash
hostname
whoami
ls /samba/administracion
ls /samba/ventas
exit
```

Vuelves a estar en `debian-cliente`. Conecta ahora con `ventas1`:

💻 **`debian-cliente`**:
```bash
ssh ventas1@192.168.1.10
```

🖥️ **`debian-servidor` (vía SSH)**:
```bash
who
exit
```

El comando `who` muestra los usuarios conectados actualmente, incluyendo la IP de origen de las conexiones SSH.

> **C6.4** — Mensaje de fingerprint y confirmación con `yes` en la primera conexión.
>
> **C6.5** — Sesión SSH activa mostrando `hostname` y `whoami` con valores del servidor.
>
> **C6.6** — Comandos `ls /samba/administracion` y `ls /samba/ventas` ejecutados remotamente.
>
> **C6.7** — Resultado de `who` mostrando las sesiones SSH activas con las IPs de origen.

---

## Parte 3. Transferir archivos con `scp`

> Los comandos `scp` se ejecutan **siempre desde `debian-cliente`**, no desde dentro de una sesión SSH.

💻 **`debian-cliente`**

**Enviar un archivo del cliente al servidor:**

```bash
echo "Archivo enviado con scp desde el cliente" > prueba_scp.txt
scp prueba_scp.txt admin1@192.168.1.10:/home/admin1/
```

Verifica que el archivo llegó:

```bash
ssh admin1@192.168.1.10 "ls -l /home/admin1/"
```

**Recibir un archivo del servidor:**

```bash
ssh admin1@192.168.1.10 "echo 'Creado en el servidor' > /home/admin1/desde_servidor.txt"
scp admin1@192.168.1.10:/home/admin1/desde_servidor.txt .
cat desde_servidor.txt
```

**Copiar un directorio completo** (opción `-r` para recursivo):

```bash
scp -r admin1@192.168.1.10:/home/admin1 /tmp/copia_admin1
ls -l /tmp/copia_admin1
```

**Qué debes explicar en el PDF:**
- Por qué SSH es más seguro que Telnet (el protocolo que reemplazó).
- Qué ventaja tiene `scp` frente al servidor FTP con IIS que configuraste en la Tarea 8.
- Qué significa el mensaje de fingerprint que aparece la primera vez y por qué es importante.

> **C6.8** — Envío de archivo al servidor con `scp` y verificación remota con `ls`.
>
> **C6.9** — Descarga de archivo desde el servidor con `scp` y verificación local con `cat`.
>
> **C6.10** — Copia recursiva de directorio con `scp -r` y listado del resultado.

---

## Errores típicos del ejercicio 6

**«Connection refused» al conectar por SSH**  
El servicio SSH no está activo en el servidor. Ejecuta `sudo systemctl status ssh` en el servidor.

**«Permission denied» al introducir la contraseña**  
La contraseña es incorrecta. Cámbiala en el servidor: `sudo passwd admin1`.

**«WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED»**  
La huella del servidor cambió (por ejemplo, tras una reinstalación). En el cliente, edita `~/.ssh/known_hosts` y elimina la línea con `192.168.1.10`, luego reconecta.

---

# Ejercicio 7 — Apache: servidor web básico

## Descripción general

Instalarás Apache en `debian-servidor` y publicarás una página web accesible desde `debian-cliente` y desde los equipos Windows.

---

## Parte 1. Instalar Apache

🖥️ **`debian-servidor`**

```bash
sudo apt update
sudo apt install apache2
sudo systemctl status apache2
ss -tlnp | grep :80
```

> **C7.1** — Instalación de Apache completada.
>
> **C7.2** — Estado del servicio `apache2` con `systemctl status` mostrando `active (running)`.
>
> **C7.3** — Puerto 80 en escucha con `ss -tlnp`.

---

## Parte 2. Probar la página por defecto

🖥️ **`debian-servidor`** — comprueba con `curl`:

```bash
curl http://localhost
```

💻 **`debian-cliente`** — abre el navegador web y accede a:

```
http://192.168.1.10
```

Debe aparecer la página de bienvenida «Apache2 Debian Default Page».

> **C7.4** — Resultado de `curl http://localhost` en el servidor.
>
> **C7.5** — Página de bienvenida de Apache cargada en el navegador de `debian-cliente`.

---

## Parte 3. Crear la página principal personalizada

🖥️ **`debian-servidor`**

Haz una copia de seguridad:

```bash
sudo cp /var/www/html/index.html /var/www/html/index.html.copia
sudo nano /var/www/html/index.html
```

Contenido (sustituye «Nombre y apellidos» por los tuyos):

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
        <tr><td>Samba</td><td>139 / 445</td><td>Recursos compartidos (compatible con Windows)</td></tr>
        <tr><td>NFS</td><td>2049</td><td>Recursos compartidos entre sistemas Linux</td></tr>
        <tr><td>Apache</td><td>80</td><td>Servidor web HTTP</td></tr>
    </table>
    <p><a href="servicios.html">Ver detalles de la red</a></p>
</body>
</html>
```

💻 **`debian-cliente`** — recarga la página pulsando `Ctrl + F5` para evitar la caché.

> **C7.6** — Copia de seguridad de `index.html` con `ls -l` mostrando ambos archivos.
>
> **C7.7** — Contenido del nuevo `index.html` en el editor.
>
> **C7.8** — Página personalizada cargada en el navegador de `debian-cliente`, mostrando tu nombre.

---

## Parte 4. Crear una segunda página

🖥️ **`debian-servidor`**

```bash
sudo nano /var/www/html/servicios.html
```

Contenido (**sustituye las IPs de Windows por las reales de tu práctica**):

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Detalles de la red</title>
</head>
<body>
    <h1>Configuración de la red</h1>
    <h2>Equipos de la infraestructura</h2>
    <ul>
        <li>debian-servidor (Linux): 192.168.1.10</li>
        <li>debian-cliente (Linux): 192.168.1.11</li>
        <li>cliente1 Windows (Tarea 8): 192.168.1.20</li>
        <li>cliente2 Windows (Tarea 8): 192.168.1.21</li>
    </ul>
    <h2>Grupos y permisos en Samba</h2>
    <ul>
        <li>Carpeta <strong>publica</strong>: acceso de lectura para todos sin contraseña</li>
        <li>Carpeta <strong>administracion</strong>: lectura y escritura solo para el grupo administracion</li>
        <li>Carpeta <strong>ventas</strong>: escritura para ventas; solo lectura para administracion</li>
    </ul>
    <h2>Recursos NFS</h2>
    <ul>
        <li>/nfs/lectura: acceso de solo lectura para toda la red 192.168.1.0/24</li>
        <li>/nfs/escritura: acceso de lectura y escritura para 192.168.1.11</li>
    </ul>
    <p><a href="index.html">Volver al inicio</a></p>
</body>
</html>
```

💻 **`debian-cliente`** — accede a `http://192.168.1.10/servicios.html` y comprueba que el enlace desde `index.html` también funciona.

> **C7.9** — Contenido de `servicios.html` en el editor.
>
> **C7.10** — Página `servicios.html` cargada correctamente en el navegador del cliente.

---

## Parte 5. Acceso desde Windows

🪟 **Windows (`cliente1` o `cliente2`)** — abre el navegador y accede a:

```
http://192.168.1.10
```

> **C7.11** — Página personalizada cargada desde un navegador en una máquina Windows (si está disponible).

---

## Errores típicos del ejercicio 7

**El cliente sigue viendo la página de bienvenida de Apache**  
Pulsa `Ctrl + F5` para forzar la recarga sin caché.

**Error 403 Forbidden**  
Apache no tiene permisos para leer el archivo. Ejecuta:
```bash
sudo chmod 755 /var/www/html/
sudo chown -R www-data:www-data /var/www/html/
```

**El enlace a `servicios.html` no funciona**  
Comprueba que el nombre del archivo es exactamente `servicios.html` (minúsculas). Linux distingue mayúsculas y minúsculas.

---

# Ejercicio 8 — Diagnóstico de red con herramientas TCP/IP

## Tabla comparativa Linux / Windows

| Herramienta Linux | Equivalente Windows | Función |
|---|---|---|
| `ip a` | `ipconfig /all` | Ver interfaces y direcciones IP |
| `ip route` | `route print` | Ver tabla de rutas |
| `ping` | `ping` | Comprobar conectividad básica |
| `traceroute` | `tracert` | Ver los saltos hasta el destino |
| `ss -tlnp` | `netstat -an` | Ver puertos en escucha |
| `nslookup` | `nslookup` | Consultar resolución de nombres DNS |
| `journalctl` | Visor de eventos | Consultar registros del sistema |

---

## Parte 1. Configuración de red completa

🖥️ **`debian-servidor`**

```bash
ip a
ip route
```

`ip a` muestra todas las interfaces con sus IPs (equivalente a `ipconfig /all`). `ip route` muestra la tabla de rutas (equivalente a `route print`). Debes ver una ruta por defecto a través de la interfaz NAT y la red `192.168.1.0/24` a través de la interfaz interna.

> **C8.1** — Resultado de `ip a` en el servidor mostrando ambas interfaces con sus IPs.
>
> **C8.2** — Resultado de `ip route` en el servidor mostrando las rutas activas.

---

## Parte 2. Comprobación de conectividad por niveles

💻 **`debian-cliente`**

```bash
ping -c 4 127.0.0.1
ping -c 4 192.168.1.11
ping -c 4 192.168.1.10
ping -c 4 8.8.8.8
ping -c 4 google.com
ping -c 4 debian-servidor
```

El último ping comprueba que la resolución de nombres local mediante `/etc/hosts` funciona correctamente.

> **C8.3** — Resultado de todos los ping por niveles desde el cliente, todos correctos.

---

## Parte 3. `traceroute`

💻 **`debian-cliente`**

```bash
sudo apt install traceroute
traceroute 192.168.1.10
traceroute 8.8.8.8
```

El primer `traceroute` mostrará un único salto (misma red local). El segundo mostrará varios saltos: primero el servidor (router), después los routers de Internet.

> **C8.4** — Resultado de `traceroute 192.168.1.10` mostrando el único salto local.

---

## Parte 4. Puertos en escucha con `ss`

🖥️ **`debian-servidor`**

```bash
ss -tlnp
```

Identifica los puertos de los servicios instalados:

| Puerto | Servicio | Instalado en |
|---|---|---|
| 22 | SSH | Ejercicio 6 |
| 80 | Apache | Ejercicio 7 |
| 139 | Samba (NetBIOS) | Ejercicio 3 |
| 445 | Samba (SMB directo) | Ejercicio 3 |
| 2049 | NFS | Ejercicio 5 |

> **C8.5** — Resultado completo de `ss -tlnp` con todos los servicios activos visibles.
>
> **C8.6** — Identificación escrita de al menos dos puertos: servicio al que pertenecen y por qué están abiertos.

---

## Parte 5. Consulta DNS con `nslookup`

💻 **`debian-cliente`**

`nslookup` pertenece al paquete `dnsutils`, que no viene instalado en sistemas mínimos. Instálalo si es necesario:

```bash
sudo apt install dnsutils
```

```bash
nslookup google.com
nslookup debian-servidor
```

La consulta a `google.com` debe devolver una dirección IP. La consulta a `debian-servidor` devolverá un error: es **completamente normal**. `nslookup` consulta los servidores DNS externos y no tiene acceso a `/etc/hosts`.

> **C8.7** — Resultado de ambas consultas `nslookup`, con la explicación de por qué el error en `debian-servidor` es normal.

---

## Parte 6. Registros del sistema con `journalctl`

🖥️ **`debian-servidor`**

```bash
sudo journalctl -u ssh -n 20
sudo journalctl -u apache2 -n 20
sudo journalctl -u smbd -n 20
```

- `-u nombre` → filtra los registros del servicio indicado.
- `-n 20` → muestra las últimas 20 líneas.

Localiza en SSH los eventos de las conexiones del Ejercicio 6. En Apache, busca las peticiones GET de las páginas del Ejercicio 7.

> **C8.8** — Resultado de `journalctl -u ssh -n 20` con eventos de conexión visibles.
>
> **C8.9** — Resultado de `journalctl -u apache2 -n 20` con peticiones web visibles.

---

## Parte 7. Explicación comparativa final

Escribe en el PDF una comparativa entre las herramientas de Windows de la Tarea 8 y sus equivalentes en Linux. Para cada par indica qué información obtienes y cuándo fue útil en esta práctica:

- **`ipconfig /all` / `ip a`:** qué muestra cada uno y diferencias en el formato.
- **`ping`:** mismo nombre en ambos sistemas, ¿alguna diferencia de comportamiento?
- **`tracert` / `traceroute`:** resultado con un único salto cuando el destino está en la misma red.
- **`netstat -an` / `ss -tlnp`:** puertos en Windows (21, 80 de IIS) frente a Linux (22, 80, 139, 445, 2049).
- **Visor de eventos / `journalctl`:** qué tipo de información ofrece cada uno y cuál te parece más cómodo.

Esta parte no requiere capturas adicionales. Solo texto en el PDF.

---

# Conclusión final de la práctica

Al terminar esta práctica habrás construido una infraestructura cliente-servidor Linux completamente funcional integrada con los equipos Windows de la Tarea 8 en la misma red interna.

Los bloques que has trabajado son:

- Instalación de Debian en modo consola en VirtualBox, sin entorno gráfico.
- Red local en el rango `192.168.1.X` con dos adaptadores y enrutamiento persistente gestionado con systemd.
- Usuarios y grupos de Linux alineados con los de Windows para autenticación transparente en Samba.
- Samba con tres recursos compartidos y permisos equivalentes a los de la Tarea 8.
- NFS para compartición nativa entre sistemas Linux sin autenticación por usuario.
- SSH como alternativa segura al acceso remoto y herramienta de transferencia de archivos.
- Apache como servidor web equivalente al IIS de Windows.
- Diagnóstico de red con las herramientas Linux equivalentes a las de Windows.

La diferencia más importante entre Windows y Linux en administración de redes no es qué se puede hacer (las capacidades son equivalentes), sino cómo se hace: en Linux todo se configura mediante archivos de texto editables desde la terminal, lo que lo hace más predecible, automatizable y reproducible. Un administrador que entiende los archivos de configuración puede reproducir un servidor completo desde cero con un conjunto de comandos; en Windows eso requeriría navegar por decenas de ventanas de configuración.
