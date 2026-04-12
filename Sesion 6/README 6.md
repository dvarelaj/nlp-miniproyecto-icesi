# 🤖 ChatBot RAG — Salud y Seguridad en el Trabajo (SST) Colombia

Proyecto final del curso de Modelos Generativos. Implementación de un chatbot conversacional basado en **Retrieval-Augmented Generation (RAG)** especializado en Seguridad y Salud en el Trabajo (SST) y el sistema de Riesgos Laborales (ARL) en Colombia.

---

## 📋 Descripción

El bot responde preguntas sobre accidentes de trabajo, enfermedades laborales, prevención de riesgos y rehabilitación, fundamentando cada respuesta en artículos de Wikipedia en español sobre SST colombiana. A diferencia de un LLM genérico, el sistema **cita la fuente de cada respuesta** y rechaza responder cuando la información no está en su base de conocimiento, evitando alucinaciones.

### ¿Por qué RAG y no fine-tuning?

| Criterio | Fine-tuning | RAG (este proyecto) |
|---|---|---|
| Actualización del conocimiento | Reentrenar el modelo | Reemplazar documentos |
| Trazabilidad de fuentes | ❌ | ✅ cita el artículo |
| Costo computacional | Alto | Bajo |
| Tiempo de implementación | Días/semanas | Horas |

---

## 🏗️ Arquitectura

```
Wikipedia ES (SST Colombia)
        │
        ▼
  WebBaseLoader  ──►  RecursiveCharacterTextSplitter  ──►  chunks
                                                               │
                                                               ▼
                                                  OllamaEmbeddings (nomic-embed-text)
                                                               │
                                                               ▼
                                                       Chroma VectorStore
                                                               │
  Pregunta del usuario  ──►  Retriever (top-4 coseno)  ◄──────┘
                                        │
                                        ▼
                           ChatOllama (llama3) + Prompt
                                        │
                                        ▼
                                 Respuesta + Fuente
                                        │
                                        ▼
                               Interfaz Gradio
```

---

## 📚 Corpus utilizado

Artículos de Wikipedia en español sobre SST y ARL Colombia:

| Artículo | URL |
|---|---|
| Salud y seguridad en el trabajo | [enlace](https://es.wikipedia.org/wiki/Salud_y_seguridad_en_el_trabajo) |
| Accidente de trabajo | [enlace](https://es.wikipedia.org/wiki/Accidente_de_trabajo) |
| Enfermedad laboral | [enlace](https://es.wikipedia.org/wiki/Enfermedad_laboral) |
| Sistema general de riesgos laborales | [enlace](https://es.wikipedia.org/wiki/Sistema_general_de_riesgos_laborales) |
| Seguridad industrial | [enlace](https://es.wikipedia.org/wiki/Seguridad_industrial) |
| Higiene industrial | [enlace](https://es.wikipedia.org/wiki/Higiene_industrial) |
| Ergonomía | [enlace](https://es.wikipedia.org/wiki/Ergonoma) |

> **Nota:** El corpus original estaba planteado sobre `positiva.gov.co`, pero el sitio usa renderizado JavaScript (SPA), lo que impide la extracción con BeautifulSoup. Se migró a Wikipedia como fuente equivalente de conocimiento sobre SST, lo cual constituye en sí mismo un hallazgo técnico documentado en el notebook.

---

## 🛠️ Stack tecnológico

| Componente | Herramienta |
|---|---|
| Framework RAG | [LangChain](https://python.langchain.com/) |
| LLM local | [Llama 3 (8B)](https://ollama.com/library/llama3) via [Ollama](https://ollama.com) |
| Embeddings | [nomic-embed-text](https://ollama.com/library/nomic-embed-text) via Ollama |
| Vector store | [Chroma](https://www.trychroma.com/) |
| Web scraping | LangChain `WebBaseLoader` + BeautifulSoup4 |
| Interfaz | [Gradio](https://gradio.app/) |
| Visualizaciones | Matplotlib, WordCloud |
| Entorno | Google Colab (GPU T4) |

---

## 🚀 Cómo ejecutar

### Requisitos
- Google Colab con GPU T4 habilitada (Runtime → Change runtime type → T4 GPU)
- Conexión a internet (para descargar modelos Ollama y scraping de Wikipedia)

### Pasos

1. Clona el repositorio o descarga el notebook:
   ```bash
   git clone https://github.com/tu-usuario/rag-sst-colombia.git
   ```

2. Abre `modelos_generativos_RAG.ipynb` en Google Colab.

3. Ejecuta las celdas **en orden**. El notebook está dividido en 10 pasos:

   | Paso | Descripción | Tiempo aprox. |
   |---|---|---|
   | 1 | Instalación de dependencias | ~3 min |
   | 2 | Configuración de Ollama + descarga de modelos | ~8 min |
   | 3 | Carga de documentos desde Wikipedia | ~1 min |
   | 4 | Exploración del corpus (plots) | ~30 seg |
   | 5 | Chunking | ~30 seg |
   | 6 | Generación de embeddings + VectorStore | ~2 min |
   | 7 | Construcción de la cadena RAG | ~30 seg |
   | 8 | Pruebas del sistema | ~3 min |
   | 9 | Análisis de calidad del retriever (matriz) | ~2 min |
   | 10 | Interfaz Gradio | inmediato |

4. En el Paso 10 se generará un **link público de Gradio** (`https://xxxx.gradio.live`) válido por 72 horas para interactuar con el bot.

---

## 📊 Resultados y hallazgos

### Exploración del corpus
Los 7 artículos de Wikipedia presentaron longitudes heterogéneas (entre ~5,000 y ~60,000 caracteres), generando un total de **176 chunks** tras el proceso de división con `chunk_size=800` y `chunk_overlap=150`.

### Matriz de relevancia del retriever
El análisis de retrieval mostró que el sistema es preciso para la mayoría de consultas temáticas. Las preguntas sobre *accidente de trabajo* recuperaron 4/4 chunks del artículo correspondiente. Se identificó un **sesgo por densidad de contenido**: el artículo `Accidente_de_trabajo` domina el retrieval en consultas de temas relacionados porque es el más extenso del corpus.

### Calidad de respuestas
El modelo `llama3` con `temperature=0.1` produjo respuestas coherentes con citación de fuente. Ante preguntas fuera del corpus, el bot respondió correctamente que no tenía información suficiente.

---

## 📁 Estructura del repositorio

```
rag-sst-colombia/
│
├── modelos_generativos_RAG.ipynb   # Notebook principal (ejecutar en Colab)
└── README.md                       # Este archivo
```

---

## ⚠️ Limitaciones conocidas

- El VectorStore no persiste entre sesiones de Colab; al reiniciar el runtime hay que reejecutar desde el Paso 6.
- Fuentes con renderizado JavaScript (como `positiva.gov.co`) no son accesibles con BeautifulSoup y requerirían Selenium o Playwright.
- El corpus cubre conceptos generales de SST. Para un sistema productivo se deberían incorporar resoluciones del MinTrabajo y circulares oficiales de las ARL.

---

## 📖 Referencias

- Lewis et al. (2020). [Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](http://arxiv.org/abs/2005.11401)
- [LangChain Documentation](https://python.langchain.com/docs/)
- [Ollama Model Library](https://ollama.com/library)
- [Chroma Documentation](https://docs.trychroma.com/)
- [Gradio Documentation](https://gradio.app/docs/)
