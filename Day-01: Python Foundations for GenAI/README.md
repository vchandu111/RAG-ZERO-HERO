# Day 1 — Python Foundations for GenAI

## 1. Beginner-Friendly Introduction

Welcome to Day 1! Before we dive into RAG and AI, we need to ensure you have a solid foundation in Python programming. Python is the primary language used in Generative AI development, and understanding its core concepts will make everything else much easier.

**Why this matters for RAG:**

- RAG systems are built primarily in Python
- You'll need to work with data structures (lists, dictionaries) to handle documents
- File handling is essential for reading PDFs, text files, and web content
- API interactions are crucial for connecting to LLM services
- Understanding functions and classes helps organize RAG code

**Real-world context:**
Think of Python as your toolkit. Just like a carpenter needs to know how to use a hammer before building a house, you need Python skills before building RAG systems. Every RAG application you'll build will use these fundamental concepts.

---

## 2. Deep-Dive Explanation

### Core Python Concepts for GenAI

#### 2.1 Data Structures

**Lists** - Ordered collections of items

```python
documents = ["doc1.txt", "doc2.txt", "doc3.txt"]
chunks = []  # Empty list to store text chunks
```

**Dictionaries** - Key-value pairs (perfect for storing metadata)

```python
document_info = {
    "filename": "article.pdf",
    "page_count": 10,
    "author": "John Doe",
    "chunks": []
}
```

**Tuples** - Immutable ordered collections

```python
api_config = ("https://api.openai.com", "v1", "gpt-4")
```

#### 2.2 File Handling

Reading and writing files is essential for RAG:

```
File → Read → Process → Store
```

**Reading text files:**

```python
with open("document.txt", "r", encoding="utf-8") as file:
    content = file.read()
```

**Writing to files:**

```python
with open("output.txt", "w", encoding="utf-8") as file:
    file.write("Processed content")
```

#### 2.3 Functions and Classes

**Functions** - Reusable blocks of code

```python
def chunk_text(text, chunk_size=100):
    """Split text into chunks of specified size"""
    chunks = []
    for i in range(0, len(text), chunk_size):
        chunks.append(text[i:i+chunk_size])
    return chunks
```

**Classes** - Organizing related functionality

```python
class DocumentProcessor:
    def __init__(self, filename):
        self.filename = filename
        self.content = ""

    def load(self):
        with open(self.filename, "r") as f:
            self.content = f.read()

    def get_word_count(self):
        return len(self.content.split())
```

#### 2.4 Working with APIs

RAG systems interact with APIs (like OpenAI):

```
Your Code → HTTP Request → API → Response → Your Code
```

**Basic API interaction pattern:**

```python
import requests

def call_api(url, data):
    response = requests.post(url, json=data)
    return response.json()
```

#### 2.5 List Comprehensions and Generators

**List comprehensions** - Concise way to create lists

```python
# Traditional way
squares = []
for x in range(10):
    squares.append(x**2)

# List comprehension
squares = [x**2 for x in range(10)]
```

**Generators** - Memory-efficient iteration

```python
def chunk_generator(text, chunk_size):
    for i in range(0, len(text), chunk_size):
        yield text[i:i+chunk_size]
```

---

## 3. Instructor Examples

### Example 1: Reading and Processing a Text File

```python
def read_and_process_file(filename):
    """Read a file and return processed content"""
    try:
        with open(filename, "r", encoding="utf-8") as file:
            content = file.read()

        # Basic processing
        lines = content.split("\n")
        word_count = len(content.split())

        return {
            "content": content,
            "lines": len(lines),
            "words": word_count
        }
    except FileNotFoundError:
        print(f"File {filename} not found!")
        return None

# Usage
result = read_and_process_file("sample.txt")
if result:
    print(f"Lines: {result['lines']}, Words: {result['words']}")
```

### Example 2: Text Chunking Function

```python
def chunk_text(text, chunk_size=200, overlap=50):
    """
    Split text into overlapping chunks

    Args:
        text: Input text to chunk
        chunk_size: Size of each chunk
        overlap: Number of characters to overlap between chunks
    """
    chunks = []
    start = 0

    while start < len(text):
        end = start + chunk_size
        chunk = text[start:end]
        chunks.append(chunk)
        start = end - overlap  # Overlap for context

    return chunks

# Usage
long_text = "This is a very long document..." * 100
chunks = chunk_text(long_text, chunk_size=200, overlap=50)
print(f"Created {len(chunks)} chunks")
```

### Example 3: Working with Dictionaries for Document Metadata

```python
class Document:
    def __init__(self, filename, content):
        self.filename = filename
        self.content = content
        self.metadata = {
            "word_count": len(content.split()),
            "char_count": len(content),
            "chunks": []
        }

    def add_chunk(self, chunk_text, chunk_id):
        chunk_data = {
            "id": chunk_id,
            "text": chunk_text,
            "length": len(chunk_text)
        }
        self.metadata["chunks"].append(chunk_data)

    def get_summary(self):
        return {
            "filename": self.filename,
            "words": self.metadata["word_count"],
            "chunks": len(self.metadata["chunks"])
        }

# Usage
doc = Document("article.txt", "This is the content of the article...")
doc.add_chunk("First chunk", 1)
doc.add_chunk("Second chunk", 2)
print(doc.get_summary())
```

### Example 4: Simple API Request Pattern

```python
import requests
import json

def make_api_request(url, payload, headers=None):
    """Make a POST request to an API"""
    default_headers = {"Content-Type": "application/json"}
    if headers:
        default_headers.update(headers)

    try:
        response = requests.post(url, json=payload, headers=default_headers)
        response.raise_for_status()  # Raises exception for bad status codes
        return response.json()
    except requests.exceptions.RequestException as e:
        print(f"API request failed: {e}")
        return None

# Usage pattern (you'll use this with OpenAI API later)
# payload = {"prompt": "Hello, world!"}
# result = make_api_request("https://api.example.com/endpoint", payload)
```

---

## 4. Student Practice Tasks

### Task 1: File Reader Function

Write a function that reads a file and returns:

- The content as a string
- The number of sentences (split by periods)
- A list of unique words (lowercase)

### Task 2: Dictionary Manipulation

Create a dictionary to store information about 3 documents. Each document should have:

- `title`
- `author`
- `word_count`
- `chunks` (a list)

Then write a function that finds the document with the most words.

### Task 3: Text Processing

Write a function that:

1. Takes a long string of text
2. Removes all punctuation
3. Converts to lowercase
4. Splits into words
5. Returns a dictionary with word frequencies

### Task 4: Chunking with Metadata

Modify the chunking function to also return metadata for each chunk:

- Chunk number
- Start position
- End position
- Word count

### Task 5: Error Handling

Write a robust file reader that handles:

- File not found errors
- Permission errors
- Encoding errors
- Empty files

### Task 6: List Comprehension Challenge

Convert this code to use list comprehensions:

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
even_squares = []
for num in numbers:
    if num % 2 == 0:
        even_squares.append(num ** 2)
```

---

## 5. Summary / Key Takeaways

- **Lists and dictionaries** are essential for storing documents and metadata in RAG systems
- **File handling** with `with` statements ensures proper resource management
- **Functions** help organize code and make it reusable
- **Classes** provide structure for complex data and operations
- **API interactions** will be crucial when connecting to LLM services
- **List comprehensions** make code more Pythonic and readable
- **Error handling** is important for robust applications
- **Text processing** (chunking, splitting) is fundamental to RAG

---

## 6. Further Reading (Optional)

- Python Official Documentation: [docs.python.org](https://docs.python.org/3/)
- Real Python: Great tutorials on Python fundamentals
- Python `requests` library documentation for API calls
- PEP 8: Python style guide for writing clean code

---

**Next up:** Day 2 will introduce you to Generative AI and Large Language Models!
