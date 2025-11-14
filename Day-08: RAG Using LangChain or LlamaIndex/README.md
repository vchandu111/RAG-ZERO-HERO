# Day 8 — RAG Using LangChain or LlamaIndex

## 1. Beginner-Friendly Introduction

Now that you've built RAG from scratch, it's time to learn the frameworks that make it easier! **LangChain** and **LlamaIndex** are popular frameworks that abstract away the complexity and provide powerful features out of the box.

**Why use frameworks?**
- **Faster development**: Pre-built components
- **Best practices**: Built-in optimizations
- **More features**: Advanced capabilities
- **Community**: Well-documented and supported
- **Production-ready**: Battle-tested code

**LangChain vs. LlamaIndex:**
- **LangChain**: General-purpose LLM framework, flexible
- **LlamaIndex**: Specialized for RAG, data-focused

**Today's goal:**
Learn to build RAG systems using these frameworks, understanding when to use which.

---

## 2. Deep-Dive Explanation

### 2.1 LangChain Overview

**What is LangChain?**
A framework for building LLM applications with:
- Document loaders
- Text splitters
- Vector stores
- Chains (workflows)
- Agents (autonomous systems)

**Key Components:**
- **Document Loaders**: Load from various sources
- **Text Splitters**: Chunk documents
- **Embeddings**: Generate embeddings
- **Vector Stores**: Store and search
- **Retrievers**: Retrieve relevant docs
- **Chains**: Combine components
- **LLMs**: Language models

### 2.2 LangChain RAG Pipeline

**Components:**
```
Document Loader → Text Splitter → Embeddings → Vector Store
                                                      ↓
User Query → Embeddings → Retriever → Context + Query → LLM → Answer
```

**LangChain Abstractions:**
- `Document`: Text with metadata
- `TextSplitter`: Chunks documents
- `Embeddings`: Embedding interface
- `VectorStore`: Vector database interface
- `Retriever`: Retrieval interface
- `Chain`: Composable workflows

### 2.3 LlamaIndex Overview

**What is LlamaIndex?**
A data framework for LLM applications, optimized for RAG:
- Data connectors
- Indexing
- Querying
- Retrieval
- Response synthesis

**Key Concepts:**
- **Index**: Structured data representation
- **Nodes**: Chunks with metadata
- **Retrievers**: Find relevant nodes
- **Query Engines**: Answer questions
- **Response Synthesizers**: Generate answers

### 2.4 LlamaIndex RAG Pipeline

**Components:**
```
Documents → Load Data → Parse → Build Index
                              ↓
Query → Retrieve Nodes → Synthesize Response → Answer
```

**LlamaIndex Abstractions:**
- `Document`: Source document
- `Node`: Chunk with metadata
- `Index`: Structured data store
- `Retriever`: Retrieval logic
- `QueryEngine`: Query interface
- `ResponseSynthesizer`: Answer generation

### 2.5 When to Use Which?

**Use LangChain when:**
- Building general LLM applications
- Need flexibility and customization
- Want to combine multiple tools
- Building agents or complex workflows

**Use LlamaIndex when:**
- Focused on RAG applications
- Need advanced retrieval strategies
- Want optimized indexing
- Building data-centric applications

**You can use both!** They complement each other.

---

## 3. Instructor Examples

### Example 1: LangChain RAG

```python
from langchain.document_loaders import PyPDFLoader, TextLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import Chroma
from langchain.chains import RetrievalQA
from langchain.llms import OpenAI
import os

# Setup
os.environ["OPENAI_API_KEY"] = "your-key"

# 1. Load documents
loader = PyPDFLoader("document.pdf")
documents = loader.load()

# 2. Split text
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200
)
chunks = text_splitter.split_documents(documents)

# 3. Create embeddings and vector store
embeddings = OpenAIEmbeddings()
vectorstore = Chroma.from_documents(
    documents=chunks,
    embedding=embeddings
)

# 4. Create retriever
retriever = vectorstore.as_retriever(search_kwargs={"k": 3})

# 5. Create QA chain
qa_chain = RetrievalQA.from_chain_type(
    llm=OpenAI(temperature=0),
    chain_type="stuff",
    retriever=retriever,
    return_source_documents=True
)

# 6. Query
result = qa_chain({"query": "What is the main topic?"})
print(result["result"])
print(f"Sources: {len(result['source_documents'])}")
```

### Example 2: LangChain with Chat Models

```python
from langchain.chat_models import ChatOpenAI
from langchain.chains import ConversationalRetrievalChain
from langchain.memory import ConversationBufferMemory

# Use chat model
llm = ChatOpenAI(temperature=0, model_name="gpt-3.5-turbo")

# Add memory for conversation
memory = ConversationBufferMemory(
    memory_key="chat_history",
    return_messages=True
)

# Create conversational chain
qa_chain = ConversationalRetrievalChain.from_llm(
    llm=llm,
    retriever=retriever,
    memory=memory
)

# Query with conversation
result = qa_chain({"question": "What is Python?"})
result = qa_chain({"question": "What are its main features?"})  # Remembers context
```

### Example 3: LlamaIndex RAG

```python
from llama_index import VectorStoreIndex, SimpleDirectoryReader, ServiceContext
from llama_index.llms import OpenAI
import os

os.environ["OPENAI_API_KEY"] = "your-key"

# 1. Load documents
documents = SimpleDirectoryReader("./documents").load_data()

# 2. Create index (handles chunking, embedding, storage)
index = VectorStoreIndex.from_documents(documents)

# 3. Create query engine
query_engine = index.as_query_engine()

# 4. Query
response = query_engine.query("What is the main topic?")
print(response)
print(f"Source nodes: {len(response.source_nodes)}")
```

### Example 4: LlamaIndex with Custom Settings

```python
from llama_index import (
    VectorStoreIndex,
    ServiceContext,
    StorageContext
)
from llama_index.embeddings import OpenAIEmbedding
from llama_index.node_parser import SimpleNodeParser
from llama_index.vector_stores import ChromaVectorStore
import chromadb

# Custom service context
service_context = ServiceContext.from_defaults(
    llm=OpenAI(temperature=0, model="gpt-3.5-turbo"),
    embed_model=OpenAIEmbedding(),
    node_parser=SimpleNodeParser.from_defaults(chunk_size=500)
)

# Custom vector store
chroma_client = chromadb.Client()
chroma_collection = chroma_client.create_collection("rag_docs")
vector_store = ChromaVectorStore(chroma_collection=chroma_collection)
storage_context = StorageContext.from_defaults(vector_store=vector_store)

# Create index with custom settings
index = VectorStoreIndex.from_documents(
    documents,
    service_context=service_context,
    storage_context=storage_context
)

# Query
query_engine = index.as_query_engine(similarity_top_k=3)
response = query_engine.query("Your question here")
```

### Example 5: Comparing Both Frameworks

```python
# LangChain approach
from langchain.chains import RetrievalQA

langchain_qa = RetrievalQA.from_chain_type(
    llm=llm,
    retriever=retriever,
    return_source_documents=True
)

# LlamaIndex approach
from llama_index import VectorStoreIndex

llamaindex_index = VectorStoreIndex.from_documents(documents)
llamaindex_qa = llamaindex_index.as_query_engine()

# Both achieve similar results with different APIs
```

---

## 4. Student Practice Tasks

### Task 1: LangChain RAG Setup
Set up a basic LangChain RAG system:
- Load documents
- Create vector store
- Build QA chain
- Test with queries

### Task 2: LlamaIndex RAG Setup
Set up a basic LlamaIndex RAG system:
- Load documents
- Create index
- Build query engine
- Test with queries

### Task 3: Custom Configuration
Configure both frameworks with:
- Custom chunk sizes
- Different embedding models
- Various LLM parameters
- Compare results

### Task 4: Advanced Retrieval
Experiment with:
- Different retrieval strategies
- Metadata filtering
- Reranking
- Hybrid search

### Task 5: Framework Comparison
Build the same RAG system with both frameworks and compare:
- Code complexity
- Performance
- Features
- Ease of use

### Task 6: Integration
Combine LangChain and LlamaIndex components in a single system.

---

## 5. Summary / Key Takeaways

- **LangChain**: General-purpose LLM framework, flexible and composable
- **LlamaIndex**: RAG-optimized framework, data-centric
- **Both are powerful**: Choose based on your needs
- **Pre-built components**: Save development time
- **Best practices**: Frameworks include optimizations
- **Active communities**: Well-documented and supported
- **Production-ready**: Battle-tested code
- **Can combine**: Use both frameworks together
- **Learning curve**: Worth it for complex applications
- **Abstraction**: Understand what's happening under the hood

---

## 6. Further Reading (Optional)

- LangChain Documentation
- LlamaIndex Documentation
- Framework comparison articles
- Community examples and tutorials

---

**Next up:** Day 9 will cover advanced RAG techniques!

