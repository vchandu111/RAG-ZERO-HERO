# Day 6 — Assignment

## Instructions

Complete these tasks to build your first RAG systems. You'll implement the complete RAG pipeline: Retrieval → Augmentation → Generation. Make sure you have all dependencies:

```bash
pip install openai chromadb numpy
```

**Important:**
- Test with real documents
- Experiment with different K values
- Try various prompt templates
- Document what works best

---

## Tasks

### Task 1: Basic RAG System

Build a complete RAG system `basic_rag.py`:

**Components:**
1. Document storage (ChromaDB)
2. Retrieval function
3. Augmentation function
4. Generation function
5. Complete query pipeline

**Requirements:**
- Create a `BasicRAG` class
- Methods: `add_documents()`, `query()`
- Retrieve top 3 chunks
- Simple prompt template
- Return answer and sources

**Test with:** 10+ documents on a specific topic

**Deliverable:** `task1_basic_rag.py`

---

### Task 2: RAG with Source Citations

Enhance your RAG to include citations `rag_with_citations.py`:

**Features:**
1. Store source metadata with documents
2. Include source info in prompt
3. Generate answers with citations
4. Format: "According to [Source]..."

**Requirements:**
- Track document sources
- Include in augmented prompt
- LLM should cite sources in answer
- Return structured results with citations

**Test with:** Documents from different sources

**Deliverable:** `task2_rag_citations.py`

---

### Task 3: Similarity Threshold Filtering

Implement similarity-based filtering `filtered_rag.py`:

**Features:**
1. Set similarity threshold
2. Filter retrieved chunks
3. Only use chunks above threshold
4. Handle case when no chunks pass threshold

**Requirements:**
- Configurable threshold (0.0 to 1.0)
- Show similarity scores
- Test with different thresholds
- Compare results

**Test with:** Various queries and thresholds

**Deliverable:** `task3_filtered_rag.py`

---

### Task 4: Multi-Query RAG

Implement query expansion `multi_query_rag.py`:

**Features:**
1. Generate query variations
2. Search with each variation
3. Combine and deduplicate results
4. Use combined results for answer

**Query expansion ideas:**
- Paraphrase the question
- Extract key terms
- Generate related questions

**Requirements:**
- Create 2-3 query variations
- Search with each
- Merge results (remove duplicates)
- Use merged chunks for answer

**Deliverable:** `task4_multi_query_rag.py`

---

### Task 5: RAG Evaluation System

Build an evaluation framework `rag_evaluator.py`:

**Features:**
1. Test dataset (questions + expected answers)
2. Run RAG on test questions
3. Compare generated vs expected answers
4. Calculate metrics (accuracy, similarity)

**Metrics to implement:**
- Exact match
- Semantic similarity (embedding-based)
- Contains key terms
- Answer length comparison

**Requirements:**
- Create test dataset (5-10 Q&A pairs)
- Run evaluation
- Calculate and display metrics
- Identify failure cases

**Deliverable:** `task5_rag_evaluator.py`

---

## One Mini Project

### 🚀 Build a Full RAG System From Scratch

Create a complete RAG application `rag_system.py` that implements all the concepts learned.

**Features:**

1. **Document Management:**
   - Load documents from files (PDF, TXT)
   - Extract and chunk text
   - Generate embeddings
   - Store in vector database
   - Manage multiple document collections

2. **RAG Pipeline:**
   - Complete retrieval system
   - Configurable K value
   - Similarity threshold filtering
   - Query expansion (optional)
   - Augmentation with citations
   - Generation with LLM

3. **Interactive Interface:**
   ```
   === RAG System ===
   1. Add documents
   2. Ask a question
   3. View indexed documents
   4. Configure settings
   5. Evaluate system
   6. Export results
   7. Exit
   ```

4. **Settings Configuration:**
   - K value (number of chunks)
   - Similarity threshold
   - LLM model selection
   - Temperature
   - Max tokens
   - Enable/disable query expansion

5. **Advanced Features:**
   - Multiple collections
   - Metadata filtering
   - Search history
   - Answer quality scoring
   - Source highlighting
   - Export conversations

6. **Evaluation Tools:**
   - Test with sample questions
   - Compare different configurations
   - Performance metrics
   - Quality assessment

**Requirements:**
- Use classes for organization
- Support multiple file formats
- Implement all RAG components
- Add comprehensive error handling
- Create user-friendly CLI
- Store configurations
- Generate detailed reports

**Example Usage:**
```bash
python rag_system.py

=== RAG System ===
Choose: 1

Enter document path: ./documents
Processing...
✓ Indexed 5 documents
✓ Created 23 chunks

Choose: 2

Question: What is machine learning?
[Searching...]

Answer:
Machine learning is a subset of artificial intelligence that enables systems to learn from data...

Sources:
1. [0.89] ai_textbook.pdf, Page 5
2. [0.85] ml_guide.pdf, Page 2
3. [0.82] intro_ai.pdf, Page 10

[1] Ask another question
[2] View full sources
[3] Main menu
```

**Deliverables:**
- `rag_system.py` - Main application
- `config.json` - Configuration template
- `requirements.txt` - Dependencies
- `README_rag.md` - Usage guide
- Sample test dataset
- Example outputs

---

## Expected Output Section

### Task 1 Expected Output:
```python
rag = BasicRAG()
rag.add_documents(["Doc 1 text...", "Doc 2 text..."])

result = rag.query("What is Python?")
# Output:
{
    "answer": "Python is a high-level programming language...",
    "sources": [
        "Python is a programming language created in 1991...",
        "Python supports multiple programming paradigms..."
    ]
}
```

### Task 2 Expected Output:
```python
result = rag_with_citations.query("What is RAG?")
# Output:
{
    "answer": "According to document1.pdf, RAG stands for Retrieval-Augmented Generation...",
    "sources": [
        {"text": "...", "source": "document1.pdf", "page": 3},
        {"text": "...", "source": "document2.pdf", "page": 1}
    ]
}
```

### Task 3 Expected Output:
```
Query: "machine learning"
Threshold: 0.7

Retrieved 5 chunks, 3 above threshold (0.7)
Using top 3 chunks for answer...

Answer: [Generated answer using filtered chunks]
```

### Mini Project Expected Output:

The RAG system should provide:
- Fast document indexing
- Accurate retrieval
- Clear, cited answers
- Configurable settings
- Professional interface

**Example session:**
```
=== RAG System ===
Choose: 2

Question: How does neural network training work?

[Retrieving relevant chunks...]
[Generating answer...]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Answer:
Neural network training involves feeding data through the network, calculating errors, and adjusting weights through backpropagation...

Sources (Top 3):
1. [0.91] neural_networks.pdf | Page 12
2. [0.87] deep_learning.pdf | Page 5
3. [0.84] ai_basics.pdf | Page 8

Similarity scores shown in brackets
```

---

## Submission Checklist

- [ ] Task 1: Basic RAG working
- [ ] Task 2: Citations implemented
- [ ] Task 3: Filtering functional
- [ ] Task 4: Multi-query working
- [ ] Task 5: Evaluation system complete
- [ ] Mini project: Full RAG system
- [ ] All components tested
- [ ] Code is well-documented
- [ ] Error handling implemented

**Remember:** RAG combines retrieval and generation - both parts are important!

**Good luck!** 🚀

