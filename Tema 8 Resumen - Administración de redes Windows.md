# Tema 8 — Administración de una red Windows

# 1. Introducción a las redes Windows

Una red informática es un conjunto de equipos conectados entre sí para compartir información y recursos.

En un entorno Windows, las redes permiten:

* compartir archivos,
* compartir impresoras,
* administrar usuarios,
* acceder a otros equipos,
* trabajar de forma colaborativa.

Windows puede organizar las redes principalmente de dos formas:

* grupo de trabajo,
* dominio.

---

## Grupo de trabajo

En un grupo de trabajo:

* cada equipo administra sus propios usuarios,
* no existe un servidor central,
* todos los equipos tienen una administración independiente.

Es habitual en:

* hogares,
* pequeñas oficinas,
* redes simples.

Su principal ventaja es la sencillez, aunque presenta inconvenientes:

* peor control de seguridad,
* administración menos eficiente,
* dificultad para gestionar muchos equipos.

---

## Dominio

En un dominio existe un servidor central que administra:

* usuarios,
* contraseñas,
* permisos,
* políticas de seguridad.

Esto permite:

* iniciar sesión desde distintos equipos,
* controlar toda la red centralizadamente,
* aplicar configuraciones comunes.

Los dominios son habituales en:

* empresas,
* centros educativos,
* organizaciones grandes.

---

# 2. Direccionamiento IP y configuración de red

Para que un equipo pueda comunicarse en una red necesita una configuración IP correcta.

Los elementos fundamentales son:

| Elemento          | Función                     |
| ----------------- | --------------------------- |
| Dirección IP      | Identifica al equipo        |
| Máscara de subred | Define la red               |
| Gateway           | Permite salir a otras redes |
| DNS               | Traduce nombres a IP        |

---

## Dirección IP

La dirección IP identifica de forma única a un equipo dentro de una red.

Ejemplo:

```text id="ip1"
192.168.1.15
```

Es importante que:

* no haya IPs duplicadas,
* todos los equipos estén en la misma red,
* el gateway sea correcto.

---

## DHCP e IP estática

### DHCP

Permite asignar automáticamente:

* IP,
* máscara,
* gateway,
* DNS.

Ventajas:

* configuración automática,
* menos errores,
* administración sencilla.

### IP estática

La configuración se realiza manualmente.

Se utiliza normalmente en:

* servidores,
* impresoras,
* dispositivos importantes.

---

## Diagnóstico básico

Cuando hay problemas de red se debe comprobar:

* si el equipo tiene IP,
* si existe conectividad,
* si funcionan los DNS,
* si hay acceso al gateway.

### Herramientas esenciales

```powershell id="ww1"
ipconfig
```

Muestra la configuración IP.

```powershell id="ww2"
ping
```

Comprueba comunicación entre equipos.

Lo importante no es memorizar comandos, sino comprender:

* cómo se comunican los equipos,
* qué significa cada parámetro,
* cómo detectar errores de conectividad.

---

# 3. Compartición de recursos

Una de las funciones principales de las redes Windows es compartir recursos.

Los recursos más habituales son:

* carpetas,
* archivos,
* impresoras.

---

## Permisos de compartición y NTFS

Windows utiliza dos sistemas de permisos:

* permisos de compartición,
* permisos NTFS.

### Permisos de compartición

Se aplican cuando se accede desde la red.

### Permisos NTFS

Se aplican localmente sobre archivos y carpetas.

El sistema siempre aplica:

* el permiso más restrictivo.

---

## Importancia de los permisos

Una mala configuración puede:

* permitir acceso no autorizado,
* impedir trabajar a usuarios válidos,
* provocar pérdida de información.

Por ello es fundamental:

* asignar permisos correctamente,
* limitar accesos innecesarios,
* organizar usuarios mediante grupos.

---

# 4. Usuarios y grupos en Windows

Windows utiliza usuarios y grupos para controlar acceso y seguridad.

Cada usuario tiene:

* nombre,
* contraseña,
* permisos,
* perfil personal.

---

## Tipos de usuarios

### Administradores

Tienen control total del sistema.

### Usuarios estándar

Tienen permisos limitados.

### Invitados

Acceso muy restringido.

---

## Grupos

Los grupos simplifican la administración.

En lugar de asignar permisos individualmente:

* se asignan a grupos,
* los usuarios heredan permisos del grupo.

Esto facilita:

* organización,
* administración,
* seguridad.

---

## Objetivos de la administración de usuarios

* proteger el sistema,
* limitar acciones peligrosas,
* controlar acceso a recursos,
* facilitar administración de la red.

---

# 5. Administración remota

Windows permite administrar equipos remotamente mediante:

* Escritorio remoto (RDP).

Esto permite:

* controlar equipos a distancia,
* realizar soporte técnico,
* administrar servidores.

---

## Funcionamiento del escritorio remoto

El usuario visualiza y controla el equipo remoto como si estuviera físicamente delante.

Para utilizarlo:

* el servicio debe estar activado,
* el firewall debe permitir conexiones,
* el usuario debe tener permisos.

---

## Riesgos y seguridad

El acceso remoto puede ser peligroso si:

* se usan contraseñas débiles,
* se exponen servicios innecesarios,
* no se controla el acceso.

Por ello es importante:

* limitar usuarios autorizados,
* utilizar redes seguras,
* mantener el sistema actualizado.

---

# 6. Servicios del sistema

Los servicios son procesos que funcionan en segundo plano sin intervención directa del usuario.

Ejemplos:

* red,
* impresión,
* actualizaciones,
* antivirus.

---

## Tipos de inicio

Un servicio puede configurarse como:

* automático,
* manual,
* deshabilitado.

---

## Importancia de los servicios

Muchos problemas del sistema ocurren porque:

* un servicio está detenido,
* un servicio falla,
* existe un conflicto entre servicios.

Por ello, un administrador debe saber:

* identificar servicios importantes,
* comprobar su estado,
* reiniciarlos si es necesario.

---

# 7. Firewall de Windows

El firewall protege el sistema filtrando conexiones de red.

Permite:

* bloquear tráfico,
* permitir aplicaciones,
* controlar puertos abiertos.

---

## Reglas de entrada y salida

### Entrada

Controlan conexiones que llegan al equipo.

### Salida

Controlan conexiones que salen del equipo.

---

## Importancia del firewall

El firewall ayuda a:

* evitar accesos no autorizados,
* proteger servicios,
* reducir riesgos de ataques.

Un firewall mal configurado puede:

* bloquear servicios legítimos,
* dejar expuesto el sistema.

---

# 8. Administración de discos

Windows permite gestionar discos y particiones gráficamente.

---

## Conceptos importantes

### Partición

División lógica de un disco.

### Sistema de archivos

Organiza la información almacenada.

El más habitual en Windows es:

* NTFS.

---

## Funciones principales

* crear particiones,
* formatear discos,
* asignar letras,
* ampliar volúmenes.

---

## Importancia de la gestión de discos

Una mala administración puede provocar:

* pérdida de datos,
* errores de arranque,
* problemas de almacenamiento.

---

# 9. Diagnóstico y resolución de problemas

La administración de redes implica detectar y solucionar incidencias.

Los problemas más habituales son:

* falta de conectividad,
* DNS incorrectos,
* permisos mal configurados,
* firewall bloqueando conexiones.

---

## Proceso de diagnóstico

Es importante comprobar:

1. Configuración IP.
2. Comunicación con otros equipos.
3. Resolución DNS.
4. Acceso a recursos.

---

## Herramientas esenciales

```powershell id="ww3"
ping
```

Comprueba conectividad.

```powershell id="ww4"
nslookup
```

Consulta resolución DNS.

Lo más importante es interpretar correctamente:

* qué está fallando,
* dónde se encuentra el problema,
* qué componente provoca el error.

---

# 10. Copias de seguridad

Las copias de seguridad permiten recuperar información ante:

* fallos,
* errores humanos,
* ataques,
* averías.

---

## Tipos de copia

### Completa

Copia todos los datos.

### Incremental

Copia solo cambios desde la última copia.

### Diferencial

Copia cambios desde la última copia completa.

---

## Objetivos del backup

* evitar pérdida de datos,
* garantizar continuidad,
* restaurar sistemas rápidamente.

Las copias de seguridad son una de las tareas más importantes en administración de sistemas.

