Miniproyecto: Procesamiento de Texto con Técnicas Clásicas (NLP)
Este proyecto forma parte del curso de Procesamiento del Lenguaje Natural. Consiste en la implementación de un flujo de trabajo para el análisis de texto y sentimiento utilizando un información real de quejas de servicios de la solución web de una Aseguradora.

Información
La base de datos original consiste en un registro de quejas del sector asegurador correspondiente a enero de 2026. El conjunto de datos incluye variables como:

Descripción: Texto detallado del reclamo o falla técnica reportada por el usuario.

Tipificación Interna: Categorización administrativa de la queja.

Metadatos: Fechas de apertura/cierre, canal de comunicación y lugar de ocurrencia.

Tratamiento Ético y Anonimización
Por razones de seguridad de la información y cumplimiento de la Ley de Protección de Datos Personales, la base de datos original no se encuentra cargada en este repositorio público. En su lugar, se realizó un proceso de pre-procesamiento fuera de línea para generar el archivo quejas_anonimizadas_muestra.csv. El proceso consistió en:

Limpieza de Caracteres: Se eliminaron caracteres especiales, se normalizaron espacios y se trataron tildes mediante normalización Unicode NFKD.

Anonimización mediante Regex:

Identificaciones: Se enmascararon números de cédula y NIT (de 7 a 11 dígitos) con y sin separadores de miles.

Correos Electrónicos: Se reemplazaron los usuarios por la etiqueta correo_anonimo manteniendo el dominio para análisis de canal.

Nombres Propios: Se sustituyeron nombres de personas (como empleadores o trabajadores mencionados) detectados mediante análisis de entidades (NER).

Muestreo: Se seleccionó una muestra representativa de 100 registros para demostrar la funcionalidad del código sin comprometer la privacidad del corpus completo.

Cómo ejecutar
Los notebooks están configurados para descargar automáticamente el archivo anonimizado directamente desde la URL "raw" de este repositorio, permitiendo una ejecución fluida en Google Colab.
