# DS-RPC-01 — Internal Chatbot with Role-Based Access Control

A RAG-based internal chatbot built with FastAPI that serves personalized responses based on the user's role. Built as part of the **DS RPC-01 Resume Project Challenge**.

---

## What This Project Does

This application is an internal company chatbot that:

- Answers employee questions using **Retrieval-Augmented Generation (RAG)**
- Enforces **role-based access control (RBAC)** so different users see different information
- Uses **FastAPI** as the backend with HTTP Basic Authentication built in
- Retrieves relevant documents from a knowledge base before generating a response

---

## Project Structure

```
DS-RPC-01-Internal-chatbot-with-role-based-access-control/
├── app/                  # Main application code
│   ├── main.py           # FastAPI entry point, auth logic
│   ├── rag.py            # RAG pipeline (retrieval + generation)
│   ├── roles.py          # Role definitions and access rules
│   └── ...
├── resources/            # Knowledge base documents
├── pyproject.toml        # Project dependencies
├── python-version.txt    # Required Python version
└── .gitignore
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend Framework | FastAPI |
| Authentication | HTTP Basic Auth (FastAPI HTTPBasic) |
| RAG Pipeline | LangChain / LlamaIndex *(customize as needed)* |
| LLM | OpenAI / local model *(customize as needed)* |
| Vector Store | FAISS / ChromaDB *(customize as needed)* |

---

## Getting Started

### 1. Prerequisites

- Python 3.10+ (see `python-version.txt`)
- pip or uv package manager

### 2. Clone the Repository

```bash
git clone https://github.com/Musawir456/DS-RPC-01-Internal-chatbot-with-role-based-access-control.git
cd DS-RPC-01-Internal-chatbot-with-role-based-access-control
```

### 3. Install Dependencies

```bash
pip install -e .
```

Or using uv:

```bash
uv sync
```

### 4. Run the App

```bash
uvicorn app.main:app --reload
```

The API will be available at `http://localhost:8000`

### 5. API Docs

Visit `http://localhost:8000/docs` for the interactive Swagger UI.

---

## Authentication

This project uses **HTTP Basic Authentication** via FastAPI's `HTTPBasic` dependency.

```python
from fastapi.security import HTTPBasic, HTTPBasicCredentials

security = HTTPBasic()

@app.get("/chat")
def chat(credentials: HTTPBasicCredentials = Depends(security)):
    # role is determined by the username
    ...
```

You can test it by passing credentials in the Swagger UI or via curl:

```bash
curl -u admin:password http://localhost:8000/chat?query="What is the leave policy?"
```

---

## Role-Based Access Control

Different users have different roles, and each role has access to a different subset of documents in the knowledge base.

| Role | Access Level |
|---|---|
| `admin` | Full access to all documents |
| `hr` | HR policies, employee records |
| `engineer` | Technical docs, engineering guides |
| `finance` | Financial reports, budgets |

Roles are assigned per user and control which documents are passed to the RAG retriever.

---

## How RAG Works Here

```
User Query
    ↓
Role Check → Filter allowed documents
    ↓
Retrieve relevant chunks from vector store
    ↓
Pass chunks + query to LLM
    ↓
Return grounded response
```

This ensures the chatbot never surfaces information outside the user's permission level.

---

## Environment Variables

Create a `.env` file in the root directory:

```env
OPENAI_API_KEY=your_key_here
SECRET_KEY=your_secret
```

---

## Resources

- [DS RPC-01 Challenge Page](https://datascienceproject.com/rpc-01) *(update with actual link)*
- [FastAPI Docs](https://fastapi.tiangolo.com)
- [FastAPI Security — HTTP Basic](https://fastapi.tiangolo.com/advanced/security/http-basic-auth/)

---

## License

This is a starter repository for the DS RPC-01 Resume Project Challenge. Fork it, build on it, and make it your own.
