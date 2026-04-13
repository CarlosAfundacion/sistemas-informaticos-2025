# PRÁCTICA

# ADMINISTRACIÓN DE REDES EN WINDOWS

---

# Introducción

En esta práctica vas a trabajar los bloques fundamentales de la administración básica de redes en Windows dentro de un entorno de máquina virtual. A lo largo de los ejercicios aprenderás a configurar dos equipos en red, reutilizar usuarios y grupos locales, compartir carpetas, aplicar permisos, acceder a recursos mediante rutas UNC, configurar servicios FTP y web con IIS y utilizar herramientas básicas de diagnóstico de red.

El objetivo no es solo que sigas una serie de pasos, sino que comprendas:

* qué hace cada herramienta o comando,
* en qué situaciones se utiliza,
* qué información devuelve,
* por qué una configuración funciona o falla,
* y cómo comprobar que el resultado es correcto.

Esta práctica está pensada para realizarse en **Windows 10** dentro de **VirtualBox**, utilizando:

* la interfaz gráfica de Windows,
* las herramientas administrativas del sistema,
* y la consola **CMD** cuando se indique.

A lo largo de la práctica trabajarás con una pequeña red simulada de empresa formada por dos equipos Windows.

---

## Qué vas a aprender en esta práctica

Al finalizar, deberías ser capaz de:

* configurar dos equipos Windows dentro de una misma red interna,
* entender la diferencia entre una red NAT y una red interna en VirtualBox,
* asignar direcciones IP estáticas,
* comprobar conectividad entre equipos,
* reutilizar usuarios y grupos locales para controlar accesos,
* compartir carpetas en red,
* diferenciar permisos de compartición y permisos NTFS,
* acceder a recursos compartidos con rutas UNC,
* comprender qué es un recurso oculto,
* gestionar credenciales de red en Windows 10,
* instalar y configurar un servidor FTP con IIS,
* instalar y configurar un sitio web con IIS,
* y realizar comprobaciones básicas de red.

---

## Entrega

La práctica deberá entregarse en un único documento PDF que incluya:

* capturas de pantalla claras y completas,
* los comandos utilizados cuando se soliciten,
* explicaciones breves y técnicas cuando se indiquen,
* y debajo de cada captura una línea con el texto:

**Qué demuestra esta captura**

No se aceptarán capturas recortadas que oculten información importante ni un documento desordenado.

---

## Recomendaciones antes de empezar

1. Sigue el orden de los ejercicios.
2. No continúes si un paso anterior no funciona correctamente.
3. Cuando algo falle, intenta identificar si el problema es de:

   * red,
   * usuarios o permisos,
   * firewall,
   * o servicio mal configurado.
4. En esta práctica son tan importantes las configuraciones como las comprobaciones.
5. Guarda el estado de las máquinas virtuales con frecuencia usando instantáneas de VirtualBox, especialmente antes de cada ejercicio nuevo.

---

## Estimación de tiempo

Esta práctica está diseñada para completarse en **7 sesiones de aproximadamente 100 minutos**. La distribución orientativa es la siguiente:

* Ejercicio 1: 2 sesiones
* Ejercicios 2 y 3: 2 sesiones
* Ejercicios 4 y 5: 1 sesión
* Ejercicio 6: 1 sesión
* Ejercicios 7 y 8: 1 sesión

Los tiempos pueden variar según los problemas que aparezcan. Eso es normal y forma parte del aprendizaje.

---

# Conceptos previos necesarios

---

## 1. Qué es una dirección IP

Una dirección IP identifica a un equipo dentro de una red. Si dos equipos están en la misma red y tienen una configuración correcta, podrán comunicarse entre sí.

Ejemplo:

```
192.168.1.10
```

---

## 2. Qué es la máscara de subred

La máscara indica qué parte de la IP corresponde a la red y qué parte al equipo.

En esta práctica usarás:

```
255.255.255.0
```

De forma sencilla, eso significa que los equipos cuya dirección empiece por `192.168.1.` pertenecerán a la misma red local.

---

## 3. Qué es DHCP y por qué aquí no se usa

DHCP significa **Dynamic Host Configuration Protocol**. Es un servicio que asigna automáticamente una dirección IP a cada equipo cuando se conecta a la red. En redes domésticas y empresariales reales, el router suele hacer esta función.

En esta práctica usarás una **red interna de VirtualBox**, que es una red aislada sin router ni servidor DHCP. Eso significa que si dejas la configuración en automático, el equipo se quedará sin IP válida o se asignará una dirección que empieza por `169.254.x.x`, con la que no podrá comunicarse con el otro equipo.

Por eso asignarás las IPs manualmente (IPs estáticas), y dejarás en blanco los campos de **puerta de enlace** y **DNS**:

* La **puerta de enlace** es la dirección del router que permite salir a Internet. Como no hay router ni Internet en esta red aislada, no se necesita.
* El **DNS** es el servicio que traduce nombres como `google.com` a direcciones IP. Como no se va a usar Internet, tampoco se necesita.

---

## 4. Qué es una red NAT y qué es una red interna en VirtualBox

### NAT

Permite a una máquina virtual salir a Internet utilizando la red del equipo anfitrión. Es útil para navegar o descargar software, pero no permite que las máquinas virtuales se comuniquen directamente entre sí de forma sencilla.

### Red interna

Permite que varias máquinas virtuales se comuniquen entre sí dentro de una red privada aislada. No tienen acceso a Internet, pero sí pueden verse entre ellas. Es la opción adecuada para simular una red local de empresa.

---

## 5. Qué es un grupo de trabajo

Un grupo de trabajo permite que varios equipos Windows formen parte de un mismo entorno de red local. No es un dominio ni centraliza usuarios, pero facilita la identificación de equipos dentro de una red pequeña.

En esta práctica los dos equipos pertenecerán al grupo de trabajo:

```
EMPRESA
```

---

## 6. Qué es una ruta UNC

Una ruta UNC es la forma estándar de acceder a recursos compartidos en Windows.

Ejemplo:

```
\\cliente1\ventas
```

Esto significa:

* `cliente1`: el nombre del equipo remoto,
* `ventas`: el nombre del recurso compartido.

---

## 7. Qué diferencia hay entre permisos de compartición y permisos NTFS

### Permisos de compartición

Se aplican cuando una carpeta se accede a través de la red. Se configuran en la pestaña **Compartir** de las propiedades de la carpeta.

### Permisos NTFS

Se aplican sobre el sistema de archivos local del disco. Se configuran en la pestaña **Seguridad**. Afectan tanto al acceso local como al acceso en red.

### Regla importante

Cuando una carpeta se accede por red, se aplican **ambos** permisos a la vez. El acceso final será el resultado del permiso **más restrictivo** entre los dos.

Por ejemplo:

* si la compartición permite escribir,
* pero NTFS solo permite leer,
* el usuario solo podrá leer.

En esta práctica configurarás los permisos NTFS con precisión y dejarás los permisos de compartición con acceso amplio, de modo que sea NTFS quien controle realmente quién puede hacer qué.

---

## 8. Cómo funciona la autenticación al acceder a recursos en red

Este punto es importante y causa muchos errores si no se entiende.

Cuando accedes desde `cliente2` a una carpeta compartida en `cliente1`, Windows necesita saber quién eres para decidir si tienes permiso. Lo hace autenticándote en `cliente1`.

La clave está en que **los usuarios que importan son los de `cliente1`**, no los de `cliente2`. Da igual con qué usuario hayas iniciado sesión en `cliente2`; lo que decide el acceso son las cuentas que existen en `cliente1` y los permisos que tienen en sus carpetas compartidas.

Lo que ocurre concretamente:

* Windows intentará autenticarte en `cliente1` usando el mismo nombre y contraseña con los que estás en `cliente2`.
* Si en `cliente1` existe una cuenta con ese mismo nombre y contraseña, la autenticación es automática y transparente.
* Si no existe, o si la contraseña no coincide, Windows mostrará una ventana pidiendo credenciales. En ese momento debes introducir el nombre y la contraseña de una cuenta que **sí exista en `cliente1`**.

**Conclusión práctica:** Los usuarios con los que vayas a probar los accesos deben existir en `cliente1`. Si solo los creaste en `cliente2`, el acceso será denegado.

---

## 9. Qué es SMB

SMB significa **Server Message Block**. Es el protocolo que usa Windows por debajo para compartir carpetas e impresoras en red. Cuando accedes a una ruta UNC como `\\cliente1\ventas`, Windows usa SMB para establecer esa conexión. No necesitas configurarlo manualmente, pero es útil saber que existe porque puede estar bloqueado por el firewall.

---

## 10. Qué es IIS

IIS significa **Internet Information Services**. Es el conjunto de servicios de red de Microsoft para publicar contenido como páginas web, FTP y otros servicios. No viene instalado por defecto en Windows 10 y deberás activarlo desde las características del sistema operativo.

---

## 11. Qué hace el firewall de Windows

El firewall controla las conexiones entrantes y salientes del equipo. Aunque una red esté bien configurada, el firewall puede impedir que un equipo responda al ping, que se acceda a una carpeta compartida, que funcione el FTP, o que se cargue una página web. Por eso, en algunos apartados será necesario crear reglas que permitan determinadas conexiones.

---

## 12. Cómo navegar por la configuración en Windows 10

Windows 10 tiene dos interfaces de configuración que pueden confundir:

* **Panel de control:** la interfaz clásica, más completa para administración de red y usuarios.
* **Configuración:** la interfaz moderna accesible desde el engranaje del menú inicio.

En esta práctica se usará principalmente el **Panel de control**. La forma más directa de abrirlo es pulsar `Win + R`, escribir `control` y pulsar Enter.

Muchas herramientas también se pueden abrir desde `Win + R` con su nombre de ejecutable, y en esta práctica se indicará cuándo sea posible.

---

## 13. Qué es el Visor de eventos

El Visor de eventos (`eventvwr.msc`) registra todo lo que ocurre en el sistema: errores, advertencias, conexiones, fallos de autenticación, etc. Cuando algo no funciona y no sabes por qué, el Visor de eventos suele darte una pista. Los registros más relevantes para esta práctica están en **Registros de Windows → Seguridad** y **Registros de Windows → Sistema**.

---

# Ejercicio 1 — Configuración de red de dos equipos Windows

## Descripción general

En este ejercicio vas a preparar dos equipos Windows para que funcionen dentro de una misma red interna. Este paso es imprescindible porque el resto de la práctica depende de que ambos equipos puedan verse entre sí.

---

## Qué se pide

* Máquina 1 — nombre: `cliente1`, grupo de trabajo: `EMPRESA`, IP: `192.168.1.10`
* Máquina 2 — nombre: `cliente2`, grupo de trabajo: `EMPRESA`, IP: `192.168.1.11`
* Máscara de subred en ambos: `255.255.255.0`
* Sin puerta de enlace ni DNS (ver concepto previo 3).

---

## Parte 1. Clonar la máquina

### Por qué es importante reinicializar la MAC

Cada tarjeta de red tiene una dirección MAC única. Si las dos máquinas tienen la misma MAC, el switch virtual no puede distinguirlas y la red no funcionará. Al clonar, VirtualBox ofrece la opción de generar una MAC nueva para el clon; debe estar marcada.

### Pasos

1. Apaga la máquina Windows base.
2. En VirtualBox, clic derecho sobre la máquina → **Clonar**.
3. Marca la opción **Reinicializar la dirección MAC de todas las tarjetas de red**.
4. Finaliza el clonado.

### Qué debes comprobar

Verifica las MACs en VirtualBox: selecciona cada máquina → **Configuración** → **Red** → despliega **Avanzado**. Deben ser distintas.

### Capturas obligatorias

**C1.1** Proceso de clonación con la opción de reinicializar la MAC marcada.
**C1.2** Configuración de red de ambas máquinas donde se aprecian MACs distintas.

---

## Parte 2. Cambiar el nombre de los equipos y el grupo de trabajo

### Pasos

1. Inicia la máquina original. Pulsa `Win + R` → escribe `control` → Enter.
2. Ve a **Sistema y seguridad** → **Sistema** → **Cambiar configuración**.
3. Haz clic en **Cambiar...**.
4. Escribe `cliente1` en **Nombre de equipo**, selecciona **Grupo de trabajo** y escribe `EMPRESA`.
5. Acepta y reinicia cuando Windows lo solicite.
6. Repite en la máquina clonada con el nombre `cliente2` y el mismo grupo `EMPRESA`.

### Capturas obligatorias

**C1.3** Nombre del equipo y grupo de trabajo en `cliente1`.
**C1.4** Nombre del equipo y grupo de trabajo en `cliente2`.

---

## Parte 3. Comprobar la red por defecto (NAT)

### Pasos

En CMD de cada equipo:

```cmd
ipconfig
ping google.com
```

### Qué debes anotar

La IP actual (probablemente `10.0.2.x` con NAT) y que hay acceso a Internet. Este es el comportamiento esperado con NAT.

### Capturas obligatorias

**C1.5** `ipconfig` y `ping google.com` en `cliente1`.
**C1.6** `ipconfig` y `ping google.com` en `cliente2`.

---

## Parte 4. Cambiar a red interna

### Pasos

1. Apaga ambas máquinas.
2. En VirtualBox, selecciona `cliente1` → **Configuración** → **Red**.
3. Cambia **Conectado a:** de NAT a **Red interna**.
4. En **Nombre**, escribe exactamente: `intranet`
5. Repite en `cliente2` con exactamente el mismo nombre `intranet`.

> **Importante:** El nombre debe ser idéntico en ambas máquinas, incluyendo mayúsculas y minúsculas. `intranet` y `Intranet` son redes distintas.

### Capturas obligatorias

**C1.7** Adaptador en red interna de `cliente1`.
**C1.8** Adaptador en red interna de `cliente2`.

---

## Parte 5. Comprobar que no hay Internet

### Pasos

Enciende ambas máquinas y ejecuta en CMD:

```cmd
ipconfig
ping google.com
```

### Qué debes comprobar

No hay acceso a Internet. La IP puede aparecer como no configurada o con `169.254.x.x` (autoconfiguración sin DHCP). Esto es lo esperado; lo solucionarás en la siguiente parte.

### Capturas obligatorias

**C1.9** `ipconfig` y `ping google.com` tras cambiar a red interna.

---

## Parte 6. Asignar IP estática

### Pasos

1. Pulsa `Win + R` → escribe `ncpa.cpl` → Enter (abre directamente Conexiones de red).
2. Clic derecho sobre el adaptador → **Propiedades**.
3. Selecciona **Protocolo de Internet versión 4 (TCP/IPv4)** → **Propiedades**.
4. Selecciona **Usar la siguiente dirección IP** y rellena:

**En `cliente1`:** IP `192.168.1.10`, máscara `255.255.255.0`, puerta de enlace y DNS en blanco.

**En `cliente2`:** IP `192.168.1.11`, máscara `255.255.255.0`, puerta de enlace y DNS en blanco.

5. Acepta. Ejecuta `ipconfig` en ambos equipos para verificar.

### Capturas obligatorias

**C1.10** Configuración IPv4 estática en `cliente1`.
**C1.11** Configuración IPv4 estática en `cliente2`.
**C1.12** Resultado de `ipconfig` en ambos equipos.

---

## Parte 7. Probar conectividad entre máquinas

### Pasos

Desde `cliente1`:

```cmd
ping 192.168.1.11
```

Desde `cliente2`:

```cmd
ping 192.168.1.10
```

### Qué debes comprobar

Probablemente falle con **tiempo de espera agotado**. No significa que la red esté mal; el firewall está bloqueando el ICMP. Lo solucionarás en la siguiente parte.

### Capturas obligatorias

**C1.13** Resultado del ping inicial (aunque falle, captura el resultado).

---

## Parte 8. Crear una regla de firewall para permitir el ping

### Por qué es necesario

El firewall bloquea por defecto las peticiones de eco ICMP entrantes. Hay que crear una regla que las permita explícitamente en **ambos equipos**.

### Pasos

1. `Win + R` → `wf.msc` → Enter.
2. Panel izquierdo → **Reglas de entrada** → Panel derecho → **Nueva regla...**
3. Selecciona **Personalizada** → Siguiente.
4. **Todos los programas** → Siguiente.
5. **Tipo de protocolo: ICMPv4** → clic en **Personalizar...** → marca **Petición en eco** → Acepta.
6. Siguiente en las pantallas de ámbito (valores por defecto).
7. **Permitir la conexión** → Siguiente.
8. Marca los tres perfiles: Dominio, Privada, Pública.
9. Nombre: `Permitir ping` → Finalizar.
10. **Repite exactamente el mismo proceso en el otro equipo.**
11. Vuelve a hacer los pings.

### Si sigue fallando después de crear la regla

* Comprueba que la regla aparece habilitada en la lista (icono con marca verde).
* Verifica con `ipconfig` que las IPs siguen siendo correctas.
* Asegúrate de que el nombre de la red interna es exactamente el mismo en ambas máquinas.

### Capturas obligatorias

**C1.14** Regla `Permitir ping` visible en la lista de reglas de entrada.
**C1.15** Ping correcto desde `cliente1` a `cliente2`.
**C1.16** Ping correcto desde `cliente2` a `cliente1`.

---

# Ejercicio 2 — Verificación de usuarios y grupos locales

## Descripción general

En una práctica anterior ya creaste usuarios y grupos locales. Aquí vas a verificar que esa base existe y está correctamente configurada, porque los ejercicios de compartición y permisos dependen directamente de ella.

---

## Herramienta

`Win + R` → `lusrmgr.msc` → Enter. Abre la consola de usuarios y grupos locales.

---

## Teoría necesaria

### Qué es un usuario local

Una cuenta creada en un equipo concreto. Pertenece a esa máquina y no existe automáticamente en otras. En esta práctica, los usuarios que importan para el acceso a los recursos compartidos son los que existen en `cliente1`.

### Qué es un grupo local

Una agrupación de usuarios para facilitar la administración de permisos. En lugar de asignar permisos usuario por usuario, se asignan al grupo completo.

### Por qué los usuarios no deben ser administradores

Un administrador siempre puede acceder a todo en Windows. Si usas cuentas de administrador para probar los permisos, nunca verás un acceso denegado aunque la configuración esté mal.

---

## Parte 1. Comprobar que existen los grupos

Verifica que existen `administracion` y `ventas` en `cliente1`.

**C2.1** Ventana de grupos mostrando `administracion` y `ventas`.

---

## Parte 2. Comprobar que existen usuarios suficientes

Al menos dos usuarios en `administracion` y dos en `ventas`.

**C2.2** Ventana de usuarios mostrando las cuentas existentes.

---

## Parte 3. Verificar la pertenencia a grupos

**Desde el grupo:** doble clic sobre el grupo → ver lista de miembros.
**Desde el usuario:** doble clic sobre el usuario → pestaña **Miembro de**.

**C2.3** Miembros del grupo `administracion`.
**C2.4** Miembros del grupo `ventas`.

---

## Parte 4. Comprobar que no son administradores

Abre las propiedades de uno de los usuarios → pestaña **Miembro de**. Solo debe aparecer su grupo de departamento, no `Administradores`.

**C2.5** Propiedades de uno de los usuarios mostrando que solo pertenece a su grupo de departamento.

---

## Parte 5. Completar solo si falta algo

Si algún grupo o usuario no existe, créalo. Si todo está preparado, no es necesario rehacerlo.

**C2.6** Solo si es necesario, captura de la creación o corrección realizada.

---

# Ejercicio 3 — Compartición de recursos y permisos NTFS

## Descripción general

Vas a crear una estructura de carpetas que representa dos departamentos de empresa, aplicar permisos NTFS y compartir las carpetas en red.

---

## Teoría necesaria

### Qué es NTFS

Es el sistema de archivos de Windows. Permite asignar permisos detallados sobre carpetas y archivos, tanto para acceso local como en red.

### Qué significa "Modificar"

Permite leer, escribir, crear, cambiar y borrar archivos. Es el permiso habitual para quien trabaja activamente con una carpeta.

### Qué significa "Lectura y ejecución"

Permite abrir archivos y acceder a la carpeta, pero no modificar ni crear archivos nuevos.

### Qué es la herencia de permisos y qué ocurre al desactivarla

Las carpetas heredan permisos de las carpetas superiores. Para controlar con precisión quién entra en una carpeta concreta, hay que romper esa cadena.

Al desactivar la herencia, Windows pregunta qué hacer con los permisos actuales:

* **Convertir los permisos heredados en permisos explícitos:** mantiene los permisos actuales pero los hace independientes. Usa siempre esta opción; después eliminarás los que no interesen.
* **Quitar todos los permisos heredados:** elimina todo. No uses esta opción en esta práctica.

---

## Parte 1. Crear la estructura de carpetas

En `cliente1`, crea:

```
C:\Empresa
 ├── Administracion
 └── Ventas
```

Dentro de cada carpeta añade un archivo `.txt` y una imagen (puede ser cualquier imagen de `C:\Windows\Web\Wallpaper`).

**C3.1** Estructura de carpetas en `C:\Empresa`.
**C3.2** Contenido de `Administracion`.
**C3.3** Contenido de `Ventas`.

---

## Parte 2. Configurar permisos NTFS en Administracion

### Qué se pide

* `administracion` → **Modificar**
* `ventas` → sin acceso

### Pasos detallados

#### 1. Abrir opciones avanzadas de seguridad

Clic derecho sobre `C:\Empresa\Administracion` → **Propiedades** → pestaña **Seguridad** → **Opciones avanzadas**.

#### 2. Desactivar la herencia

Haz clic en **Deshabilitar herencia**. En el cuadro que aparece, selecciona **Convertir los permisos heredados en permisos explícitos en este objeto**.

#### 3. Limpiar permisos innecesarios

En la lista de entradas, selecciona **Usuarios** (si existe) y haz clic en **Quitar**. Repite con cualquier otra entrada que no sea `SYSTEM` ni `Administradores`. Estos dos deben conservarse siempre. Acepta la ventana de opciones avanzadas.

#### 4. Añadir el grupo administracion

De vuelta en la pestaña Seguridad, haz clic en **Editar...** → **Agregar...**. Escribe `administracion` y haz clic en **Comprobar nombres**.

> **Si Windows dice que no encuentra el nombre:** haz clic en **Ubicaciones...** y selecciona el nombre del equipo local (`cliente1`) en la lista, en lugar del dominio que aparece por defecto. Después vuelve a hacer clic en **Comprobar nombres**.

El nombre debe aparecer subrayado. Acepta. Marca **Modificar** en la columna Permitir. Acepta todos los cuadros.

#### 5. Verificar que ventas no tiene acceso

El grupo `ventas` no debe aparecer en la lista. Si aparece, selecciónalo y haz clic en **Quitar**.

### Si el botón Quitar está desactivado para alguna entrada

Significa que esa entrada todavía está como heredada. Vuelve a **Opciones avanzadas** y asegúrate de haber desactivado la herencia correctamente en el paso 2.

### Capturas obligatorias

**C3.4** Pestaña Seguridad de `Administracion` con los permisos finales.
**C3.5** Opciones avanzadas mostrando la herencia desactivada.
**C3.6** Permiso de `administracion` con Modificar marcado.

---

## Parte 3. Configurar permisos NTFS en Ventas

### Qué se pide

* `ventas` → **Modificar**
* `administracion` → **Lectura y ejecución**

### Pasos

Sigue los mismos pasos de la parte anterior para desactivar la herencia y limpiar permisos. Después:

1. Añade `ventas` con **Modificar** (usa **Ubicaciones...** si no lo encuentra).
2. Añade `administracion` con **Lectura y ejecución**.

   > Al marcar Lectura y ejecución, Windows también marcará automáticamente **Mostrar el contenido de la carpeta** y **Lectura**. Esto es correcto. Asegúrate de que **Modificar** queda desmarcado.

### Capturas obligatorias

**C3.7** Pestaña Seguridad de `Ventas` con los permisos finales.
**C3.8** Detalle del permiso de `ventas` (Modificar marcado).
**C3.9** Detalle del permiso de `administracion` (solo Lectura y ejecución).

---

## Parte 4. Compartir ambas carpetas por red

### Qué se pide

* `Administracion` → nombre de recurso `admin`
* `Ventas` → nombre de recurso `ventas`

### Pasos

1. Clic derecho sobre `C:\Empresa\Administracion` → **Propiedades** → pestaña **Compartir** → **Uso compartido avanzado...**.
2. Marca **Compartir esta carpeta**.
3. Nombre del recurso: `admin`.
4. Haz clic en **Permisos** y cambia el permiso de **Todos** a **Control total** en la columna Permitir.

   > Esto puede parecer permisivo, pero los permisos reales de acceso ya los controla NTFS. Si dejas solo Lectura aquí, ningún usuario podrá escribir aunque NTFS se lo permita, porque el permiso más restrictivo gana.

5. Acepta todos los cuadros. Repite el proceso en `Ventas` con el nombre `ventas` y también Control total.

### Capturas obligatorias

**C3.10** Recurso compartido `admin` configurado (mostrando nombre y ruta de red).
**C3.11** Recurso compartido `ventas` configurado.

---

# Ejercicio 4 — Acceso a recursos compartidos desde otro equipo

## Descripción general

Vas a comprobar desde `cliente2` si la configuración de `cliente1` funciona con usuarios reales.

---

## Teoría necesaria

### Cómo funciona la autenticación (repaso)

Recuerda el concepto previo 8: los usuarios que importan son los de `cliente1`. Cuando accedas desde `cliente2`, Windows intentará autenticarte en `cliente1` con las credenciales que tengas activas. Si no existen en `cliente1`, te pedirá otras.

### Qué es el Administrador de credenciales y por qué importa

Windows guarda en caché las credenciales que usas para conectarte a equipos remotos. Si te has conectado a `\\cliente1` con `admin1`, Windows usará esas credenciales automáticamente las próximas veces sin preguntarte. Para probar con distintos usuarios, debes borrar esas credenciales guardadas antes de cada cambio.

### Cómo limpiar las credenciales almacenadas

**Método rápido desde CMD:**

```cmd
net use * /delete
```

Escribe `S` y pulsa Enter si pide confirmación. Este comando cierra todas las conexiones de red de la sesión actual.

**Método alternativo — Administrador de credenciales:**

1. `Win + R` → `control` → Enter.
2. **Cuentas de usuario** → **Administrador de credenciales** → **Credenciales de Windows**.
3. Busca entradas que contengan `cliente1` → haz clic en cada una → **Quitar**.

Después de limpiar, la próxima vez que accedas a `\\cliente1`, Windows pedirá las credenciales de nuevo.

---

## Parte 1. Acceder a los recursos compartidos de cliente1

### Pasos

1. En `cliente2`, pulsa `Win + R` y escribe `\\cliente1`.
2. Si pide credenciales, introduce las de un usuario que exista en `cliente1`.

### Qué debes comprobar

Deben aparecer los recursos `admin` y `ventas`.

### Si no aparece nada o hay error de acceso

* Comprueba que el ping entre equipos sigue funcionando.
* Verifica que las carpetas están compartidas en `cliente1`.
* Es posible que el firewall de `cliente1` esté bloqueando SMB. En el firewall avanzado de `cliente1`, busca la regla **Compartir archivos e impresoras (SMB de entrada)** y asegúrate de que está habilitada.

### Capturas obligatorias

**C4.1** Vista de los recursos compartidos al acceder a `\\cliente1`.

---

## Parte 2. Probar acceso con un usuario de administración

### Qué se pide

Verificar que un usuario del grupo `administracion`:

* puede trabajar en `admin`,
* puede leer en `ventas`,
* no puede modificar `ventas`.

### Pasos

1. Accede a `\\cliente1` con credenciales de `admin1`.
2. Entra en `admin` y crea un archivo nuevo (clic derecho → Nuevo → Documento de texto).
3. Entra en `ventas` y abre el archivo de texto.
4. Intenta modificar ese archivo o borrarlo.

### Qué debes comprobar

* Crear en `admin`: correcto.
* Leer en `ventas`: correcto.
* Modificar o borrar en `ventas`: error de acceso denegado.

### Si puedes modificar en ventas con admin1 y no debería ser posible

Revisa los permisos NTFS de `Ventas` en `cliente1`. El grupo `administracion` solo debe tener Lectura y ejecución. Si tiene Modificar, corrígelo.

### Capturas obligatorias

**C4.2** Archivo creado en `admin` por `admin1`.
**C4.3** Archivo de `ventas` abierto correctamente.
**C4.4** Mensaje de error al intentar modificar o borrar en `ventas`.

---

## Parte 3. Probar acceso con un usuario de ventas

### Qué se pide

Verificar que un usuario del grupo `ventas`:

* puede trabajar en `ventas`,
* no puede entrar en `admin`.

### Pasos

1. Limpia las credenciales:

```cmd
net use * /delete
```

Cierra también cualquier ventana del explorador apuntando a `\\cliente1`.

2. Vuelve a acceder a `\\cliente1` introduciendo credenciales de `ventas1`.
3. Entra en `ventas` y crea o modifica un archivo.
4. Intenta entrar en `admin`.

### Qué debes comprobar

* `ventas1` puede trabajar en `ventas`: correcto.
* Al intentar entrar en `admin`: mensaje de acceso denegado.

### Capturas obligatorias

**C4.5** Acceso correcto de `ventas1` a la carpeta `ventas`.
**C4.6** Archivo creado o modificado en `ventas` por `ventas1`.
**C4.7** Mensaje de acceso denegado al intentar entrar en `admin`.

---

# Ejercicio 5 — Recursos compartidos ocultos y recursos administrativos

## Teoría necesaria

### Qué es un recurso oculto

Un recurso compartido cuyo nombre termina en `$`. No aparece en el listado cuando alguien explora `\\nombre_equipo`, pero sí es accesible escribiendo la ruta completa si se conoce el nombre exacto.

### Qué es `C$`

Un recurso administrativo oculto que Windows crea automáticamente y que representa la unidad C. Su acceso está restringido a cuentas con privilegios administrativos y en Windows 10 puede estar bloqueado incluso para administradores locales por política de seguridad.

---

## Parte 1. Crear un recurso oculto

### Pasos

1. Crea la carpeta `C:\Privado` en `cliente1` con algún archivo dentro.
2. Clic derecho → **Propiedades** → **Compartir** → **Uso compartido avanzado...**
3. Marca **Compartir esta carpeta** y escribe el nombre: `Privado$`
4. Acepta.

**C5.1** Carpeta `Privado` creada con archivo dentro.
**C5.2** Recurso compartido `Privado$` configurado.

---

## Parte 2. Acceder al recurso oculto

### Pasos

1. Desde `cliente2`, abre `\\cliente1` y observa si `Privado$` aparece en el listado.
2. Abre Run y escribe la ruta directa: `\\cliente1\Privado$`

### Qué debes demostrar

* El recurso no aparece en el listado.
* Sí es accesible por ruta directa.

**C5.3** Vista de `\\cliente1` donde `Privado$` no aparece en la lista.
**C5.4** Acceso correcto al contenido de `\\cliente1\Privado$`.

---

## Parte 3. Probar acceso a `\\cliente1\C$`

### Pasos

1. Desde `cliente2`, escribe en Run: `\\cliente1\C$`
2. Observa el resultado. Anota el mensaje exacto que aparece.

### Qué debes explicar en el PDF

* Si se ha permitido o no el acceso.
* Con qué usuario lo intentaste.
* Por qué crees que ocurre ese resultado.

En la mayoría de los casos el acceso es denegado. En Windows 10 existe una política de seguridad que bloquea el acceso a recursos administrativos remotos incluso para cuentas de administrador local, precisamente para evitar que alguien con una cuenta de administrador pueda acceder al disco completo de otro equipo simplemente compartiendo la misma contraseña.

**C5.5** Resultado del intento de acceso a `\\cliente1\C$`.
**C5.6** Explicación escrita del resultado.

---

# Ejercicio 6 — Servicio FTP en IIS

## Teoría necesaria

### Qué es FTP

**File Transfer Protocol.** Permite transferir archivos entre un cliente y un servidor. Es un protocolo no cifrado (las credenciales viajan en texto plano), pero útil para prácticas de aprendizaje.

### Puerto

Por defecto el puerto **21** para el canal de control (comandos). La transferencia de datos usa un canal separado.

### Modo activo y modo pasivo

* **Modo activo:** el servidor intenta conectarse al cliente para transferir datos. Suele fallar cuando hay firewall.
* **Modo pasivo:** el cliente abre ambas conexiones. Es mucho más compatible con firewalls.

El cliente FTP de CMD usa modo activo por defecto. Si al ejecutar `ls` o `put` recibes un error, escribe `passive` dentro de la sesión y vuelve a intentarlo.

### Por qué puede fallar FTP

* Firewall bloqueando el puerto 21.
* Cliente FTP no instalado en `cliente2`.
* El usuario de FTP no existe como cuenta local en `cliente1`.
* La carpeta del sitio sin permisos para `IIS_IUSRS`.
* Modo activo bloqueado (solución: usar `passive`).

---

## Parte 0. Verificar que el cliente FTP está disponible en cliente2

### Pasos

1. En `cliente2`, abre CMD y escribe `ftp`.
2. **Si aparece `ftp>`**: el cliente está disponible. Escribe `quit` para salir.
3. **Si el comando no se reconoce:**

   * `Win + R` → `control` → **Programas** → **Activar o desactivar características de Windows**.
   * Busca **Cliente FTP** y márcalo. Acepta. Reinicia si es necesario.

**C6.0** Resultado de ejecutar `ftp` en CMD de `cliente2`.

---

## Parte 1. Instalar IIS con soporte FTP en cliente1

### Pasos

1. `Win + R` → `control` → **Programas** → **Activar o desactivar características de Windows**.
2. Despliega **Servicios de Internet Information Services** y marca:

   * Dentro de **Servidor FTP**: **Extensibilidad de FTP** y **Servicio FTP**.
   * **Herramientas de administración web**.
   * Dentro de **Servicio World Wide Web**: **Características HTTP comunes**.
3. Acepta y espera a que termine.

**C6.1** Ventana de características con IIS y FTP seleccionados.

---

## Parte 2. Preparar la carpeta del sitio FTP

### Por qué es importante

IIS no accede a las carpetas con tu cuenta de usuario. Usa una cuenta interna llamada **IIS_IUSRS**. Si esa cuenta no tiene permisos, los usuarios recibirán errores aunque todo lo demás esté bien configurado.

### Pasos

1. Crea la carpeta `C:\FTP` en `cliente1`.
2. Clic derecho → **Propiedades** → **Seguridad** → **Editar...** → **Agregar...**
3. Escribe `IIS_IUSRS` → **Comprobar nombres**.

   > Si Windows no encuentra el nombre, haz clic en **Ubicaciones...** y selecciona el equipo local (`cliente1`). Después vuelve a hacer **Comprobar nombres**.

4. Acepta. Marca **Modificar** en Permitir. Acepta.

**C6.2** Permisos de `C:\FTP` mostrando que `IIS_IUSRS` tiene Modificar.

---

## Parte 3. Crear el sitio FTP en IIS

### Pasos

1. `Win + R` → `inetmgr` → Enter (Administrador de IIS).
2. Clic derecho sobre **Sitios** → **Agregar sitio FTP...**
3. Nombre: `FTP_Empresa`. Ruta física: `C:\FTP`. Siguiente.
4. Enlace: IP `192.168.1.10`, puerto `21`, sin SSL. Siguiente.
5. Autenticación: marca **Básica** (desmarca **Anónima**).
6. Autorización: **Usuarios especificados** → escribe `admin1`. Marca **Lectura** y **Escritura**.
7. Finaliza.

**C6.3** Pantalla de autenticación y autorización del asistente FTP.
**C6.4** Sitio FTP iniciado en IIS.

---

## Parte 4. Configurar el firewall para FTP

### Pasos

1. `wf.msc` → **Reglas de entrada**.
2. Busca si hay alguna regla de FTP habilitada. Si no:

   * **Nueva regla...** → **Puerto** → **TCP**, puerto `21` → **Permitir la conexión** → todos los perfiles → nombre `Permitir FTP puerto 21`.

**C6.5** Regla de firewall para FTP habilitada o creada.

---

## Parte 5. Verificar que el servicio FTP está activo

`Win + R` → `services.msc` → busca **Microsoft FTP Service** → estado **En ejecución**.

**C6.6** Servicio Microsoft FTP Service en estado En ejecución.

---

## Parte 6. Conectar desde cliente2 y subir un archivo

### Pasos

1. En `cliente2`, crea `prueba_ftp.txt` en el escritorio con el Bloc de notas.
2. Abre CMD y navega al escritorio:

```cmd
cd %USERPROFILE%\Desktop
```

3. Conéctate:

```cmd
ftp 192.168.1.10
```

4. Introduce el usuario (`admin1`) y la contraseña cuando se soliciten.

   > **Si el usuario es rechazado:** recuerda que debe existir como cuenta local en `cliente1`. Verifica en `cliente1` con `lusrmgr.msc` que la cuenta existe y que la contraseña es correcta.

5. Lista el contenido:

```cmd
ls
```

   > **Si `ls` da error o timeout:** escribe `passive` y pulsa Enter para cambiar al modo pasivo. Repite `ls`.

6. Sube el archivo:

```cmd
put prueba_ftp.txt
```

7. Lista de nuevo para confirmar:

```cmd
ls
```

8. Sal:

```cmd
bye
```

9. Verifica en `cliente1` que el archivo está en `C:\FTP`.

**C6.7** Conexión FTP desde `cliente2` con inicio de sesión correcto.
**C6.8** Subida del archivo con `put`.
**C6.9** Listado final mostrando el archivo subido.
**C6.10** Vista en `cliente1` del archivo en `C:\FTP`.

---

# Ejercicio 7 — Servicio web en IIS

## Teoría necesaria

### Qué es un servidor web

Un servicio que responde a peticiones HTTP devolviendo páginas y recursos. Cuando un navegador accede a `http://192.168.1.10`, envía una petición HTTP al servidor que escucha en esa IP.

### Puerto HTTP

Por defecto el puerto **80**. Si ya está en uso por otro sitio de IIS, el tuyo no podrá iniciarse.

### Qué es `index.html`

El archivo que el servidor sirve por defecto cuando se accede a un directorio sin especificar archivo. Si accedes a `http://192.168.1.10`, el servidor buscará automáticamente `index.html`.

### Diferencia entre `localhost` e IP

`localhost` equivale a `127.0.0.1`. Si el sitio de IIS está configurado con la IP `192.168.1.10`, puede no responder a peticiones dirigidas a `localhost`, ya que IIS usa la IP del enlace para decidir qué sitio mostrar.

### Permisos de la carpeta del sitio

IIS usa la cuenta **IIS_IUSRS** para leer los archivos del sitio. Sin permiso de Lectura para esa cuenta, el servidor devolverá un **error 403 Prohibido**.

---

## Parte 1. Preparar la carpeta y sus permisos

1. Crea `C:\Web` en `cliente1`.
2. Clic derecho → **Propiedades** → **Seguridad** → **Editar...** → **Agregar...**
3. Escribe `IIS_IUSRS` → **Comprobar nombres** (usa **Ubicaciones...** si no lo encuentra, igual que en el ejercicio de FTP).
4. Acepta. Marca **Lectura** en Permitir. Acepta.

**C7.1** Permisos de `C:\Web` mostrando que `IIS_IUSRS` tiene Lectura.

---

## Parte 2. Crear el sitio web en IIS

### Pasos

1. `inetmgr` → clic derecho sobre **Sitios** → **Agregar sitio web...**
2. Nombre: `Web_Empresa`. Ruta: `C:\Web`. IP: `192.168.1.10`. Puerto: `80`.
3. Acepta.

> **Si el puerto 80 ya está en uso:** en el panel izquierdo de IIS, localiza el **Sitio web predeterminado**, haz clic derecho y selecciona **Detener**. Después inicia tu sitio `Web_Empresa`.

**C7.2** Configuración del sitio web en IIS.
**C7.3** Sitio `Web_Empresa` iniciado.

---

## Parte 3. Crear la página index.html

### Pasos

1. Abre el Bloc de notas y escribe:

```html
<html>
<head>
    <title>Web de cliente1</title>
</head>
<body>
    <h1>Página de prueba del servidor web</h1>
    <p>Realizado por: Nombre y apellidos del alumno</p>
</body>
</html>
```

2. **Archivo** → **Guardar como** → navega a `C:\Web`.
3. En el campo de nombre escribe el nombre **entre comillas**: `"index.html"`

   > Las comillas son imprescindibles. Sin ellas, el Bloc de notas añade `.txt` automáticamente y el archivo quedaría como `index.html.txt`, que el servidor no reconocerá.

4. En **Tipo** selecciona **Todos los archivos**. Guarda.

### Qué debes comprobar

En `C:\Web` el archivo debe llamarse exactamente `index.html`. Si aparece como `index.html.txt`, bórralo y repite el proceso.

**C7.4** Archivo `index.html` en `C:\Web` con el nombre correcto.

---

## Parte 4. Comprobar el firewall para el puerto 80

Si la página no carga desde `cliente2`, puede ser el firewall de `cliente1` bloqueando HTTP.

1. `wf.msc` → **Reglas de entrada**.
2. Busca una regla de IIS o HTTP habilitada. Si no existe, crea una:

   * **Nueva regla...** → **Puerto** → **TCP**, puerto `80` → **Permitir la conexión** → todos los perfiles → nombre `Permitir HTTP puerto 80`.

**C7.5** Regla de firewall para el puerto 80 habilitada o creada.

---

## Parte 5. Acceder a la web desde cliente2

1. En `cliente2`, abre el navegador y escribe: `http://192.168.1.10`
2. La página debe cargarse mostrando el contenido del `index.html`.

**Si aparece error 403:** revisa los permisos de `IIS_IUSRS` en `C:\Web`.
**Si aparece error de conexión:** verifica que el sitio está iniciado en IIS y que la regla de firewall está activa.

**C7.6** Página web cargada correctamente desde `cliente2` mostrando tu nombre.

---

## Parte 6. Probar acceso local desde cliente1

En `cliente1`, abre el navegador y prueba por separado:

```
http://localhost
http://192.168.1.10
```

### Qué debes observar y explicar en el PDF

* Si ambas direcciones cargan la página.
* Si alguna da un resultado diferente o un error.
* Por qué crees que ocurre.

Pista: `localhost` equivale a `127.0.0.1`. El sitio tiene su enlace configurado con la IP `192.168.1.10`. IIS puede no saber que `localhost` corresponde a ese sitio y mostrar el sitio predeterminado o un error.

**C7.7** Prueba con `http://localhost`.
**C7.8** Prueba con `http://192.168.1.10`.
**C7.9** Explicación escrita del resultado observado.

---

# Ejercicio 8 — Diagnóstico básico de red

## Descripción general

Un técnico debe saber diagnosticar problemas de red además de configurar servicios. En este ejercicio usarás las herramientas clásicas de Windows para observar la configuración del sistema y el estado de las conexiones.

---

## Teoría necesaria

**`ipconfig`** — Muestra la configuración IP de los adaptadores. Con `/all` añade MAC, estado DHCP, DNS, etc.

**`ping`** — Envía paquetes ICMP y verifica si hay conectividad básica con otro equipo.

**`tracert`** — Muestra el camino completo que siguen los paquetes hasta el destino, salto a salto.

**`netstat`** — Muestra conexiones activas y puertos en escucha. Útil para verificar si un servicio está realmente activo en el puerto que debería.

---

## Parte 1. Configuración IP

En CMD de `cliente1`:

```cmd
ipconfig
ipconfig /all
```

Identifica: nombre del adaptador, IP, máscara, si DHCP está deshabilitado (debe estarlo), y la dirección MAC en el resultado de `/all`.

**C8.1** Resultado de `ipconfig`.
**C8.2** Resultado de `ipconfig /all`.

---

## Parte 2. Conectividad

Desde `cliente2`:

```cmd
ping 192.168.1.10
```

Desde `cliente1`:

```cmd
ping 192.168.1.11
```

**C8.3** Ping correcto en ambas direcciones.

---

## Parte 3. `tracert`

Desde `cliente2`:

```cmd
tracert 192.168.1.10
```

Al estar en la misma red local sin routers intermedios, verás un único salto. Eso confirma que los dos equipos están directamente conectados en la misma red.

**C8.4** Resultado de `tracert` mostrando el único salto.

---

## Parte 4. `netstat`

Con FTP y web activos en `cliente1`, ejecuta en ese equipo:

```cmd
netstat -an
```

Para paginar la salida:

```cmd
netstat -an | more
```

(Barra espaciadora para avanzar, `Q` para salir.)

Busca líneas como:

```
TCP    0.0.0.0:21    0.0.0.0:0    LISTENING
TCP    0.0.0.0:80    0.0.0.0:0    LISTENING
```

`LISTENING` indica que hay un servicio activo esperando conexiones en ese puerto. Si un servicio no aparece aquí, no está realmente activo aunque esté configurado en IIS.

**C8.5** Resultado de `netstat -an`.
**C8.6** Recorte mostrando el puerto 21 o el 80 en estado LISTENING.

---

## Parte 5. Visor de eventos

1. `Win + R` → `eventvwr.msc` → Enter.
2. Despliega **Registros de Windows**.
3. Abre **Sistema** para ver eventos de servicios y arranques.
4. Abre **Seguridad** para ver eventos de autenticación.
5. Localiza algún evento relacionado con la práctica, como un inicio de sesión correcto o fallido de uno de los usuarios que usaste.

El Visor de eventos es la primera herramienta que debe consultar un técnico cuando algo falla y no sabe por qué. Windows registra aquí el motivo exacto de la mayoría de los errores.

**C8.7** Visor de eventos mostrando al menos un evento relevante de Sistema o Seguridad.

---

## Parte 6. Explicación final de comandos

Escribe en tu PDF una explicación de cada herramienta indicando concretamente para qué te ha servido en esta práctica:

* **`ipconfig`**: qué información obtuviste y en qué momento fue útil.
* **`ping`**: cuándo lo usaste y qué resultado esperabas.
* **`tracert`**: qué significa que haya un solo salto en tu red.
* **`netstat`**: qué encontraste y cómo lo interpretaste.
* **Visor de eventos**: qué tipo de información ofrece y por qué es útil para un técnico.

No se trata de definiciones. Se trata de demostrar que entiendes para qué sirve cada herramienta en el contexto de lo que has configurado.

Esta parte no requiere captura adicional, solo texto explicativo en el PDF.

---

# Conclusión final de la práctica

Al terminar esta práctica habrás configurado una pequeña infraestructura Windows de red local con dos equipos, comprobando conectividad, autenticación, compartición de recursos y publicación de servicios.

Esta práctica combina varios bloques de aprendizaje importantes:

* red local en máquina virtual,
* usuarios y grupos locales como base del control de acceso,
* permisos NTFS y su interacción con permisos de compartición,
* recursos compartidos y acceso remoto autenticado,
* gestión de credenciales de red en Windows 10,
* servicios FTP y web mediante IIS,
* y diagnóstico de red con herramientas de CMD.

No se trata solo de que los pasos funcionen, sino de que entiendas por qué funcionan y qué papel desempeña cada elemento. La competencia más valiosa que puedes desarrollar es saber identificar si un problema es de red, de permisos, de firewall o de configuración del servicio, y saber qué herramienta usar para diagnosticarlo.
