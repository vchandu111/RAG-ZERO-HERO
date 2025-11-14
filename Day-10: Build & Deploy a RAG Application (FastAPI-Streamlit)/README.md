# Day 10 — Build & Deploy a RAG Application (FastAPI/Streamlit)

## 1. Beginner-Friendly Introduction

Congratulations! You've reached the final day. Today, you'll build and deploy a complete RAG application that others can use. You'll create a web interface and API so your RAG system is accessible and production-ready.

**What you'll build:**

- **FastAPI Backend**: REST API for your RAG system
- **Streamlit Frontend**: User-friendly web interface
- **Deployment**: Make it accessible to others

**Why this matters:**

- Real applications need interfaces
- APIs allow integration with other systems
- Web interfaces make systems accessible
- Deployment makes your work usable

**Real-world context:**
Your RAG system is powerful, but it's just code. To make it useful, you need:

- A way for users to interact (web UI)
- A way for other systems to use it (API)
- A way to access it from anywhere (deployment)

---

## 2. Deep-Dive Explanation

### 2.1 FastAPI for RAG Backend

**What is FastAPI?**
A modern Python web framework for building APIs:

- Fast and performant
- Automatic API documentation
- Type hints support
- Easy to use

**Why FastAPI for RAG?**

- Handles async operations well
- Great for ML/AI applications
- Automatic validation
- Easy to deploy

**Key Components:**

- **Routes**: API endpoints
- **Models**: Request/response schemas
- **Dependencies**: Reusable components
- **Middleware**: Cross-cutting concerns

### 2.2 Streamlit for RAG Frontend

**What is Streamlit?**
A Python framework for building web apps:

- Simple and intuitive
- Great for data/ML apps
- No frontend knowledge needed
- Fast development

**Why Streamlit for RAG?**

- Perfect for interactive AI apps
- Easy to add file uploads
- Simple chat interfaces
- Quick prototyping

**Key Components:**

- **Widgets**: Inputs, buttons, displays
- **Layout**: Organize your UI
- **State**: Manage app state
- **Session**: User sessions

### 2.3 Application Architecture

**Complete System:**

```
┌─────────────┐
│  Streamlit  │  User Interface
│   Frontend  │
└──────┬──────┘
       │ HTTP
       ▼
┌─────────────┐
│   FastAPI   │  REST API
│   Backend   │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│  RAG System │  Your RAG Pipeline
└─────────────┘
```

### 2.4 API Design

**Essential Endpoints:**

- `POST /index` - Index documents
- `POST /query` - Query RAG system
- `GET /health` - Health check
- `GET /stats` - System statistics
- `DELETE /documents/{id}` - Remove document

**Request/Response Models:**

- Structured data
- Validation
- Type safety
- Documentation

### 2.5 Deployment Options

**Local Deployment:**

- Run on your machine
- Access via localhost
- Good for testing

**Cloud Deployment:**

- **Heroku**: Easy, free tier
- **Railway**: Simple deployment
- **Render**: Free hosting
- **AWS/GCP/Azure**: Production scale

**Containerization:**

- Docker for packaging
- Easy deployment
- Consistent environment

---

## 3. Instructor Examples

### Example 1: FastAPI RAG Backend

```python
from fastapi import FastAPI, UploadFile, File, HTTPException
from pydantic import BaseModel
from typing import List, Optional
import os

app = FastAPI(title="RAG API", version="1.0.0")

# Your RAG system (from previous days)
from rag_system import RAGSystem

rag = RAGSystem()

# Request/Response Models
class QueryRequest(BaseModel):
    question: str
    k: Optional[int] = 3

class QueryResponse(BaseModel):
    answer: str
    sources: List[dict]
    processing_time: float

class IndexResponse(BaseModel):
    message: str
    chunks_indexed: int

# API Endpoints
@app.get("/")
async def root():
    return {"message": "RAG API is running"}

@app.get("/health")
async def health():
    return {"status": "healthy"}

@app.post("/index", response_model=IndexResponse)
async def index_document(file: UploadFile = File(...)):
    """Index a document"""
    try:
        # Save uploaded file
        file_path = f"temp/{file.filename}"
        os.makedirs("temp", exist_ok=True)

        with open(file_path, "wb") as f:
            content = await file.read()
            f.write(content)

        # Index document
        chunks = rag.index_document(file_path)

        # Clean up
        os.remove(file_path)

        return IndexResponse(
            message=f"Document indexed successfully",
            chunks_indexed=chunks
        )
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))

@app.post("/query", response_model=QueryResponse)
async def query_rag(request: QueryRequest):
    """Query the RAG system"""
    import time
    start_time = time.time()

    try:
        result = rag.query(request.question, k=request.k)
        processing_time = time.time() - start_time

        return QueryResponse(
            answer=result["answer"],
            sources=result["sources"],
            processing_time=processing_time
        )
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))

@app.get("/stats")
async def get_stats():
    """Get system statistics"""
    stats = rag.get_statistics()
    return stats

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

### Example 2: Streamlit RAG Frontend

```python
import streamlit as st
import requests
import time

# API URL
API_URL = "http://localhost:8000"

# Page config
st.set_page_config(
    page_title="RAG Application",
    page_icon="🤖",
    layout="wide"
)

# Title
st.title("🤖 RAG Application")
st.markdown("Ask questions about your documents!")

# Sidebar
with st.sidebar:
    st.header("📄 Document Management")

    # File upload
    uploaded_file = st.file_uploader(
        "Upload a document",
        type=["pdf", "txt"],
        help="Upload PDF or TXT files"
    )

    if uploaded_file:
        if st.button("Index Document"):
            with st.spinner("Indexing document..."):
                files = {"file": uploaded_file.getvalue()}
                response = requests.post(
                    f"{API_URL}/index",
                    files=files
                )

                if response.status_code == 200:
                    result = response.json()
                    st.success(f"✅ {result['message']}")
                    st.info(f"Indexed {result['chunks_indexed']} chunks")
                else:
                    st.error("Failed to index document")

    # Statistics
    if st.button("View Statistics"):
        response = requests.get(f"{API_URL}/stats")
        if response.status_code == 200:
            stats = response.json()
            st.json(stats)

# Main area
st.header("💬 Ask a Question")

# Query input
question = st.text_input(
    "Enter your question:",
    placeholder="What is the main topic?",
    key="question_input"
)

# K value
k = st.slider("Number of sources:", 1, 10, 3)

# Query button
if st.button("Ask", type="primary") and question:
    with st.spinner("Thinking..."):
        # Make API request
        response = requests.post(
            f"{API_URL}/query",
            json={"question": question, "k": k}
        )

        if response.status_code == 200:
            result = response.json()

            # Display answer
            st.subheader("📝 Answer")
            st.write(result["answer"])

            # Display sources
            st.subheader("📚 Sources")
            for i, source in enumerate(result["sources"], 1):
                with st.expander(f"Source {i} (Similarity: {source.get('similarity', 'N/A'):.2f})"):
                    st.write(source.get("text", ""))
                    if "metadata" in source:
                        st.caption(f"Source: {source['metadata']}")

            # Processing time
            st.caption(f"⏱️ Processed in {result['processing_time']:.2f} seconds")
        else:
            st.error("Failed to get answer")

# Chat history (optional)
if "chat_history" not in st.session_state:
    st.session_state.chat_history = []

# Display history
if st.session_state.chat_history:
    st.header("💭 Chat History")
    for i, (q, a) in enumerate(reversed(st.session_state.chat_history[-5:]), 1):
        with st.expander(f"Q{i}: {q}"):
            st.write(a)
```

### Example 3: Complete Deployment Setup

```python
# requirements.txt
fastapi==0.104.1
uvicorn==0.24.0
streamlit==1.28.0
openai==1.3.0
chromadb==0.4.15
pypdf==3.17.0
requests==2.31.0
python-multipart==0.0.6

# Dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]

# docker-compose.yml
version: '3.8'
services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      - OPENAI_API_KEY=${OPENAI_API_KEY}
    volumes:
      - ./data:/app/data
```

---

## 4. Student Practice Tasks

### Task 1: FastAPI Backend

Build a FastAPI backend with:

- Document indexing endpoint
- Query endpoint
- Health check
- Statistics endpoint

### Task 2: Streamlit Frontend

Create a Streamlit UI with:

- File upload
- Query interface
- Answer display
- Source display

### Task 3: Integration

Connect Streamlit to FastAPI:

- Make API calls
- Handle errors
- Display results
- Add loading states

### Task 4: Error Handling

Add comprehensive error handling:

- API errors
- File errors
- Validation errors
- User-friendly messages

### Task 5: Deployment

Deploy your application:

- Local deployment
- Cloud deployment (choose one)
- Docker containerization
- Environment variables

### Task 6: Documentation

Create documentation:

- API documentation
- User guide
- Deployment instructions
- README file

---

## 5. Summary / Key Takeaways

- **FastAPI** provides a fast, modern API framework
- **Streamlit** makes building UIs simple
- **APIs** enable integration with other systems
- **Web interfaces** make systems accessible
- **Deployment** makes your work usable
- **Docker** simplifies deployment
- **Error handling** is crucial for production
- **Documentation** helps users
- **Testing** ensures reliability
- **You've built a complete RAG system!** 🎉

---

## 6. Further Reading (Optional)

- FastAPI Documentation
- Streamlit Documentation
- Docker Documentation
- Deployment guides (Heroku, Railway, Render)
- API design best practices

---

**Congratulations on completing the 10-day RAG roadmap!** 🎊

You now have:

- ✅ Python foundations
- ✅ LLM understanding
- ✅ Prompt engineering skills
- ✅ Data extraction capabilities
- ✅ Embedding knowledge
- ✅ RAG system building
- ✅ Framework experience
- ✅ Advanced techniques
- ✅ Deployment skills

**Keep building and learning!** 🚀
