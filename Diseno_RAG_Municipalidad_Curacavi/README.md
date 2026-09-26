# Asistente de Trámites Municipales — Agente IA con RAG

Proyecto de la Primera Evaluación Parcial de **ISY0101 Ingeniería de Soluciones con IA** (Duoc UC).

Agente conversacional que orienta a vecinos de la **Ilustre Municipalidad de Curacaví** sobre trámites
municipales (permisos de circulación, licencias de conducir, patentes comerciales, estado de solicitudes y
plazos), respondiendo solo con información recuperada de documentos municipales (fuente interna) y de
normativa nacional (fuente externa), citando siempre la fuente. Usa exclusivamente capas gratuitas.

Toda la solución está en un único notebook de Google Colab:
**[`notebooks/asistente_tramites_colab.ipynb`](notebooks/asistente_tramites_colab.ipynb)** —
[abrir en Colab](https://colab.research.google.com/github/Thoorcito/Diseno_RAG_Municipalidad_Curacavi/blob/main/notebooks/asistente_tramites_colab.ipynb).

**Integrantes:** <nombre 1> (Thoorcito) · <nombre 2> (Diazg898) · <nombre 3> (Niveo21)

## Componentes

| Componente | Tecnología | Sección del notebook |
|---|---|---|
| LLM | OpenRouter, modelo gratuito con tool calling (por defecto `minimax/minimax-m3:free`) | §4 |
| Prompts | v1 (zero-shot), v2 (estructurado) y v3 (v2 + few-shot) | §11 |
| Embeddings | `models/gemini-embedding-001` (768 dim), Google AI Studio | §8 |
| Base vectorial | FAISS, un índice interno y uno externo | §8 |
| RAG base | `RunnableSequence` (retriever → prompt → LLM) con citas | §10 |
| Herramientas | búsqueda interna, búsqueda externa, estado de solicitud, plazos hábiles, fecha actual | §12 |
| Agente | LangChain `create_agent` (tool calling) | §13 |
| Control de contexto | top-k 4, umbral de similitud calibrado, memoria de 4 turnos | §9, §14 |
| Control del ciclo del agente | máx. 4 herramientas y 6 llamadas al LLM por pregunta, cierre forzado, reintento ante 429 | §13 |
| Evaluación | dataset de 10 preguntas con referencia, 6 evaluadores, LangSmith | §16–17 |
| Trazabilidad | registro JSONL de cada consulta: herramientas, fuentes, respuesta y latencia | §13 |

![Arquitectura](docs/img/arquitectura.png)

Detalle de la arquitectura: [`docs/arquitectura.md`](docs/arquitectura.md).

## Estructura

```
├── README.md
├── notebooks/
│   └── asistente_tramites_colab.ipynb   # código completo de la solución
├── data/
│   ├── interna/docs/                    # documentos municipales (+ procedimiento simulado)
│   ├── interna/solicitudes.csv          # registros simulados de solicitudes
│   ├── externa/                         # leyes y fichas ChileAtiende
│   └── preguntas_prueba.json            # 10 preguntas con respuesta de referencia
├── docs/
│   ├── arquitectura.md
│   └── img/arquitectura.png             # diagrama (boceto de diseño)
├── capturas/                            # evidencia visual de las pruebas (ver capturas/README.md)
├── evidencias/                          # resultados generados por el notebook
└── vectorstore/gemini/                  # índices FAISS generados por el notebook
```

La carpeta `data/` es necesaria: el notebook la descarga al clonar el repositorio.

## Ejecución en Google Colab

1. Crear las claves gratuitas:

| Clave | Uso | Dónde se crea |
|---|---|---|
| `OPENROUTER_API_KEY` | chat del agente | https://openrouter.ai/keys |
| `GEMINI_API_KEY` | embeddings y LLM evaluador | https://ai.google.dev → Get API key |
| `LANGCHAIN_API_KEY` (opcional) | trazas y experimentos | https://smith.langchain.com → Settings → API Keys |

2. Abrir el notebook con el enlace [abrir en Colab](https://colab.research.google.com/github/Thoorcito/Diseno_RAG_Municipalidad_Curacavi/blob/main/notebooks/asistente_tramites_colab.ipynb).
3. *Entorno de ejecución → Ejecutar todas*. El notebook clona este repositorio, instala las dependencias y
   pide las claves con `getpass` (no se ven al escribir ni quedan guardadas en el notebook).
4. La primera vez, la sección 8 construye los índices (≈ 10–12 min por la cuota gratuita de Google). Si
   `vectorstore/gemini/` ya está en el repositorio, se cargan en segundos.
5. Al final (sección 17) se descarga `entrega_AAAAMMDD_HHMM.zip` con `evidencias/` y `vectorstore/`, que se
   suben a este repositorio.

## Recorrido del notebook

| Sección | Contenido | |
|---|---|---|
| 1 | Caso organizacional: problema, objetivos medibles, datos, restricciones, por qué agente + RAG |  |
| 2–3 | Instalación, clonación del repositorio, credenciales y parámetros | — |
| 4 | Conexión a OpenRouter, verificación de tool calling, streaming y evaluador Gemini |  |
| 5–7 | Fuentes internas/externas, carga, limpieza y chunking con metadato de artículo |  |
| 8 | Embeddings Gemini e índices FAISS por lotes (reanudables) |  |
| 9 | Recuperación con similitud coseno y calibración del umbral con datos | |
| 10 | RAG base con `RunnableSequence` (dentro, normativa y fuera de contexto) |  |
| 11 | Prompts v1 / v2 / v3 y comparación con el mismo contexto |  |
| 12 | Herramientas y sus pruebas sin LLM | |
| 13 | Agente, control del ciclo y diagrama de arquitectura |  |
| 14 | Memoria de ventana y streaming |  |
| 15 | Traza pregunta → herramienta → contexto → respuesta | |
| 16–17 | Evaluación (LangSmith), tabla de métricas, gráfico y evidencias |  |
| 18–19 | Verificación final, decisiones, problemas resueltos, conclusiones |  |

## Datos

- **Internos:** documentos del portal de Transparencia de la municipalidad (ver `data/interna/docs/FUENTES.md`),
  un procedimiento de atención **simulado** y `solicitudes.csv` con 30 solicitudes **simuladas** sin datos personales.
- **Externos:** Ley 18.695, Ley 19.880, DL 3.063, Ley 18.290 y fichas de ChileAtiende (ver `data/externa/FUENTES.md`).
- **Prueba:** `data/preguntas_prueba.json`, 10 preguntas (requisitos, plazos, normativa, folio, cálculo, fuera de
  alcance, información inexistente e inyección de prompt), cada una con respuesta de referencia, herramientas
  requeridas y fuentes esperadas.

## Parámetros RAG

`CHUNK_SIZE=800`, `CHUNK_OVERLAP=120`, fragmentos de menos de 50 caracteres descartados, `TOP_K=4`,
`MIN_SIMILITUD` calibrado en la sección 9, `MAX_TURNOS_HISTORIAL=4`, temperatura 0.1. Los fragmentos bajo el
umbral se descartan y la herramienta responde `SIN_RESULTADOS`, lo que el prompt obliga a informar en vez de
inventar. Todos los parámetros están en la celda *Parámetros de la solución* (sección 3).

## Prompts

| Versión | Técnica | Qué resuelve |
|---|---|---|
| v1 | Zero-shot mínimo (línea base) | Mide qué falla sin reglas |
| v2 | Zero-shot estructurado: rol, alcance, uso de herramientas, reglas y formato | Anti-alucinación, citas, privacidad, rechazo de inyección |
| v3 | v2 + few-shot (3 ejemplos) | Formato y comportamiento estables en "sin resultados" y "fuera de alcance" |

Los ejemplos de v3 usan trámites distintos a los del set de prueba para no filtrar las respuestas esperadas.
El cálculo de plazos se delega a código Python (técnica PAL), no al LLM.

## Evaluación y resultados

Métricas (evaluadores con la firma de LangSmith `(run, example) → {"key", "score"}`):
`herramienta_correcta`, `retrieval`, `cita_fuentes` y `privacidad` (deterministas); `correctness` y
`faithfulness` (LLM evaluador Gemini, distinto del modelo evaluado).

Resultados en modo rápido (6 preguntas por versión) — *completar con la tabla de la sección 17*:

| Prompt | herramienta_correcta | retrieval | cita_fuentes | privacidad | correctness | faithfulness | latencia (s) |
|---|---|---|---|---|---|---|---|
| v1 | | | | | | | |
| v2 | | | | | | | |
| v3 | | | | | | | |

Evidencia por pregunta en `evidencias/resultados_<version>_rapida.md` y experimentos en el proyecto
`Asistente-Tramites-Curacavi` de LangSmith.

## Evidencias de pruebas

Capturas de la ejecución en Colab (qué debe mostrar cada una: [`capturas/README.md`](capturas/README.md)).

| | |
|---|---|
| ![Modelo y tool calling](capturas/01_modelo_tool_calling.png) **1.** Modelo gratuito que usa herramientas (§4) | ![Chunking](capturas/02_chunking.png) **2.** Fragmentos por fuente (§7) |
| ![Índices FAISS](capturas/03_indices_faiss.png) **3.** Índices FAISS construidos (§8) | ![Calibración](capturas/04_calibracion_umbral.png) **4.** Calibración del umbral (§9) |
| ![RAG base](capturas/05_rag_base.png) **5.** RAG base con fuentes y fuera de contexto (§10) | ![Prompts](capturas/06_comparacion_prompts.png) **6.** Comparación de prompts v1/v2/v3 (§11) |
| ![Herramientas](capturas/07_pruebas_herramientas.png) **7.** Pruebas de herramientas (§12) | ![Memoria](capturas/08_memoria_streaming.png) **8.** Memoria y streaming (§14) |
| ![Traza documentos](capturas/09_traza_documentos.png) **9.** Traza: documento interno y ley (§15) | ![Traza datos](capturas/10_traza_folio_plazo.png) **10.** Traza: folio y plazo hábil (§15) |
| ![Métricas](capturas/11_metricas.png) **11.** Tabla de métricas (§16–17) | ![Gráfico](capturas/12_grafico_evaluacion.png) **12.** Gráfico de evaluación (§17) |
| ![LangSmith experimentos](capturas/13_langsmith_experimentos.png) **13.** Experimentos en LangSmith | ![LangSmith traza](capturas/14_langsmith_traza.png) **14.** Traza de una consulta en LangSmith |

## Si el modelo deja de estar disponible o se agota la cuota

- Los modelos gratuitos de OpenRouter cambian con frecuencia: la sección 4 busca uno gratuito con tool calling y lo
  prueba con una sola solicitud antes de usarlo.
- La capa gratuita de OpenRouter permite ~50 solicitudes al día. Las respuestas se guardan en
  `evidencias/cache_respuestas.json`: si la cuota se agota, el notebook se detiene con un aviso y, tras subir
  `evidencias/` al repositorio, la siguiente ejecución continúa desde donde quedó.

## Limitaciones conocidas

- Los PDF escaneados sin texto no se indexan (no hay OCR).
- Los registros de solicitudes y el procedimiento interno son simulados.
- La información es tan actual como los documentos indexados; no hay actualización automática.
- El historial conversacional se limita a 4 turnos; en conversaciones largas se pierde el contexto inicial.
- La calidad de la respuesta depende del modelo gratuito disponible y de su soporte de tool calling; los
  proveedores gratuitos pueden registrar los prompts, por lo que solo se procesan datos públicos o simulados.

## Uso de herramientas de IA

<Declarar aquí qué herramientas de IA usaron y para qué, igual que en el informe.>

## Referencias

- Biblioteca del Congreso Nacional de Chile. (2003). *Ley N° 19.880*. https://www.bcn.cl/leychile/navegar?i=210676
- Biblioteca del Congreso Nacional de Chile. (2006). *DFL 1, texto refundido de la Ley N° 18.695*. https://www.bcn.cl/leychile/navegar?i=251693
- Biblioteca del Congreso Nacional de Chile. (1996). *Decreto 2385, texto refundido del DL N° 3.063*. https://bcn.cl/30226
- Biblioteca del Congreso Nacional de Chile. (2007). *DFL 1, texto refundido de la Ley N° 18.290*. https://bcn.cl/3f03u
- ChileAtiende. (2026). *Permiso de circulación*. https://www.chileatiende.gob.cl/fichas/9611
- ChileAtiende. (2026). *Licencias de conducir*. https://www.chileatiende.gob.cl/fichas/20592
- Araya, C. R. (2026). *Ingeniería de soluciones con inteligencia artificial* [Repositorio]. GitHub. https://github.com/iam-docente/ingenieria-de-soluciones-con-IA
- Lewis, P., et al. (2020). Retrieval-augmented generation for knowledge-intensive NLP tasks. *NeurIPS, 33*. https://arxiv.org/abs/2005.11401
