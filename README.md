# 🏋️‍♂️ Gym DataOps Pipeline E2E

Este proyecto consiste en el diseño y despliegue de una arquitectura de datos moderna, automatizada y desacoplada (End-to-End) para la gestión operativa y el análisis de abandono (*churn*) de clientes en un centro deportivo. 

La solución elimina por completo la intervención humana en la fase de recopilación y preparación de la información, transformando un entorno tradicional en un pipeline de datos robusto y escalable.

---

## 🏗️ Arquitectura del Sistema

El flujo de datos está completamente desacoplado, separando la capa de generación, el almacenamiento en la nube y el motor de cómputo/procesamiento:

1. **Fase de Generación (Capa de Origen - Python):** Se utiliza **Python y Pandas** en el entorno de Google Colab para simular, estructurar y actualizar de forma programática el comportamiento de la masa de clientes (escalado a 50.000 registros).
2. **Fase de Almacenamiento (Capa Cloud / Data Lake - Google Drive):** El script de Python exporta y sobrescribe directamente el archivo **CSV (`datos_gimnasio_cloud.csv`)** en un directorio centralizado de la nube. Actúa como nuestro almacén central de datos crudos.
3. **Fase de Procesamiento ETL (Capa Lógica - Apps Script):** Un motor orquestador en **JavaScript (Google Apps Script)** extrae el archivo remoto "al vuelo" desde la memoria, realiza la limpieza, calcula las métricas críticas y aísla a los usuarios en estado de baja (`Churn = 1`).
4. **Fase de Consumo y Alerta (Capa de BI y Salida):** Los datos limpios consolidan la base de datos intermedia en **Google Sheets**, alimentando de forma simétrica un **Dashboard Operativo Interactivo** (gráficos renderizados en tiempo real) y disparando notificaciones ejecutivas formateadas en HTML a través de la API de **Gmail**.

## 🛠️ Tecnologías Utilizadas

* **Python:** Motor de generación masiva y modelado inicial de datos.
* **Pandas Library:** Manipulación y estructuración eficiente de DataFrames.
* **JavaScript (Google Apps Script):** Lógica del servidor cloud, orquestación del pipeline ETL y automatización de servicios.
* **Google Drive Cloud Storage:** Repositorio centralizado de datos (Simulación conceptual de un entorno *AWS S3* o *Data Lake* corporativo).
* **Google Sheets & Charts API:** Interfaz de usuario, persistencia intermedia y Dashboard interactivo de Business Intelligence (BI).
* **Gmail API (`GmailApp`):** Sistema automatizado de alertas y reportes a los stakeholders.

---

## ⚡ Rendimiento (Big Data Challenge / Stress Test)

Para medir los "caballos de fuerza" de la infraestructura y entender cómo se comportan las arquitecturas profesionales al pasar de unos pocos datos a un procesamiento masivo, sometimos el pipeline a una prueba de estrés escalando el volumen a **50.000 clientes**.

Medimos el tiempo exacto invertido por cada motor para realizar la misma operación matemática (calcular el promedio de edad de toda la masa crítica):

| Entorno / Carril | Mecanismo de Procesamiento | Tiempo Invertido | Enfoque Técnico |
| :--- | :--- | :--- | :--- |
| **JavaScript (Apps Script)** | Bucle tradicional secuencial (`for`) fila por fila | **1.288 ms** | Procesa de forma aislada y lineal en memoria. |
| **Python (Pandas)** | Optimización y computación vectorial en bloque | **1.56 ms** | Cómputo en paralelo (estructurado en C por debajo). |

### 🏁 Veredicto Técnico
La prueba demuestra de manera empírica que **Python + Pandas procesa volúmenes masivos de datos casi 1.000 veces más rápido** que un bucle secuencial clásico en JavaScript. 

Mientras que Apps Script se ve penalizado por recorrer las 50.000 filas una a una y sufrir la latencia de la lectura del CSV desde la nube, Pandas coge la columna entera como un vector en la memoria RAM y realiza la operación en un solo ciclo de reloj. Esto justifica por qué las grandes arquitecturas de datos operacionales (como las utilizadas en entornos corporativos reales como *OPPLUS* o el *Grupo BBVA*) delegan la computación pesada en motores tabulares optimizados y dejan las hojas de cálculo únicamente como la interfaz final de consumo visual de Business Intelligence.
