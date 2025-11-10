# 🖥️ Hardware de un Sistema Informático

## 1. Computadores digitales y su evolución

Los **computadores digitales** son los que usamos hoy en día. Procesan la información en forma de **bits** (0 y 1), representando los estados **encendido (1)** y **apagado (0)** de los transistores que los componen.
Estos bits se agrupan en **bytes** (8 bits) y son la base de todas las operaciones del ordenador.

Un **ordenador digital** trabaja con datos **discretos y finitos**, frente a los **analógicos**, que operan con señales **continuas e infinitas** (como una radio o un termómetro antiguo).
Aunque los sistemas digitales pierden algo de precisión al convertir señales analógicas, son **más exactos, rápidos y resistentes al ruido**, además de ser **más económicos y reproducibles**.

---

### 📜 Evolución de los computadores

| Generación      | Periodo       | Tecnología                                     | Características principales                                        |
| --------------- | ------------- | ---------------------------------------------- | ------------------------------------------------------------------ |
| **1ª**          | 1940-1960     | Válvulas de vacío                              | Muy grandes, poco potentes. Ej: *ENIAC* (30 toneladas).            |
| **2ª**          | 1960-1965     | Transistores                                   | Menor tamaño y consumo. Mayor fiabilidad.                          |
| **3ª**          | 1965-1975     | Circuitos integrados                           | Más velocidad y miniaturización.                                   |
| **4ª**          | 1975-presente | Microprocesadores                              | Integración total de la CPU en un chip (arquitectura Von Neumann). |
| **5ª**          | 1985-presente | IA, sistemas expertos                          | Incorporación de redes neuronales y aprendizaje automático.        |
| **Actual (6ª)** | 2020-hoy      | Computación cuántica, IA y chips neuromórficos | Procesamiento paralelo masivo, hardware para IA y bajo consumo.    |

---

## 2. Arquitectura Von Neumann

Diseñada por **John Von Neumann** en 1945, esta arquitectura sigue siendo la base de los ordenadores modernos.
Su idea clave fue que **los datos y las instrucciones se almacenan juntos en la memoria principal**, de forma que el procesador no distingue entre ambos.

### 🧩 Elementos principales

1. **CPU (Unidad Central de Proceso)**
   Controla, coordina y ejecuta las instrucciones.
2. **Memoria principal (RAM)**
   Guarda temporalmente datos e instrucciones.
3. **Unidades de entrada/salida (E/S)**
   Permiten la comunicación con el exterior (teclado, disco, monitor...).
4. **Buses**
   Conectan todos los componentes.

### ⚙️ Tipos de buses

* **Bus de datos:** transporta información entre memoria y CPU.
* **Bus de direcciones:** indica dónde se guardan los datos.
* **Bus de control:** coordina las operaciones de lectura/escritura.

En los ordenadores modernos, los **buses de 64 bits** permiten transmitir 8 bytes en paralelo, como una autopista de 64 carriles.

---

### 🔁 Ciclo de ejecución de una instrucción

1. **Búsqueda (fetch):** la CPU obtiene la instrucción desde la memoria (registro CP → RI).
2. **Decodificación:** la Unidad de Control interpreta qué hacer.
3. **Ejecución:** la ALU realiza la operación (suma, comparación, etc.).
4. **Almacenamiento:** el resultado se guarda en un registro o en memoria.

Este proceso se repite millones de veces por segundo.

---

### 🧠 Jerarquía de memoria

| Nivel | Tipo               | Capacidad   | Velocidad | Volatilidad |
| ----- | ------------------ | ----------- | --------- | ----------- |
| 1     | Registros          | Muy pequeña | Máxima    | Sí          |
| 2     | Caché (L1, L2, L3) | Pequeña     | Muy alta  | Sí          |
| 3     | RAM                | Media       | Alta      | Sí          |
| 4     | SSD / HDD          | Alta        | Media     | No          |
| 5     | Nube / Red         | Muy alta    | Baja      | No          |

---

## 3. La CPU o microprocesador

El **microprocesador** es el “cerebro” del ordenador. Está formado por millones de transistores integrados en una pequeña pastilla de silicio.

### 🔹 Partes principales

* **ALU (Unidad Aritmético-Lógica):** realiza operaciones matemáticas y lógicas.
* **UC (Unidad de Control):** gestiona la secuencia de ejecución.
* **Registros:** almacenan datos temporales y direcciones.
* **Caché:** memoria ultrarrápida para evitar acceder constantemente a la RAM.

### 🔹 Características técnicas

* **Frecuencia de reloj:** GHz (1 GHz = 1.000 millones de ciclos/segundo).
* **Litografía:** tamaño de los transistores (actualmente 3-5 nm).
* **Núcleos:** cada uno ejecuta un proceso.
* **Hilos:** subdivisiones virtuales (Hyper-Threading, SMT).
* **TDP:** consumo térmico (entre 35 y 150 W).
* **Arquitectura:** x86-64 (Intel/AMD), ARM (Apple M-series, móviles, portátiles ultraligeros).

### 🔹 Memoria caché

* **L1:** muy rápida, separada para datos e instrucciones (32-128 KB).
* **L2:** por núcleo (256 KB–1 MB).
* **L3:** compartida (8-96 MB en procesadores actuales).
  A mayor nivel → mayor tamaño, menor velocidad.

### 🔹 Refrigeración

Ventiladores, disipadores, **refrigeración líquida** o incluso **soluciones híbridas** en equipos de alto rendimiento.

---

## 4. Memoria RAM

La **RAM (Random Access Memory)** es la memoria principal donde se cargan temporalmente los programas y datos en ejecución.
Es **volátil**, se borra al apagar el equipo.

### 🧱 Tipos y evolución

| Tipo     | Contactos | Voltaje   | Frecuencia        | Año      |
| -------- | --------- | --------- | ----------------- | -------- |
| DDR      | 184       | 2.5 V     | 266–400 MHz       | 2000     |
| DDR2     | 240       | 1.8 V     | 533–1200 MHz      | 2003     |
| DDR3     | 240       | 1.5 V     | 800–2133 MHz      | 2007     |
| DDR4     | 288       | 1.2 V     | 2133–3600 MHz     | 2014     |
| **DDR5** | **288**   | **1.1 V** | **4800–8800 MHz** | **2021** |

### ⚙️ Parámetros importantes

* **Velocidad:** frecuencia de reloj (MHz o GHz).
* **Latencia (CL):** tiempo que tarda en acceder al primer dato.
* **Ancho de banda:** datos transferidos por segundo.
* **Canales:** dual, triple o quad channel (duplican o multiplican la velocidad efectiva).

Ejemplo:
Una DDR4-3200 (3,2 GHz) en doble canal puede superar los **51 GB/s** de transferencia.

---

## 5. Componentes principales de un ordenador

### 🧰 Caja o chasis

* Tipos: **torre, semitorre, mini-ITX, sobremesa**.
* Funciones: albergar y proteger los componentes, ventilar y organizar cables.
* Se clasifican por su **factor de forma**, que debe coincidir con el de la **placa base** (ATX, micro-ATX, mini-ITX…).

---

### ⚡ Fuente de alimentación (PSU)

Convierte la corriente alterna (230 V) en continua (12 V, 5 V y 3.3 V).
Su potencia depende del hardware (500-850 W son habituales).

**Certificación 80 PLUS**: mide eficiencia energética (Bronze → Titanium).
**Conectores:**

* ATX 24 pines (placa base)
* EPS 8 pines (CPU)
* PCIe 6/8 pines (GPU)
* SATA y Molex (almacenamiento y periféricos)

---

### 🎮 Tarjeta gráfica (GPU)

Convierte los datos digitales en imágenes visibles.

* **GPU:** procesador gráfico (NVIDIA, AMD, Intel Arc).
* **VRAM:** memoria de vídeo GDDR6 o GDDR6X.
* **Conectores:** HDMI 2.1, DisplayPort 2.1, USB-C DisplayLink.
  Las GPUs modernas también realizan tareas de **IA y computación paralela** (CUDA, OpenCL).

---

## 6. Placa base (Motherboard)

Es el circuito que conecta todos los componentes del sistema.
Define la **compatibilidad** entre CPU, memoria, tarjetas y conectores.

### 🔹 Elementos principales

* **Zócalo del procesador** (LGA 1700, AM5, etc.)
* **Ranuras DIMM** para RAM
* **Conectores de energía** (ATX 24 p, CPU 8 p)
* **Ranuras de expansión PCIe** (x1, x4, x16)
* **Conectores SATA y M.2 (NVMe)**
* **Chipset:** controla buses, puertos y comunicaciones
* **BIOS/UEFI:** firmware de inicio y configuración
* **Panel frontal y trasero:** USB, audio, red, vídeo, etc.

### 🔹 Chipset

Antes se dividía en:

* **Northbridge:** CPU ↔ RAM y GPU (ya integrado en el procesador).
* **Southbridge:** periféricos, USB, SATA, audio, red.

Hoy todo está unificado en un **único chipset** conectado al procesador.

### 🔹 BIOS y UEFI

La **BIOS (Basic Input/Output System)** fue reemplazada por la **UEFI**, con soporte para discos GPT, arranque seguro, interfaz gráfica y actualizaciones fáciles desde el propio firmware.

---

## 7. Dispositivos de almacenamiento

### 💾 Tipos

1. **HDD (disco duro mecánico):** barato y duradero, pero lento.

   * Capacidad: hasta 20 TB.
   * Velocidad: 100–200 MB/s.
   * 5400 o 7200 rpm.
2. **SSD (unidad de estado sólido):** rápido y silencioso.

   * Sin partes móviles.
   * Velocidad: 500 MB/s (SATA) a 14 GB/s (PCIe 5.0 NVMe).
   * Limitadas reescrituras, aunque muy elevadas (≈ 1 PBW).
3. **NVMe (PCIe 4.0/5.0):** formato M.2 de 1.8″ o tarjetas PCIe.
4. **Discos externos / Pendrives / Tarjetas SD:** portátiles y de bajo coste.

| Interfaz      | Velocidad | Uso actual         |
| ------------- | --------- | ------------------ |
| IDE/PATA      | 133 MB/s  | Obsoleto           |
| SATA III      | 600 MB/s  | Estándar doméstico |
| NVMe PCIe 4.0 | 7 GB/s    | Gama alta          |
| NVMe PCIe 5.0 | 14 GB/s   | Última generación  |
| SAS           | 12 Gb/s   | Servidores         |

---

## 8. Periféricos de entrada y salida

Los **periféricos** amplían las capacidades del sistema informático.

### 🖱️ Entrada

Teclado, ratón, escáner, cámaras, micrófonos, lectores de huellas, tabletas digitalizadoras…

### 🖥️ Salida

Monitores, impresoras, altavoces, proyectores, trazadores (plotter).

### 🔄 Entrada/Salida

Tarjetas de red, pantallas táctiles, unidades USB.

---

### 🔹 Monitores

* **Tecnologías:** LCD, LED, OLED, Mini-LED.
* **Resoluciones comunes:** Full HD (1920×1080), 2K, 4K, 8K.
* **Frecuencia:** 60-240 Hz.
* **Conectores:** HDMI, DisplayPort, USB-C.
* **Dot pitch:** menor distancia entre píxeles = mayor nitidez.

---

### 🖨️ Impresoras

* **Matriciales:** impacto, papel continuo (en desuso).
* **Inyección de tinta:** alta calidad, pero consumibles caros.
* **Láser:** económicas a gran volumen, muy usadas en empresas.

---

### 📸 Escáner

Convierte imágenes analógicas en digitales (medido en **ppp** o **dpi**).
Ejemplo: una foto de 6×4 pulgadas a 300 ppp produce una imagen de **2 megapíxeles**.

---

### 🔌 Conexiones y protocolos modernos

| Estándar              | Año  | Velocidad |
| --------------------- | ---- | --------- |
| USB 2.0               | 2000 | 60 MB/s   |
| USB 3.2 Gen 2x2       | 2019 | 20 Gb/s   |
| USB 4 / Thunderbolt 4 | 2021 | 40 Gb/s   |
| HDMI 2.1              | 2020 | 48 Gb/s   |
| DisplayPort 2.1       | 2022 | 80 Gb/s   |

---

## 9. Montaje del ordenador

### 🧩 Pasos generales

1. **Instalar la placa base** en la caja con separadores.
2. **Colocar el procesador** en su zócalo y aplicar **pasta térmica**.
3. **Montar el disipador o ventilador** y conectar a CPU_FAN.
4. **Insertar la RAM** en las ranuras adecuadas.
5. **Conectar la fuente** a la placa base (24 p + 8 p).
6. **Instalar discos SSD/HDD** y conectar datos (SATA/NVMe) y energía.
7. **Añadir GPU o tarjetas PCIe**.
8. **Conectar cables del panel frontal, USB y audio.**
9. **Verificar conexiones** y arrancar.

### ⚠️ Medidas de seguridad

* Desconectar el equipo de la red eléctrica.
* Usar **pulsera antiestática** o tocar una superficie metálica.
* No forzar conectores.
* Mantener orden y limpieza en el área de trabajo.

---

## 🔮 Tendencias actuales del hardware (2025)

* **Procesadores híbridos** (núcleos P/E: rendimiento y eficiencia).
* **RAM DDR5 y PCIe 5.0** como nuevos estándares.
* **GPUs con núcleos Tensor y Ray-Tracing**.
* **Almacenamiento NVMe 5.0 y SSDs de 14 GB/s.**
* **UEFI avanzada con actualizaciones automáticas.**
* **Placas base con Wi-Fi 6E, Bluetooth 5.3 y USB 4.0.**
* **Computación cuántica y chips ARM de alto rendimiento.**

---
