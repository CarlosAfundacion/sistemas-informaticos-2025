

#  Práctica de Subneting e IPv4

Cada ejercicio vale 2 puntos.

---

### 1. Conversión y Análisis de IPs

1.  Detecta si hay alguna **IP inválida** en la lista y táchala.
2.  Convierte las IPs válidas a binario.
3.  Indica su Clase (A, B, C...) y su Máscara por defecto. Usando la siguiente tabla:
   
<img width="945" height="322" alt="imagen" src="https://github.com/user-attachments/assets/d56d1411-79ca-4aac-b6de-9c2ae20a22d1" />


| Dirección IP Decimal | Dirección IP en Binario (32 bits) | Clase | Máscara por defecto |
| :--- | :--- | :---: | :---: |
| **192.168.1.1** | `___________________________________` | | |
| **10.0.5.1** | `___________________________________` | | |
| **172.16.0.25** | `___________________________________` | | |
| **256.1.0.1** | `___________________________________` | | |
| **223.0.0.1** | `___________________________________` | | |

> **Nota:** Recuerda que las "Clases" son un concepto clásico. Hoy en día usamos CIDR (/24, /20, etc.), pero es fundamental conocer las clases para entender las máscaras por defecto.

---

### 2. Cálculo de Subredes (Nivel 1: Fácil)
**Escenario:** Tienes la red **192.168.50.0/24**.
Necesitamos dividirla para crear **4 subredes** distintas.

1.  ¿Cuántos bits necesitas "robar" a la parte de host? ($2^n \ge 4$) -> **Bits:** ____
2.  ¿Cuál es la nueva máscara (CIDR)? -> **/____**
3.  Calcula el "Tamaño del Bloque" (Salto): ________

**Completa la tabla:**
| Nº | Dirección de Red | 1ª IP Utilizable | Última IP Utilizable | Dirección de Broadcast | Máscara Subred |
|:--:|:--- |:--- |:--- |:--- |:--- |
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |

---

### 3. Cálculo de Subredes (Nivel 2: Intermedio)
**Escenario:** Tienes la red **172.16.0.0/16**.
Necesitamos dividirla para obtener **8 subredes** (ej: Planta 1, Planta 2, Cafetería...).

1.  Bits prestados ($2^n \ge 8$): ____
2.  Nueva Máscara CIDR: **/____**
3.  Tamaño del Bloque (Ojo, trabajamos en el **3er octeto**): ________

**Completa la tabla (primeras 4 subredes):**
| Nº | Dirección de Red | 1ª IP Utilizable | Última IP Utilizable | Dirección de Broadcast | Máscara Subred |
|:--:|:--- |:--- |:--- |:--- |:--- |
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |
| 4 | | | | | |

---

### 4. Cálculo de Subredes (Nivel 3: Difícil)
**Escenario:** Un proveedor de internet nos da la red **10.0.0.0/8**.
Necesitamos crear **10 subredes** gigantes.

1.  Bits prestados ($2^n \ge 10$): ____
2.  Nueva Máscara CIDR: **/____**
3.  Tamaño del Bloque (Trabajamos en el **2º octeto**): ________

**Completa la tabla (solo las 3 primeras):**
| Nº | Dirección de Red | 1ª IP Utilizable | Última IP Utilizable | Dirección de Broadcast | Máscara Subred |
|:--:|:--- |:--- |:--- |:--- |:--- |
| 1 | | | | | |
| 2 | | | | | |
| 3 | | | | | |

---

### 5. Escenario real (Despliegue Web)
Vas a desplegar una arquitectura de microservicios en la nube. Te asignan la red `192.168.100.0/24`. Debes dividirla en **4 partes iguales** para separar lógicamente los siguientes servicios:

1.  **Subred 1:** Frontend (Balanceador de carga)
2.  **Subred 2:** Backend (API Django)
3.  **Subred 3:** Base de Datos (MySQL)
4.  **Subred 4:** Backups y Logs

**Pregunta:**
Si configuras el servidor de **Base de Datos** con la **primera IP disponible** de su subred correspondiente (la nº 3)...

*   ¿Qué dirección IP tendrá la Base de Datos? __________________
*   ¿Cuál será su Puerta de Enlace (Gateway) si usamos la última IP disponible de esa subred? __________________
*   ¿Cuál es la dirección de Broadcast de la subred de Backend? __________________
