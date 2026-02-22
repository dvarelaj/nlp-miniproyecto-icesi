# Mini-Proyecto NLP Clásico — Análisis de Quejas ARL

> **Curso:** Procesamiento de Lenguaje Natural  
> **Autores:** Diana Varela, Daniel García, Farid Sandoval

---

## Descripción del proyecto

Este proyecto aplica técnicas clásicas de **Procesamiento de Lenguaje Natural (NLP)** sobre un corpus real de quejas, peticiones y reclamos registrados por una Administradora de Riesgos Laborales (ARL) colombiana.

El dataset contiene descripciones textuales anonimizadas de casos recibidos a través de múltiples canales (llamadas telefónicas, WhatsApp, plataforma web), provenientes de distintas regionales del país, los textos fueron previamente anonimizados reemplazando datos personales con etiquetas genéricas como `[ID_ANONIMIZADO]`.

---

## Dataset

| Campo | Descripción |
|-------|-------------|
| `compania` | Empresa (ARL) |
| `regional` | Zona geográfica del reporte |
| `fecha de apertura / cierre` | Ciclo de vida del caso |
| `tipo` | QUEJA, PETICIÓN, etc. |
| `estado` | Abierto / Cerrado |
| `medio de recepcion` | Canal de entrada (llamada, WhatsApp, etc.) |
| `descripcion_anonimizada` | **Texto libre de la queja** — columna principal de análisis |
| `y_true` | Etiqueta de sentimiento: `neg`, `neu`, `pos` |
| `justificacion_tecnica` | Tipo de incidente detectado |

---

## Técnicas NLP aplicadas

### 1. Exploración del corpus
Se carga el dataset y se identifica su estructura. Se agrega una columna de longitud en palabras para identificar la queja más representativa para el análisis profundo: la queja **#100** con **337 palabras**, un reclamo formal escrito por un empleador por fallas técnicas recurrentes en la plataforma web de la ARL.

### 2. Reconocimiento de Entidades Nombradas (NER)
Se aplica el modelo `es_core_news_sm` para detectar entidades (PER, ORG, LOC, MISC) en la queja más larga. Se identificaron personas como `javier garzon` (correctamente) y errores como clasificar `contratacion` como PER.

**Limitación encontrada:** El modelo presenta dificultades con textos técnicos del dominio de salud ocupacional y con problemas de codificación UTF-8 (`seã±ores` por `señores`).

### 3. Tokenización, POS Tagging y Lematización
Se analiza morfológica y sintácticamente cada token de la queja, extrayendo:
- Categoría gramatical (NOUN, VERB, ADJ, ADP...)
- Relación de dependencia (nsubj, obj, ROOT, amod...)
- Lema canónico (ej. `fallas → falla`, `tecnicas → tecnico`)

**Hallazgo clave:** La ausencia de puntuación formal en los textos hace que SpaCy detecte todo el documento como **una sola oración** (347 tokens, 1 oración), lo que impacta negativamente la segmentación y el análisis posterior.

### 4. Pattern Matching
Se implementa un matcher con SpaCy para detectar automáticamente reportes de errores y fallas dentro del texto, usando el patrón:
```
LEMMA in [error, falla, fallar, problema] + ADJ* (opcionales)
```
Se encontraron **13 coincidencias** en la queja analizada, incluyendo frases como `falla técnica recurrente` y `fallas técnicas persistentes`, lo que confirma la alta densidad de terminología de incidente técnico en este caso.

### 5. Análisis de frecuencia de lemas (corpus completo)
Se procesan las primeras 100 quejas del corpus, extrayendo lemas filtrados (sin stop words, sin puntuación) y graficando las **15 palabras más frecuentes**. La lematización agrupa variantes morfológicas del mismo concepto, ofreciendo una imagen más real de los temas dominantes.

---

## Conclusiones

### Sobre el corpus
- El dataset es **heterogéneo en estilo y longitud**: conviven transcripciones orales muy cortas y coloquiales con cartas formales extensas. Esta variabilidad es un desafío real para pipelines de NLP uniformes.
- La mayoría de los registros están relacionados con problemas de acceso a la plataforma digital de la ARL, certificados de afiliación y errores de sistema.

### Sobre las técnicas aplicadas
- **NER** es útil pero requiere ajuste para este dominio: el modelo pequeño de SpaCy comete errores de clasificación sistemáticos en textos técnicos y ve afectada su precisión por problemas de encoding.
- **El Pattern Matching** resultó ser la técnica más práctica y robusta del ejercicio: permite identificar automáticamente tipos de queja con reglas transparentes y sin necesidad de entrenamiento adicional.
- **La lematización** mejora significativamente el análisis de frecuencia, agrupando `ingresar`, `ingresando`, `ingresó` bajo un mismo concepto.
- **La segmentación de oraciones** falla en textos sin puntuación formal, lo cual es la norma en este tipo de corpus de atención al cliente.

### Sobre el modelo `es_core_news_sm`
El modelo preentrenado con noticias en español es un buen punto de partida pero presenta limitaciones claras en este contexto:
- Errores en NER con vocabulario técnico del dominio.
- Clasificación POS incorrecta de nombres propios poco comunes.
- Sensibilidad a problemas de encoding.

Para un uso en producción dentro de la ARL, se recomienda explorar modelos más grandes (`es_core_news_md` o `es_core_news_lg`) o considerar un ajuste fino con datos del dominio de salud ocupacional y servicio al cliente.

---
