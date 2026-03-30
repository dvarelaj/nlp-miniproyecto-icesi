# Mini-Proyecto: Generación de Texto Financiero con GPT-2

Fine-tuning de un modelo GPT-2 en español sobre un corpus de artículos financieros de Wikipedia, con análisis comparativo antes y después del entrenamiento.

---

## Descripción

Este proyecto aplica fine-tuning al modelo [`mrm8488/spanish-gpt2`](https://huggingface.co/mrm8488/spanish-gpt2) usando 1,500 artículos financieros extraídos de Wikipedia en español. Se comparan las capacidades generativas del modelo base contra el modelo especializado mediante métricas cuantitativas y análisis cualitativos.

---

## Flujo del Proyecto

```
1. Configuración del entorno
2. Carga del modelo base (mrm8488/spanish-gpt2)
3. Definición de funciones de generación y evaluación
4. Generación baseline (sin fine-tuning)
5. Construcción del corpus financiero (Wikipedia ES)
6. Fine-tuning supervisado
7. Comparación base vs. modelo financiero
8. Análisis: perplejidad, sentimiento y vocabulario
9. Conclusiones
```

---

## Configuración del Entrenamiento

| Parámetro | Valor |
|---|---|
| Modelo base | `mrm8488/spanish-gpt2` |
| Dataset | Wikipedia ES — artículos de economía y finanzas |
| Artículos | 1,500 |
| Block size | 128 tokens |
| Épocas | 5 |
| Learning rate | 1e-5 |
| Warmup steps | 100 |
| Weight decay | 0.01 |
| Split train/val | 85% / 15% |
| Dispositivo | CUDA (Tesla T4) |
| Semilla | 42 |

---

## Estrategias de Decodificación Evaluadas

| Estrategia | Descripción |
|---|---|
| Greedy | Siempre elige el token más probable |
| Top-k (k=50) | Muestrea entre los k tokens más probables |
| Top-p (p=0.92) | Muestrea con probabilidad acumulada ≥ p |
| Temperature 0.5 | Generación conservadora |
| Temperature 1.2 | Generación creativa |

---

## Resultados Principales

**Curva de pérdida:**
- Train loss: `2.9669` → `2.6249`
- Val loss: `2.8082` → `2.7803`

**Perplejidad promedio:**
- Modelo base: `118.3`
- Modelo fine-tuned: `144.7`
  > La mayor perplejidad en textos periodísticos cortos se explica porque el corpus de entrenamiento tiene estilo enciclopédico. En textos similares a Wikipedia el modelo fine-tuned sí mejora (hasta +9.4%).

**Ejemplo de generación (Greedy):**

| Modelo | Salida |
|---|---|
| Base | *"...de la familia de la familia de la familia..."* (repetición) |
| Fine-tuned | *"...de $1,000 a $2,000 millones de dólares. El Banco de la República..."* |

---

## Instalación

```bash
git clone https://github.com/tu-usuario/gpt-financiero.git
cd gpt-financiero
pip install transformers datasets accelerate pysentimiento wordcloud matplotlib seaborn
```

---

## Uso

Abre el notebook en Google Colab o Jupyter:

```bash
jupyter notebook mini_proyecto_gpt_financiero.ipynb
```

> **Nota:** Se recomienda GPU con al menos 8 GB de VRAM. El proyecto fue desarrollado sobre una Tesla T4 (15.6 GB).

---

## Estructura del Repositorio

```
gpt-financiero/
├── mini_proyecto_gpt_financiero.ipynb   # Notebook principal
├── README.md                            # Este archivo
├── corpus_stats.png                     # Análisis exploratorio del corpus
├── wordcloud.png                        # Nube de palabras financieras
├── curva_perdida.png                    # Curva de pérdida train/val
├── perplejidad.png                      # Comparación de perplejidad
├── sentimiento.png                      # Distribución de sentimiento
└── vocabulario.png                      # Frecuencia de términos financieros
```

---

## Dependencias

- Python 3.12
- PyTorch 2.10 + CUDA 12.8
- Transformers 5.0
- Datasets 4.0
- pysentimiento
- WordCloud
- Matplotlib / Seaborn

---

## Análisis Adicionales

Más allá del fine-tuning básico, el proyecto incluye:

- **EDA del corpus:** distribución de longitudes, tokens por artículo y frecuencia de términos financieros
- **Nube de palabras** del vocabulario dominante en el corpus
- **Análisis de sentimiento** con modelo BERT en español (`pysentimiento`) comparando el tono del texto generado por ambos modelos
- **Análisis de vocabulario financiero** midiendo la frecuencia de términos especializados por cada 1,000 palabras generadas

---

## Conclusiones

El fine-tuning sobre el corpus financiero produce un cambio cualitativo claro: el modelo base cae en secuencias repetitivas, mientras que el modelo especializado genera texto con mayor coherencia temática, incluye cifras concretas y mantiene una estructura narrativa de artículo económico.

La estrategia **top-p con p=0.92** ofrece el mejor balance entre coherencia y variedad en el modelo fine-tuned.

---

## Referencias

- [mrm8488/spanish-gpt2](https://huggingface.co/mrm8488/spanish-gpt2)
- [Wikipedia ES — wikimedia/wikipedia](https://huggingface.co/datasets/wikimedia/wikipedia)
- [pysentimiento](https://github.com/pysentimiento/pysentimiento)
- Radford et al. — *Language Models are Unsupervised Multitask Learners* (GPT-2)
