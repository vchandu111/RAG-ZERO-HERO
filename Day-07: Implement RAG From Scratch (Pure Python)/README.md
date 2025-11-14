# Day 7 — Implement RAG From Scratch (Pure Python)

## 1. Beginner-Friendly Introduction

Today, you'll build a complete RAG system using only pure Python—no frameworks! This deep dive will help you understand every component and how they work together. By building from scratch, you'll gain a solid foundation before using frameworks like LangChain.

**Why build from scratch?**
- Understand every component deeply
- No "magic" - you see how everything works
- Customize any part you want
- Better debugging skills
- Foundation for using frameworks later

**What you'll build:**
A complete, production-ready RAG system with:
- Document processing
- Embedding generation
- Vector storage and search
- Prompt construction
- LLM integration
- Answer generation

---

## 2. Deep-Dive Explanation

### 2.1 System Architecture

**Complete RAG System Components:**

```
┌─────────────────┐
│  Document Loader │
└────────┬─────────┘
         │
         ▼
┌─────────────────┐
│  Text Chunker    │
└────────┬─────────┘
         │
         ▼
┌─────────────────┐
│ Embedding Model │
└────────┬─────────┘
         │
         ▼
┌─────────────────┐
│  Vector Store   │
└────────┬─────────┘
         │
         ▼
┌─────────────────┐
│  Query Handler  │
└────────┬─────────┘
         │
         ▼
┌─────────────────┐
│  RAG Pipeline   │
└─────────────────┘
```

### 2.2 Component Design

**1. Document Loader**
- Read various file formats
- Extract text
- Handle errors

**2. Text Chunker**
- Split into manageable pieces
- Preserve context
- Add metadata

**3. Embedding Generator**
- Call OpenAI API
- Handle batching
- Cache embeddings

**4. Vector Store**
- Store embeddings
- Implement similarity search
- Manage metadata

**5. Query Processor**
- Convert query to embedding
- Search vector store
- Rank results

**6. RAG Pipeline**
- Orchestrate all components
- Handle errors
- Return formatted results

### 2.3 Implementation Strategy

**Class Structure:**
```python
class RAGSystem:
    - document_loader
    - chunker
    - embedding_generator
    - vector_store
    - llm_client
    
    Methods:
    - load_documents()
    - index_documents()
    - query()
    - get_stats()
```

**Error Handling:**
- API failures
- File errors
- Empty results
- Invalid inputs

**Configuration:**
- Chunk size
- K value
- Similarity threshold
- LLM parameters

---

## 3. Instructor Examples

### Example 1: Complete RAG System Structure

```python
import os
import openai
import json
import numpy as np
from typing import List, Dict, Optional

class DocumentLoader:
    """Load documents from various sources"""
    
    def load_text_file(self, filepath: str) -> str:
        """Load text from .txt file"""
        with open(filepath, 'r', encoding='utf-8') as f:
            return f.read()
    
    def load_pdf(self, filepath: str) -> str:
        """Load text from PDF"""
        import pypdf
        text = ""
        with open(filepath, 'rb') as f:
            reader = pypdf.PdfReader(f)
            for page in reader.pages:
                text += page.extract_text() + "\n"
        return text

class TextChunker:
    """Split text into chunks"""
    
    def __init__(self, chunk_size: int = 500, overlap: int = 50):
        self.chunk_size = chunk_size
        self.overlap = overlap
    
    def chunk_text(self, text: str, source: str = "unknown") -> List[Dict]:
        """Split text into chunks with metadata"""
        words = text.split()
        chunks = []
        
        for i in range(0, len(words), self.chunk_size - self.overlap):
            chunk_words = words[i:i + self.chunk_size]
            chunk_text = " ".join(chunk_words)
            
            chunks.append({
                "text": chunk_text,
                "source": source,
                "chunk_id": len(chunks) + 1,
                "word_count": len(chunk_words)
            })
        
        return chunks

class EmbeddingGenerator:
    """Generate embeddings using OpenAI"""
    
    def __init__(self, model: str = "text-embedding-ada-002"):
        self.model = model
        openai.api_key = os.getenv("OPENAI_API_KEY")
    
    def generate(self, text: str) -> List[float]:
        """Generate embedding for single text"""
        text = text.replace("\n", " ")
        response = openai.Embedding.create(
            model=self.model,
            input=text
        )
        return response['data'][0]['embedding']
    
    def generate_batch(self, texts: List[str]) -> List[List[float]]:
        """Generate embeddings for multiple texts"""
        texts = [t.replace("\n", " ") for t in texts]
        response = openai.Embedding.create(
            model=self.model,
            input=texts
        )
        return [item['embedding'] for item in response['data']]

class VectorStore:
    """Simple vector store using in-memory storage"""
    
    def __init__(self):
        self.embeddings = []
        self.chunks = []
        self.metadata = []
    
    def add(self, embeddings: List[List[float]], chunks: List[Dict]):
        """Add embeddings and chunks to store"""
        self.embeddings.extend(embeddings)
        self.chunks.extend(chunks)
        self.metadata.extend([c.get("metadata", {}) for c in chunks])
    
    def search(self, query_embedding: List[float], k: int = 3) -> List[Dict]:
        """Search for top K similar chunks"""
        if not self.embeddings:
            return []
        
        # Calculate similarities
        similarities = []
        query_vec = np.array(query_embedding)
        
        for emb in self.embeddings:
            emb_vec = np.array(emb)
            similarity = np.dot(query_vec, emb_vec) / (
                np.linalg.norm(query_vec) * np.linalg.norm(emb_vec)
            )
            similarities.append(similarity)
        
        # Get top K
        top_indices = np.argsort(similarities)[::-1][:k]
        
        results = []
        for idx in top_indices:
            results.append({
                "chunk": self.chunks[idx],
                "similarity": float(similarities[idx]),
                "metadata": self.metadata[idx]
            })
        
        return results

class RAGSystem:
    """Complete RAG system"""
    
    def __init__(self):
        self.loader = DocumentLoader()
        self.chunker = TextChunker()
        self.embedder = EmbeddingGenerator()
        self.vector_store = VectorStore()
        openai.api_key = os.getenv("OPENAI_API_KEY")
    
    def index_document(self, filepath: str):
        """Load, chunk, and index a document"""
        # Load
        if filepath.endswith('.txt'):
            text = self.loader.load_text_file(filepath)
        elif filepath.endswith('.pdf'):
            text = self.loader.load_pdf(filepath)
        else:
            raise ValueError(f"Unsupported file type: {filepath}")
        
        # Chunk
        chunks = self.chunker.chunk_text(text, source=filepath)
        
        # Generate embeddings
        chunk_texts = [c["text"] for c in chunks]
        embeddings = self.embedder.generate_batch(chunk_texts)
        
        # Store
        self.vector_store.add(embeddings, chunks)
        
        return len(chunks)
    
    def query(self, question: str, k: int = 3) -> Dict:
        """Complete RAG query"""
        # 1. Retrieve
        query_embedding = self.embedder.generate(question)
        results = self.vector_store.search(query_embedding, k)
        
        if not results:
            return {"answer": "No relevant documents found.", "sources": []}
        
        # 2. Augment
        context = "\n\n".join([
            f"[Source: {r['chunk']['source']}]\n{r['chunk']['text']}"
            for r in results
        ])
        
        prompt = f"""Answer the question using the following context.

Context:
{context}

Question: {question}

Answer based only on the provided context."""
        
        # 3. Generate
        response = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",
            messages=[{"role": "user", "content": prompt}],
            temperature=0.3,
            max_tokens=300
        )
        
        answer = response.choices[0].message.content
        
        return {
            "answer": answer,
            "sources": [r['chunk'] for r in results],
            "similarities": [r['similarity'] for r in results]
        }

# Usage
rag = RAGSystem()
rag.index_document("document.pdf")
result = rag.query("What is the main topic?")
print(result["answer"])
```

### Example 2: Enhanced RAG with Configuration

```python
class ConfigurableRAG(RAGSystem):
    """RAG system with configuration options"""
    
    def __init__(self, config: Dict):
        super().__init__()
        self.config = config
        self.chunker = TextChunker(
            chunk_size=config.get("chunk_size", 500),
            overlap=config.get("overlap", 50)
        )
        self.embedder = EmbeddingGenerator(
            model=config.get("embedding_model", "text-embedding-ada-002")
        )
    
    def query(self, question: str, k: Optional[int] = None) -> Dict:
        """Query with configurable parameters"""
        k = k or self.config.get("k", 3)
        threshold = self.config.get("similarity_threshold", 0.0)
        
        # Retrieve
        query_embedding = self.embedder.generate(question)
        results = self.vector_store.search(query_embedding, k * 2)  # Get more, filter
        
        # Filter by threshold
        filtered = [r for r in results if r['similarity'] >= threshold][:k]
        
        if not filtered:
            return {"answer": "No relevant documents found.", "sources": []}
        
        # Rest of the pipeline...
        # (similar to previous example)
```

---

## 4. Student Practice Tasks

### Task 1: Core Components
Implement each component separately:
- DocumentLoader
- TextChunker
- EmbeddingGenerator
- VectorStore

Test each independently.

### Task 2: Integration
Combine all components into a RAGSystem class. Test the complete pipeline.

### Task 3: Error Handling
Add comprehensive error handling:
- API failures
- File errors
- Empty results
- Invalid inputs

### Task 4: Configuration System
Create a configuration system that allows:
- Adjusting chunk size
- Changing K value
- Setting similarity threshold
- Configuring LLM parameters

### Task 5: Performance Optimization
Optimize your system:
- Batch embedding generation
- Cache embeddings
- Efficient similarity search
- Progress indicators

### Task 6: Testing
Create test cases:
- Unit tests for each component
- Integration tests
- End-to-end tests

---

## 5. Summary / Key Takeaways

- **Building from scratch** deepens understanding
- **Modular design** makes components reusable
- **Error handling** is crucial for production
- **Configuration** allows flexibility
- **Vector search** uses cosine similarity
- **Batching** improves efficiency
- **Metadata** enables filtering and citations
- **Testing** ensures reliability
- **Pure Python** = no framework dependencies
- **Foundation** for understanding frameworks

---

## 6. Further Reading (Optional)

- NumPy documentation (for vector operations)
- OpenAI API best practices
- Software design patterns
- Unit testing in Python

---

**Next up:** Day 8 will introduce you to LangChain and LlamaIndex frameworks!

