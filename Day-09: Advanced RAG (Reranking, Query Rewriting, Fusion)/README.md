# Day 9 — Advanced RAG (Reranking, Query Rewriting, Fusion)

## 1. Beginner-Friendly Introduction

Today, you'll learn advanced techniques that make RAG systems production-ready! These techniques improve answer quality, handle complex queries, and make your system more robust.

**What are Advanced RAG Techniques?**
- **Reranking**: Improve retrieval by reordering results
- **Query Rewriting**: Transform queries for better retrieval
- **Fusion**: Combine multiple retrieval strategies
- **Hybrid Search**: Mix semantic and keyword search

**Why these matter:**
- Basic RAG works, but advanced techniques make it better
- Production systems need these optimizations
- Handle edge cases and complex queries
- Improve answer accuracy and relevance

**Real-world context:**
Think of basic RAG as a simple search engine. Advanced RAG is like Google—it uses multiple strategies, reranks results, understands query intent, and combines different signals to give you the best answer.

---

## 2. Deep-Dive Explanation

### 2.1 Reranking

**What is Reranking?**
After retrieving initial results, rerank them using a more sophisticated model to improve order.

**Why Rerank?**
- Initial retrieval may miss subtle relevance
- Reranking models are more accurate
- Better final results

**How it works:**
```
Initial Retrieval (Top 10) → Reranking Model → Reordered Top 5
```

**Reranking Models:**
- Cross-encoders (BERT-based)
- Specialized reranking models
- Custom scoring functions

**Benefits:**
- Better top results
- Improved answer quality
- More relevant context

### 2.2 Query Rewriting

**What is Query Rewriting?**
Transform user queries to improve retrieval:
- Expand queries with synonyms
- Generate multiple query variations
- Reformulate for better matching
- Extract key terms

**Techniques:**
1. **Query Expansion**: Add related terms
2. **Query Decomposition**: Break into sub-queries
3. **Query Reformulation**: Rephrase for clarity
4. **Hybrid Queries**: Combine semantic + keyword

**Example:**
```
Original: "How to train model?"
Rewritten: ["How to train machine learning model?", "model training process", "train ML model"]
```

### 2.3 Fusion Techniques

**What is Fusion?**
Combine results from multiple retrieval strategies:
- Different embedding models
- Keyword + semantic search
- Multiple query variations
- Different chunk sizes

**Fusion Methods:**
1. **Reciprocal Rank Fusion (RRF)**: Combine rankings
2. **Weighted Fusion**: Weight different sources
3. **Deduplication**: Remove duplicates
4. **Score Normalization**: Normalize before combining

**Benefits:**
- More comprehensive retrieval
- Better coverage
- Reduces misses

### 2.4 Hybrid Search

**What is Hybrid Search?**
Combine semantic search (embeddings) with keyword search (BM25/TF-IDF):
- Semantic: Understands meaning
- Keyword: Exact matches
- Best of both worlds

**Implementation:**
```
Query → [Semantic Search] → Results 1
      → [Keyword Search]  → Results 2
      → [Fusion]          → Final Results
```

### 2.5 Advanced RAG Pipeline

**Enhanced Pipeline:**
```
User Query
    ↓
Query Rewriting (multiple variations)
    ↓
Multiple Retrieval Strategies
    ↓
Fusion (combine results)
    ↓
Reranking (improve order)
    ↓
Top K Chunks
    ↓
Augmentation + Generation
    ↓
Answer
```

---

## 3. Instructor Examples

### Example 1: Query Rewriting

```python
import openai

def rewrite_query(original_query):
    """Generate query variations"""
    prompt = f"""Generate 3 different ways to ask this question that might retrieve different relevant documents.

Original question: {original_query}

Generate 3 variations, each on a new line:"""
    
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": prompt}],
        temperature=0.7,
        max_tokens=150
    )
    
    variations = response.choices[0].message.content.strip().split("\n")
    variations = [v.strip("- ").strip() for v in variations if v.strip()]
    
    return [original_query] + variations

# Usage
query = "How does machine learning work?"
variations = rewrite_query(query)
print(variations)
# Output: [
#     "How does machine learning work?",
#     "What is the process of machine learning?",
#     "How do ML algorithms learn from data?",
#     "Explain the mechanism of machine learning"
# ]
```

### Example 2: Reciprocal Rank Fusion

```python
def reciprocal_rank_fusion(results_list, k=60):
    """
    Combine multiple ranked result lists using RRF
    
    Args:
        results_list: List of result lists, each with (doc_id, score)
        k: RRF constant (typically 60)
    """
    doc_scores = {}
    
    for results in results_list:
        for rank, (doc_id, score) in enumerate(results, 1):
            if doc_id not in doc_scores:
                doc_scores[doc_id] = 0
            doc_scores[doc_id] += 1 / (k + rank)
    
    # Sort by score
    fused_results = sorted(
        doc_scores.items(),
        key=lambda x: x[1],
        reverse=True
    )
    
    return fused_results

# Usage
results1 = [("doc1", 0.9), ("doc2", 0.8), ("doc3", 0.7)]
results2 = [("doc2", 0.85), ("doc1", 0.82), ("doc4", 0.75)]
results3 = [("doc3", 0.88), ("doc1", 0.80), ("doc5", 0.72)]

fused = reciprocal_rank_fusion([results1, results2, results3])
print(fused)
# Output: [("doc1", 0.083), ("doc2", 0.075), ("doc3", 0.068), ...]
```

### Example 3: Reranking with Cross-Encoder

```python
from sentence_transformers import CrossEncoder

class Reranker:
    def __init__(self, model_name="cross-encoder/ms-marco-MiniLM-L-6-v2"):
        self.model = CrossEncoder(model_name)
    
    def rerank(self, query, documents, top_k=5):
        """Rerank documents for a query"""
        # Create query-document pairs
        pairs = [[query, doc] for doc in documents]
        
        # Get scores
        scores = self.model.predict(pairs)
        
        # Sort by score
        ranked_indices = sorted(
            range(len(scores)),
            key=lambda i: scores[i],
            reverse=True
        )
        
        # Return top K
        reranked = [
            {
                "document": documents[i],
                "score": float(scores[i]),
                "rank": rank + 1
            }
            for rank, i in enumerate(ranked_indices[:top_k])
        ]
        
        return reranked

# Usage
reranker = Reranker()
documents = ["Doc 1 text...", "Doc 2 text...", "Doc 3 text..."]
reranked = reranker.rerank("What is Python?", documents, top_k=3)
for item in reranked:
    print(f"Rank {item['rank']}: Score {item['score']:.3f}")
```

### Example 4: Hybrid Search

```python
from rank_bm25 import BM25Okapi
import numpy as np

class HybridSearch:
    def __init__(self, documents, embeddings):
        self.documents = documents
        self.embeddings = embeddings
        
        # Setup BM25 (keyword search)
        tokenized_docs = [doc.split() for doc in documents]
        self.bm25 = BM25Okapi(tokenized_docs)
    
    def semantic_search(self, query_embedding, k=10):
        """Semantic search using embeddings"""
        similarities = []
        for emb in self.embeddings:
            similarity = np.dot(query_embedding, emb) / (
                np.linalg.norm(query_embedding) * np.linalg.norm(emb)
            )
            similarities.append(similarity)
        
        top_indices = np.argsort(similarities)[::-1][:k]
        return [(idx, similarities[idx]) for idx in top_indices]
    
    def keyword_search(self, query, k=10):
        """Keyword search using BM25"""
        tokenized_query = query.split()
        scores = self.bm25.get_scores(tokenized_query)
        top_indices = np.argsort(scores)[::-1][:k]
        return [(idx, scores[idx]) for idx in top_indices]
    
    def hybrid_search(self, query, query_embedding, k=5, alpha=0.5):
        """Combine semantic and keyword search"""
        # Get results from both
        semantic_results = self.semantic_search(query_embedding, k*2)
        keyword_results = self.keyword_search(query, k*2)
        
        # Normalize scores
        semantic_scores = {idx: score for idx, score in semantic_results}
        keyword_scores = {idx: score for idx, score in keyword_results}
        
        # Normalize to 0-1 range
        max_sem = max(semantic_scores.values()) if semantic_scores else 1
        max_key = max(keyword_scores.values()) if keyword_scores else 1
        
        # Combine scores
        combined_scores = {}
        all_indices = set(semantic_scores.keys()) | set(keyword_scores.keys())
        
        for idx in all_indices:
            sem_score = semantic_scores.get(idx, 0) / max_sem if max_sem > 0 else 0
            key_score = keyword_scores.get(idx, 0) / max_key if max_key > 0 else 0
            combined_scores[idx] = alpha * sem_score + (1 - alpha) * key_score
        
        # Return top K
        top_indices = sorted(
            combined_scores.items(),
            key=lambda x: x[1],
            reverse=True
        )[:k]
        
        return top_indices

# Usage
hybrid = HybridSearch(documents, embeddings)
results = hybrid.hybrid_search("Python programming", query_embedding, k=5)
```

### Example 5: Complete Advanced RAG Pipeline

```python
class AdvancedRAG:
    def __init__(self):
        self.vector_store = None
        self.reranker = Reranker()
        # ... other components
    
    def query(self, question, k=5):
        """Advanced RAG with all techniques"""
        # 1. Query Rewriting
        query_variations = rewrite_query(question)
        
        # 2. Multiple Retrievals
        all_results = []
        for query_var in query_variations:
            results = self.vector_store.search(query_var, k=k*2)
            all_results.append(results)
        
        # 3. Fusion
        fused_results = reciprocal_rank_fusion(all_results)
        
        # 4. Reranking
        top_docs = [self.vector_store.get_doc(doc_id) for doc_id, _ in fused_results[:k*2]]
        reranked = self.reranker.rerank(question, top_docs, top_k=k)
        
        # 5. Augment and Generate
        context = "\n\n".join([item["document"] for item in reranked])
        answer = self.generate_answer(question, context)
        
        return {
            "answer": answer,
            "sources": reranked
        }
```

---

## 4. Student Practice Tasks

### Task 1: Query Rewriting
Implement query rewriting that:
- Generates 3-5 query variations
- Uses LLM to create variations
- Tests if variations improve retrieval

### Task 2: Reranking
Add reranking to your RAG system:
- Use a reranking model
- Compare results before/after reranking
- Measure improvement

### Task 3: Fusion
Implement fusion:
- Combine results from multiple queries
- Use RRF or weighted fusion
- Compare fused vs single retrieval

### Task 4: Hybrid Search
Build hybrid search:
- Combine semantic + keyword search
- Tune the alpha parameter
- Compare with pure semantic search

### Task 5: Complete Advanced Pipeline
Combine all techniques:
- Query rewriting
- Multiple retrievals
- Fusion
- Reranking
- Generation

### Task 6: Evaluation
Evaluate advanced techniques:
- Compare answer quality
- Measure retrieval improvement
- Test with various queries

---

## 5. Summary / Key Takeaways

- **Reranking** improves result order using sophisticated models
- **Query rewriting** generates variations for better retrieval
- **Fusion** combines multiple retrieval strategies
- **Hybrid search** mixes semantic and keyword search
- **Advanced techniques** significantly improve RAG quality
- **Production systems** benefit from these optimizations
- **Trade-offs**: More complexity vs better results
- **Evaluation** is crucial to measure improvements
- **Combining techniques** often works best
- **Start simple**, add complexity as needed

---

## 6. Further Reading (Optional)

- "Reciprocal Rank Fusion" paper
- Cross-encoder models documentation
- BM25 algorithm explanation
- RAG evaluation metrics
- Production RAG best practices

---

**Next up:** Day 10 - Build and deploy a complete RAG application!

