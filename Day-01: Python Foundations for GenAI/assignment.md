# Day 1 — Assignment

## Instructions

Complete the following tasks to reinforce your Python foundations. Write all code in separate Python files (`.py`). Test your code thoroughly and make sure it runs without errors. 

**Important:** 
- Use proper error handling
- Add comments to explain your code
- Follow Python naming conventions (snake_case for functions/variables)
- Test with different inputs to ensure your code is robust

---

## Tasks

### Task 1: Document Statistics Calculator

Create a function `calculate_document_stats(filename)` that:
- Reads a text file
- Returns a dictionary with:
  - Total characters (including spaces)
  - Total words
  - Total sentences (split by `.`, `!`, `?`)
  - Average words per sentence
  - Most common word (and its frequency)

**Test file:** Create a `sample.txt` file with at least 5 sentences to test your function.

---

### Task 2: Text Chunker with Overlap

Implement a function `chunk_with_overlap(text, chunk_size, overlap)` that:
- Splits text into chunks of `chunk_size` characters
- Each chunk overlaps with the previous one by `overlap` characters
- Returns a list of dictionaries, each containing:
  - `chunk_id`: Sequential number (1, 2, 3...)
  - `text`: The chunk text
  - `start_pos`: Starting character position
  - `end_pos`: Ending character position
  - `word_count`: Number of words in chunk

**Example:**
```python
text = "This is a sample text for chunking."
chunks = chunk_with_overlap(text, chunk_size=10, overlap=3)
# Should create overlapping chunks
```

---

### Task 3: Document Manager Class

Create a `DocumentManager` class that:
- Can load multiple documents
- Stores each document with metadata (filename, content, word_count)
- Has a method to find documents by keyword (searches in content)
- Has a method to get statistics across all documents
- Has a method to export all document info to a JSON file

**Requirements:**
- Use a dictionary to store documents (key: filename, value: document data)
- Implement `add_document(filename, content)`
- Implement `search_documents(keyword)` → returns list of matching filenames
- Implement `get_all_stats()` → returns summary statistics
- Implement `export_to_json(output_file)` → saves all document data

---

### Task 4: Text Preprocessing Function

Write a function `preprocess_text(text)` that:
- Converts text to lowercase
- Removes all punctuation (keep spaces)
- Removes extra whitespace (multiple spaces → single space)
- Removes leading/trailing whitespace
- Returns the cleaned text

**Bonus:** Also create a function that removes stop words (common words like "the", "a", "an", "is", etc.)

---

### Task 5: File Batch Processor

Create a function `process_multiple_files(file_list, output_dir)` that:
- Takes a list of file paths
- Reads each file
- Processes it (calculate stats, chunk it, etc.)
- Saves processed results to `output_dir`
- Returns a summary report

**Requirements:**
- Handle errors gracefully (skip files that can't be read)
- Create output directory if it doesn't exist
- Save each file's stats as a separate JSON file
- Return a dictionary with success/failure counts

---

## One Mini Project

### 📘 Build a Document Analyzer Tool

Create a complete Python script `document_analyzer.py` that:

1. **Takes command-line arguments:**
   - Input file or directory
   - Output format (JSON, TXT, or both)
   - Chunk size (optional, default 200)

2. **For a single file:**
   - Reads the file
   - Calculates statistics (words, sentences, characters)
   - Chunks the text
   - Generates a word frequency report
   - Exports results

3. **For a directory:**
   - Processes all `.txt` files in the directory
   - Creates a summary report
   - Exports individual file reports

4. **Output includes:**
   - Document statistics
   - Top 10 most common words
   - Chunk information
   - Processing timestamp

**Example usage:**
```bash
python document_analyzer.py input.txt --output json --chunk-size 200
python document_analyzer.py ./documents/ --output both
```

**Requirements:**
- Use `argparse` for command-line arguments
- Implement proper error handling
- Use classes to organize your code
- Include docstrings for all functions
- Make it user-friendly with clear output messages

**Deliverables:**
- `document_analyzer.py` - Main script
- `requirements.txt` - List of dependencies (if any)
- Sample output files showing the results

---

## Expected Output Section

### Task 1 Expected Output:
```python
stats = calculate_document_stats("sample.txt")
# Output: {
#     'characters': 245,
#     'words': 42,
#     'sentences': 5,
#     'avg_words_per_sentence': 8.4,
#     'most_common_word': ('the', 5)
# }
```

### Task 2 Expected Output:
```python
chunks = chunk_with_overlap("Long text here...", 20, 5)
# Output: [
#     {'chunk_id': 1, 'text': 'Long text here...', 'start_pos': 0, 'end_pos': 20, 'word_count': 4},
#     {'chunk_id': 2, 'text': 'here...more text', 'start_pos': 15, 'end_pos': 35, 'word_count': 3},
#     ...
# ]
```

### Mini Project Expected Output:

When you run the document analyzer:
- Clear console output showing progress
- Generated JSON/TXT files with analysis results
- Summary statistics displayed in terminal
- Error messages for any files that couldn't be processed

**Example console output:**
```
Document Analyzer Tool
=====================
Processing: sample.txt
✓ File processed successfully
  - Words: 1,234
  - Sentences: 45
  - Chunks: 12
  - Top word: 'the' (45 occurrences)
Results saved to: output/sample_analysis.json
```

---

## Submission Checklist

- [ ] All 5 tasks completed and tested
- [ ] Mini project fully functional
- [ ] Code is well-commented
- [ ] Error handling implemented
- [ ] Code follows Python best practices
- [ ] All files run without errors

**Good luck!** 🚀

