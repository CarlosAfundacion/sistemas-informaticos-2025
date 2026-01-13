# Práctica de VLSM

**Instrucciones**
1.  **Ordena** siempre los requisitos de mayor a menor cantidad de hosts.
2.  **Calcula** la nueva máscara (CIDR), la Dirección de Red, el Rango Útil y el Broadcast.
3.  **Indica** dónde empieza la siguiente red (aunque no se use) para verificar que no hay solapamiento.

---

### Ejercicio 1: La PYME en crecimiento (Clase C)
Una pequeña empresa de software necesita segmentar su red local para mejorar la seguridad.
*   **Red Base:** `192.168.100.0/24`

**Requisitos (Desordenados):**
1.  **Departamento de Finanzas:** 12 hosts.
2.  **Departamento de Desarrolladores:** 55 hosts.
3.  **Servidores Locales:** 25 hosts.
4.  **Wi-Fi Invitados:** 10 hosts.

**Tabla de Resultados:**

| Nombre Subred | Hosts Requeridos | Máscara (CIDR) | Dirección de Red | Primer IP | Última IP | Broadcast |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| | | / | | | | |
| | | / | | | | |
| | | / | | | | |
| | | / | | | | |

---

### Ejercicio 2: Conectividad Sucursal (Clase C)
Una oficina bancaria tiene una red LAN principal y necesita conectarse con la central y un cajero automático externo mediante enlaces dedicados.
*   **Red Base:** `200.50.10.0/24`

**Requisitos (Desordenados):**
1.  **Enlace WAN a Central:** 2 hosts.
2.  **Red de Empleados (LAN):** 110 hosts.
3.  **Enlace a Cajero Automático:** 2 hosts.
4.  **Cámaras de Seguridad:** 50 hosts.

**Tabla de Resultados:**

| Nombre Subred | Hosts Requeridos | Máscara (CIDR) | Dirección de Red | Primer IP | Última IP | Broadcast |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| | | / | | | | |
| | | / | | | | |
| | | / | | | | |
| | | / | | | | |

---

### Ejercicio 3: Ajuste Máximo (Clase C)
Un sistema de domótica (IoT) requiere subredes exactas. No hay margen para desperdiciar direcciones IP grandes.
*   **Red Base:** `192.168.1.0/24`

**Requisitos (Desordenados):**
1.  **Sensores Zona Norte:** 30 hosts.
2.  **Iluminación Inteligente:** 126 hosts.
3.  **Sensores Zona Sur:** 62 hosts.

**Tabla de Resultados:**

| Nombre Subred | Hosts Requeridos | Máscara (CIDR) | Dirección de Red | Primer IP | Última IP | Broadcast |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| | | / | | | | |
| | | / | | | | |
| | | / | | | | |

---

### Ejercicio 4: El Campus Universitario (Clase B)
Una universidad está reorganizando su direccionamiento IP. Aquí trabajarás modificando el **tercer octeto**.
*   **Red Base:** `172.16.0.0/16`

**Requisitos (Desordenados):**
1.  **Facultad de Ciencias (Labs):** 500 hosts.
2.  **Facultad de Letras:** 200 hosts.
3.  **Edificio Central (Wi-Fi Alumnos):** 1000 hosts.

**Tabla de Resultados:**

| Nombre Subred | Hosts Requeridos | Máscara (CIDR) | Dirección de Red | Primer IP | Última IP | Broadcast |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| | | / | | | | |
| | | / | | | | |
| | | / | | | | |

---

### Ejercicio 5: La Multinacional (Clase A)
Un proveedor de servicios necesita asignar bloques grandes a sus clientes corporativos.
*   **Red Base:** `10.0.0.0/8`

**Requisitos (Desordenados):**
1.  **Cliente Corporativo "B":** 2000 hosts.
2.  **Cliente Corporativo "A":** 4000 hosts.
3.  **Centro de Datos Propio:** 1000 hosts.

**Tabla de Resultados:**

| Nombre Subred | Hosts Requeridos | Máscara (CIDR) | Dirección de Red | Primer IP | Última IP | Broadcast |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| | | / | | | | |
| | | / | | | | |
| | | / | | | | |

---

### Ejercicio 6: Empresa de Servicios Gestionados (Clase C)

Una empresa de servicios IT ofrece conectividad segmentada a distintos departamentos y necesita optimizar al máximo su red.

* **Red Base:** `192.168.50.0/24`

**Requisitos (Desordenados):**

1. **Departamento Técnico:** 70 hosts.
2. **Administración:** 20 hosts.
3. **Sala de Formación:** 30 hosts.
4. **Gestión de Impresoras:** 6 hosts.

**Tabla de Resultados:**

| Nombre Subred | Hosts Requeridos | Máscara (CIDR) | Dirección de Red | Primer IP | Última IP | Broadcast |
| :------------ | :--------------- | :------------- | :--------------- | :-------- | :-------- | :-------- |
|               |                  | /              |                  |           |           |           |
|               |                  | /              |                  |           |           |           |
|               |                  | /              |                  |           |           |           |
|               |                  | /              |                  |           |           |           |



---

### Ejercicio 7: Hospital Comarcal (Clase B)

Un hospital necesita reorganizar su red interna separando servicios críticos, personal y dispositivos médicos.
El direccionamiento debe realizarse **trabajando principalmente sobre el tercer octeto**.

* **Red Base:** `172.20.0.0/16`

**Requisitos (Desordenados):**

1. **Dispositivos Médicos (IoT):** 800 hosts.
2. **Personal Sanitario:** 1500 hosts.
3. **Administración y Gestión:** 400 hosts.
4. **Zona Wi-Fi Pacientes:** 300 hosts.

**Tabla de Resultados:**

| Nombre Subred | Hosts Requeridos | Máscara (CIDR) | Dirección de Red | Primer IP | Última IP | Broadcast |
| :------------ | :--------------- | :------------- | :--------------- | :-------- | :-------- | :-------- |
|               |                  | /              |                  |           |           |           |
|               |                  | /              |                  |           |           |           |
|               |                  | /              |                  |           |           |           |
|               |                  | /              |                  |           |           |           |



---

### Ejercicio 8: Proveedor Cloud Regional (Clase A – salto en 2º octeto)

Un proveedor de servicios cloud necesita asignar grandes bloques de direcciones IP a distintos clientes empresariales.
El direccionamiento debe realizarse mediante **VLSM** y **al menos una de las subredes debe provocar un salto en el segundo octeto**.

* **Red Base:** `10.0.0.0/8`

**Requisitos (Desordenados):**

1. **Cliente Enterprise:** 60.000 hosts.
2. **Cliente Corporativo:** 20.000 hosts.
3. **Infraestructura Interna:** 5.000 hosts.
4. **Servicios de Monitorización:** 500 hosts.

**Tabla de Resultados:**

| Nombre Subred | Hosts Requeridos | Máscara (CIDR) | Dirección de Red | Primer IP | Última IP | Broadcast |
| :------------ | :--------------- | :------------- | :--------------- | :-------- | :-------- | :-------- |
|               |                  | /              |                  |           |           |           |
|               |                  | /              |                  |           |           |           |
|               |                  | /              |                  |           |           |           |
|               |                  | /              |                  |           |           |           |

