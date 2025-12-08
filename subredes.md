# Guía Subneting


## 1. ¿Qué es y por qué lo hacemos?
Imaginad que tenéis una **pizza gigante** (vuestra red principal). Si sois 4 amigos, no tiene sentido que uno se coma toda la pizza y los otros miren. **Subneting es cortar esa pizza en porciones más pequeñas** para repartirla eficientemente.

*   **Sin subneting:** Una sola red gigante donde todos gritan a la vez (mucho tráfico de broadcast).
*   **Con subneting:** Pequeñas redes organizadas, más seguras y rápidas.

---

## 2. Herramientas previas (La "Caja de Herramientas")
Tabla de salvación para convertir binario a decimal:

| Bit | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
|:---:|:---:|:--:|:--:|:--:|:-:|:-:|:-:|:-:|
| **Potencia ($2^n$)** | $2^7$ | $2^6$ | $2^5$ | $2^4$ | $2^3$ | $2^2$ | $2^1$ | $2^0$ |

> **Regla de Oro:** Una dirección IPv4 tiene 32 bits divididos en 4 octetos.
> `/24` significa que los primeros 24 bits son RED y los últimos 8 son HOSTS.

---

## 3. Las Fórmulas Mágicas
Solo necesitáis recordar dos fórmulas:

1.  **Para calcular Subredes (cuántos trozos de pizza):**
    $$2^S \ge \text{número de subredes necesarias}$$
    *(Donde $S$ son los bits que "robamos" a la parte de host).*

2.  **Para calcular Hosts (cuántos ordenadores caben):**
    $$2^H - 2 \ge \text{número de hosts necesarios}$$
    *(Donde $H$ son los bits que sobran. El $-2$ es porque la primera IP es la Red y la última el Broadcast).*

---

## 4. Paso a paso

### Escenario
Nos dan la red **192.168.1.0/24** (la típica de casa).
El jefe nos pide crear **4 subredes** para distintos departamentos (Ventas, RRHH, Devs, Dirección).

### Paso 1: Calcular cuántos bits necesitamos robar($S$)
Usamos la fórmula de subredes: $2^S \ge 4$.
*   $2^1 = 2$ (No llega)
*   $2^2 = 4$ (¡Exacto!)

Necesitamos **robar 2 bits** a la parte de host.

### Paso 2: Calcular la Nueva Máscara
La máscara original era `/24`.
$$24 (\text{original}) + 2 (\text{robados}) = /26$$

**En binario:**
Antes: `11111111.11111111.11111111.00000000` (/24)
Ahora: `11111111.11111111.11111111.11000000` (/26) -> Los dos unos del último octeto son los robados.

**En decimal (Para configurarciones):**
Miramos el último octeto: `11000000`.
Sumamos posiciones de la tabla: $128 + 64 = 192$.
**Nueva máscara:** `255.255.255.192`

### Paso 3: Calcular el "Número Mágico" (El Salto)
Este es el truco para no volverse loco sumando binarios.
$$256 - \text{Mascara Decimal del octeto modificado} = \text{SALTO}$$

En nuestro caso:
$$256 - 192 = 64$$

**¡Nuestras subredes van de 64 en 64!**

### Paso 4: Construir la tabla de subredes
Empezamos en 0 y sumamos el "número mágico" (64).

1.  **Subred 1:** 192.168.1.**0**
2.  **Subred 2:** 192.168.1.**64**
3.  **Subred 3:** 192.168.1.**128**
4.  **Subred 4:** 192.168.1.**192**
5.  *(Siguiente salto sería 256, que es el tope, así que paramos)*.

### Paso 5: Rellenar los detalles (IPs útiles y Broadcast)
Para cada subred:
*   **Primera IP útil:** ID de Red + 1.
*   **Broadcast:** La IP justo antes de la siguiente subred.
*   **Última IP útil:** Broadcast - 1.

| Nombre | ID de Red | Primera IP (Host) | Última IP (Host) | Broadcast |
| :--- | :--- | :--- | :--- | :--- |
| **Subred A** | **192.168.1.0** | 192.168.1.1 | 192.168.1.62 | 192.168.1.63 |
| **Subred B** | **192.168.1.64** | 192.168.1.65 | 192.168.1.126 | 192.168.1.127 |
| **Subred C** | **192.168.1.128** | 192.168.1.129 | 192.168.1.190 | 192.168.1.191 |
| **Subred D** | **192.168.1.192** | 192.168.1.193 | 192.168.1.254 | 192.168.1.255 |

---

## 5. Ejemplo inverso (Por número de hosts)
*A veces el problema no es "¿cuántas redes?", sino "¿cuántos ordenadores?".*

**Escenario:** Red 192.168.10.0/24. Necesitamos subredes que soporten **30 ordenadores (hosts)** cada una.

### Paso 1: Buscar $H$ (bits de host)
Fórmula: $2^H - 2 \ge 30$.
*   $2^4 = 16 - 2 = 14$ (No llega)
*   $2^5 = 32 - 2 = 30$ (¡Justo!)

Necesitamos dejar **5 bits** libres para hosts ($H=5$).

### Paso 2: Calcular la Nueva Máscara
Total de bits IPv4 = 32.
Si reservamos 5 para hosts, los de red son: $32 - 5 = 27$.
**Nueva máscara CIDR:** `/27`

**En binario:**
`11111111.11111111.11111111.11100000` (Quedan 5 ceros al final).

**En decimal:**
Los 3 bits de red en el último octeto son `11100000`.
$128 + 64 + 32 = 224$.
Mascara: `255.255.255.224`.

### Paso 3: Número Mágico
$$256 - 224 = 32$$
El salto es de **32**.

### Paso 4: Listado rápido
*   192.168.10.0
*   192.168.10.32
*   192.168.10.64
*   ... y así sucesivamente.

---
