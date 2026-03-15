# Clasificación de Artículos Científicos (arXiv) con Transformers

Este proyecto aplica técnicas de **Deep Learning** de última generación, específicamente arquitecturas basadas en **Transformers**, sobre un corpus de 20,000 resúmenes (abstracts) de artículos científicos de la plataforma arXiv publicados desde el 2022.

El objetivo principal es comparar la capacidad de comprensión contextual de los Transformers frente a los modelos tradicionales (RNN/LSTM) y redes densas simples vistos anteriormente, enfocándonos en la clasificación multiclase del Top 10 de categorías científicas.

## Herramientas Utilizadas
* **Hugging Face Transformers**: Implementación de modelos basados en atención.
* **DistilBERT**: Modelo de lenguaje pre-entrenado y optimizado para eficiencia.
* **TensorFlow / Keras**: Framework principal para el fine-tuning de la red.
* **Scikit-Learn**: Procesamiento de etiquetas (`LabelEncoder`) y métricas de evaluación.
* **Pandas & NumPy**: Limpieza y estructuración del dataset de 20,000 registros.
* **Matplotlib & Seaborn**: Visualización de curvas de aprendizaje y rendimiento.

## Estructura del Proyecto
El notebook sigue un flujo de trabajo profesional dividido en:

1. **Ingesta de Datos**: Extracción desde la API de arXiv (OAI-PMH) filtrando por fecha y volumen para asegurar datos recientes.
2. **Preprocesamiento de Transformers**: Uso del **Tokenizer de DistilBERT** que implementa *WordPiece*, permitiendo manejar términos científicos complejos sin perder información por palabras fuera de vocabulario.
3. **Manejo de Desbalance**: Aplicación de `class_weights` para asegurar que el modelo aprenda de categorías minoritarias y no se sesgue hacia *Machine Learning* (cs.LG).
4. **Fine-Tuning**: Entrenamiento de la arquitectura Transformer adaptando sus pesos originales al dominio específico de la jerga científica.

## Comparativa de Resultados

| Técnica | Modelo | Accuracy | Observación Principal |
| :--- | :--- | :---: | :--- |
| **Baseline** | Random Forest + TF-IDF | 84.5% | Rápido, pero ignora el orden de las palabras. |
| **Técnica 2** | Embeddings Promediados | 89.4% | Muy eficiente captando semántica global. |
| **Técnica 3** | LSTM (Recurrente) | 85.0% | Robusta, pero difícil de optimizar en textos largos. |
| **Técnica 4** | **DistilBERT (Transformer)** | **>92%** | **Ganador:** Entiende el contexto técnico profundo. |

## Conclusiones y Hallazgos
* **Superioridad Contextual**: Los Transformers demostraron una precisión superior al diferenciar categorías similares (ej. *cs.CL* vs *cs.LG*) gracias a sus mecanismos de auto-atención.
* **Control de Sesgo**: Al integrar pesos balanceados, logramos que la red mantuviera un *Recall* alto incluso en las categorías con menos ejemplos.
* **Reflexión Final**: El uso de modelos pre-entrenados (*Transfer Learning*) reduce drásticamente el tiempo de desarrollo y supera a las arquitecturas diseñadas desde cero para tareas complejas de NLP científico.

**Maestría en Inteligencia Artificial Aplicada** **Universidad Icesi - 2026**
