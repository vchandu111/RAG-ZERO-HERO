# Day 8 — Assignment

## Instructions

Build RAG systems using LangChain and LlamaIndex frameworks. Compare both approaches and understand their strengths. Install required libraries:

```bash
pip install langchain llama-index openai chromadb
```

**Important:**
- Try both frameworks
- Compare approaches
- Experiment with configurations
- Document differences
- Understand when to use which

---

## Tasks

### Task 1: LangChain RAG System

Build a complete RAG system using LangChain `langchain_rag.py`:

**Components:**
1. Document loading (PDF/TXT)
2. Text splitting
3. Vector store (ChromaDB)
4. Retrieval QA chain
5. Query interface

**Requirements:**
- Use LangChain components
- Support multiple document types
- Configurable chunking
- Return sources with answers
- Handle errors gracefully

**Test with:** Multiple documents

**Deliverable:** `task1_langchain_rag.py`

---

### Task 2: LlamaIndex RAG System

Build a complete RAG system using LlamaIndex `llamaindex_rag.py`:

**Components:**
1. Document loading
2. Index creation
3. Query engine
4. Response synthesis
5. Query interface

**Requirements:**
- Use LlamaIndex components
- Custom service context
- Configurable settings
- Source retrieval
- Error handling

**Test with:** Same documents as Task 1

**Deliverable:** `task2_llamaindex_rag.py`

---

### Task 3: Framework Configuration Comparison

Create a comparison tool `framework_comparison.py`:

**Compare:**
- Code complexity (lines of code)
- Setup time
- Query performance
- Feature availability
- Ease of customization

**Requirements:**
- Build same RAG system with both
- Measure performance metrics
- Document differences
- Create comparison report

**Deliverable:** `task3_comparison.py` + comparison report

---

### Task 4: Advanced Features Exploration

Explore advanced features in both frameworks:

**LangChain:**
- Conversational memory
- Different chain types
- Agents
- Custom retrievers

**LlamaIndex:**
- Different index types
- Advanced retrievers
- Response modes
- Node postprocessors

**Requirements:**
- Implement 2-3 advanced features from each
- Document what each does
- Show examples

**Deliverable:** `task4_advanced_features.py`

---

### Task 5: Hybrid Approach

Create a system that uses both frameworks `hybrid_rag.py`:

**Ideas:**
- Use LangChain for document loading
- Use LlamaIndex for indexing
- Combine retrieval strategies
- Use best of both worlds

**Requirements:**
- Integrate both frameworks
- Explain why you chose each component
- Make it work seamlessly

**Deliverable:** `task5_hybrid_rag.py`

---

## One Mini Project

### 🚀 Build a Framework Comparison RAG Application

Create a comprehensive application `framework_rag_comparison.py` that demonstrates both LangChain and LlamaIndex.

**Features:**

1. **Dual Framework Support:**
   - Switch between LangChain and LlamaIndex
   - Same documents, different frameworks
   - Side-by-side comparison

2. **Document Management:**
   - Load documents once
   - Index with both frameworks
   - Compare indexing time
   - Compare storage size

3. **Query Interface:**
   ```
   === Framework RAG Comparison ===
   1. Load documents
   2. Index with LangChain
   3. Index with LlamaIndex
   4. Query (LangChain)
   5. Query (LlamaIndex)
   6. Compare frameworks
   7. Performance metrics
   8. Exit
   ```

4. **Comparison Features:**
   - Side-by-side query results
   - Performance metrics (time, tokens)
   - Answer quality comparison
   - Source comparison
   - Code complexity metrics

5. **Advanced Analysis:**
   - Response time comparison
   - Token usage comparison
   - Answer similarity
   - Source overlap
   - Quality scoring

6. **Reporting:**
   - Generate comparison reports
   - Export results
   - Visualize differences
   - Recommendations

**Requirements:**
- Clean, modular code
- Both frameworks fully implemented
- Fair comparison methodology
- Detailed documentation
- Performance metrics
- User-friendly interface

**Example Usage:**
```python
app = FrameworkComparison()
app.load_documents("./documents/")

# Index with both
app.index_langchain()
app.index_llamaindex()

# Compare
results = app.compare_query("What is machine learning?")
print("LangChain:", results["langchain"]["answer"])
print("LlamaIndex:", results["llamaindex"]["answer"])
print("Similarity:", results["similarity_score"])
```

**Deliverables:**
- `framework_rag_comparison.py` - Main application
- `requirements.txt` - Dependencies
- `README_frameworks.md` - Documentation
- Comparison report template
- Example outputs

---

## Expected Output Section

### Task 1 Expected Output:
```python
# LangChain RAG
from langchain.chains import RetrievalQA

qa_chain = RetrievalQA.from_chain_type(...)
result = qa_chain({"query": "What is Python?"})

# Output:
{
    "result": "Python is a programming language...",
    "source_documents": [...]
}
```

### Task 2 Expected Output:
```python
# LlamaIndex RAG
index = VectorStoreIndex.from_documents(documents)
query_engine = index.as_query_engine()
response = query_engine.query("What is Python?")

# Output:
ResponseObject with:
- response: "Python is a programming language..."
- source_nodes: [...]
```

### Task 3 Expected Output:
```
=== Framework Comparison ===

LangChain:
- Setup time: 2.3s
- Query time: 1.2s
- Code lines: 45
- Features: High flexibility

LlamaIndex:
- Setup time: 1.8s
- Query time: 0.9s
- Code lines: 28
- Features: RAG-optimized

Recommendation: Use LlamaIndex for RAG-focused apps
```

### Mini Project Expected Output:

The comparison app should provide:
- Fair side-by-side comparisons
- Detailed metrics
- Clear recommendations
- Professional interface

**Example session:**
```
=== Framework RAG Comparison ===
Choose: 6

Query: "What is RAG?"

LangChain Result:
Answer: RAG stands for Retrieval-Augmented Generation...
Time: 1.2s | Tokens: 150

LlamaIndex Result:
Answer: RAG (Retrieval-Augmented Generation) is...
Time: 0.9s | Tokens: 145

Comparison:
- Answer similarity: 0.87
- Time difference: 0.3s (LlamaIndex faster)
- Token difference: 5 tokens
- Source overlap: 2/3 documents
```

---

## Submission Checklist

- [ ] Task 1: LangChain RAG working
- [ ] Task 2: LlamaIndex RAG working
- [ ] Task 3: Comparison complete
- [ ] Task 4: Advanced features explored
- [ ] Task 5: Hybrid approach implemented
- [ ] Mini project: Full comparison app
- [ ] Both frameworks tested
- [ ] Differences documented
- [ ] Code is well-documented

**Remember:** Frameworks save time, but understanding the fundamentals (Day 7) is crucial!

**Good luck!** 🚀

