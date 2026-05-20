# Tema 9 Resumen — Administración de redes en Linux

# 1. Introducción a las redes en Linux

Linux es uno de los sistemas más utilizados en redes y servidores debido a:

* estabilidad,
* seguridad,
* rendimiento,
* flexibilidad.

Gran parte de Internet funciona sobre servidores Linux.

Linux permite:

* configurar redes,
* administrar servicios,
* compartir recursos,
* publicar servidores web,
* gestionar conexiones remotas.

---

# 2. Configuración de red en Linux

Para que un sistema Linux pueda comunicarse necesita:

* dirección IP,
* máscara,
* gateway,
* DNS.

Linux identifica las interfaces de red mediante nombres como:

* `eth0`,
* `enp0s3`.

---

## Configuración dinámica y estática

### DHCP

La configuración se obtiene automáticamente.

Ventajas:

* simplicidad,
* menos errores,
* administración rápida.

### Configuración estática

La IP se asigna manualmente.

Se utiliza en:

* servidores,
* routers,
* servicios de red.

---

## Importancia de la configuración IP

Una mala configuración puede provocar:

* falta de conexión,
* imposibilidad de acceder a Internet,
* fallos de comunicación entre equipos.

Por ello es fundamental comprender:

* cómo se estructura una red,
* qué función tiene cada parámetro,
* cómo detectar errores.

---

## Herramienta básica

```bash id="rr1"
ip addr
```

Muestra configuración de red.

---

# 3. Diagnóstico de conectividad

Los administradores deben verificar continuamente:

* conectividad,
* rutas,
* puertos abiertos,
* resolución DNS.

---

## Conectividad

El primer paso es comprobar:

* si existe comunicación con otros equipos,
* si el gateway responde,
* si hay acceso a Internet.

### Herramienta básica

```bash id="rr2"
ping
```

Comprueba conectividad.

---

## Puertos y conexiones

Cada servicio utiliza puertos específicos.

Ejemplos:

* SSH → 22
* HTTP → 80
* HTTPS → 443

Es importante comprobar:

* qué puertos están abiertos,
* qué servicios están escuchando.

### Herramienta básica

```bash id="rr3"
ss
```

Muestra conexiones y puertos activos.

---

# 4. DNS y resolución de nombres

Los DNS traducen nombres de dominio a direcciones IP.

Sin DNS sería necesario memorizar IPs numéricas.

Ejemplo:

```text id="rr4"
google.com → 142.250.x.x
```

---

## Funcionamiento básico

Cuando el usuario introduce una web:

1. El sistema consulta el DNS.
2. Obtiene la IP.
3. Se conecta al servidor.

---

## Archivos importantes

### `/etc/hosts`

Permite crear resoluciones manuales locales.

### `/etc/resolv.conf`

Configura servidores DNS.

---

## Importancia del DNS

Muchos problemas de red realmente son problemas DNS:

* páginas que no cargan,
* servidores inaccesibles,
* lentitud de conexión.

---

# 5. SSH

SSH es uno de los servicios más importantes en Linux.

Permite:

* administrar sistemas remotamente,
* transferir archivos,
* ejecutar comandos seguros.

---

## Ventajas de SSH

* conexión cifrada,
* administración remota segura,
* automatización.

SSH es fundamental porque muchos servidores Linux:

* no tienen entorno gráfico,
* se administran completamente de forma remota.

---

## Seguridad en SSH

Es importante:

* usar contraseñas seguras,
* limitar acceso root,
* cambiar configuraciones inseguras,
* utilizar claves SSH cuando sea posible.

---

# 6. Compartición de archivos

Linux puede compartir recursos mediante distintos servicios.

---

## Samba

Permite compartir archivos entre Linux y Windows.

Es muy utilizado en:

* empresas,
* centros educativos,
* redes mixtas.

---

## NFS

Permite compartir carpetas entre sistemas Linux.

Es más rápido y eficiente en entornos Linux puros.

---

## Aspectos importantes

* permisos,
* usuarios autorizados,
* seguridad,
* accesos remotos.

Una mala configuración puede:

* exponer archivos,
* permitir accesos indebidos,
* comprometer información.

---

# 7. Servidor web Apache

Apache es uno de los servidores web más utilizados del mundo.

Permite:

* alojar páginas web,
* publicar aplicaciones,
* servir contenido web.

---

## Funcionamiento básico

Cuando un usuario accede a una web:

1. El navegador realiza una petición HTTP.
2. Apache recibe la petición.
3. Devuelve el contenido solicitado.

---

## Virtual Hosts

Apache puede alojar varias webs en un único servidor mediante Virtual Hosts.

Esto permite:

* múltiples dominios,
* separación de sitios,
* mejor organización.

---

## Importancia de Apache

Es fundamental en:

* desarrollo web,
* hosting,
* servidores empresariales.

---

# 8. Firewall y seguridad

Linux utiliza firewalls para controlar tráfico de red.

El firewall decide:

* qué conexiones se permiten,
* qué conexiones se bloquean.

---

## Objetivos del firewall

* proteger servicios,
* evitar accesos no autorizados,
* reducir superficie de ataque.

---

## Puertos y servicios

Cada servicio expuesto supone un posible riesgo.

Por ello es importante:

* abrir solo puertos necesarios,
* cerrar servicios innecesarios,
* limitar accesos.

---

# 9. Servicios en Linux

Linux utiliza servicios para ejecutar funciones del sistema.

Ejemplos:

* SSH,
* Apache,
* Samba,
* bases de datos.

---

## Administración de servicios

Un administrador debe saber:

* iniciar servicios,
* detenerlos,
* reiniciarlos,
* comprobar estado,
* activar inicio automático.

---

## Importancia de los servicios

Muchos problemas de servidores ocurren porque:

* un servicio se detuvo,
* falló una configuración,
* existe un conflicto.

---

# 10. Monitorización y logs

Linux registra continuamente información del sistema.

Los logs permiten:

* detectar errores,
* investigar fallos,
* supervisar actividad.

---

## Información registrada

* accesos,
* errores,
* servicios,
* seguridad,
* conexiones.

---

## Importancia de los logs

Los logs son fundamentales para:

* diagnóstico,
* auditoría,
* seguridad,
* mantenimiento.

Muchos ataques y errores solo pueden detectarse revisando registros.

---

# 11. Seguridad básica en Linux

La mayoría de servidores Linux están conectados permanentemente a redes e Internet.

Por ello la seguridad es crítica.

---

## Buenas prácticas

### Actualizaciones

Corrigen vulnerabilidades.

### Firewall

Controla accesos.

### Permisos

Limita acceso a información.

### SSH seguro

Reduce riesgos de acceso remoto.

### Contraseñas robustas

Evitan accesos indebidos.

---

## Objetivo de la seguridad

La seguridad busca:

* proteger información,
* garantizar disponibilidad,
* evitar accesos no autorizados,
* reducir riesgos de ataques y errores.
