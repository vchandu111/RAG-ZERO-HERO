# Day 3 — Assignment

## Instructions

Complete these tasks to master prompt engineering. You'll create various prompt templates and test them with the OpenAI API. Focus on:
- Clarity and specificity
- Proper structure
- Effective use of examples
- Context handling

**Important:**
- Test all prompts with actual API calls
- Compare different prompt variations
- Document what works and what doesn't
- Save your best prompts as reusable templates

---

## Tasks

### Task 1: Prompt Improvement Challenge

Take these 5 vague prompts and rewrite them to be specific, clear, and effective:

1. "Tell me about machine learning"
2. "Fix this code"
3. "Summarize this"
4. "What's the best way?"
5. "Explain this document"

For each:
- Write an improved version
- Explain why your version is better
- Test both versions with the API
- Compare the results

**Deliverable:** `task1_prompt_improvements.py` with both old and new prompts, plus comparison results

---

### Task 2: Role-Based Prompt System

Create a system that generates prompts based on different AI roles:

**Roles to implement:**
- `coding_tutor`: Explains programming concepts to beginners
- `business_analyst`: Analyzes business problems
- `creative_writer`: Helps with creative writing
- `data_scientist`: Explains data science concepts
- `technical_writer`: Creates technical documentation

**Requirements:**
- Create a function `generate_role_prompt(role, user_input)`
- Each role should have a distinct personality and style
- Test each role with the same input to see how responses differ

**Deliverable:** `task2_role_prompts.py`

---

### Task 3: Few-Shot Learning Templates

Create few-shot prompt templates for these tasks:

1. **Text Classification**: Classify customer reviews as positive, negative, or neutral
2. **Format Conversion**: Convert informal text to formal business language
3. **Information Extraction**: Extract names, dates, and locations from text
4. **Code Translation**: Convert Python code to pseudocode

**Requirements:**
- Each template should have 3-5 examples
- Create reusable functions
- Test with new inputs

**Deliverable:** `task3_fewshot_templates.py`

---

### Task 4: Chain-of-Thought Problem Solver

Build a problem-solving system using chain-of-thought prompting:

**Problem types to handle:**
- Math word problems
- Logic puzzles
- Code debugging scenarios
- Decision-making problems

**Requirements:**
- Create a function that formats problems with CoT instructions
- The prompt should encourage step-by-step reasoning
- Extract and display the reasoning steps from the response

**Example:**
```python
problem = "If 3 apples cost $2, how much do 9 apples cost?"
solution = solve_with_cot(problem)
# Should show: Step 1, Step 2, Step 3, Final Answer
```

**Deliverable:** `task4_cot_solver.py`

---

### Task 5: RAG Prompt Template Builder

Create a comprehensive RAG prompt template system:

**Features:**
1. **Context Injection**: Add retrieved documents to prompt
2. **Citation Support**: Include instructions for citing sources
3. **Answer Formatting**: Specify output format (paragraph, bullet points, JSON)
4. **Fallback Handling**: Instructions for when context is insufficient
5. **Multi-document Synthesis**: Handle multiple relevant documents

**Requirements:**
- Create a class `RAGPromptBuilder`
- Methods:
  - `add_context(documents)` - Add retrieved documents
  - `set_question(question)` - Set the question
  - `set_format(format_type)` - Set output format
  - `enable_citations(enable)` - Toggle citation requirements
  - `build()` - Generate final prompt

**Example usage:**
```python
builder = RAGPromptBuilder()
builder.add_context(["Doc 1 content", "Doc 2 content"])
builder.set_question("What is RAG?")
builder.set_format("bullet_points")
builder.enable_citations(True)
prompt = builder.build()
```

**Deliverable:** `task5_rag_prompt_builder.py`

---

## One Mini Project

### 🎯 Build a Prompt Engineering Playground

Create an interactive application `prompt_playground.py` that allows users to experiment with different prompt engineering techniques.

**Features:**

1. **Main Menu:**
   ```
   === Prompt Engineering Playground ===
   1. Basic Prompt Tester
   2. Role-Based Prompts
   3. Few-Shot Learning
   4. Chain-of-Thought
   5. RAG Prompt Builder
   6. Prompt Comparison Tool
   7. Save/Load Prompts
   8. Exit
   ```

2. **Basic Prompt Tester:**
   - Enter a prompt
   - Adjust parameters (temperature, max_tokens)
   - View response
   - Rate the response quality (1-5)
   - Save prompts and ratings

3. **Role-Based Prompts:**
   - Select from predefined roles
   - Enter your input
   - See how different roles respond
   - Create custom roles

4. **Few-Shot Learning:**
   - Add examples interactively
   - Test with new inputs
   - Compare results with/without examples
   - Save example sets

5. **Chain-of-Thought:**
   - Enter a problem
   - View step-by-step reasoning
   - Extract and highlight reasoning steps
   - Compare with direct answers

6. **RAG Prompt Builder:**
   - Add context documents
   - Set question
   - Configure options (citations, format, etc.)
   - Generate and test prompt
   - Save templates

7. **Prompt Comparison Tool:**
   - Enter multiple prompt variations
   - Test all with the same input
   - Side-by-side comparison
   - Quality scoring
   - Export comparison results

8. **Save/Load Prompts:**
   - Save successful prompts to JSON
   - Load saved prompts
   - Organize by category
   - Search prompts

**Advanced Features:**
- **A/B Testing**: Compare two prompts statistically
- **Prompt Library**: Pre-built prompts for common tasks
- **Response Analyzer**: Analyze response quality (length, structure, etc.)
- **Token Optimizer**: Suggest ways to reduce token usage
- **Export Options**: Export prompts and results to various formats

**Requirements:**
- Use classes for organization
- Store prompts and results in JSON files
- Implement a clean CLI interface
- Add color coding for better UX (optional)
- Include help/documentation
- Handle errors gracefully

**Example Interaction:**
```
=== Prompt Engineering Playground ===
Choose option: 1

Enter your prompt: Explain quantum computing
Temperature [0.7]: 0.5
Max tokens [200]: 150

[Processing...]

Response:
Quantum computing uses quantum mechanical phenomena...

Tokens: 87
Rate this response (1-5): 4

[1] Try different parameters
[2] Save this prompt
[3] Compare with another prompt
[4] Main menu
```

**Deliverables:**
- `prompt_playground.py` - Main application
- `prompt_templates.json` - Saved prompt templates
- `requirements.txt` - Dependencies
- `README_playground.md` - Usage guide
- Sample output demonstrating all features

---

## Expected Output Section

### Task 1 Expected Output:
```
=== Prompt Comparison ===

Original: "Tell me about machine learning"
Improved: "Explain machine learning in 3 paragraphs, covering:
1. What it is
2. Common applications
3. Key algorithms"

Results:
- Original: Generic, unfocused response (156 tokens)
- Improved: Structured, comprehensive response (203 tokens)
- Quality: Improved version is 40% more informative
```

### Task 2 Expected Output:
```
=== Role-Based Prompts ===
Input: "How do I learn Python?"

Coding Tutor:
"Start with basics: variables, data types, and functions. 
Practice daily with small projects..."

Business Analyst:
"Python is valuable for data analysis. Focus on pandas 
and data visualization libraries..."

[Different perspectives based on role]
```

### Task 4 Expected Output:
```
=== Chain-of-Thought Solver ===
Problem: "If 3 apples cost $2, how much do 9 apples cost?"

Step 1: Identify what we need to find
→ Cost of 9 apples

Step 2: Find cost per apple
→ $2 ÷ 3 = $0.67 per apple

Step 3: Calculate cost of 9 apples
→ $0.67 × 9 = $6

Final Answer: 9 apples cost $6
```

### Task 5 Expected Output:
```python
builder = RAGPromptBuilder()
builder.add_context([
    "RAG combines retrieval and generation...",
    "Vector databases store embeddings..."
])
builder.set_question("How does RAG work?")
builder.set_format("bullet_points")
builder.enable_citations(True)

Generated Prompt:
"""
You are a helpful assistant...

Documents:
[Document 1]
RAG combines retrieval and generation...

[Document 2]
Vector databases store embeddings...

Question: How does RAG work?

Instructions:
- Answer using the documents above
- Cite your sources
- Format as bullet points
...
"""
```

### Mini Project Expected Output:

The playground should provide a comprehensive, user-friendly interface for experimenting with prompts:

- Intuitive menu navigation
- Real-time prompt testing
- Side-by-side comparisons
- Quality metrics and analysis
- Export capabilities
- Professional presentation

**Example session:**
```
=== Prompt Engineering Playground ===
1. Basic Prompt Tester
...
Choose: 6

=== Prompt Comparison Tool ===
Enter prompt 1: Explain AI simply
Enter prompt 2: You are a teacher. Explain AI to a 10-year-old.

[Testing both prompts...]

Results:
Prompt 1: 145 tokens, Generic explanation
Prompt 2: 167 tokens, Age-appropriate, engaging explanation

Winner: Prompt 2 (Better engagement, clearer structure)
```

---

## Submission Checklist

- [ ] Task 1: Prompt improvements completed and tested
- [ ] Task 2: Role-based system functional
- [ ] Task 3: Few-shot templates created
- [ ] Task 4: CoT solver working
- [ ] Task 5: RAG prompt builder implemented
- [ ] Mini project: Full playground application
- [ ] All prompts tested with API
- [ ] Results documented and compared
- [ ] Code is well-organized and commented

**Remember:** Good prompts are the foundation of great RAG systems!

**Good luck!** 🚀

