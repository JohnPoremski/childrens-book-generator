# Children's Book Generator

FastAPI app that turns a character description, art style, and moral of the
story into a short illustrated picture book: plot beats written as text, and
a matching illustration generated for each page. Optional rhyming mode.

Plot beats are generated with an OpenRouter chat completions model
(`openai/gpt-4o-mini`). Illustrations are generated through OpenRouter's
image endpoint (`google/gemini-3.1-flash-image`), with the first page's
image passed back in as a reference on every later page so the character and
art style stay consistent across the book.

## Running locally

```
pip install -r requirements.txt
uvicorn book_app:app --host 0.0.0.0 --port 8000
```

Requires `OPENAI_API_KEY` in the environment, set to an OpenRouter API key
(the `openai` client points at `https://openrouter.ai/api/v1`). Image
generation requires OpenRouter account credits, separate from chat
completions.

## Deploying

Set `OPENAI_API_KEY` as a secret/environment variable on the host — do not
commit it. `Procfile` starts the app with `uvicorn book_app:app --host
0.0.0.0 --port $PORT`, binding whatever port the platform assigns.

Generated books are written to `data/childrens_books/` on the container's
local disk; this is not persisted across redeploys.
