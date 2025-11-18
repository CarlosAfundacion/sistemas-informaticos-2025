### **Resolución del Problema 1**

Se nos presentan los siguientes datos:
*   **P1:** Duración = 6 ms, Tiempo de llegada = 0 ms
*   **P2:** Duración = 5 ms, Tiempo de llegada = 1 ms
*   **P3:** Duración = 2 ms, Tiempo de llegada = 3 ms
*   **P4:** Duración = 4 ms, Tiempo de llegada = 4 ms
*   **Round Robin (RR):** Quantum (Q) = 2 ms

Calcularemos el Tiempo de Finalización (TF), Tiempo de Retorno (TR) y Tiempo de Espera (TE) para cada proceso.

*   **Tiempo de Retorno (TR)** = Tiempo de Finalización - Tiempo de Llegada
*   **Tiempo de Espera (TE)** = Tiempo de Retorno - Duración

---

#### **Algoritmo FIFO (First-In, First-Out)**

Los procesos se ejecutan en el orden en que llegan: P1 → P2 → P3 → P4.

**Ejecución:**
1.  **P1** empieza en t=0 y termina en t=6.
2.  **P2** empieza en t=6 y termina en t=11 (6+5).
3.  **P3** empieza en t=11 y termina en t=13 (11+2).
4.  **P4** empieza en t=13 y termina en t=17 (13+4).

**Tabla de resultados para FIFO:**

| Proceso | Duración | T. Llegada (t) | T. Finalización (TF) | T. Retorno (TR) | T. Espera (TE) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| P1 | 6 | 0 | 6 | 6 | 0 |
| P2 | 5 | 1 | 11 | 10 | 5 |
| P3 | 2 | 3 | 13 | 10 | 8 |
| P4 | 4 | 4 | 17 | 13 | 9 |
| **Totales** | | | | **39** | **22** |

*   **Tiempo medio de espera:** 22 ms / 4 = **5,5 ms**
*   **Tiempo medio de retorno:** 39 ms / 4 = **9,75 ms**

---

#### **Algoritmo SJF (Shortest Job First - No Apropiativo)**

El sistema ejecuta el proceso más corto de entre los que han llegado.

**Ejecución:**
1.  **t=0:** Solo ha llegado P1. Se ejecuta **P1**. Termina en t=6.
2.  **t=6:** P1 ha terminado. Han llegado P2 (Dur=5), P3 (Dur=2) y P4 (Dur=4). El más corto es **P3**. Se ejecuta. Empieza en t=6 y termina en t=8 (6+2).
3.  **t=8:** P3 ha terminado. Quedan P2 (Dur=5) y P4 (Dur=4). El más corto es **P4**. Se ejecuta. Empieza en t=8 y termina en t=12 (8+4).
4.  **t=12:** P4 ha terminado. Solo queda **P2**. Se ejecuta. Empieza en t=12 y termina en t=17 (12+5).

**Tabla de resultados para SJF:**

| Proceso | Duración | T. Llegada (t) | T. Finalización (TF) | T. Retorno (TR) | T. Espera (TE) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| P1 | 6 | 0 | 6 | 6 | 0 |
| P2 | 5 | 1 | 17 | 16 | 11 |
| P3 | 2 | 3 | 8 | 5 | 3 |
| P4 | 4 | 4 | 12 | 8 | 4 |
| **Totales** | | | | **35** | **18** |

*   **Tiempo medio de espera:** 18 ms / 4 = **4,5 ms**
*   **Tiempo medio de retorno:** 35 ms / 4 = **8,75 ms**

---

#### **Algoritmo RR (Round Robin, Q=2)**

Cada proceso se ejecuta por un máximo de 2 ms. Si no termina, vuelve al final de la cola.

**Ejecución:**
*   **t=0-2:** P1 se ejecuta (quedan 4ms). Llega P2 en t=1. Cola: [P2, P1].
*   **t=2-4:** P2 se ejecuta (quedan 3ms). Llega P3 en t=3 y P4 en t=4. Cola: [P1, P3, P4, P2].
*   **t=4-6:** P1 se ejecuta (quedan 2ms). Cola: [P3, P4, P2, P1].
*   **t=6-8:** P3 se ejecuta (quedan 0ms). **P3 termina**. Cola: [P4, P2, P1].
*   **t=8-10:** P4 se ejecuta (quedan 2ms). Cola: [P2, P1, P4].
*   **t=10-12:** P2 se ejecuta (quedan 1ms). Cola: [P1, P4, P2].
*   **t=12-14:** P1 se ejecuta (quedan 0ms). **P1 termina**. Cola: [P4, P2].
*   **t=14-16:** P4 se ejecuta (quedan 0ms). **P4 termina**. Cola: [P2].
*   **t=16-17:** P2 se ejecuta (quedan 0ms). **P2 termina**.

**Tabla de resultados para RR (Q=2):**

| Proceso | Duración | T. Llegada (t) | T. Finalización (TF) | T. Retorno (TR) | T. Espera (TE) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| P1 | 6 | 0 | 14 | 14 | 8 |
| P2 | 5 | 1 | 17 | 16 | 11 |
| P3 | 2 | 3 | 8 | 5 | 3 |
| P4 | 4 | 4 | 16 | 12 | 8 |
| **Totales** | | | | **47** | **30** |

*   **Tiempo medio de espera:** 30 ms / 4 = **7,5 ms**
*   **Tiempo medio de retorno:** 47 ms / 4 = **11,75 ms**

### **Resolución del Problema 2**

Se nos presentan los siguientes datos:
*   **P1:** Duración = 5 ms, Tiempo de llegada = 0 ms
*   **P2:** Duración = 7 ms, Tiempo de llegada = 1 ms
*   **P3:** Duración = 3 ms, Tiempo de llegada = 2 ms
*   **P4:** Duración = 4 ms, Tiempo de llegada = 5 ms
*   **Round Robin (RR):** Quantum (Q) = 3 ms

---

#### **Algoritmo FIFO (First-In, First-Out)**

Los procesos se ejecutan en el orden en que llegan: P1 → P2 → P3 → P4.

**Ejecución:**
1.  **P1** empieza en t=0 y termina en t=5.
2.  **P2** empieza en t=5 y termina en t=12 (5+7).
3.  **P3** empieza en t=12 y termina en t=15 (12+3).
4.  **P4** empieza en t=15 y termina en t=19 (15+4).

**Tabla de resultados para FIFO:**

| Proceso | Duración | T. Llegada (t) | T. Finalización (TF) | T. Retorno (TR) | T. Espera (TE) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| P1 | 5 | 0 | 5 | 5 | 0 |
| P2 | 7 | 1 | 12 | 11 | 4 |
| P3 | 3 | 2 | 15 | 13 | 10 |
| P4 | 4 | 5 | 19 | 14 | 10 |
| **Totales** | | | | **43** | **24** |

*   **Tiempo medio de espera:** 24 ms / 4 = **6 ms**
*   **Tiempo medio de retorno:** 43 ms / 4 = **10,75 ms**

---

#### **Algoritmo SJF (Shortest Job First - No Apropiativo)**

**Ejecución:**
1.  **t=0:** Solo ha llegado P1. Se ejecuta **P1**. Termina en t=5.
2.  **t=5:** P1 ha terminado. Han llegado P2 (Dur=7), P3 (Dur=3) y P4 (Dur=4). El más corto es **P3**. Se ejecuta. Empieza en t=5 y termina en t=8 (5+3).
3.  **t=8:** P3 ha terminado. Quedan P2 (Dur=7) y P4 (Dur=4). El más corto es **P4**. Se ejecuta. Empieza en t=8 y termina en t=12 (8+4).
4.  **t=12:** P4 ha terminado. Solo queda **P2**. Se ejecuta. Empieza en t=12 y termina en t=19 (12+7).

**Tabla de resultados para SJF:**

| Proceso | Duración | T. Llegada (t) | T. Finalización (TF) | T. Retorno (TR) | T. Espera (TE) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| P1 | 5 | 0 | 5 | 5 | 0 |
| P2 | 7 | 1 | 19 | 18 | 11 |
| P3 | 3 | 2 | 8 | 6 | 3 |
| P4 | 4 | 5 | 12 | 7 | 3 |
| **Totales** | | | | **36** | **17** |

*   **Tiempo medio de espera:** 17 ms / 4 = **4,25 ms**
*   **Tiempo medio de retorno:** 36 ms / 4 = **9 ms**

---

#### **Algoritmo RR (Round Robin, Q=3)**

**Ejecución:**
*   **t=0-3:** P1 se ejecuta (quedan 2ms). Llegan P2 (t=1) y P3 (t=2). Cola: [P2, P3, P1].
*   **t=3-6:** P2 se ejecuta (quedan 4ms). Llega P4 (t=5). Cola: [P3, P1, P4, P2].
*   **t=6-9:** P3 se ejecuta (quedan 0ms). **P3 termina**. Cola: [P1, P4, P2].
*   **t=9-11:** P1 se ejecuta (quedan 0ms). **P1 termina**. Cola: [P4, P2].
*   **t=11-14:** P4 se ejecuta (quedan 1ms). Cola: [P2, P4].
*   **t=14-17:** P2 se ejecuta (quedan 1ms). Cola: [P4, P2].
*   **t=17-18:** P4 se ejecuta (quedan 0ms). **P4 termina**. Cola: [P2].
*   **t=18-19:** P2 se ejecuta (quedan 0ms). **P2 termina**.

**Tabla de resultados para RR (Q=3):**

| Proceso | Duración | T. Llegada (t) | T. Finalización (TF) | T. Retorno (TR) | T. Espera (TE) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| P1 | 5 | 0 | 11 | 11 | 6 |
| P2 | 7 | 1 | 19 | 18 | 11 |
| P3 | 3 | 2 | 9 | 7 | 4 |
| P4 | 4 | 5 | 18 | 13 | 9 |
| **Totales** | | | | **49** | **30** |

*   **Tiempo medio de espera:** 30 ms / 4 = **7,5 ms**
*   **Tiempo medio de retorno:** 49 ms / 4 = **12,25 ms**
