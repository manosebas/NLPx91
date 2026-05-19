# NLPx91

**Proyecto Final — Procesamiento de Lenguaje Natural**
**Autor:** Manolo Iniguez
**Universidad San Francisco de Quito (USFQ)**

---

Sistema Agentic RAG en español que responde preguntas sobre el Mundial 2026 y la plataforma 91 de predicciones (noventayuno.com).

## Stack

- OpenAI `gpt-4o-mini` + `text-embedding-3-small`
- LangChain + LangGraph
- Chroma (vector store local)
- RAGAS para evaluación

## Setup

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

Crear archivo `.env` en la raíz:

```ini
OPENAI_API_KEY=sk-...
OPENAI_MODEL=gpt-4o-mini
KAGGLE_USERNAME=tu_usuario
KAGGLE_KEY=tu_kaggle_key
```

## Ejecución

Correr los notebooks en orden numérico (`00` a `09`). El `10-live-test.ipynb` es para probar el agente de forma interactiva.
