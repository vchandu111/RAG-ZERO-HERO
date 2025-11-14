# Day 9 — Assignment

## Instructions

Implement advanced RAG techniques to improve your system. These techniques make RAG production-ready. Install required libraries:

```bash
pip install sentence-transformers rank-bm25 openai numpy
```

**Important:**
- Implement each technique separately first
- Compare before/after results
- Measure improvements
- Document your findings

---

## Tasks

### Task 1: Query Rewriting System

Build a query rewriting system `query_rewriter.py`:

**Features:**
1. Generate query variations using LLM
2. Extract key terms
3. Expand with synonyms
4. Create multiple query formulations

**Requirements:**
- Generate 3-5 variations per query
- Test if variations improve retrieval
- Compare retrieval results
- Document improvements

**Test with:** Various question types

**Deliverable:** `task1_query_rewriter.py`

---

### Task 2: Reranking Implementation

Add reranking to your RAG system `reranking_rag.py`:

**Features:**
1. Use cross-encoder model for reranking
2. Rerank initial retrieval results
3. Compare before/after rankings
4. Measure improvement

**Requirements:**
- Install sentence-transformers
- Use a reranking model
- Rerank top 10, return top 5
- Show score improvements

**Test with:** Various queries and measure improvement

**Deliverable:** `task2_reranking_rag.py`

---

### Task 3: Fusion Techniques

Implement fusion `fusion_rag.py`:

**Features:**
1. Reciprocal Rank Fusion (RRF)
2. Weighted fusion
3. Combine multiple retrieval results
4. Deduplication

**Requirements:**
- Implement RRF algorithm
- Test with 2-3 different retrieval strategies
- Compare fused vs single retrieval
- Measure improvement

**Deliverable:** `task3_fusion_rag.py`

---

### Task 4: Hybrid Search

Build hybrid search `hybrid_search.py`:

**Features:**
1. Semantic search (embeddings)
2. Keyword search (BM25)
3. Combine both with weights
4. Tune alpha parameter

**Requirements:**
- Implement BM25 keyword search
- Combine with semantic search
- Test different alpha values (0.0 to 1.0)
- Find optimal balance

**Deliverable:** `task4_hybrid_search.py`

---

### Task 5: Complete Advanced RAG

Combine all techniques `advanced_rag.py`:

**Pipeline:**
1. Query rewriting
2. Multiple retrievals (with variations)
3. Fusion (combine results)
4. Reranking (improve order)
5. Generation

**Requirements:**
- Integrate all techniques
- Make it configurable
- Compare with basic RAG
- Measure overall improvement

**Deliverable:** `task5_advanced_rag.py`

---

## One Mini Project

### 🚀 Build a Production-Ready Advanced RAG System

Create a complete advanced RAG application `production_advanced_rag.py` with all optimization techniques.

**Features:**

1. **Complete Advanced Pipeline:**
   - Query rewriting
   - Multiple retrieval strategies
   - Fusion
   - Reranking
   - Generation

2. **Configuration System:**
   - Enable/disable each technique
   - Tune parameters
   - A/B testing mode
   - Performance vs quality trade-offs

3. **Multiple Retrieval Strategies:**
   - Semantic search
   - Keyword search
   - Hybrid search
   - Metadata filtering
   - Custom retrievers

4. **Advanced Features:**
   - Query expansion
   - Query decomposition
   - Multi-query fusion
   - Reranking with cross-encoders
   - Answer quality scoring

5. **Evaluation System:**
   - Compare techniques
   - Measure improvements
   - A/B testing
   - Performance metrics
   - Quality metrics

6. **Interactive Interface:**
   ```
   === Advanced RAG System ===
   1. Index documents
   2. Query (Basic RAG)
   3. Query (Advanced RAG)
   4. Compare techniques
   5. Configure settings
   6. Evaluation mode
   7. Statistics
   8. Exit
   ```

7. **Reporting:**
   - Technique comparison
   - Performance reports
   - Quality improvements
   - Recommendations

**Requirements:**
- All advanced techniques implemented
- Configurable and modular
- Comprehensive evaluation
- Production-ready quality
- Detailed documentation

**Example Usage:**
```python
rag = ProductionAdvancedRAG()

# Configure
rag.configure({
    "query_rewriting": True,
    "reranking": True,
    "fusion": True,
    "hybrid_search": True
})

# Query
result = rag.query("What is machine learning?")
print(result["answer"])
print(f"Improvement: {result['improvement_metrics']}")
```

**Deliverables:**
- `production_advanced_rag.py` - Main system
- `config_advanced.json` - Configuration
- `requirements.txt` - Dependencies
- `README_advanced.md` - Documentation
- Evaluation report template

---

## Expected Output Section

### Task 1 Expected Output:
```python
variations = rewrite_query("How does ML work?")
# Output:
[
    "How does machine learning work?",
    "What is the process of machine learning?",
    "How do ML algorithms learn?",
    "Explain machine learning mechanism",
    "How is machine learning implemented?"
]

# Test retrieval improvement
basic_results = retrieve("How does ML work?")
advanced_results = retrieve_multiple(variations)
# Advanced finds 40% more relevant documents
```

### Task 2 Expected Output:
```
Before Reranking:
1. Doc A (0.85)
2. Doc B (0.82)
3. Doc C (0.80)

After Reranking:
1. Doc C (0.92) ← Better match!
2. Doc A (0.88)
3. Doc B (0.85)

Improvement: Top result relevance increased by 8%
```

### Task 3 Expected Output:
```
Single Retrieval: Found 3 relevant docs
Fusion (3 strategies): Found 5 relevant docs
Improvement: 67% more relevant results
```

### Task 5 Expected Output:
```
=== Advanced RAG Query ===
Question: "What is Python?"

[Query Rewriting] Generated 4 variations
[Multiple Retrieval] Found 12 candidates
[Fusion] Combined to 8 unique results
[Reranking] Reordered top 5
[Generation] Generated answer

Answer: Python is a high-level programming language...

Improvement Metrics:
- Retrieval: +45% relevant docs
- Answer quality: +23% improvement
- Response time: +0.3s (acceptable)
```

### Mini Project Expected Output:

The advanced RAG system should demonstrate:
- Significant quality improvements
- Configurable techniques
- Comprehensive evaluation
- Production-ready features

**Example session:**
```
=== Advanced RAG System ===
Choose: 3

Question: "Explain neural networks"

[Advanced Pipeline Running...]
✓ Query rewritten: 4 variations
✓ Retrieved: 15 candidates
✓ Fused: 8 unique results
✓ Reranked: Top 5 selected
✓ Generated answer

Answer:
Neural networks are computing systems inspired by...

Sources (Top 5, reranked):
1. [0.94] neural_networks.pdf | Page 3
2. [0.91] deep_learning.pdf | Page 1
3. [0.89] ai_basics.pdf | Page 7
...

Comparison with Basic RAG:
- Answer quality: +28% improvement
- Source relevance: +35% improvement
- Response time: +0.4s
```

---

## Submission Checklist

- [ ] Task 1: Query rewriting working
- [ ] Task 2: Reranking implemented
- [ ] Task 3: Fusion functional
- [ ] Task 4: Hybrid search working
- [ ] Task 5: Complete advanced pipeline
- [ ] Mini project: Production system
- [ ] All techniques tested
- [ ] Improvements measured
- [ ] Code well-documented

**Remember:** Advanced techniques make the difference between a prototype and production system!

**Good luck!** 🚀

