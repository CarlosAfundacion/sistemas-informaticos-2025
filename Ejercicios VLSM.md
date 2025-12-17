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


