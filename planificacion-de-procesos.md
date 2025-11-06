

# Planificación de procesos: FIFO, SJF, SRT y Round Robin

Los sistemas operativos multitarea gestionan la ejecución de múltiples procesos utilizando **algoritmos de planificación**, cuyo objetivo principal es **decidir qué proceso obtiene el procesador (CPU)** en cada instante.

Cada algoritmo tiene ventajas y desventajas según el entorno (por ejemplo, servidores, ordenadores personales o sistemas interactivos).

A continuación, se estudia un ejemplo donde los algoritmos **FIFO**, **SJF**, **SRT** y **Round Robin (Q=2)** producen resultados distintos.
Esto permitirá comparar su comportamiento y comprender conceptos clave como la **preempción**.

---

##  Concepto previo: Preempción

La **preempción** ocurre cuando el sistema operativo **interrumpe** un proceso que estaba en ejecución para **asignar la CPU a otro proceso** que tiene **mayor prioridad o menor tiempo restante**.

 En algoritmos **no expropiativos**, el proceso actual **nunca se interrumpe** hasta que termina.
 En algoritmos **expropiativos**, **sí puede ser interrumpido** si llega uno que debe ejecutarse antes.

---

##  Datos del ejemplo

| Proceso | Duración (ms) | Tiempo de llegada (t) |
| ------- | ------------- | --------------------- |
| **P1**  | 6             | 0                     |
| **P2**  | 4             | 2                     |
| **P3**  | 2             | 4                     |

---

#  FIFO (First In, First Out)

###  Funcionamiento

* Es el algoritmo más simple: **el primero en llegar es el primero en ejecutarse**.
* No hay **preempción**: un proceso se ejecuta hasta el final antes de que empiece el siguiente.

###  Paso a paso

1. **t=0:** llega P1 → comienza a ejecutarse.
2. **t=2:** llega P2 → debe esperar a que P1 termine.
3. **t=4:** llega P3 → también espera.
4. **t=6:** termina P1 → entra P2.
5. **t=10:** termina P2 → entra P3.
6. **t=12:** termina P3.

**Orden:** P1 → P2 → P3


###  Cálculos

| Proceso | TF | TR (TF−lleg) | TE (TR−CPU) |
| ------- | -: | -----------: | ----------: |
| P1      |  6 |            6 |           0 |
| P2      | 10 |            8 |           4 |
| P3      | 12 |            8 |           6 |

**Tiempos medios:**

* Espera media = (0+4+6)/3 = **3,33**
* Turnaround medio = (6+8+8)/3 = **7,33**

Si no te queda claro, puedes ver este [vídeo](https://www.youtube.com/watch?v=kXRfbsqcVTg&list=PLfOTrXMEu79APWooqUXvktu3PyD1639Z0&index=6)

---

#  SJF (Shortest Job First, no expropiativo)

###  Funcionamiento

* El sistema elige siempre **el proceso con menor duración total** entre los que **ya han llegado**.
* Una vez empieza un proceso, **no se interrumpe** (no hay preempción).

###  Paso a paso

1. **t=0:** solo está P1 (6) → se ejecuta.
2. **t=2:** llega P2 (4) → espera; P1 sigue.
3. **t=4:** llega P3 (2) → espera; P1 sigue.
4. **t=6:** termina P1 → elige entre P2 (4) y P3 (2): ejecuta **P3** (más corto).
5. **t=8:** termina P3 → ejecuta **P2**.
6. **t=12:** termina P2.

**Orden:** P1 → P3 → P2


###  Cálculos

| Proceso | TF | TR | TE |
| ------- | -: | -: | -: |
| P1      |  6 |  6 |  0 |
| P2      | 12 | 10 |  6 |
| P3      |  8 |  4 |  2 |

**Tiempos medios:**

* Espera media = (0+6+2)/3 = **2,67**
* Turnaround medio = (6+10+4)/3 = **6,67**

Si no te queda claro, puedes ver este [vídeo](https://www.youtube.com/watch?v=miIi7udHjiU&list=PLfOTrXMEu79APWooqUXvktu3PyD1639Z0&index=7)


---

# SRT (Shortest Remaining Time, expropiativo)

### Funcionamiento

* Variante expropiativa de SJF.
* En cada instante, el sistema ejecuta **el proceso con menor tiempo restante de CPU**.
* Si llega un proceso con duración más corta que el tiempo restante del actual, **se interrumpe el proceso en ejecución (preempción)** y se ejecuta el nuevo.

### Paso a paso

1. **t=0–2:** llega solo **P1(6)** → ejecuta P1 (quedan 4 ms).
2. **t=2:** llega **P2(4)**.

   * Compara: P1(restante=4) y P2(4) → igual → sigue P1.
3. **t=4:** llega **P3(2)**.

   * Compara: P1(restante=2), P2(4), P3(2).
   * Hay empate entre P1 y P3, pero **P3 acaba de llegar** → sigue P1.
4. **t=4–6:** ejecuta **P1** → termina.
5. **t=6–8:** retoma **P3** (le quedaban 2 ms) → termina.
6. **t=8–12:** ejecuta **P2** (queda completo).

**Orden de ejecución:** P1 → (preempción) → P3 → P1 → P2



###  Cálculos

| Proceso | TF | TR | TE |
| ------- | -: | -: | -: |
| P1      |  8 |  8 |  2 |
| P2      | 12 | 10 |  6 |
| P3      |  6 |  2 |  0 |

**Tiempos medios:**

* Espera media = (2+6+0)/3 = **2,67**
* Turnaround medio = (8+10+2)/3 = **6,67**

 Observa que los promedios coinciden con SJF, pero los **tiempos individuales** cambian porque SRT da **preferencia inmediata a los procesos cortos recién llegados**.

Si no te queda claro, puedes ver este [vídeo](https://www.youtube.com/watch?v=miIi7udHjiU&list=PLfOTrXMEu79APWooqUXvktu3PyD1639Z0&index=8)

---

#  RR (Round Robin, Q = 2 ms)

### Funcionamiento

* Cada proceso recibe un **cuanto de tiempo (Q)** fijo, en este caso **2 ms**.
* Cuando un proceso agota su cuanto, **se interrumpe (preempción)** y pasa al **final de la cola de listos** si no ha terminado.
* Es **expropiativo** y equitativo: todos los procesos tienen oportunidad de ejecutarse regularmente.

### Paso a paso

**Cola inicial:** t=0 → [P1].

* **0–2:** P1 ejecuta (le quedan 4 ms).
* **t=2:** llega P2(4).

  * Cola: [P2, P1].
* **2–4:** ejecuta P2 (le quedan 2 ms).
* **t=4:** llega P3(2).

  * Cola: [P1, P3, P2].
* **4–6:** ejecuta P1 (le quedan 2 ms).
* **6–8:** ejecuta P3 (termina).
* **8–10:** ejecuta P2 (termina).
* **10–12:** ejecuta P1 (termina).

**Orden:** P1 → P2 → P1 → P3 → P2 → P1



### Cálculos

| Proceso | TF | TR | TE |
| ------- | -: | -: | -: |
| P1      | 12 | 12 |  6 |
| P2      | 10 |  8 |  4 |
| P3      |  8 |  4 |  2 |

**Tiempos medios:**

* Espera media = (6+4+2)/3 = **4,00**
* Turnaround medio = (12+8+4)/3 = **8,00**

Si no te queda claro, puedes ver este [vídeo](https://www.youtube.com/watch?v=miIi7udHjiU&list=PLfOTrXMEu79APWooqUXvktu3PyD1639Z0&index=10)

---

# Comparativa general

| Algoritmo    | Tipo            | Preempción | Orden de ejecución          | Espera media | Turnaround medio | Comentario                                                |
| ------------ | --------------- | ---------- | --------------------------- | ------------ | ---------------- | --------------------------------------------------------- |
| **FIFO**     | No expropiativo | NO          | P1 → P2 → P3                | 3,33         | 7,33             | Simple, pero puede castigar procesos cortos.              |
| **SJF**      | No expropiativo | NO          | P1 → P3 → P2                | 2,67         | 6,67             | Eficiente, pero injusto para procesos largos.             |
| **SRT**      | Expropiativo    | SÍ          | P1 → P3 → P1 → P2           | 2,67         | 6,67             | Prioriza procesos cortos recién llegados; reduce esperas. |
| **RR (Q=2)** | Expropiativo    | SÍ            | P1 → P2 → P1 → P3 → P2 → P1 | 4,00         | 8,00             | Equitativo; ideal para sistemas interactivos.             |

---

# Conclusiones

1. **FIFO** es justo cronológicamente, pero **ineficiente**: un proceso largo puede bloquear a todos los demás.
2. **SJF** mejora la eficiencia media, pero **no interrumpe** al proceso actual, por lo que los cortos que lleguen después **deben esperar**.
3. **SRT** introduce la **preempción**: si llega un proceso más corto, el actual se detiene y el nuevo se ejecuta.

   * Minimiza el **tiempo medio de espera**.
   * Pero puede perjudicar a los procesos largos (problema de *starvation*).
4. **Round Robin** es también **expropiativo**, pero equitativo: todos avanzan por turnos de igual duración.

   * Mejora la **interactividad**, aunque penaliza la eficiencia total.

# Ampliación

Si te parece interesante, puedes ver cómo funciona el algoritmo de planificación apropiativo por [Prioridades Fijas](https://www.youtube.com/watch?v=YPTfcCHiYss&list=PLfOTrXMEu79APWooqUXvktu3PyD1639Z0&index=9)

---

