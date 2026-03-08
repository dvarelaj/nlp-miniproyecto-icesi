## Clasificación de Artículos Científicos (arXiv) con NLP y Deep Learning
Este proyecto aplica técnicas avanzadas de Procesamiento de Lenguaje Natural (NLP) y Deep Learning para clasificar automáticamente resúmenes (abstracts) de artículos científicos de la plataforma arXiv en sus categorías correspondientes.

**Objetivo**
Desarrollar un pipeline robusto capaz de identificar la categoría temática de una investigación basándose exclusivamente en su resumen técnico, superando los desafíos de vocabulario especializado y el desbalance natural de las clases.

**El Dataset**
Origen: Extracción en tiempo real vía API OAI-PMH de arXiv (Artículos 2022-2026).

Volumen: 20,000 registros iniciales.

Filtro: Selección del Top 10 de categorías más frecuentes para garantizar la representatividad.

Desafío: Presencia de sintaxis LaTeX, fórmulas y terminología técnica densa.

**Tecnologías Utilizadas**
Procesamiento: NLTK, Regex, SpaCy (limpieza y normalización).

Modelado Clásico: Scikit-Learn (TF-IDF + Random Forest).

Deep Learning: TensorFlow/Keras (Embeddings, Global Average Pooling, LSTM).

Visualización: Matplotlib, Seaborn, WordCloud.

**Metodología y Técnicas**
Se implementaron y compararon tres aproximaciones:

Baseline (Random Forest): Modelo estadístico basado en frecuencias de palabras. Alcanzó un 84.5% de precisión.

Embeddings Simples (Global Average Pooling): Red neuronal que aprende el significado semántico. Resultó ser el modelo ganador con ~90% de accuracy.

Red Recurrente (LSTM): Arquitectura secuencial diseñada para capturar el orden de las ideas. Alcanzó un 85%, demostrando robustez tras aplicar padding='pre'.

**Estrategias de Innovación**
Manejo de Desbalance: Implementación de class_weights para penalizar errores en categorías minoritarias.

Optimización de Secuencias: Uso estratégico de Padding Pre para evitar la pérdida de memoria en celdas LSTM.

Limpieza Especializada: Filtro avanzado para eliminar ruido de comandos LaTeX sin perder el contexto científico.

**Conclusión**
El proyecto demuestra que para textos científicos cortos (abstracts), los modelos de Embeddings que capturan la semántica global son más eficientes y precisos que las arquitecturas secuenciales complejas o los modelos estadísticos tradicionales.
