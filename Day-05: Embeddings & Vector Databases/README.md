# Day 5 — Embeddings & Vector Databases

## 1. Beginner-Friendly Introduction

Today, you'll learn about **embeddings** and **vector databases**—the technology that makes RAG retrieval possible. This is where the magic happens: converting text into numbers that capture meaning, and storing them so we can find similar content quickly.

**What are Embeddings?**
Embeddings are numerical representations of text that capture semantic meaning. Similar texts have similar embeddings (close numbers), allowing computers to understand meaning mathematically.

**Why this matters for RAG:**
- Embeddings convert text chunks into searchable vectors
- Vector databases store and retrieve similar content efficiently
- When you ask a question, we find the most relevant chunks using similarity search
- This is the "Retrieval" part of RAG!

**Real-world context:**
Imagine a library where books are organized by meaning, not alphabetically. When you ask "Tell me about dogs," the system finds all books about dogs, even if they don't contain the exact word "dogs" (maybe they say "canines" or "pets"). Embeddings make this possible!

---

## 2. Deep-Dive Explanation

### 2.1 What are Embeddings?

**Text Embeddings** are dense vectors (arrays of numbers) that represent text in a high-dimensional space.

**Key Properties:**
- **Semantic similarity**: Similar meanings → similar vectors
- **Fixed dimensions**: Each embedding has the same length (e.g., 1536 for OpenAI)
- **Dense**: Most values are non-zero (unlike sparse representations)

**Example:**
```
"dog" → [0.2, -0.5, 0.8, ..., 0.1]  (1536 numbers)
"puppy" → [0.19, -0.48, 0.79, ..., 0.12]  (very similar!)
"car" → [-0.3, 0.6, -0.2, ..., -0.5]  (very different!)
```

### 2.2 How Embeddings Work

**The Process:**
```
Text → Embedding Model → Vector (Array of Numbers)
```

**Embedding Models:**
- **OpenAI**: `text-embedding-ada-002` or `text-embedding-3-small`
- **Sentence Transformers**: Open-source alternatives
- **Custom models**: Trained on specific domains

**Dimensions:**
- OpenAI ada-002: 1536 dimensions
- OpenAI 3-small: 1536 dimensions
- Sentence-BERT: 384 or 768 dimensions
- More dimensions = more detail (but slower, more storage)

### 2.3 Similarity Search

**Cosine Similarity:**
Measures the angle between two vectors (0 to 1):
- 1.0 = Identical meaning
- 0.9 = Very similar
- 0.5 = Somewhat related
- 0.0 = Unrelated

**Formula (simplified):**
```
similarity = dot_product(vec1, vec2) / (magnitude(vec1) * magnitude(vec2))
```

**Why Cosine Similarity?**
- Focuses on direction, not magnitude
- Works well for text embeddings
- Range: -1 to 1 (usually 0 to 1 for normalized embeddings)

### 2.4 Vector Databases

**What is a Vector Database?**
A specialized database optimized for storing and searching high-dimensional vectors.

**Key Features:**
- Fast similarity search
- Handles millions of vectors
- Supports metadata filtering
- Efficient indexing (ANN - Approximate Nearest Neighbor)

**Popular Vector Databases:**
- **ChromaDB**: Simple, Python-native
- **Pinecone**: Cloud-based, scalable
- **Weaviate**: Open-source, feature-rich
- **Qdrant**: Fast, Rust-based
- **FAISS**: Facebook's library (not a full DB)

### 2.5 ChromaDB Basics

**Why ChromaDB for Learning?**
- Easy to use
- No external services needed
- Perfect for prototyping
- Python-native

**Core Concepts:**
- **Collection**: Container for vectors and metadata
- **Documents**: Your text chunks
- **Embeddings**: Vector representations
- **Metadata**: Additional info (source, page, etc.)

**Basic Operations:**
1. Create collection
2. Add documents (auto-generates embeddings)
3. Query for similar documents
4. Retrieve with metadata

### 2.6 The Embedding Pipeline

**Complete Flow:**
```
Text Chunks → Embedding Model → Vectors → Vector DB → Index
                                                      ↓
Query → Embedding Model → Query Vector → Similarity Search → Top K Results
```

**Steps:**
1. **Chunk documents** (from Day 4)
2. **Generate embeddings** for each chunk
3. **Store in vector DB** with metadata
4. **Query**: Convert question to embedding
5. **Search**: Find most similar chunks
6. **Retrieve**: Get top K results

---

## 3. Instructor Examples

### Example 1: Generating Embeddings with OpenAI

```python
import openai
import os

openai.api_key = os.getenv("OPENAI_API_KEY")

def get_embedding(text, model="text-embedding-ada-002"):
    """Generate embedding for text"""
    text = text.replace("\n", " ")  # Replace newlines
    
    response = openai.Embedding.create(
        model=model,
        input=text
    )
    
    return response['data'][0]['embedding']

# Usage
text = "Python is a programming language"
embedding = get_embedding(text)
print(f"Embedding dimension: {len(embedding)}")  # 1536
print(f"First 5 values: {embedding[:5]}")
```

### Example 2: Batch Embedding Generation

```python
def get_embeddings_batch(texts, model="text-embedding-ada-002"):
    """Generate embeddings for multiple texts"""
    # Clean texts
    texts = [text.replace("\n", " ") for text in texts]
    
    response = openai.Embedding.create(
        model=model,
        input=texts
    )
    
    # Extract embeddings
    embeddings = [item['embedding'] for item in response['data']]
    return embeddings

# Usage
texts = [
    "Python is a programming language",
    "Dogs are loyal pets",
    "Machine learning uses algorithms"
]
embeddings = get_embeddings_batch(texts)
print(f"Generated {len(embeddings)} embeddings")
```

### Example 3: Simple Similarity Calculation

```python
import numpy as np

def cosine_similarity(vec1, vec2):
    """Calculate cosine similarity between two vectors"""
    vec1 = np.array(vec1)
    vec2 = np.array(vec2)
    
    dot_product = np.dot(vec1, vec2)
    norm1 = np.linalg.norm(vec1)
    norm2 = np.linalg.norm(vec2)
    
    if norm1 == 0 or norm2 == 0:
        return 0.0
    
    return dot_product / (norm1 * norm2)

# Usage
embedding1 = get_embedding("dog")
embedding2 = get_embedding("puppy")
embedding3 = get_embedding("car")

similarity_dog_puppy = cosine_similarity(embedding1, embedding2)
similarity_dog_car = cosine_similarity(embedding1, embedding3)

print(f"Dog-Puppy similarity: {similarity_dog_puppy:.3f}")  # ~0.85
print(f"Dog-Car similarity: {similarity_dog_car:.3f}")  # ~0.30
```

### Example 4: ChromaDB Basics

```python
import chromadb
from chromadb.config import Settings

# Initialize ChromaDB (in-memory for simplicity)
client = chromadb.Client(Settings(anonymized_telemetry=False))

# Create or get a collection
collection = client.create_collection(name="documents")

# Add documents
documents = [
    "Python is a high-level programming language",
    "Dogs are loyal and friendly animals",
    "Machine learning is a subset of AI"
]

ids = ["doc1", "doc2", "doc3"]
metadatas = [
    {"source": "python_book", "page": 1},
    {"source": "animal_guide", "page": 5},
    {"source": "ai_textbook", "page": 10}
]

collection.add(
    documents=documents,
    ids=ids,
    metadatas=metadatas
)

# Query for similar documents
results = collection.query(
    query_texts=["programming languages"],
    n_results=2
)

print("Similar documents:")
for i, doc in enumerate(results['documents'][0]):
    print(f"{i+1}. {doc}")
    print(f"   Metadata: {results['metadatas'][0][i]}")
```

### Example 5: Complete Embedding Pipeline

```python
class EmbeddingPipeline:
    def __init__(self, embedding_model="text-embedding-ada-002"):
        self.embedding_model = embedding_model
        self.client = chromadb.Client()
        self.collection = None
    
    def create_collection(self, name):
        """Create a new collection"""
        self.collection = self.client.create_collection(name=name)
        return self.collection
    
    def add_documents(self, texts, ids=None, metadatas=None):
        """Add documents to collection (ChromaDB auto-generates embeddings)"""
        if ids is None:
            ids = [f"doc_{i}" for i in range(len(texts))]
        
        self.collection.add(
            documents=texts,
            ids=ids,
            metadatas=metadatas
        )
    
    def search(self, query_text, n_results=5, filter_metadata=None):
        """Search for similar documents"""
        query_params = {
            "query_texts": [query_text],
            "n_results": n_results
        }
        
        if filter_metadata:
            query_params["where"] = filter_metadata
        
        results = self.collection.query(**query_params)
        
        return {
            "documents": results['documents'][0],
            "metadatas": results['metadatas'][0],
            "distances": results['distances'][0]
        }
    
    def get_stats(self):
        """Get collection statistics"""
        count = self.collection.count()
        return {"total_documents": count}

# Usage
pipeline = EmbeddingPipeline()
pipeline.create_collection("my_docs")

# Add documents
texts = ["Document 1 text...", "Document 2 text..."]
metadatas = [{"source": "book1"}, {"source": "book2"}]
pipeline.add_documents(texts, metadatas=metadatas)

# Search
results = pipeline.search("What is Python?", n_results=3)
for doc, metadata in zip(results["documents"], results["metadatas"]):
    print(f"Found: {doc[:50]}... (Source: {metadata['source']})")
```

---

## 4. Student Practice Tasks

### Task 1: Embedding Generator
Create a function that:
- Takes a list of texts
- Generates embeddings for each
- Returns embeddings with metadata
- Handles API errors

### Task 2: Similarity Calculator
Build a tool that:
- Takes two texts
- Generates embeddings
- Calculates cosine similarity
- Explains the similarity score

### Task 3: ChromaDB Setup
Set up ChromaDB and:
- Create a collection
- Add 10 sample documents
- Query for similar documents
- Display results with metadata

### Task 4: Batch Processing
Create a system that:
- Processes multiple documents
- Generates embeddings in batches
- Stores in ChromaDB
- Shows progress

### Task 5: Similarity Search
Implement a search function that:
- Takes a query
- Finds top 5 most similar documents
- Returns results with similarity scores
- Filters by metadata if needed

### Task 6: Embedding Visualization
(Advanced) Use dimensionality reduction (PCA/t-SNE) to visualize embeddings in 2D and see how similar texts cluster together.

---

## 5. Summary / Key Takeaways

- **Embeddings** convert text to numerical vectors that capture meaning
- **Similar texts** have similar embeddings (high cosine similarity)
- **Embedding models** like OpenAI's ada-002 generate 1536-dimensional vectors
- **Vector databases** (like ChromaDB) store and search embeddings efficiently
- **Cosine similarity** measures how similar two embeddings are (0-1 scale)
- **ChromaDB** is easy to use for learning and prototyping
- **The pipeline**: Text → Embedding → Store → Query → Retrieve
- **Metadata** helps filter and organize stored documents
- **Batch processing** is more efficient than one-by-one
- **Similarity search** finds relevant chunks for RAG queries

---

## 6. Further Reading (Optional)

- OpenAI Embeddings Guide
- ChromaDB Documentation
- Sentence Transformers library
- Vector Database Comparison articles
- "The Illustrated Word2vec" (understanding embeddings conceptually)

---

**Next up:** Day 6 will combine everything into a complete RAG system!

