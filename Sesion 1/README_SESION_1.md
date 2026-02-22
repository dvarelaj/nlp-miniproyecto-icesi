# Miniproyecto: Procesamiento de Texto con Técnicas Clásicas (NLP)

**Materia:** Procesamiento de Lenguaje Natural (NLP) — Universidad Icesi  
**Integrantes:** Daniel Garcia, Farid Sandoval y Diana Varela

---

## Descripción General

Este miniproyecto implementa un flujo completo de análisis de texto sobre un corpus real de quejas registradas por una Administradora de Riesgos Laborales (ARL) colombiana, compuesto por dos actividades que progresan desde el procesamiento lingüístico clásico hasta la inferencia con modelos de lenguaje basados en Transformers.

El dataset contiene 101 registros anonimizados de quejas, peticiones y reclamos recibidos por múltiples canales (llamadas, WhatsApp, plataforma web) desde distintas regionales del país, con una anonimización realizada previamente mediante expresiones regulares y NER, sustituyendo datos personales por etiquetas genéricas en cumplimiento de la Ley de Protección de Datos Personales.

---

## Estructura del Proyecto

```
Sesion 1/
├── Actividad #6 — NLP Clásico/
│   ├── procesamiento_nlp_clasico.ipynb
│   └── README_6.md
├── Actividad #7 — Análisis de Sentimientos/
│   ├── analisis_sentimientos_quejas.ipynb
│   └── README_7.md
└── Data Base/
    └── Quejas_Anonimizadas_Muestra_Etiqueta.csv
```

---

## Actividad 6 — NLP Clásico con SpaCy

Aplicación de técnicas clásicas de procesamiento de lenguaje sobre la queja más representativa del corpus (337 palabras) y sobre las primeras 100 quejas para el análisis de frecuencia, cubriendo NER, tokenización, POS tagging, lematización y pattern matching.

→ Ver análisis detallado y conclusiones en [`README_6.md`]

---

## Actividad 7 — Análisis de Sentimientos con Transformers

Evaluación del modelo `pysentimiento` (basado en BERT para español) sobre las 101 quejas anonimizadas, contrastando la predicción automática del modelo contra un etiquetado manual (*ground truth*) para medir su desempeño mediante Accuracy, F1-Score y Matriz de Confusión.

→ Ver análisis detallado y conclusiones en [`README_7.md`]

---

## Cómo ejecutar

Los notebooks están configurados para descargarse y ejecutarse directamente en Google Colab, cargando el dataset automáticamente desde la URL pública del repositorio sin necesidad de configuración adicional.
