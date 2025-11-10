
# 💾 Software de un Sistema Informático

## 1. Introducción al software

El **software** es el conjunto de **programas, datos e instrucciones** que permiten que un sistema informático funcione y ejecute tareas.
Mientras el **hardware** es la parte física, el software es la **parte lógica** o intangible del sistema.
Ambos son inseparables: el hardware sin software no puede hacer nada, y el software sin hardware no puede ejecutarse.

Los programas son creados por **programadores** mediante **lenguajes de programación**, y su misión es controlar, gestionar o ampliar las capacidades del hardware para que el usuario pueda realizar tareas concretas.

---

## 2. Clasificación del software

El software se clasifica, según su función, en tres grandes categorías:

### 🧠 2.1. Software de sistema

Es el software **básico y fundamental** que permite la interacción entre el hardware y el usuario.
Incluye los **sistemas operativos**, **controladores de dispositivos** y **herramientas del sistema**.

Su objetivo principal es **gestionar los recursos del hardware** (CPU, memoria, dispositivos de almacenamiento, red, etc.) y ofrecer una **plataforma estable para la ejecución de programas de aplicación**.

#### Componentes principales:

* **Sistema operativo (SO):** coordina la ejecución de procesos, controla el hardware y proporciona una interfaz al usuario.
* **Controladores o drivers:** programas que permiten al SO comunicarse con los periféricos.
* **Utilidades del sistema:** herramientas para el mantenimiento y optimización del equipo (antivirus, desfragmentador, gestor de tareas, copias de seguridad, etc.).

---

### 🧩 2.2. Software de programación

Conjunto de herramientas que utilizan los programadores para **crear y probar aplicaciones**.
Incluye:

* **Editores de texto** (Visual Studio Code, Sublime Text).
* **Compiladores e intérpretes** (gcc, javac, Python).
* **Depuradores** (debuggers).
* **Entornos de desarrollo integrados (IDE)** como Eclipse, IntelliJ IDEA, Visual Studio, Android Studio o PyCharm.
* **Sistemas de control de versiones** como Git y plataformas colaborativas (GitHub, GitLab).

El software de programación traduce el **código fuente** (lenguaje humano) a **código máquina** (entendible por el hardware).

---

### 💼 2.3. Software de aplicación

Permite al usuario realizar **tareas específicas**. Es el software que el usuario final utiliza directamente.

Ejemplos:

* Ofimática: Microsoft Office, LibreOffice, Google Workspace.
* Diseño gráfico: Adobe Photoshop, GIMP, AutoCAD.
* Desarrollo web: VSCode, PhpStorm, Dreamweaver.
* Navegadores: Chrome, Firefox, Edge.
* ERP/CRM, bases de datos, aplicaciones científicas, videojuegos, etc.

Existen aplicaciones **de escritorio**, **web**, **móviles** y **en la nube (SaaS)**.

---

## 3. Software libre, propietario y de código abierto

El **modelo de licencia** define los derechos de uso, modificación y redistribución del software.

| Tipo de software                 | Definición                                                                                                         | Ejemplos                         |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------ | -------------------------------- |
| **Libre**                        | Puede usarse, modificarse y distribuirse libremente. No siempre es gratuito.                                       | Linux, LibreOffice, GIMP         |
| **Propietario**                  | Pertenece a una empresa o autor. El usuario solo tiene derecho de uso.                                             | Windows, macOS, Microsoft Office |
| **Código abierto (Open Source)** | El código fuente está disponible para consulta y modificación, aunque puede tener restricciones.                   | Android, Firefox, MySQL          |
| **Software gratuito (Freeware)** | No tiene coste de uso, pero no permite modificarlo.                                                                | Google Chrome, Skype             |
| **Shareware**                    | Se distribuye para prueba gratuita durante un tiempo o con funciones limitadas.                                    | WinRAR, antivirus de prueba      |
| **Adware**                       | Gratuito pero con publicidad.                                                                                      | Spotify Free                     |
| **Copyleft**                     | Licencia que garantiza la libertad de uso y modificación, pero exige mantener esa libertad en versiones derivadas. | Licencias GPL, LGPL              |

Las licencias más conocidas en software libre son **GPL, MIT, Apache, BSD y Creative Commons**.

---

## 4. El sistema operativo (SO)

El **sistema operativo** es el componente central del software de sistema.
Es un conjunto de programas que **administran los recursos del hardware y coordinan las operaciones del equipo**.

### 🎯 Funciones principales del sistema operativo

1. **Gestión del procesador (CPU):** controla la ejecución de procesos, planifica tareas y administra el uso del tiempo de CPU.
2. **Gestión de la memoria:** asigna y libera memoria RAM según las necesidades de los procesos.
3. **Gestión del almacenamiento:** controla los discos duros y SSD, organiza los archivos en sistemas de ficheros (NTFS, ext4, APFS…).
4. **Gestión de entrada/salida:** coordina los dispositivos periféricos mediante drivers.
5. **Gestión de usuarios y seguridad:** autentica usuarios, asigna permisos y protege el sistema.
6. **Interfaz con el usuario:** proporciona entorno de uso (línea de comandos o interfaz gráfica).

---

### 🧮 Tipos de sistemas operativos (según finalidad)

| Tipo                                 | Características                                                                      | Ejemplo                          |
| ------------------------------------ | ------------------------------------------------------------------------------------ | -------------------------------- |
| **Monousuario / Multiusuario**       | Permite uso por una o varias personas simultáneamente.                               | MS-DOS / Linux                   |
| **Monotarea / Multitarea**           | Ejecuta una o varias tareas al mismo tiempo.                                         | MS-DOS / Windows, macOS          |
| **Monoprocesador / Multiprocesador** | Usa una o varias CPUs.                                                               | Windows 11, Linux                |
| **Centralizado / Distribuido**       | En los distribuidos, varios equipos comparten recursos.                              | UNIX, sistemas en la nube        |
| **Tiempo real**                      | Responde en intervalos estrictos, usado en control industrial, robótica, automoción. | QNX, VxWorks, sistemas embebidos |

---

### 💻 Sistemas operativos más comunes (2025)

| Plataforma                  | Sistemas principales                                                     |
| --------------------------- | ------------------------------------------------------------------------ |
| **Escritorio / Portátil**   | Windows 10/11, macOS Sonoma, Ubuntu, Debian, Fedora                      |
| **Servidores**              | Linux (CentOS Stream, Ubuntu Server), Windows Server, Red Hat Enterprise |
| **Móviles**                 | Android 14, iOS 18, HarmonyOS                                            |
| **Dispositivos integrados** | Raspberry Pi OS, Arduino OS, FreeRTOS                                    |
| **Nube y virtualización**   | VMware ESXi, Proxmox, Hyper-V, KVM, Docker, Kubernetes                   |

Los sistemas modernos combinan **interfaz gráfica (GUI)** y **línea de comandos (CLI)**.
Ejemplo: Linux ofrece GNOME/KDE y terminal bash; Windows ofrece PowerShell y GUI.

---

## 5. Estructura interna de un sistema operativo

Un sistema operativo se organiza en **capas o niveles**, cada una con funciones específicas.

### 🔸 1. Núcleo o kernel

Es la parte más importante. Controla el hardware y gestiona los recursos.
Tipos:

* **Monolítico:** todo el kernel en un único bloque (Linux).
* **Microkernel:** modular, los servicios se ejecutan como procesos separados (Minix, macOS).
* **Híbrido:** mezcla ambos modelos (Windows NT, Android).

### 🔸 2. Gestores del sistema

Módulos que controlan CPU, memoria, archivos, dispositivos y seguridad.

### 🔸 3. Intérprete de comandos o shell

Permite al usuario interactuar con el sistema mediante instrucciones (CLI) o entorno gráfico (GUI).
Ejemplo: Bash en Linux, CMD/PowerShell en Windows, Terminal en macOS.

### 🔸 4. Librerías del sistema

Conjunto de rutinas y funciones que facilitan el acceso a los recursos hardware y software.
Ejemplo: librerías .dll en Windows, .so en Linux.

---

## 6. Interfaces del sistema operativo

### 🧾 Tipos de interfaces

1. **Línea de comandos (CLI):** basada en texto, rápida y flexible.
   Ejemplos: CMD, PowerShell, Bash, Zsh.

2. **Interfaz gráfica (GUI):** orientada al usuario, usa ventanas, menús e iconos.
   Ejemplos: GNOME, KDE, Windows, macOS.

3. **Interfaces táctiles y de voz:** habituales en móviles y dispositivos inteligentes.
   Ejemplos: Siri, Google Assistant, Alexa.

El desarrollo actual tiende a interfaces **híbridas** (táctil + voz + visual) y sistemas **multiplataforma**.

---

## 7. Instalación y administración del software

### 💿 Tipos de instalación

* **Desde medios físicos:** DVD, USB, ISO.
* **Desde Internet:** descarga directa, tiendas oficiales (Microsoft Store, Snap, Flatpak, App Store, Google Play).
* **Instalación silenciosa o automatizada:** sin intervención del usuario, usada en empresas.
* **Instalación en red o nube:** el software se ejecuta desde servidores remotos.

### ⚙️ Métodos de distribución modernos

* **Instaladores empaquetados:** MSI, EXE, PKG, DEB, RPM.
* **Contenedores y virtualización:** Docker, Flatpak, Snap.
* **Aplicaciones portables:** no requieren instalación (ejecutables en USB).
* **Aplicaciones web progresivas (PWA):** se ejecutan en navegador pero funcionan offline.

### 🔐 Seguridad y mantenimiento

* Actualizaciones automáticas.
* Copias de seguridad.
* Antivirus y firewall.
* Control de permisos y usuarios.
* Monitorización del rendimiento.

---

## 8. Software y virtualización

La **virtualización** permite ejecutar varios sistemas operativos en un mismo hardware físico.
Se logra mediante un **hipervisor**, que asigna recursos a cada máquina virtual.

### 🔹 Tipos de virtualización

* **Virtualización completa:** emula todo el hardware (VirtualBox, VMware, Hyper-V).
* **Paravirtualización:** el SO invitado es consciente de que está virtualizado (Xen).
* **Virtualización de contenedores:** a nivel de sistema operativo (Docker, LXC, Kubernetes).

### 🔹 Ventajas

* Ahorro de costes y energía.
* Mayor seguridad y aislamiento.
* Facilidad de copia, despliegue y pruebas.
* Uso eficiente de los recursos.

---

## 9. Software y servicios en la nube

La **computación en la nube (cloud computing)** consiste en ofrecer servicios informáticos a través de Internet.
El software ya no se instala necesariamente en el equipo local, sino que se ejecuta o almacena en servidores remotos.

### ☁️ Modelos de servicio

| Tipo                                     | Descripción                                     | Ejemplo                        |
| ---------------------------------------- | ----------------------------------------------- | ------------------------------ |
| **IaaS (Infraestructura como Servicio)** | Alquiler de servidores, redes y almacenamiento. | AWS EC2, Google Compute Engine |
| **PaaS (Plataforma como Servicio)**      | Entorno completo para desarrollar aplicaciones. | Heroku, Google App Engine      |
| **SaaS (Software como Servicio)**        | Aplicaciones listas para usar vía web.          | Google Workspace, Office 365   |

### 📦 Ventajas

* Accesible desde cualquier dispositivo.
* Copias de seguridad automáticas.
* Escalabilidad y actualización continua.
* Pago por uso (suscripción o consumo).

---

## 10. Software y sostenibilidad

El software también influye en la **eficiencia energética y sostenibilidad** del sistema informático:

* **Optimización de recursos:** reducir el uso innecesario de CPU y memoria.
* **Actualizaciones y soporte prolongado:** alargar la vida útil de los equipos.
* **Código limpio y eficiente:** menor consumo energético.
* **Uso de software libre:** favorece la reutilización y reduce residuos digitales.

Los sistemas modernos incluyen **modos de ahorro de energía**, **gestión dinámica de frecuencia** y **suspensión inteligente**.

---
