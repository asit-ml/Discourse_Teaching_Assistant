# Discourse Teaching Assistant

An AI-powered virtual teaching assistant for the IIT Madras Tools in Data Science course. The API retrieves relevant course and Discourse content from a local SQLite knowledge base, then uses an OpenAI-compatible AI proxy to generate a concise answer with supporting links.

**Live application:** https://discourse-teaching-assistant.vercel.app/

**API endpoint:** https://discourse-teaching-assistant.vercel.app/api/

## What It Does

- Accepts student questions through a FastAPI endpoint
- Generates embeddings for the question
- Retrieves the most relevant knowledge-base chunks using cosine similarity
- Produces a short answer with up to two supporting links
- Supports an optional base64-encoded image field in the request schema
- Returns a predictable JSON response for frontend integration

## Technology Stack

- Python 3.10+
- FastAPI and Pydantic
- SQLite for the embedded knowledge base
- NumPy for similarity scoring
- `aiohttp` for asynchronous AI-proxy requests
- Vercel Python configuration via `vercel.json`

## API

### `POST /api/`

Request body:

```json
{
  "question": "What is the submission deadline?",
  "image": null
}
```

Response body:

```json
{
  "answer": "A concise answer based on the indexed course material.",
  "links": [
    {
      "url": "https://example.com/reference",
      "text": "Related course reference"
    }
  ]
}
```

## Run Locally

```bash
git clone https://github.com/23f1000318/Discourse_Teaching_Assistant.git
cd Discourse_Teaching_Assistant

python -m venv .venv
source .venv/bin/activate       # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Create a `.env` file with the raw AI proxy token:

```env
AIPROXY_TOKEN=your_token_here
```

Start the API with:

```bash
uvicorn app:app --reload --host 0.0.0.0 --port 8000
```

The API will be available at `http://localhost:8000/api/`; interactive documentation is available at `/docs`.

## Data and Configuration

- `knowledge_base.db` contains the indexed markdown chunks and embeddings used for retrieval.
- `AIPROXY_TOKEN` is required for both embedding and chat-completion requests.
- The application currently uses `text-embedding-3-small` for embeddings and `gpt-4o-mini` for answer generation through the configured AI proxy.

Do not commit real API tokens. Restrict CORS and configure production secrets before exposing the service publicly.

## Deployment

The repository includes `vercel.json` for a Vercel Python deployment. Configure `AIPROXY_TOKEN` as a Vercel environment variable, deploy the repository, and verify `POST /api/` after deployment.

## License

See [LICENSE](LICENSE) for license information.
