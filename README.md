# NLPx91 — Sistema Agentic RAG sobre Mundial 2026 + Plataforma 91

**Proyecto Final — Procesamiento de Lenguaje Natural**
**Autor:** Manolo Iniguez
**Universidad San Francisco de Quito (USFQ)**

---

## Resumen

Asistente conversacional **Agentic RAG en español** que responde preguntas sobre el **Mundial de Fútbol 2026** y la **plataforma 91 de predicciones** (noventayuno.com). Combina dos corpus complementarios y un agente con **tool calling** que decide cuál fuente consultar según la pregunta.

- **Corpus externo (Mundial):** 438 docs `.md` — Wikipedia ES + Reglamento FIFA 2026 + Kaggle histórico + Calendario 104 partidos → **4497 chunks**
- **Corpus interno (Plataforma 91):** 55 docs `.md` con frontmatter YAML → **316 chunks**
- **Embeddings:** OpenAI `text-embedding-3-small` (1536-dim)
- **LLM:** OpenAI `gpt-4o-mini` (`temperature=0`)
- **Framework:** LangGraph (`StateGraph` con 3 nodos)
- **Evaluación:** RAGAS (4 métricas) + 2 métricas custom sobre 40 Q/A reales

## Resultado clave

El agente Agentic supera al baseline RAG clásico en **3/4 métricas RAGAS** y cumple **3/4 targets absolutos calibrados** para `gpt-4o-mini` como judge.

| Métrica | Baseline | Agente | Δ | Target | Base | Agente |
|---|---:|---:|---:|---:|:---:|:---:|
| Faithfulness | 0.643 | **0.767** | +0.124 | 0.70 | ❌ | ✅ |
| Answer Relevancy | 0.691 | 0.476 | −0.215 | 0.65 | ✅ | ❌ |
| Answer Correctness | 0.572 | **0.645** | +0.073 | 0.60 | ❌ | ✅ |
| Factual Correctness | 0.542 | **0.628** | +0.086 | 0.60 | ❌ | ✅ |

**Killer multi-hop:** en preguntas que cruzan ambos corpus el baseline obtiene scores cercanos a cero (0.000/0.000/0.041/0.000) mientras el agente alcanza 0.875/0.655/0.685/0.508 — diferencia dramática que valida la arquitectura agéntica.

**Métricas custom:**
- Fuente recall: baseline 0.737, agente 0.763
- Tool routing accuracy (agente): **0.925**

---

## Arquitectura

```
Usuario pregunta en español
            │
   Agente ReAct (LLM)
            │
       ¿Qué necesita?
       ├── buscar_mundial   (corpus externo, 4497 chunks)
       └── buscar_plataforma (corpus interno, 316 chunks)
            │
   Respuesta con citas a fuentes
```

Grafo LangGraph simplificado:
```
START → generate_query_or_respond
          ├── (sin tool_calls) → END
          └── (tool_calls)     → retrieve → generate_answer → END
```

---

## Estructura del proyecto

```
.
├── Corpus_91/                    # Corpus interno plataforma 91 (55 docs)
│   ├── identidad/  predicciones/  rankings/  tribus/  torneos/
│   ├── monedas-tienda/  perfil-referidos/  navegacion/
│   └── legal/  privacidad/  registro/
│
├── Corpus_Mundial/               # Corpus externo Mundial (438 docs)
│   ├── wikipedia/                # 174 docs (selecciones, estadios, jugadores, conceptos)
│   ├── reglamento-fifa/          # 53 docs (PDF oficial parseado)
│   ├── kaggle/                   # 107 docs (Mundiales 1930-2022)
│   │   ├── mundiales-por-edicion/
│   │   └── equipos-historico/
│   └── calendario-2026/          # 104 docs (1 por partido del fixture)
│
├── notebooks/
│   ├── 00-build-corpus-calendar2026.ipynb   # Calendario 104 partidos
│   ├── 01-build-corpus-wikipedia.ipynb      # Wikipedia ES
│   ├── 02-build-corpus-reglamento.ipynb     # Reglamento FIFA (PDF)
│   ├── 03-build-corpus-kaggle.ipynb         # Mundiales históricos
│   ├── 04-build-index.ipynb                 # Chunking + embeddings + Chroma
│   ├── 05-build-agent.ipynb                 # Agente LangGraph + baseline
│   ├── 06-build-eval-set.ipynb              # Eval set 40 Q/A
│   ├── 07-run-systems.ipynb                 # Correr baseline + agente
│   ├── 08-eval-ragas.ipynb                  # Evaluación RAGAS
│   ├── 09-entregable.ipynb                  # Notebook entregable consolidado
│   ├── 10-live-test.ipynb                   # Demo interactiva
│   ├── eval_set.jsonl                       # 40 preguntas con ground truth
│   ├── results_baseline.jsonl               # Respuestas del baseline
│   ├── results_agent.jsonl                  # Respuestas del agente
│   ├── ragas_baseline.csv, ragas_agent.csv  # Scores RAGAS por pregunta
│   └── ragas_scores.json                    # Métricas consolidadas
│
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Setup

### 1. Clonar y crear entorno virtual

```bash
git clone https://github.com/manosebas/NLPx91.git
cd NLPx91

python -m venv .venv
.venv\Scripts\activate          # Windows PowerShell
# source .venv/bin/activate     # macOS / Linux

pip install -r requirements.txt
```

### 2. Variables de entorno

Crear archivo `.env` en la raíz:

```ini
OPENAI_API_KEY=sk-...
OPENAI_MODEL=gpt-4o-mini
KAGGLE_USERNAME=tu_usuario
KAGGLE_KEY=tu_kaggle_key
```

- `OPENAI_API_KEY`: necesaria para embeddings + LLM. Cobertura del curso o tu propia cuenta.
- `KAGGLE_USERNAME` / `KAGGLE_KEY`: solo si vas a re-correr NB03 (Kaggle → docs históricos). Obtener en kaggle.com → Settings → API → Create New Token.

---

## Ejecución

El proyecto está dividido en 11 notebooks que se corren en orden:

| # | Notebook | Fase | Output |
|---|---|---|---|
| 00 | `00-build-corpus-calendar2026.ipynb` | F4B | 104 docs calendario Mundial 2026 |
| 01 | `01-build-corpus-wikipedia.ipynb` | F3 | 174 docs Wikipedia ES |
| 02 | `02-build-corpus-reglamento.ipynb` | F3 | 53 docs Reglamento FIFA |
| 03 | `03-build-corpus-kaggle.ipynb` | F3 | 107 docs Mundiales históricos |
| 04 | `04-build-index.ipynb` | F5 | Chroma persistente (4813 chunks) |
| 05 | `05-build-agent.ipynb` | F6 | Agente LangGraph + baseline |
| 06 | `06-build-eval-set.ipynb` | F7 | Eval set validado (40 Q/A) |
| 07 | `07-run-systems.ipynb` | F8a | Respuestas baseline + agente (JSONL) |
| 08 | `08-eval-ragas.ipynb` | F8b | Scores RAGAS + métricas custom |
| 09 | `09-entregable.ipynb` | F9 | Notebook consolidado para entrega |
| 10 | `10-live-test.ipynb` | demo | Pregunta interactiva al agente |

**Nota Windows:** los notebooks 07 y 08 están separados a propósito por un conflicto de DLLs (`tokenizers` Rust) entre `ragas`, `langgraph` y `chromadb`. Reiniciar el kernel entre ambos.

---

## Stack

- **Python 3.13**
- **OpenAI API** (`text-embedding-3-small`, `gpt-4o-mini`)
- **Chroma** — vector store local persistente
- **LangChain + LangGraph** — orquestación del agente y tool calling
- **RAGAS** — evaluación automática con LLM-as-judge
- **pandas, pypdf, wikipedia-api, kagglehub, python-frontmatter** — utilidades de corpus

---

## Decisiones de diseño documentadas

1. **Dos colecciones Chroma separadas** (`mundial`, `plataforma`) alineadas con las dos tools del agente para evitar ruido cross-corpus.
2. **Grafo simplificado de 3 nodos** (sin `grade_documents` ni `rewrite_question` del patrón completo): elimina loops infinitos en multi-hop y context overflow.
3. **Splitters reescritos en Python puro** para esquivar conflicto `tokenizers` ↔ `chromadb` en Windows.
4. **`text-embedding-3-small` en lugar de modelo local** (`multilingual-e5-base`) por estabilidad del kernel en Windows y simplicidad del stack.
5. **Calibración de targets V1 → V2** según el LLM-judge (`gpt-4o-mini` da scores ~0.05-0.10 más bajos que `gpt-4-turbo` usado en papers RAGAS).
6. **Eval set manual** (40 Q/A escritas a mano contra docs reales) en lugar de set sintético generado por LLM.

---

## Licencia

Proyecto académico — uso educativo USFQ.
