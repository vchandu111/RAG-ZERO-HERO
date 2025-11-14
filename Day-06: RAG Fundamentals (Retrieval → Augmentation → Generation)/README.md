# Day 6 — RAG Fundamentals (Retrieval → Augmentation → Generation)

## 1. Beginner-Friendly Introduction

Today, you'll learn the complete RAG pipeline! **Retrieval-Augmented Generation** combines the best of both worlds: the knowledge retrieval of search engines and the language understanding of LLMs.

**What is RAG?**
RAG is a technique that:
1. **Retrieves** relevant information from your documents
2. **Augments** the LLM's prompt with this context
3. **Generates** accurate, sourced answers

**Why RAG matters:**
- Solves LLM limitations (hallucination, outdated info)
- Provides accurate, verifiable answers
- Uses your own documents as knowledge base
- Enables domain-specific AI applications

**Real-world context:**
Instead of asking an LLM "What's in my company handbook?" (which it doesn't know), RAG:
1. Searches your handbook documents
2. Finds relevant sections
3. Gives those sections to the LLM
4. LLM answers based on YOUR documents

---

## 2. Deep-Dive Explanation

### 2.1 The RAG Pipeline

**Complete Flow:**
```
User Question
    ↓
[1. RETRIEVAL]
    ↓
Query Embedding → Vector Search → Top K Chunks
    ↓
[2. AUGMENTATION]
    ↓
Context + Question → Formatted Prompt
    ↓
[3. GENERATION]
    ↓
LLM → Answer with Sources
```

### 2.2 Step 1: Retrieval

**What happens:**
1. Convert user question to embedding
2. Search vector database for similar chunks
3. Retrieve top K most relevant chunks
4. Return chunks with metadata

**Key decisions:**
- **K value**: How many chunks? (typically 3-5)
- **Similarity threshold**: Minimum similarity score?
- **Metadata filtering**: Filter by source, date, etc.?

**Example:**
```
Question: "What is Python?"
→ Embedding: [0.1, -0.3, 0.8, ...]
→ Search vector DB
→ Retrieve: [Chunk about Python, Chunk about programming, ...]
```

### 2.3 Step 2: Augmentation

**What happens:**
1. Combine retrieved chunks into context
2. Format context with clear structure
3. Add question to prompt
4. Include instructions for the LLM

**Prompt Structure:**
```
System: You are a helpful assistant...
Context:
[Chunk 1]
[Chunk 2]
[Chunk 3]

Question: {user_question}

Answer based on the context above.
```

**Best practices:**
- Clearly separate chunks
- Include source information
- Limit context size (token budget)
- Add instructions for citation

### 2.4 Step 3: Generation

**What happens:**
1. Send augmented prompt to LLM
2. LLM generates answer using context
3. Extract answer from response
4. Optionally extract citations

**LLM Configuration:**
- **Temperature**: Lower (0.3-0.5) for factual answers
- **Max tokens**: Based on expected answer length
- **Model**: GPT-3.5-turbo or GPT-4

### 2.5 Complete RAG System Components

**Required Components:**
1. **Document Store**: Where chunks are stored
2. **Embedding Model**: Converts text to vectors
3. **Vector Database**: Stores and searches embeddings
4. **LLM**: Generates final answers
5. **Prompt Template**: Formats context + question

**Data Flow:**
```
Documents → Chunks → Embeddings → Vector DB
                                    ↓
User Question → Embedding → Search → Retrieved Chunks
                                    ↓
Retrieved Chunks + Question → Prompt → LLM → Answer
```

### 2.6 RAG vs. Traditional Search

**Traditional Search:**
- Keyword matching
- Exact text search
- May miss semantic matches

**RAG:**
- Semantic understanding
- Finds conceptually similar content
- Understands context and meaning

**Example:**
- Question: "How do I train a model?"
- Keyword search: Finds "train" and "model" separately
- RAG: Finds content about "machine learning training", "model training", etc.

---

## 3. Instructor Examples

### Example 1: Simple RAG Pipeline

```python
import openai
import chromadb
from chromadb.config import Settings

class SimpleRAG:
    def __init__(self):
        self.client = chromadb.Client(Settings())
        self.collection = None
        openai.api_key = os.getenv("OPENAI_API_KEY")
    
    def setup(self, collection_name="documents"):
        """Initialize collection"""
        self.collection = self.client.create_collection(name=collection_name)
    
    def add_documents(self, texts, ids=None, metadatas=None):
        """Add documents to the collection"""
        if ids is None:
            ids = [f"doc_{i}" for i in range(len(texts))]
        
        self.collection.add(
            documents=texts,
            ids=ids,
            metadatas=metadatas
        )
    
    def retrieve(self, query, k=3):
        """Retrieve top K relevant chunks"""
        results = self.collection.query(
            query_texts=[query],
            n_results=k
        )
        return results['documents'][0]
    
    def augment(self, context_chunks, question):
        """Create augmented prompt"""
        context = "\n\n".join([
            f"[Document {i+1}]\n{chunk}"
            for i, chunk in enumerate(context_chunks)
        ])
        
        prompt = f"""Use the following documents to answer the question.

Documents:
{context}

Question: {question}

Answer based only on the provided documents. If the documents don't contain enough information, say so."""
        
        return prompt
    
    def generate(self, prompt):
        """Generate answer using LLM"""
        response = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",
            messages=[{"role": "user", "content": prompt}],
            temperature=0.3,
            max_tokens=300
        )
        return response.choices[0].message.content
    
    def query(self, question, k=3):
        """Complete RAG pipeline"""
        # 1. Retrieve
        chunks = self.retrieve(question, k)
        
        # 2. Augment
        prompt = self.augment(chunks, question)
        
        # 3. Generate
        answer = self.generate(prompt)
        
        return {
            "answer": answer,
            "sources": chunks
        }

# Usage
rag = SimpleRAG()
rag.setup()

# Add documents
rag.add_documents([
    "Python is a programming language created in 1991.",
    "RAG combines retrieval and generation.",
    "Machine learning uses algorithms to learn from data."
])

# Query
result = rag.query("What is Python?")
print(result["answer"])
print("\nSources:", result["sources"])
```

### Example 2: RAG with Metadata

```python
class RAGWithMetadata(SimpleRAG):
    def retrieve_with_metadata(self, query, k=3):
        """Retrieve chunks with metadata"""
        results = self.collection.query(
            query_texts=[query],
            n_results=k
        )
        return {
            "documents": results['documents'][0],
            "metadatas": results['metadatas'][0],
            "distances": results['distances'][0]
        }
    
    def augment_with_sources(self, retrieved_data, question):
        """Augment with source citations"""
        context_parts = []
        for i, (doc, metadata) in enumerate(zip(
            retrieved_data['documents'],
            retrieved_data['metadatas']
        )):
            source = metadata.get('source', f'Document {i+1}')
            context_parts.append(f"[Source: {source}]\n{doc}")
        
        context = "\n\n".join(context_parts)
        
        prompt = f"""Answer the question using the following sources.

Sources:
{context}

Question: {question}

Provide an answer and cite which source(s) you used."""
        
        return prompt
    
    def query(self, question, k=3):
        """RAG with source citations"""
        # Retrieve with metadata
        retrieved = self.retrieve_with_metadata(question, k)
        
        # Augment
        prompt = self.augment_with_sources(retrieved, question)
        
        # Generate
        answer = self.generate(prompt)
        
        return {
            "answer": answer,
            "sources": [
                {"text": doc, "metadata": meta}
                for doc, meta in zip(
                    retrieved['documents'],
                    retrieved['metadatas']
                )
            ]
        }
```

### Example 3: RAG with Similarity Filtering

```python
class FilteredRAG(SimpleRAG):
    def retrieve_with_threshold(self, query, k=5, threshold=0.7):
        """Retrieve only chunks above similarity threshold"""
        results = self.collection.query(
            query_texts=[query],
            n_results=k
        )
        
        # Filter by similarity (distance is inverse of similarity)
        # Lower distance = higher similarity
        filtered_docs = []
        filtered_metas = []
        
        for doc, meta, distance in zip(
            results['documents'][0],
            results['metadatas'][0],
            results['distances'][0]
        ):
            similarity = 1 - distance  # Convert distance to similarity
            if similarity >= threshold:
                filtered_docs.append(doc)
                filtered_metas.append(meta)
        
        return filtered_docs, filtered_metas
    
    def query(self, question, k=5, threshold=0.7):
        """RAG with similarity filtering"""
        chunks, metadata = self.retrieve_with_threshold(question, k, threshold)
        
        if not chunks:
            return {
                "answer": "I couldn't find relevant information in the documents.",
                "sources": []
            }
        
        prompt = self.augment(chunks, question)
        answer = self.generate(prompt)
        
        return {
            "answer": answer,
            "sources": chunks,
            "num_sources": len(chunks)
        }
```

---

## 4. Student Practice Tasks

### Task 1: Basic RAG Implementation
Build a simple RAG system that:
- Stores documents in ChromaDB
- Retrieves top 3 chunks for a query
- Augments prompt with context
- Generates answer using GPT-3.5

### Task 2: RAG with Citations
Enhance your RAG to:
- Include source information in answers
- Format citations properly
- Show which document each part came from

### Task 3: Similarity Threshold
Add filtering to only use chunks above a similarity threshold. Test with different thresholds and observe how it affects answers.

### Task 4: Multi-Query RAG
Implement query expansion:
- Generate multiple query variations
- Search with each variation
- Combine results
- Remove duplicates

### Task 5: RAG Evaluation
Create a simple evaluation:
- Test with known questions
- Compare answers to expected answers
- Calculate accuracy metrics

### Task 6: RAG with Metadata Filtering
Add ability to filter by metadata (e.g., only search in specific documents, date ranges, etc.)

---

## 5. Summary / Key Takeaways

- **RAG = Retrieval + Augmentation + Generation**
- **Retrieval**: Find relevant chunks using semantic search
- **Augmentation**: Combine chunks with question in a prompt
- **Generation**: LLM creates answer from augmented context
- **K value**: Number of chunks to retrieve (typically 3-5)
- **Similarity threshold**: Filter low-quality matches
- **Metadata**: Track sources for citations
- **Prompt engineering**: Critical for good RAG results
- **RAG solves**: Hallucination, outdated info, domain knowledge
- **Complete pipeline**: Documents → Embeddings → Vector DB → Query → Retrieve → Augment → Generate

---

## 6. Further Reading (Optional)

- "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks" (original RAG paper)
- LangChain RAG documentation
- LlamaIndex RAG guides
- RAG evaluation metrics

---

**Next up:** Day 7 will have you build a complete RAG system from scratch!

