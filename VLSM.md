# VLSM 

## 1. ¿Qué es VLSM?
**VLSM** significa *Máscara de Subred de Longitud Variable*.

A diferencia del subneteo clásico (FLSM), donde todas las subredes tienen el mismo tamaño (ej. todas /24), **VLSM permite dividir una red en subredes de diferentes tamaños**.

### ¿Por qué lo usamos?
*   **Eficiencia:** No desperdicias direcciones IP.
*   **Ejemplo:** Si necesitas una subred para 100 usuarios y otra para un enlace de 2 routers:
    *   Sin VLSM: Gastarías dos redes grandes.
    *   Con VLSM: Creas una red grande (/25) para los usuarios y una pequeñita (/30) para los routers.

---

## 2. La Regla de Oro (¡Muy Importante!)
Para calcular VLSM sin errores, siempre debes seguir este orden:

> **ORDENAR LOS REQUISITOS DE MAYOR A MENOR CANTIDAD DE HOSTS.**

Si no ordenas de mayor a menor, las subredes se solaparán y el cálculo fallará.

---

## 3. Fórmulas Necesarias
Necesitas saber cuántos bits de host ($h$) hacen falta para cubrir la demanda.

*   **Fórmula:** $2^h - 2 \ge \text{Hosts necesarios}$
    *(El "-2" es por la Dirección de Red y el Broadcast)*.
*   **Nueva Máscara (CIDR):** $32 - h$
*   **Salto (Block Size):** $2^h$

**Tabla rápida de potencias de 2:**
*   $2^2 = 4$ (Resta 2 = 2 hosts útiles) -> Máscara /30
*   $2^3 = 8$ (Resta 2 = 6 hosts útiles) -> Máscara /29
*   $2^4 = 16$ (Resta 2 = 14 hosts útiles) -> Máscara /28
*   $2^5 = 32$ (Resta 2 = 30 hosts útiles) -> Máscara /27
*   $2^6 = 64$ (Resta 2 = 62 hosts útiles) -> Máscara /26
*   $2^7 = 128$ (Resta 2 = 126 hosts útiles) -> Máscara /25

---

## 4. VLSM Clase C: 

**Escenario:**
Te dan la red base **192.168.1.0/24** y te piden crear subredes para los siguientes departamentos:
1.  **Marketing:** 2 hosts (Enlace punto a punto).
2.  **Ventas:** 100 hosts.
3.  **IT:** 50 hosts.

### PASO 1: Ordenar (La Regla de Oro)
1.  **Ventas:** 100 hosts (Mayor)
2.  **IT:** 50 hosts (Medio)
3.  **Marketing:** 2 hosts (Menor)

---

### PASO 2: Calcular Subred 1 (Ventas - 100 hosts)
*   **IP Inicio:** 192.168.1.0
*   **Cálculo:** Buscamos potencia de 2 que cubra 100.
    *   $2^6 = 64$ (Muy poco).
    *   $2^7 = 128$ (Perfecto). Bits de host $h = 7$.
*   **Nueva Máscara:** $32 - 7 = \textbf{/25}$ (o 255.255.255.128).
*   **Salto (Block Size):** 128.
*   **Rango:** La red va de la .0 a la .127.

| Dato | Valor |
| :--- | :--- |
| **ID Red** | 192.168.1.0 /25 |
| **Primera IP** | 192.168.1.1 |
| **Última IP** | 192.168.1.126 |
| **Broadcast** | 192.168.1.127 |

*(La siguiente red empezará en 192.168.1.128)*

---

### PASO 3: Calcular Subred 2 (IT - 50 hosts)
*   **IP Inicio:** 192.168.1.128 (Donde terminó la anterior).
*   **Cálculo:** Buscamos potencia para 50.
    *   $2^5 = 32$ (Muy poco).
    *   $2^6 = 64$ (Perfecto). Bits de host $h = 6$.
*   **Nueva Máscara:** $32 - 6 = \textbf{/26}$ (o 255.255.255.192).
*   **Salto (Block Size):** 64.
*   **Rango:** $128 + 64 = 192$ (Este es el inicio de la siguiente). El broadcast es uno menos (191).

| Dato | Valor |
| :--- | :--- |
| **ID Red** | 192.168.1.128 /26 |
| **Primera IP** | 192.168.1.129 |
| **Última IP** | 192.168.1.190 |
| **Broadcast** | 192.168.1.191 |

*(La siguiente red empezará en 192.168.1.192)*

---

### PASO 4: Calcular Subred 3 (Marketing - 2 hosts)
*   **IP Inicio:** 192.168.1.192.
*   **Cálculo:** Buscamos potencia para 2.
    *   $2^2 = 4$ (Perfecto: $4 - 2 = 2$ útiles). Bits de host $h = 2$.
*   **Nueva Máscara:** $32 - 2 = \textbf{/30}$ (o 255.255.255.252).
*   **Salto (Block Size):** 4.
*   **Rango:** $192 + 4 = 196$. El broadcast es 195.

| Dato | Valor |
| :--- | :--- |
| **ID Red** | 192.168.1.192 /30 |
| **Primera IP** | 192.168.1.193 |
| **Última IP** | 192.168.1.194 |
| **Broadcast** | 192.168.1.195 |

---

### Resumen 

| Departamento | Hosts Requeridos | Máscara | Dirección de Red | Rango Útil | Broadcast |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Ventas** | 100 | /25 | 192.168.1.0 | .1 - .126 | .127 |
| **IT** | 50 | /26 | 192.168.1.128 | .129 - .190 | .191 |
| **Marketing** | 2 | /30 | 192.168.1.192 | .193 - .194 | .195 |
| *Libre* | N/A | N/A | 192.168.1.196 | ... | ... |



## 5. VLSM en Clase B (Modificando el 3er Octeto)

**Escenario:** Tienes la red base **172.16.0.0 /16**.
**Requisitos (ya ordenados):**
1.  **Edificio Central:** 1000 hosts.
2.  **Fábrica:** 500 hosts.
3.  **Almacén:** 50 hosts.

### 1. Edificio Central (1000 hosts)
*   **Cálculo:** Necesitamos $2^{10} = 1024$. **Hosts ($h$) = 10**.
*   **Máscara:** $32 - 10 = \textbf{/22}$.
*   **Análisis del Salto:**
    *   La máscara /22 cae en el **3er octeto** (bits 17 al 24).
    *   Límite del octeto (24) - Máscara (22) = 2 bits libres.
    *   Salto = $2^2 = \textbf{4}$.
    *   *Significa que en el 3er número saltamos de 4 en 4.*
*   **Rango:**
    *   Inicio: `172.16.0.0`
    *   **Siguiente Red:** `172.16.4.0` (0 + 4)
    *   **Fin (Broadcast):** `172.16.3.255` (Uno menos que la siguiente).

### 2. Fábrica (500 hosts)
*   **Inicio:** `172.16.4.0` (Donde quedó la anterior).
*   **Cálculo:** Necesitamos $2^9 = 512$. **Hosts ($h$) = 9**.
*   **Máscara:** $32 - 9 = \textbf{/23}$.
*   **Análisis del Salto:**
    *   La máscara /23 cae en el **3er octeto**.
    *   Límite del octeto (24) - Máscara (23) = 1 bit libre.
    *   Salto = $2^1 = \textbf{2}$.
    *   *Significa que en el 3er número saltamos de 2 en 2.*
*   **Rango:**
    *   Inicio: `172.16.4.0`
    *   **Siguiente Red:** `172.16.6.0` (4 + 2)
    *   **Fin (Broadcast):** `172.16.5.255`

### 3. Almacén (50 hosts)
*   **Inicio:** `172.16.6.0` (Donde quedó la anterior).
*   **Cálculo:** Necesitamos $2^6 = 64$. **Hosts ($h$) = 6**.
*   **Máscara:** $32 - 6 = \textbf{/26}$.
*   **Análisis del Salto:**
    *   La máscara /26 cae en el **4º octeto** (bits 25 al 32). ¡Volvemos al método fácil!
    *   Salto = $2^6 = 64$.
    *   *Significa que en el 4º número saltamos de 64 en 64.*
*   **Rango:**
    *   Inicio: `172.16.6.0`
    *   **Siguiente Red:** `172.16.6.64`
    *   **Fin (Broadcast):** `172.16.6.63`

---

## 6. VLSM en Clase A (Grandes Bloques)

**Escenario:** Tienes la red base **10.0.0.0 /8**.
**Requisitos (ya ordenados):**
1.  **Región Norte:** 4000 hosts.
2.  **Región Sur:** 2000 hosts.
3.  **Enlace WAN:** 2 hosts.

### 1. Región Norte (4000 hosts)
*   **Cálculo:** Necesitamos $2^{12} = 4096$. **Hosts ($h$) = 12**.
*   **Máscara:** $32 - 12 = \textbf{/20}$.
*   **Análisis del Salto:**
    *   La máscara /20 cae en el **3er octeto** (está entre 17 y 24).
    *   Límite del octeto (24) - Máscara (20) = 4 bits libres.
    *   Salto = $2^4 = \textbf{16}$.
    *   *Saltamos de 16 en 16 en el 3er octeto.*
*   **Rango:**
    *   Inicio: `10.0.0.0`
    *   **Siguiente Red:** `10.0.16.0` (0 + 16)
    *   **Fin (Broadcast):** `10.0.15.255`

### 2. Región Sur (2000 hosts)
*   **Inicio:** `10.0.16.0`.
*   **Cálculo:** Necesitamos $2^{11} = 2048$. **Hosts ($h$) = 11**.
*   **Máscara:** $32 - 11 = \textbf{/21}$.
*   **Análisis del Salto:**
    *   La máscara /21 cae en el **3er octeto**.
    *   Límite del octeto (24) - Máscara (21) = 3 bits libres.
    *   Salto = $2^3 = \textbf{8}$.
    *   *Saltamos de 8 en 8 en el 3er octeto.*
*   **Rango:**
    *   Inicio: `10.0.16.0`
    *   **Siguiente Red:** `10.0.24.0` (16 + 8)
    *   **Fin (Broadcast):** `10.0.23.255`

### 3. Enlace WAN (2 hosts)
*   **Inicio:** `10.0.24.0`.
*   **Cálculo:** Necesitamos $2^2 = 4$. **Hosts ($h$) = 2**.
*   **Máscara:** $32 - 2 = \textbf{/30}$.
*   **Análisis del Salto:**
    *   La máscara /30 cae en el **4º octeto**.
    *   Salto = $2^2 = 4$.
*   **Rango:**
    *   Inicio: `10.0.24.0`
    *   **Siguiente Red:** `10.0.24.4`
    *   **Fin (Broadcast):** `10.0.24.3`


## Tabla Resumen 

Una vez hechos los cálculos, así es como se ve la "foto final" de las asignaciones.

| Red Base: 172.16.0.0/16 | Hosts Necesarios | Máscara / CIDR | Dirección Red | Primer IP | Última IP | Broadcast |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Edificio Central** | 1000 | /22 | 172.16.0.0 | .0.1 | .3.254 | 172.16.3.255 |
| **Fábrica** | 500 | /23 | 172.16.4.0 | .4.1 | .5.254 | 172.16.5.255 |
| **Almacén** | 50 | /26 | 172.16.6.0 | .6.1 | .6.62 | 172.16.6.63 |



| Red Base: 10.0.0.0/8 | Hosts Necesarios | Máscara / CIDR | Dirección Red | Primer IP | Última IP | Broadcast |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Región Norte** | 4000 | /20 | 10.0.0.0 | .0.0.1 | .0.15.254 | 10.0.15.255 |
| **Región Sur** | 2000 | /21 | 10.0.16.0 | .0.16.1 | .0.23.254 | 10.0.23.255 |
| **Enlace WAN** | 2 | /30 | 10.0.24.0 | .0.24.1 | .0.24.2 | 10.0.24.3 |

