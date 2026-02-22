# Mini-Proyecto: Análisis de Sentimientos con Transformers
## Evaluación de Desempeño sobre Quejas ARL (Muestra 101 registros)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dvarelaj/nlp-miniproyecto-icesi/blob/main/Sesion%201/Actividad%20%237/analisis_sentimientos_quejas.ipynb)

---

## 1.Contexto del Proyecto
En esta segunda fase del proyecto, pasamos del procesamiento lingüístico clásico (paso 6) a la **Inferencia de Modelos de Lenguaje**. El objetivo es evaluar qué tan bien una Inteligencia Artificial puede identificar la carga emocional y la postura de un usuario que interpone una queja ante una Administradora de Riesgos Laborales (ARL).

Se trabajó con el corpus previamente anonimizado para garantizar la seguridad de la información, permitiendo un análisis basado en el lenguaje técnico y administrativo del sector seguros.

---

## 2. Herramientas y Tecnologías
Para este análisis se utilizó:

* **Librería Principal:** `pysentimiento` (basada en modelos tipo BERT).
* **Modelo:** Analizador de sentimientos especializado en español.
* **Evaluación:** `scikit-learn` para la generación de métricas de desempeño.
* **Visualización:** `Seaborn` para la creación de la matriz de confusión.

---

## 3. Flujo de Trabajo
El notebook está estructurado siguiendo el ciclo de vida de evaluación de un modelo de Machine Learning:

1.  **Configuración del Entorno:** Instalación de dependencias y carga de modelos Transformers.
2.  **Ingesta de Datos:** Carga del dataset de 101 quejas anonimizadas.
3.  **Inferencia (Predicción):** Clasificación automatizada de cada queja en las categorías: `NEG` (Negativo), `NEU` (Neutral) o `POS` (Positivo).
4.  **Integración de Ground Truth:** Incorporación del etiquetado manual realizado por el analista (Diana Varela) para contrastar contra la IA.
5.  **Métricas de Calidad:** Cálculo de *Accuracy*, *Precision*, *Recall* y *F1-Score*.
6.  **Análisis de Errores:** Visualización mediante matriz de confusión para identificar sesgos del modelo.

---

## 4. Principales Hallazgos

### Desempeño del Modelo
El modelo alcanzó un **Accuracy de 0.71**, lo que indica que la IA coincidió con el criterio humano en el **71% de los casos**.

| Métrica | Resultado |
| :--- | :--- |
| **Accuracy Total** | **0.71** |
| **F1-Score (NEG)** | 0.81 |
| **Soporte de datos** | 101 quejas |

### Observaciones Clave
* **Dominio de la Negatividad:** Al ser un canal de quejas, existe un desbalance natural hacia el sentimiento negativo. El modelo demostró gran robustez identificando frustración y reportes de falla.
* **Falsos Positivos de Neutralidad:** El modelo tiende a clasificar como "Negativo" mensajes que técnicamente son "Neutrales" (como reportes de mantenimiento), debido a que asocia palabras como *error* o *falla* con una emoción negativa, aunque el usuario sea puramente informativo.
* **Reto del Lenguaje Formal:** El uso de lenguaje administrativo muy cortés ("Cordial saludo", "Agradezco su atención") suaviza el sentimiento para la IA, a veces ocultando la gravedad del reclamo subyacente.

---

## Estructura de Archivos
* `analisis_sentimientos_quejas.ipynb`: Notebook principal con todo el código y análisis.
* `Quejas_Anonimizadas_Muestra_Etiqueta.csv`: Dataset utilizado con las etiquetas reales para validación.
