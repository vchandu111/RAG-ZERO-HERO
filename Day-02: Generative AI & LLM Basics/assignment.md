# Day 2 — Assignment

## Instructions

Complete these tasks to get hands-on experience with LLMs and the OpenAI API. Make sure you have:
- An OpenAI API key (get one at platform.openai.com)
- Python `openai` library installed: `pip install openai`
- Your API key stored securely (use environment variables)

**Important:**
- Never commit your API key to version control
- Use environment variables or `.env` files
- Handle errors gracefully
- Test with different prompts and parameters

---

## Tasks

### Task 1: API Setup and First Call

Create a Python script that:
1. Loads your OpenAI API key from an environment variable
2. Makes a simple API call asking "What is Python?"
3. Prints the response
4. Displays the number of tokens used
5. Handles errors if the API key is missing or invalid

**Deliverable:** `task1_first_call.py`

---

### Task 2: Temperature Comparison Tool

Create a function that:
- Takes a prompt as input
- Sends the same prompt with 3 different temperature values: 0.1, 0.7, 1.5
- Collects all responses
- Returns a comparison showing how temperature affects output

Test with prompts like:
- "Write a haiku about coding"
- "Explain machine learning in one sentence"
- "Describe a futuristic city"

**Deliverable:** `task2_temperature_comparison.py`

---

### Task 3: Token Counter

Create a utility that:
1. Estimates token count for input text (rough estimate: 1 token ≈ 4 characters)
2. Makes an API call
3. Compares your estimate with the actual token count from the API response
4. Calculates the accuracy of your estimation

**Bonus:** Use the `tiktoken` library for more accurate token counting.

**Deliverable:** `task3_token_counter.py`

---

### Task 4: Simple Chatbot

Build a simple chatbot that:
- Maintains conversation history
- Allows multiple turns of conversation
- Remembers context from previous messages
- Has a command to clear history (type "clear" or "reset")
- Has a command to exit (type "quit" or "exit")

**Features:**
- Greet the user
- Show conversation history
- Handle empty inputs
- Display token usage after each response

**Deliverable:** `task4_chatbot.py`

---

### Task 5: Model Comparison Tool

Create a script that:
- Takes a prompt as input
- Sends it to both `gpt-3.5-turbo` and `gpt-4`
- Compares:
  - Response quality (subjective)
  - Response length
  - Token usage
  - Response time (if possible)
- Displays a side-by-side comparison

Test with prompts requiring:
- Simple factual answers
- Creative writing
- Complex reasoning
- Code generation

**Deliverable:** `task5_model_comparison.py`

---

## One Mini Project

### 🤖 Build an LLM Playground Application

Create a comprehensive Python application `llm_playground.py` that allows users to experiment with different LLM settings interactively.

**Features:**

1. **Interactive Menu:**
   ```
   === LLM Playground ===
   1. Single Prompt
   2. Conversation Mode
   3. Compare Models
   4. Parameter Tuning
   5. View History
   6. Export Results
   7. Exit
   ```

2. **Single Prompt Mode:**
   - Enter a prompt
   - Adjust temperature, max_tokens, model
   - View response
   - Save to history

3. **Conversation Mode:**
   - Multi-turn conversation
   - View full conversation history
   - Clear conversation option

4. **Compare Models:**
   - Enter a prompt
   - Automatically test with gpt-3.5-turbo and gpt-4
   - Show side-by-side comparison
   - Show cost comparison (if possible)

5. **Parameter Tuning:**
   - Test same prompt with different:
     - Temperature values (0.0 to 2.0)
     - Max tokens (50 to 500)
     - Top P values
   - Display all results for comparison

6. **View History:**
   - Show all previous prompts and responses
   - Filter by model
   - Show token usage statistics

7. **Export Results:**
   - Export conversation history to JSON
   - Export to text file
   - Include metadata (tokens, model, timestamp)

**Requirements:**
- Use classes to organize code
- Store conversation history in memory (or file)
- Implement proper error handling
- Add input validation
- Make it user-friendly with clear prompts
- Display token usage and estimated costs
- Use color coding for different types of output (optional)

**Example Interaction:**
```
=== LLM Playground ===
Choose an option: 1

Enter your prompt: Explain RAG in simple terms
Model (gpt-3.5-turbo/gpt-4) [gpt-3.5-turbo]: 
Temperature (0.0-2.0) [0.7]: 0.5
Max tokens [150]: 200

[Processing...]

Response:
RAG stands for Retrieval-Augmented Generation...

Tokens used: 45
Estimated cost: $0.0001

Save to history? (y/n): y
```

**Deliverables:**
- `llm_playground.py` - Main application
- `requirements.txt` - Dependencies
- `README_playground.md` - Brief usage instructions
- Sample output showing the application in action

---

## Expected Output Section

### Task 1 Expected Output:
```
API Key loaded successfully.
Making API call...

Response: Python is a high-level programming language...

Tokens used: 25
```

### Task 2 Expected Output:
```
=== Temperature Comparison ===
Prompt: "Write a haiku about coding"

Temperature 0.1:
Code flows like water,
Functions dance in harmony,
Logic finds its way.

Temperature 0.7:
Bits and bytes align,
Algorithms come alive,
Code becomes art form.

Temperature 1.5:
Electric dreams pulse,
Syntax sings in midnight glow,
Digital poetry blooms.

[Notice how creativity increases with temperature]
```

### Task 4 Expected Output:
```
=== Simple Chatbot ===
Hello! I'm your AI assistant. Type 'quit' to exit, 'clear' to reset.

You: Hello, my name is Bob
Assistant: Hello Bob! Nice to meet you. How can I help you today?

You: What's my name?
Assistant: Your name is Bob!

You: clear
[Conversation cleared]

You: What's my name?
Assistant: I don't have that information. Could you tell me your name?

You: quit
Goodbye!
```

### Mini Project Expected Output:

The playground should provide a smooth, interactive experience:
- Clear menu navigation
- Real-time responses
- Formatted output with proper spacing
- Error messages for invalid inputs
- History tracking and export functionality
- Professional-looking interface

**Example session:**
```
=== LLM Playground ===
1. Single Prompt
2. Conversation Mode
...
Choose: 1

Enter prompt: What is machine learning?
Model [gpt-3.5-turbo]: 
Temperature [0.7]: 
Max tokens [150]: 

Response:
Machine learning is a subset of artificial intelligence...

Tokens: 42 | Cost: $0.00008

[1] Try again
[2] Save to history
[3] Main menu
```

---

## Submission Checklist

- [ ] Task 1: API setup working
- [ ] Task 2: Temperature comparison functional
- [ ] Task 3: Token counter implemented
- [ ] Task 4: Chatbot maintains conversation
- [ ] Task 5: Model comparison working
- [ ] Mini project: Full playground application
- [ ] All code includes error handling
- [ ] API keys stored securely (not in code)
- [ ] Code is well-commented

**Remember:** Keep your API key secret! Never share it or commit it to version control.

**Good luck!** 🚀

