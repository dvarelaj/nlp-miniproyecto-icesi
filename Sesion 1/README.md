## Información
La base de datos original consiste en un registro de quejas del sector asegurador correspondiente a enero de 2026. El conjunto de datos incluye variables como:

**Descripción:** Texto detallado del reclamo o falla técnica reportada por el usuario.

**Tipificación Interna:** Categorización administrativa de la queja.

**Metadatos:** Fechas de apertura/cierre, canal de comunicación y lugar de ocurrencia.

## Tratamiento Ético y Anonimización
Por razones de seguridad de la información y cumplimiento de la Ley de Protección de Datos Personales, la base de datos original no se encuentra cargada en este repositorio público. En su lugar, se realizó un proceso de pre-procesamiento fuera de línea para generar el archivo quejas_anonimizadas_muestra.csv. El proceso consistió en:

**Limpieza de Caracteres:** Se eliminaron caracteres especiales, se normalizaron espacios y se trataron tildes mediante normalización Unicode NFKD.

***Anonimización mediante Regex:*** 

**Identificaciones:** Se enmascararon números de cédula y NIT (de 7 a 11 dígitos) con y sin separadores de miles.

**Correos Electrónicos:** Se reemplazaron los usuarios por la etiqueta correo_anonimo manteniendo el dominio para análisis de canal.

Nombres Propios: Se sustituyeron nombres de personas (como empleadores o trabajadores mencionados) detectados mediante análisis de entidades (NER).

**Muestreo:** Se seleccionó una muestra representativa de 100 registros para demostrar la funcionalidad del código sin comprometer la privacidad del corpus completo.

## Cómo ejecutar
Los notebooks están configurados para descargar automáticamente el archivo anonimizado directamente desde la URL "raw" de este repositorio, permitiendo una ejecución fluida en Google Colab.

## Conclusiones del Proyecto
Tras la implementación de las técnicas clásicas de NLP y el análisis de sentimientos sobre el corpus de quejas de la ARL, se presentan los siguientes hallazgos:

**Efectividad del Modelo:** Se logró un Accuracy de 0.71, lo que demuestra que el modelo de lenguaje basado en Transformers (pysentimiento) es capaz de identificar correctamente la polaridad de las quejas en un 71% de los casos, superando significativamente la línea base del azar.

**Calidad de la Anonimización:** El uso de Expresiones Regulares (Regex) permitió una desidentificación exitosa de datos sensibles (NITs, Cédulas y Correos), garantizando que el flujo de procesamiento de texto se realice bajo estándares éticos de privacidad, sin afectar la capacidad del modelo para entender el contexto de la reclamación.

**Análisis de Errores:** Mediante la Matriz de Confusión, se identificó que los principales errores de clasificación ocurren en quejas de tono muy formal o reportes de mantenimiento técnico, donde el modelo tiende a predecir un sentimiento negativo debido a la presencia de palabras clave de falla, aunque el usuario mantenga un tono neutral.

**Valor del "Ground Truth":** El proceso de etiquetado manual de la muestra de 101 registros fue fundamental para validar que, en el contexto de una ARL, la mayoría de las interacciones son negativas por naturaleza, lo que genera un dataset desbalanceado que requiere métricas específicas como el F1-Score para una evaluación completa.
