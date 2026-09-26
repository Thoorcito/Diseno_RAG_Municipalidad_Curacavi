# Arquitectura

La solución se organiza en una fase de **ingesta** (se ejecuta una vez) y tres módulos en línea
—**recuperación**, **procesamiento** y **generación**— coordinados por el **agente**. La **evaluación**
usa un modelo distinto (Gemini) como evaluador y LangSmith para trazas y experimentos.
Todo está implementado en el notebook [`notebooks/asistente_tramites_colab.ipynb`](../notebooks/asistente_tramites_colab.ipynb).

![Arquitectura](img/arquitectura.png)

Versión vectorial: [`img/arquitectura.svg`](img/arquitectura.svg).

## Componentes

| Módulo | Componente | Implementación | Notebook |
|---|---|---|---|
| Ingesta | Carga y limpieza | `pypdf`, eliminación de encabezados/pies repetidos, metadatos fuente/página/artículo | §6 |
| Ingesta | Chunking | `RecursiveCharacterTextSplitter` 800/120, separadores por artículo, filtro < 50 caracteres | §7 |
| Ingesta | Embeddings | `models/gemini-embedding-001` (768 dim), por lotes con pausa y reanudable | §8 |
| Recuperación | Índices | FAISS `interna` (municipio) y `externa` (leyes, ChileAtiende), `normalize_L2=True` | §8 |
| Recuperación | Herramientas | `buscar_informacion_municipal`, `buscar_normativa_externa`, `consultar_estado_solicitud`, `calcular_plazo_habil`, `fecha_actual` | §12 |
| Procesamiento | Control de contexto | top-k 4, umbral de similitud calibrado, `SIN_RESULTADOS`, formato con fuente/página/artículo/similitud | §9, §10 |
| Generación | LLM | OpenRouter (modelo `:free` con tool calling verificado), temperatura 0.1 | §4 |
| Agente | Orquestación | `create_agent` + `ToolCallLimitMiddleware` (4) + `ModelCallLimitMiddleware` (6) + cierre forzado + reintento ante 429 | §13 |
| Agente | Memoria | ventana de 4 intercambios, solo texto (`WindowChatMessageHistory`) | §14 |
| Trazabilidad | Registro | `evidencias/registro_consultas.jsonl` (pregunta, herramientas, contexto, respuesta, latencia) | §13, §17 |
| Evaluación | Métricas | herramienta correcta, retrieval, citas, privacidad (deterministas) + correctness, faithfulness (Gemini) | §16 |
| Evaluación | Experimentos | LangSmith: `prompt-v1`, `prompt-v2`, `prompt-v3` | §16 |

## Flujo de una consulta

1. El agente envía la pregunta, el prompt del sistema y el historial acotado al LLM, que **decide** qué herramienta invocar.
2. La herramienta consulta el índice FAISS, el CSV o calcula la fecha.
3. El módulo de procesamiento filtra por relevancia (top-k + umbral) y adjunta la **fuente** de cada fragmento.
4. El LLM **genera** la respuesta usando solo ese contexto y termina con "Fuentes:".

Cada consulta queda registrada con sus herramientas, fragmentos, latencia y respuesta.
