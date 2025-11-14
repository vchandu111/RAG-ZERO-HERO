# Day 4 — Assignment

## Instructions

Complete these tasks to master data extraction and chunking. You'll work with PDFs, web pages, and implement various chunking strategies. Make sure you have the required libraries installed:

```bash
pip install pypdf beautifulsoup4 requests
```

**Important:**
- Test with real files/URLs
- Handle errors gracefully
- Consider edge cases (empty files, malformed HTML, etc.)
- Document your chunking decisions

---

## Tasks

### Task 1: PDF Text Extractor

Create a comprehensive PDF extractor `pdf_extractor.py` that:

1. Extracts text from PDF files
2. Returns structured data:
   - Full text
   - Text per page (list)
   - Total pages
   - Metadata (author, title if available)
3. Handles errors (corrupted files, password-protected, etc.)
4. Optionally extracts text from specific page ranges

**Requirements:**
- Use `pypdf` library
- Add progress indication for large PDFs
- Support batch processing (multiple PDFs)

**Test with:** A multi-page PDF document

**Deliverable:** `task1_pdf_extractor.py`

---

### Task 2: Web Content Scraper

Build a web scraper `web_scraper.py` that:

1. Takes a URL as input
2. Extracts main content (removes navigation, ads, scripts)
3. Returns clean, readable text
4. Handles different website structures
5. Extracts metadata (title, author, date if available)

**Requirements:**
- Use `requests` and `BeautifulSoup`
- Add proper headers (User-Agent)
- Handle timeouts and errors
- Support both single pages and article-style pages
- Clean extracted text (remove extra whitespace, normalize)

**Test with:** 
- A news article URL
- A blog post URL
- A Wikipedia page

**Deliverable:** `task2_web_scraper.py`

---

### Task 3: Chunking Strategy Comparison

Implement three different chunking strategies and compare them:

1. **Fixed-Size Chunking**: Split by character/word count
2. **Sentence-Aware Chunking**: Respect sentence boundaries
3. **Paragraph-Aware Chunking**: Respect paragraph boundaries

**Requirements:**
- All strategies should support overlap
- Create a comparison function that:
  - Tests all three on the same text
  - Reports chunk count, average size, size variance
  - Shows sample chunks from each strategy
- Visualize differences (print sample chunks side-by-side)

**Test with:** A long text document (at least 2000 words)

**Deliverable:** `task3_chunking_comparison.py`

---

### Task 4: Text Cleaning Pipeline

Create a comprehensive text cleaning module `text_cleaner.py`:

**Cleaning functions:**
1. Remove extra whitespace
2. Normalize line breaks
3. Remove special characters (configurable)
4. Remove headers/footers (detect common patterns)
5. Fix encoding issues
6. Remove URLs/email addresses (optional)
7. Normalize quotes and dashes

**Requirements:**
- Make each cleaning step optional/configurable
- Create a `clean_text()` function that applies all steps
- Test each step individually
- Show before/after examples

**Deliverable:** `task4_text_cleaner.py`

---

### Task 5: Chunk Metadata System

Build a chunking system that stores rich metadata:

**Metadata to include:**
- `chunk_id`: Unique identifier
- `source`: Source file/URL
- `page_number`: Page number (for PDFs)
- `chunk_index`: Position in document
- `start_char`: Starting character position
- `end_char`: Ending character position
- `word_count`: Number of words
- `char_count`: Number of characters
- `timestamp`: When chunk was created
- `preview`: First 50 characters (for quick preview)

**Requirements:**
- Create a `Chunk` class to store this data
- Implement methods to:
  - Export chunks to JSON
  - Filter chunks by metadata
  - Get chunk statistics

**Deliverable:** `task5_chunk_metadata.py`

---

## One Mini Project

### 📘 Build a PDF-to-Text Extractor and Chunker

Create a complete application `document_processor.py` that processes documents and prepares them for RAG.

**Features:**

1. **Multi-Format Support:**
   - PDF files
   - Text files (.txt, .md)
   - Web URLs
   - (Optional) Word documents (.docx)

2. **Processing Pipeline:**
   ```
   Input → Extract → Clean → Chunk → Store → Report
   ```

3. **Chunking Options:**
   - Chunk size (characters/words)
   - Overlap percentage
   - Strategy (fixed, sentence-aware, paragraph-aware)
   - Minimum chunk size

4. **Output Formats:**
   - JSON (structured chunks with metadata)
   - Text file (one chunk per line)
   - CSV (chunks with metadata columns)
   - Console display (formatted)

5. **Batch Processing:**
   - Process multiple files
   - Process entire directories
   - Progress tracking
   - Error reporting

6. **Statistics and Reporting:**
   - Total chunks created
   - Average chunk size
   - Size distribution
   - Processing time
   - Source information

7. **Interactive CLI:**
   ```
   === Document Processor ===
   1. Process single file
   2. Process directory
   3. Process web URL
   4. Configure chunking settings
   5. View statistics
   6. Export results
   7. Exit
   ```

**Requirements:**
- Use classes for organization
- Implement proper error handling
- Add progress bars for long operations
- Support command-line arguments
- Create a configuration system (JSON/YAML)
- Generate detailed reports
- Store results in organized folders

**Example Usage:**
```bash
# Command line
python document_processor.py input.pdf --chunk-size 500 --overlap 50 --output json

# Interactive mode
python document_processor.py
```

**Example Output:**
```
Processing: document.pdf
✓ Extracted 15 pages
✓ Cleaned text (removed 234 extra spaces)
✓ Created 42 chunks
✓ Average chunk size: 487 words
✓ Processing time: 2.3 seconds

Chunks saved to: output/document_chunks.json
Statistics saved to: output/document_stats.txt
```

**Advanced Features (Bonus):**
- OCR support for scanned PDFs
- Table extraction
- Image extraction
- Language detection
- Duplicate detection
- Chunk quality scoring

**Deliverables:**
- `document_processor.py` - Main application
- `config.json` - Configuration file template
- `requirements.txt` - Dependencies
- `README_processor.md` - Usage documentation
- Sample output files demonstrating functionality

---

## Expected Output Section

### Task 1 Expected Output:
```python
result = extract_pdf("document.pdf")
# Output:
{
    "full_text": "Complete text...",
    "pages": [
        "Page 1 text...",
        "Page 2 text...",
        ...
    ],
    "total_pages": 15,
    "metadata": {
        "title": "Sample Document",
        "author": "John Doe"
    }
}
```

### Task 2 Expected Output:
```python
content = scrape_web("https://example.com/article")
# Output:
{
    "title": "Article Title",
    "content": "Clean article text...",
    "author": "Author Name",
    "date": "2024-01-15",
    "word_count": 1234
}
```

### Task 3 Expected Output:
```
=== Chunking Strategy Comparison ===

Text: 2500 words

Fixed-Size Chunking:
- Chunks: 5
- Avg size: 500 words
- Size variance: 0 words
- Sample: "This is the first chunk of text..."

Sentence-Aware Chunking:
- Chunks: 6
- Avg size: 417 words
- Size variance: 45 words
- Sample: "This is the first chunk. It respects..."

Paragraph-Aware Chunking:
- Chunks: 4
- Avg size: 625 words
- Size variance: 120 words
- Sample: "This is a complete paragraph. It contains..."
```

### Task 5 Expected Output:
```python
chunks = chunk_with_metadata(text, source="doc.pdf")
# Output: List of Chunk objects
[
    Chunk(
        chunk_id=1,
        source="doc.pdf",
        page_number=1,
        start_char=0,
        end_char=500,
        word_count=75,
        preview="This is the beginning of the chunk..."
    ),
    ...
]
```

### Mini Project Expected Output:

The document processor should provide:
- Clear progress indicators
- Detailed statistics
- Multiple output formats
- Error handling and reporting
- Professional CLI interface

**Example session:**
```
=== Document Processor ===
Choose option: 1

Enter file path: document.pdf
Chunk size [500]: 400
Overlap [50]: 40
Strategy [fixed/sentence/paragraph]: sentence

[Processing...]
✓ Extracted 15 pages
✓ Created 38 chunks
✓ Saved to output/document_chunks.json

Statistics:
- Total words: 15,234
- Chunks: 38
- Avg chunk size: 401 words
- Processing time: 2.1s

[1] View chunks
[2] Export to CSV
[3] Process another file
[4] Main menu
```

---

## Submission Checklist

- [ ] Task 1: PDF extractor working
- [ ] Task 2: Web scraper functional
- [ ] Task 3: Chunking comparison complete
- [ ] Task 4: Text cleaning pipeline implemented
- [ ] Task 5: Metadata system working
- [ ] Mini project: Complete document processor
- [ ] All code handles errors gracefully
- [ ] Code is well-documented
- [ ] Tested with real files/URLs

**Remember:** Good data extraction and chunking are crucial for RAG quality!

**Good luck!** 🚀

