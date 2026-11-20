# Ask My Notes

Ask questions about your PDF notes using a chat interface. The backend extracts text from PDFs, stores searchable chunks in ChromaDB, and uses Groq to answer questions based on the most relevant chunks.

## Requirements

- Python 3.10 or later
- Node.js and npm
- A [Groq API key](https://console.groq.com/keys)

## Setup

### 1. Configure the backend

In a terminal, move into the backend folder and create a virtual environment:

```powershell
cd backend
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

Create `backend/.env` and add your Groq API key:

```env
GROQ_API_KEY=your_groq_api_key
```

Put PDF files in `backend/docs/`, then index them:

```powershell
python ingest.py
```

This creates or updates the local ChromaDB index in `backend/notes_db/`.

### 2. Start the backend

From the `backend` folder, run:

```powershell
uvicorn main:app --reload
```

The API runs at `http://localhost:8000`. Interactive API documentation is available at `http://localhost:8000/docs`.

### 3. Start the frontend

In a second terminal, from the project root:

```powershell
cd frontend
npm install
npm run dev
```

Open the local URL printed by Vite in your browser. Keep the backend running while using the app.

## API

- `GET /` — backend status and indexed chunk count
- `POST /ask` — accepts `{"question": "..."}` and returns an answer and its PDF/page sources

The answer is generated from the retrieved note excerpts. If the excerpts do not contain the answer, the assistant is instructed to say that it was not found in your notes.

## Project structure

```text
backend/
  docs/         PDF files to index
  ingest.py     PDF extraction and ChromaDB indexing
  main.py       FastAPI question-answering API
  requirements.txt
frontend/
  src/          React chat interface
```
