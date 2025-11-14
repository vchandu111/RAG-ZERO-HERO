# Day 4 — Chunking & Data Extraction (PDF/Web/Docs)

## 1. Beginner-Friendly Introduction

Before you can build a RAG system, you need to extract and prepare your data. Today, you'll learn how to extract text from various sources (PDFs, websites, documents) and split it into manageable chunks—a crucial step in the RAG pipeline.

**Why this matters for RAG:**
- RAG systems need documents in text format
- Documents must be split into chunks that fit LLM context windows
- Different sources require different extraction methods
- Proper chunking improves retrieval quality

**Real-world context:**
Imagine you have a 100-page PDF and want to answer questions about it. You can't send all 100 pages to an LLM at once (token limits!). Instead, you:
1. Extract text from the PDF
2. Split it into smaller chunks (e.g., 500 words each)
3. Store these chunks for retrieval
4. When a question comes, find relevant chunks and send only those to the LLM

---

## 2. Deep-Dive Explanation

### 2.1 Data Extraction Overview

**The Pipeline:**
```
Source File → Extract Text → Clean Text → Chunk Text → Store Chunks
```

**Common Sources:**
- PDF files
- Web pages (HTML)
- Text files (.txt, .md)
- Word documents (.docx)
- CSV files
- JSON files

### 2.2 PDF Extraction

**Libraries:**
- `PyPDF2`: Basic PDF reading
- `pypdf`: Modern alternative
- `pdfplumber`: Better text extraction
- `PyMuPDF` (fitz): Fast and accurate

**Challenges:**
- Scanned PDFs (need OCR)
- Complex layouts
- Tables and images
- Multi-column text

**Basic Extraction:**
```python
import pypdf

def extract_pdf_text(filepath):
    text = ""
    with open(filepath, "rb") as file:
        reader = pypdf.PdfReader(file)
        for page in reader.pages:
            text += page.extract_text() + "\n"
    return text
```

### 2.3 Web Scraping

**Libraries:**
- `requests`: HTTP requests
- `BeautifulSoup4`: HTML parsing
- `selenium`: For JavaScript-heavy sites

**Basic Web Scraping:**
```python
import requests
from bs4 import BeautifulSoup

def extract_web_text(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.content, "html.parser")
    
    # Remove script and style elements
    for script in soup(["script", "style"]):
        script.decompose()
    
    # Get text
    text = soup.get_text()
    
    # Clean up whitespace
    lines = (line.strip() for line in text.splitlines())
    chunks = (phrase.strip() for line in lines for phrase in line.split("  "))
    text = " ".join(chunk for chunk in chunks if chunk)
    
    return text
```

### 2.4 Text Chunking Strategies

#### 2.4.1 Fixed-Size Chunking

Split text into chunks of fixed character/word count:
```
Text: [1000 chars] → Chunk1[500] + Chunk2[500]
```

**Pros:** Simple, predictable
**Cons:** May split sentences/paragraphs

#### 2.4.2 Sentence-Aware Chunking

Split at sentence boundaries:
```
Text → Sentences → Group into chunks (respecting max size)
```

**Pros:** Preserves sentence integrity
**Cons:** More complex

#### 2.4.3 Paragraph-Aware Chunking

Split at paragraph boundaries:
```
Text → Paragraphs → Group paragraphs into chunks
```

**Pros:** Preserves context
**Cons:** Chunks may vary significantly in size

#### 2.4.4 Overlapping Chunks

Add overlap between chunks for context:
```
Chunk1: [0-500] → Chunk2: [450-950] → Chunk3: [900-1400]
```

**Pros:** Maintains context across boundaries
**Cons:** More storage, potential redundancy

### 2.5 Chunking Best Practices

**Considerations:**
- **Chunk size**: 200-1000 tokens (depends on model)
- **Overlap**: 10-20% of chunk size
- **Boundaries**: Respect sentence/paragraph boundaries
- **Metadata**: Store source, position, timestamp

**Metadata to Store:**
```python
chunk_metadata = {
    "chunk_id": 1,
    "source": "document.pdf",
    "page": 3,
    "start_char": 0,
    "end_char": 500,
    "word_count": 75
}
```

### 2.6 Text Cleaning

**Common Cleaning Steps:**
1. Remove extra whitespace
2. Remove special characters (if needed)
3. Normalize encoding
4. Remove headers/footers
5. Handle line breaks

---

## 3. Instructor Examples

### Example 1: PDF Text Extraction

```python
import pypdf

def extract_pdf_text(filepath):
    """Extract text from a PDF file"""
    text = ""
    try:
        with open(filepath, "rb") as file:
            pdf_reader = pypdf.PdfReader(file)
            num_pages = len(pdf_reader.pages)
            
            for page_num in range(num_pages):
                page = pdf_reader.pages[page_num]
                page_text = page.extract_text()
                text += f"\n--- Page {page_num + 1} ---\n"
                text += page_text
                
        return text
    except Exception as e:
        print(f"Error extracting PDF: {e}")
        return None

# Usage
text = extract_pdf_text("document.pdf")
print(f"Extracted {len(text)} characters")
```

### Example 2: Web Scraping

```python
import requests
from bs4 import BeautifulSoup

def extract_web_content(url):
    """Extract main content from a webpage"""
    try:
        headers = {
            "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
        }
        response = requests.get(url, headers=headers, timeout=10)
        response.raise_for_status()
        
        soup = BeautifulSoup(response.content, "html.parser")
        
        # Remove unwanted elements
        for element in soup(["script", "style", "nav", "footer", "header"]):
            element.decompose()
        
        # Try to find main content
        main_content = soup.find("main") or soup.find("article") or soup.find("body")
        
        if main_content:
            text = main_content.get_text(separator=" ", strip=True)
            # Clean up multiple spaces
            text = " ".join(text.split())
            return text
        else:
            return soup.get_text(separator=" ", strip=True)
            
    except Exception as e:
        print(f"Error scraping {url}: {e}")
        return None

# Usage
content = extract_web_content("https://example.com/article")
```

### Example 3: Sentence-Aware Chunking

```python
import re

def chunk_text_sentences(text, chunk_size=500, overlap=50):
    """Chunk text respecting sentence boundaries"""
    # Split into sentences (simple approach)
    sentences = re.split(r'(?<=[.!?])\s+', text)
    
    chunks = []
    current_chunk = []
    current_size = 0
    
    for sentence in sentences:
        sentence_size = len(sentence)
        
        # If adding this sentence exceeds chunk size
        if current_size + sentence_size > chunk_size and current_chunk:
            # Save current chunk
            chunk_text = " ".join(current_chunk)
            chunks.append(chunk_text)
            
            # Start new chunk with overlap
            overlap_text = " ".join(current_chunk[-2:]) if len(current_chunk) >= 2 else ""
            current_chunk = [overlap_text, sentence] if overlap_text else [sentence]
            current_size = len(" ".join(current_chunk))
        else:
            current_chunk.append(sentence)
            current_size += sentence_size + 1  # +1 for space
    
    # Add final chunk
    if current_chunk:
        chunks.append(" ".join(current_chunk))
    
    return chunks

# Usage
long_text = "Sentence one. Sentence two. Sentence three..." * 50
chunks = chunk_text_sentences(long_text, chunk_size=200)
print(f"Created {len(chunks)} chunks")
```

### Example 4: Complete Document Processor

```python
class DocumentProcessor:
    def __init__(self, chunk_size=500, overlap=50):
        self.chunk_size = chunk_size
        self.overlap = overlap
        self.chunks = []
    
    def extract_from_pdf(self, filepath):
        """Extract text from PDF"""
        import pypdf
        text = ""
        with open(filepath, "rb") as file:
            reader = pypdf.PdfReader(file)
            for page in reader.pages:
                text += page.extract_text() + "\n"
        return text
    
    def extract_from_web(self, url):
        """Extract text from webpage"""
        import requests
        from bs4 import BeautifulSoup
        
        response = requests.get(url)
        soup = BeautifulSoup(response.content, "html.parser")
        for script in soup(["script", "style"]):
            script.decompose()
        return soup.get_text()
    
    def clean_text(self, text):
        """Clean extracted text"""
        # Remove extra whitespace
        text = " ".join(text.split())
        # Remove special characters (optional)
        # text = re.sub(r'[^\w\s]', '', text)
        return text
    
    def chunk_text(self, text, source="unknown"):
        """Chunk text and store with metadata"""
        words = text.split()
        chunks = []
        
        for i in range(0, len(words), self.chunk_size - self.overlap):
            chunk_words = words[i:i + self.chunk_size]
            chunk_text = " ".join(chunk_words)
            
            chunk_data = {
                "chunk_id": len(chunks) + 1,
                "text": chunk_text,
                "source": source,
                "word_count": len(chunk_words),
                "start_word": i,
                "end_word": min(i + self.chunk_size, len(words))
            }
            chunks.append(chunk_data)
        
        self.chunks.extend(chunks)
        return chunks
    
    def process_pdf(self, filepath):
        """Complete PDF processing pipeline"""
        text = self.extract_from_pdf(filepath)
        text = self.clean_text(text)
        chunks = self.chunk_text(text, source=filepath)
        return chunks

# Usage
processor = DocumentProcessor(chunk_size=200, overlap=20)
chunks = processor.process_pdf("document.pdf")
print(f"Processed {len(chunks)} chunks")
```

---

## 4. Student Practice Tasks

### Task 1: PDF Extractor
Write a function that extracts text from a PDF and returns:
- Full text
- Number of pages
- Text per page (as a list)

### Task 2: Web Scraper
Create a web scraper that:
- Takes a URL
- Extracts main content (removes nav, ads, etc.)
- Returns clean text
- Handles errors gracefully

### Task 3: Chunking Functions
Implement three chunking strategies:
- Fixed-size chunking
- Sentence-aware chunking
- Paragraph-aware chunking

Compare results on the same text.

### Task 4: Text Cleaner
Write a comprehensive text cleaning function that:
- Removes extra whitespace
- Handles encoding issues
- Removes headers/footers (if patterns detected)
- Normalizes line breaks

### Task 5: Chunk Metadata
Enhance chunking to include rich metadata:
- Source file
- Page number (for PDFs)
- Character positions
- Word count
- Timestamp

### Task 6: Multi-Format Processor
Create a processor that handles:
- PDF files
- Text files
- Web URLs
- Returns standardized chunk format

---

## 5. Summary / Key Takeaways

- **Data extraction** is the first step in RAG pipelines
- **PDF extraction** requires libraries like `pypdf` or `pdfplumber`
- **Web scraping** uses `requests` and `BeautifulSoup`
- **Chunking strategies** vary: fixed-size, sentence-aware, paragraph-aware
- **Overlapping chunks** preserve context across boundaries
- **Metadata** is crucial for tracking chunk sources
- **Text cleaning** improves chunk quality
- **Chunk size** should match your LLM's context window
- **Different sources** require different extraction methods

---

## 6. Further Reading (Optional)

- PyPDF2/PyPDF documentation
- BeautifulSoup documentation
- LangChain document loaders
- LlamaIndex data connectors
- Text chunking best practices

---

**Next up:** Day 5 will teach you about embeddings and vector databases!

