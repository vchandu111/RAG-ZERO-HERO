# Day 5 — Assignment

## Instructions

Complete these tasks to master embeddings and vector databases. You'll work with OpenAI embeddings and ChromaDB. Install required libraries:

```bash
pip install openai chromadb numpy
```

**Important:**
- Store your OpenAI API key securely
- Test with various texts to understand embeddings
- Experiment with different similarity thresholds
- Document your findings

---

## Tasks

### Task 1: Embedding Generator Tool

Create a tool `embedding_generator.py` that:

1. Takes text input (single or batch)
2. Generates embeddings using OpenAI API
3. Returns embeddings with metadata
4. Handles errors and rate limits
5. Saves embeddings to file (optional)

**Features:**
- Support single text or list of texts
- Show embedding dimensions
- Display first few values
- Calculate and display statistics (min, max, mean)

**Test with:** Various texts (short, long, different topics)

**Deliverable:** `task1_embedding_generator.py`

---

### Task 2: Similarity Calculator

Build a similarity calculator `similarity_calculator.py`:

1. Takes two texts as input
2. Generates embeddings for both
3. Calculates cosine similarity
4. Provides interpretation of the score
5. Visualizes similarity (text-based or simple plot)

**Features:**
- Calculate cosine similarity
- Provide similarity interpretation (very similar, somewhat similar, different)
- Compare multiple text pairs
- Show embedding values (first few dimensions)

**Test with:**
- Very similar texts ("dog" vs "puppy")
- Somewhat similar ("dog" vs "animal")
- Different texts ("dog" vs "computer")

**Deliverable:** `task2_similarity_calculator.py`

---

### Task 3: ChromaDB Document Store

Create a document storage system `chromadb_store.py`:

1. Initialize ChromaDB collection
2. Add documents with metadata
3. Query for similar documents
4. Retrieve documents by ID
5. Get collection statistics

**Requirements:**
- Create a class `DocumentStore`
- Methods: `add_documents()`, `search()`, `get_by_id()`, `get_stats()`
- Support metadata filtering
- Handle collection creation/loading

**Test with:** 20+ sample documents on various topics

**Deliverable:** `task3_chromadb_store.py`

---

### Task 4: Batch Embedding Processor

Build a batch processor `batch_processor.py`:

1. Process multiple documents in batches
2. Generate embeddings efficiently
3. Store in ChromaDB
4. Show progress
5. Handle errors gracefully

**Features:**
- Batch size configuration
- Progress tracking
- Error recovery (skip failed items, continue)
- Summary report

**Test with:** 50+ documents

**Deliverable:** `task4_batch_processor.py`

---

### Task 5: Semantic Search Engine

Create a semantic search engine `semantic_search.py`:

1. Index a collection of documents
2. Accept search queries
3. Return top K most similar documents
4. Display results with similarity scores
5. Support metadata filtering

**Features:**
- Search interface (CLI)
- Display top results with scores
- Show metadata for each result
- Highlight matching content (optional)
- Export search results

**Test with:** A collection of 30+ documents

**Deliverable:** `task5_semantic_search.py`

---

## One Mini Project

### 🔍 Build a Semantic Search Tool

Create a complete application `semantic_search_tool.py` that implements a semantic search system using embeddings and vector databases.

**Features:**

1. **Document Indexing:**
   - Load documents from files (PDF, TXT, etc.)
   - Extract and chunk text
   - Generate embeddings
   - Store in ChromaDB with metadata

2. **Search Interface:**
   ```
   === Semantic Search Tool ===
   1. Index documents
   2. Search
   3. View indexed documents
   4. Delete documents
   5. Collection statistics
   6. Export results
   7. Exit
   ```

3. **Search Capabilities:**
   - Natural language queries
   - Top K results (configurable)
   - Similarity score display
   - Metadata filtering
   - Search history

4. **Advanced Features:**
   - Multiple collections support
   - Hybrid search (keyword + semantic)
   - Result ranking and re-ranking
   - Search analytics
   - Export search results

5. **Statistics and Analytics:**
   - Total documents indexed
   - Average document length
   - Search performance metrics
   - Most common queries
   - Collection health

**Requirements:**
- Use classes for organization
- Support multiple file formats
- Implement progress tracking
- Add error handling
- Create a user-friendly CLI
- Store collections persistently
- Generate detailed reports

**Example Usage:**
```bash
python semantic_search_tool.py

=== Semantic Search Tool ===
Choose option: 1

Enter directory path: ./documents
Chunk size [500]: 400
Processing documents...
✓ Indexed 15 documents
✓ Created 42 chunks
✓ Generated embeddings

Choose option: 2

Enter search query: What is machine learning?
Found 5 results:

1. [Score: 0.89] Machine learning is a subset of AI...
   Source: ai_textbook.pdf, Page: 3

2. [Score: 0.85] ML algorithms learn from data...
   Source: ml_guide.pdf, Page: 1
...
```

**Deliverables:**
- `semantic_search_tool.py` - Main application
- `requirements.txt` - Dependencies
- `README_search.md` - Usage guide
- Sample indexed collection
- Example search results

---

## Expected Output Section

### Task 1 Expected Output:
```python
embedding = generate_embedding("Python is a programming language")
# Output:
{
    "dimension": 1536,
    "first_5_values": [0.012, -0.034, 0.089, ...],
    "statistics": {
        "min": -0.523,
        "max": 0.891,
        "mean": 0.001
    }
}
```

### Task 2 Expected Output:
```
=== Similarity Calculator ===
Text 1: "dog"
Text 2: "puppy"

Similarity: 0.847
Interpretation: Very similar (same concept, different word)

Text 1: "dog"
Text 2: "car"

Similarity: 0.312
Interpretation: Different (unrelated concepts)
```

### Task 3 Expected Output:
```python
store = DocumentStore("my_collection")
store.add_documents(
    texts=["Doc 1", "Doc 2"],
    metadatas=[{"source": "book1"}, {"source": "book2"}]
)

results = store.search("programming", n_results=2)
# Returns top 2 similar documents with metadata
```

### Mini Project Expected Output:

The semantic search tool should provide:
- Fast indexing of documents
- Accurate search results
- Clear similarity scores
- Rich metadata display
- Professional interface

**Example session:**
```
=== Semantic Search Tool ===
Choose: 2

Query: How does neural network work?
Searching...

Results (Top 5):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. [0.92] Neural networks are computing systems...
   📄 Source: ai_book.pdf | 📄 Page: 45

2. [0.88] A neural network consists of layers...
   📄 Source: ml_guide.pdf | 📄 Page: 12
...
```

---

## Submission Checklist

- [ ] Task 1: Embedding generator working
- [ ] Task 2: Similarity calculator functional
- [ ] Task 3: ChromaDB store implemented
- [ ] Task 4: Batch processor complete
- [ ] Task 5: Semantic search engine working
- [ ] Mini project: Complete search tool
- [ ] All code handles errors
- [ ] Code is well-documented
- [ ] Tested with real documents

**Remember:** Embeddings and vector databases are the foundation of RAG retrieval!

**Good luck!** 🚀

