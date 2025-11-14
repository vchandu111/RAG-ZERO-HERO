# Day 7 — Assignment

## Instructions

Build a complete RAG system from scratch using pure Python. No frameworks allowed! This will help you understand every component deeply. Install only basic dependencies:

```bash
pip install openai numpy pypdf
```

**Important:**
- Build each component separately first
- Test thoroughly
- Add error handling
- Document your code
- Make it production-ready

---

## Tasks

### Task 1: Core Component Implementation

Implement each core component as a separate class:

1. **DocumentLoader** (`document_loader.py`)
   - Load .txt files
   - Load .pdf files
   - Handle errors
   - Return clean text

2. **TextChunker** (`text_chunker.py`)
   - Fixed-size chunking
   - Configurable overlap
   - Add metadata
   - Return structured chunks

3. **EmbeddingGenerator** (`embedding_generator.py`)
   - Single text embedding
   - Batch embedding
   - Error handling
   - API key management

4. **VectorStore** (`vector_store.py`)
   - Store embeddings
   - Store chunks
   - Similarity search
   - Top K retrieval

**Test each component independently before integrating.**

**Deliverables:** 
- `document_loader.py`
- `text_chunker.py`
- `embedding_generator.py`
- `vector_store.py`

---

### Task 2: RAG System Integration

Create `rag_system.py` that integrates all components:

**Requirements:**
- `RAGSystem` class
- `index_document(filepath)` method
- `query(question, k=3)` method
- Complete pipeline: Load → Chunk → Embed → Store → Retrieve → Augment → Generate
- Return structured results

**Test with:** Multiple documents and various questions

**Deliverable:** `task2_rag_system.py`

---

### Task 3: Error Handling & Robustness

Enhance your RAG system with comprehensive error handling:

**Handle:**
- Missing API keys
- API rate limits
- Network errors
- File not found
- Empty documents
- No search results
- Invalid inputs

**Requirements:**
- Graceful error messages
- Fallback behaviors
- Logging (optional)
- User-friendly error reporting

**Deliverable:** `task3_robust_rag.py`

---

### Task 4: Configuration System

Create a configuration system `config_rag.py`:

**Configurable options:**
- Chunk size
- Overlap size
- K value (retrieval)
- Similarity threshold
- Embedding model
- LLM model
- Temperature
- Max tokens

**Requirements:**
- Load from JSON file
- Default values
- Validation
- Easy to modify

**Deliverable:** `task4_configurable_rag.py` + `config.json`

---

### Task 5: Performance Optimization

Optimize your RAG system:

**Optimizations:**
1. Batch embedding generation
2. Cache embeddings (save to file)
3. Efficient similarity search (use NumPy)
4. Progress indicators
5. Memory management

**Requirements:**
- Measure performance (time)
- Compare before/after
- Handle large documents
- Show progress for long operations

**Deliverable:** `task5_optimized_rag.py`

---

## One Mini Project

### 🏗️ Build a Production-Ready RAG System

Create a complete, production-ready RAG system `production_rag.py` with all features.

**Features:**

1. **Complete Component System:**
   - DocumentLoader (multiple formats)
   - TextChunker (multiple strategies)
   - EmbeddingGenerator (with caching)
   - VectorStore (persistent storage)
   - RAGPipeline (orchestration)

2. **Document Management:**
   - Index single documents
   - Index directories
   - Remove documents
   - List indexed documents
   - Document statistics

3. **Query System:**
   - Single queries
   - Batch queries
   - Query history
   - Result caching

4. **Configuration:**
   - JSON config file
   - Runtime configuration
   - Environment variables
   - Default values

5. **Error Handling:**
   - Comprehensive try-catch
   - User-friendly messages
   - Logging system
   - Recovery mechanisms

6. **Performance Features:**
   - Embedding caching
   - Batch processing
   - Progress tracking
   - Performance metrics

7. **CLI Interface:**
   ```
   === Production RAG System ===
   1. Index document
   2. Index directory
   3. Query
   4. View indexed documents
   5. Remove document
   6. Configuration
   7. Statistics
   8. Exit
   ```

8. **Advanced Features:**
   - Multiple collections
   - Export/import data
   - Search with filters
   - Answer quality metrics
   - System health check

**Requirements:**
- Clean, modular code
- Comprehensive documentation
- Error handling throughout
- Configuration system
- Performance optimizations
- User-friendly interface
- Production-ready quality

**Example Usage:**
```python
from production_rag import ProductionRAG

# Initialize
rag = ProductionRAG(config_file="config.json")

# Index documents
rag.index_document("doc1.pdf")
rag.index_directory("./documents/")

# Query
result = rag.query("What is machine learning?")
print(result["answer"])
print(f"Sources: {len(result['sources'])}")

# Statistics
stats = rag.get_statistics()
print(f"Total chunks: {stats['total_chunks']}")
```

**Deliverables:**
- `production_rag.py` - Main system
- `config.json` - Configuration template
- `requirements.txt` - Dependencies
- `README_production.md` - Documentation
- Unit tests (optional but recommended)
- Example usage script

---

## Expected Output Section

### Task 2 Expected Output:
```python
rag = RAGSystem()
rag.index_document("document.pdf")
# Output: "Indexed document.pdf: 15 chunks"

result = rag.query("What is the main topic?")
# Output:
{
    "answer": "The main topic is...",
    "sources": [
        {"text": "...", "source": "document.pdf", "chunk_id": 1},
        ...
    ],
    "similarities": [0.89, 0.85, 0.82]
}
```

### Task 4 Expected Output:
```json
// config.json
{
    "chunk_size": 500,
    "overlap": 50,
    "k": 3,
    "similarity_threshold": 0.7,
    "embedding_model": "text-embedding-ada-002",
    "llm_model": "gpt-3.5-turbo",
    "temperature": 0.3,
    "max_tokens": 300
}
```

### Mini Project Expected Output:

The production system should be:
- Robust and error-resistant
- Well-documented
- Configurable
- Performant
- User-friendly

**Example session:**
```
=== Production RAG System ===
Choose: 1

Enter document path: document.pdf
[Indexing...]
✓ Loaded document
✓ Created 15 chunks
✓ Generated embeddings
✓ Stored in vector database
Indexed successfully!

Choose: 3

Question: What is RAG?
[Processing...]

Answer:
RAG stands for Retrieval-Augmented Generation...

Sources (3):
1. [0.91] document.pdf, chunk 5
2. [0.87] document.pdf, chunk 8
3. [0.84] document.pdf, chunk 12
```

---

## Submission Checklist

- [ ] Task 1: All core components implemented
- [ ] Task 2: RAG system integrated
- [ ] Task 3: Error handling added
- [ ] Task 4: Configuration system working
- [ ] Task 5: Optimizations implemented
- [ ] Mini project: Production-ready system
- [ ] All code is well-documented
- [ ] Error handling comprehensive
- [ ] Tested with real documents
- [ ] Code follows best practices

**Remember:** Building from scratch teaches you everything!

**Good luck!** 🚀

