# Clasificación de Documentos Médicos en Español con Transformers

Este proyecto aplica técnicas de **clasificación de texto con Transformers** sobre un corpus de documentos clínicos sintéticos en español. Se utiliza el dataset **Meddies PII Spanish** (`Meddies/meddies-pii-cleaned-v1`), que contiene 49.708 registros médicos organizados en **16 tipos de documentos**: prescripciones, resultados de laboratorio, notas de enfermería, informes de imagen, formularios de consentimiento, entre otros.

El objetivo es comparar distintas estrategias de clasificación — desde modelos sin entrenamiento (zero-shot) hasta fine-tuning supervisado — evaluando el impacto del idioma del modelo y su tamaño sobre el rendimiento en un dominio altamente especializado.

## Herramientas Utilizadas

* **Hugging Face Transformers**: Implementación de modelos zero-shot y fine-tuning.
* **BETO** (`dccuchile/bert-base-spanish-wwm-cased`): BERT entrenado en corpus en español.
* **XLM-RoBERTa** (`xlm-roberta-base`): Modelo multilingüe entrenado en 100 idiomas.
* **BART-MNLI** (`facebook/bart-large-mnli`): Modelo de inferencia para clasificación zero-shot en inglés.
* **BETO-XNLI** (`Recognai/bert-base-spanish-wwm-cased-xnli`): BETO ajustado para inferencia zero-shot en español.
* **Hugging Face Datasets**: Carga y gestión del dataset desde el Hub.
* **Scikit-Learn**: Codificación de etiquetas, splits estratificados y métricas de evaluación.
* **Pandas & NumPy**: Exploración y estructuración del dataset.
* **Matplotlib & Seaborn**: Visualización de curvas de entrenamiento, matrices de confusión y análisis exploratorio.

## Estructura del Proyecto

El notebook sigue un flujo de trabajo completo dividido en:

1. **Carga y Exploración (EDA)**: Análisis de la distribución de las 16 clases y la longitud de los documentos por tipo. El dataset presenta un balance casi perfecto (~3.100 muestras por clase), condición ideal para una evaluación justa.
2. **Preparación del Target**: Uso de `document_type` como variable objetivo y codificación con `LabelEncoder`. Subsample estratificado de 8.000 ejemplos para garantizar tiempos de entrenamiento razonables en GPU T4.
3. **Zero-Shot Baseline**: Evaluación de dos modelos sin entrenamiento supervisado — BART-MNLI (inglés) y BETO-XNLI (español) — para establecer un piso de referencia y medir el impacto del idioma en clasificación zero-shot.
4. **Fine-tuning BETO**: Ajuste supervisado del modelo nativo del español con cabeza de clasificación de 16 clases, 4 épocas y `max_length=256` para capturar la longitud de los documentos clínicos.
5. **Fine-tuning XLM-RoBERTa**: Mismo proceso con el modelo multilingüe, bajo condiciones idénticas a BETO para garantizar comparabilidad.
6. **Comparación Final**: Tabla de métricas generada automáticamente, gráfica comparativa y conclusiones con hallazgos propios.

## Comparativa de Resultados

| Técnica | Modelo | Idioma | Parámetros | Accuracy | F1 weighted |
|---------|--------|--------|-----------|----------|-------------|
| **Zero-Shot** | BART-MNLI | Inglés | 406M | 6.00% | 0.026 |
| **Zero-Shot** | BETO-XNLI | Español | 110M | 5.00% | 0.038 |
| **Fine-tuning** | **BETO** | **Español** | **110M** | **81.50%** | **0.818** |
| **Fine-tuning** | XLM-RoBERTa | Multilingüe | 278M | 80.63% | 0.811 |

## Conclusiones y Hallazgos

* **El fine-tuning es indispensable en dominios especializados**: El salto de ~6% (zero-shot) a ~81% (fine-tuning) — una mejora de 75 puntos porcentuales — confirma que 16 tipos de documentos médicos no pueden distinguirse sin ejemplos de entrenamiento, independientemente del idioma o tamaño del modelo.
* **El idioma importa en zero-shot, no en fine-tuning**: BETO-XNLI superó a BART en F1 (0.038 vs 0.026) al evaluar textos en español sin entrenamiento. Con fine-tuning, ambos modelos convergen al mismo nivel de rendimiento.
* **El modelo especializado supera al multilingüe en eficiencia**: BETO (español nativo, 110M parámetros) alcanzó mejor F1 que XLM-RoBERTa (278M parámetros) en la mitad de tiempo. En la primera época, BETO ya alcanzaba F1=0.752 mientras XLM-RoBERTa llegaba apenas a 0.547.
* **Las clases narrativas son el talón de Aquiles**: `MEDICAL_RECORD` y `OUTPATIENT_EXAMINATION` fueron las más difíciles para ambos modelos. Comparten vocabulario clínico con `DISCHARGE_SUMMARY`, haciendo su distinción difícil incluso para un lector humano sin ver el encabezado del documento.

---

**Curso:** Procesamiento del Lenguaje Natural  
**Universidad Icesi — 2026**
