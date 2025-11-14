# Day 3 — Prompt Engineering Essentials

## 1. Beginner-Friendly Introduction

**Prompt Engineering** is the art and science of crafting instructions that get the best results from LLMs. Think of it as learning to communicate effectively with AI—the better your prompts, the better the AI's responses.

**Why this matters for RAG:**
- RAG systems rely heavily on well-crafted prompts
- You'll need to prompt LLMs to answer questions based on retrieved context
- Effective prompts improve answer quality and reduce hallucinations
- Prompt engineering is a core skill for building production RAG applications

**Real-world context:**
Imagine asking a librarian a vague question vs. a specific one:
- ❌ "Tell me about space" (too vague)
- ✅ "What are the key differences between stars and planets? Explain in 3 bullet points." (specific and clear)

Prompt engineering is about being that specific librarian question—clear, structured, and goal-oriented.

---

## 2. Deep-Dive Explanation

### 2.1 What is a Prompt?

A **prompt** is the input text you send to an LLM. It can be:
- A simple question
- Instructions with examples
- A conversation history
- Structured templates

**Prompt Structure:**
```
[System Message] + [Context] + [User Question] + [Format Instructions]
```

### 2.2 Core Prompt Engineering Techniques

#### 2.2.1 Be Specific and Clear

**Bad:**
```
Tell me about Python.
```

**Good:**
```
Explain Python programming language in 3 sentences, focusing on:
1. What it's used for
2. Key features
3. Why it's popular for AI
```

#### 2.2.2 Use Role-Playing

Assign a role to the AI:
```
You are an expert Python tutor. Explain variables to a beginner programmer.
```

#### 2.2.3 Provide Examples (Few-Shot Learning)

Show the AI what you want:
```
Example 1:
Input: "Python is easy"
Output: "Python is beginner-friendly"

Example 2:
Input: "AI is powerful"
Output: "AI has transformative capabilities"

Now convert: "RAG is useful"
```

#### 2.2.4 Chain-of-Thought (CoT)

Encourage step-by-step reasoning:
```
Solve: 15 * 23

Let's think step by step:
1. First, multiply 15 by 20 = 300
2. Then, multiply 15 by 3 = 45
3. Finally, add 300 + 45 = 345
```

#### 2.2.5 Output Formatting

Specify the format you want:
```
List 5 programming languages. Format as JSON:
{
  "languages": [
    {"name": "...", "year": "..."}
  ]
}
```

### 2.3 Prompt Patterns for RAG

#### 2.3.1 Context Injection Pattern

```
Use the following context to answer the question:

Context:
{retrieved_documents}

Question: {user_question}

Answer based only on the provided context. If the context doesn't contain enough information, say "I don't have enough information."
```

#### 2.3.2 Answer with Citations

```
Based on the following documents, answer the question and cite your sources:

Documents:
{document_1}
{document_2}

Question: {question}

Format your answer as:
Answer: [your answer]
Sources: [document numbers]
```

#### 2.3.3 Multi-Step Reasoning

```
Given the context below, follow these steps:
1. Identify key information
2. Analyze the relationships
3. Synthesize an answer

Context: {context}
Question: {question}
```

### 2.4 Common Prompt Mistakes

**Mistake 1: Being Too Vague**
- ❌ "Explain this"
- ✅ "Summarize the main points in 3 bullet points"

**Mistake 2: Not Providing Context**
- ❌ Asking about specific documents without including them
- ✅ Including relevant context in the prompt

**Mistake 3: Ambiguous Instructions**
- ❌ "Make it better"
- ✅ "Rewrite this sentence to be more concise and professional"

**Mistake 4: Ignoring Token Limits**
- ❌ Including too much context
- ✅ Being selective about what context to include

### 2.5 Prompt Templates

**Template 1: Question Answering**
```
Context: {context}

Question: {question}

Instructions:
- Answer based only on the provided context
- If the answer isn't in the context, say so
- Be concise but complete
```

**Template 2: Summarization**
```
Summarize the following text in {number} sentences:

{text}

Focus on: {key_points}
```

**Template 3: Extraction**
```
Extract the following information from the text:
- Names
- Dates
- Key facts

Text: {text}

Format as JSON.
```

---

## 3. Instructor Examples

### Example 1: Basic Prompt with Context

```python
def answer_with_context(context, question):
    """Answer a question using provided context"""
    prompt = f"""Use the following information to answer the question.

Information:
{context}

Question: {question}

Answer the question based only on the information provided above. 
If the information doesn't contain the answer, say "I don't have enough information."
"""
    
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": prompt}],
        temperature=0.3  # Lower temperature for factual answers
    )
    
    return response.choices[0].message.content

# Usage
context = "Python was created by Guido van Rossum in 1991."
question = "Who created Python?"
answer = answer_with_context(context, question)
print(answer)
```

### Example 2: Few-Shot Learning

```python
def classify_sentiment_fewshot(text):
    """Classify sentiment using few-shot examples"""
    prompt = f"""Classify the sentiment of the following text as positive, negative, or neutral.

Examples:
Text: "I love this product!"
Sentiment: positive

Text: "This is terrible."
Sentiment: negative

Text: "The weather is okay."
Sentiment: neutral

Now classify:
Text: "{text}"
Sentiment:"""
    
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": prompt}],
        temperature=0.1  # Very low for classification
    )
    
    return response.choices[0].message.content.strip()

# Usage
result = classify_sentiment_fewshot("This movie was amazing!")
print(result)  # positive
```

### Example 3: Chain-of-Thought Prompting

```python
def solve_problem_cot(problem):
    """Solve a problem using chain-of-thought reasoning"""
    prompt = f"""Solve the following problem step by step.

Problem: {problem}

Let's think through this step by step:
1. First, identify what we need to find
2. List the information we have
3. Determine the approach
4. Solve step by step
5. Verify the answer

Solution:"""
    
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": prompt}],
        temperature=0.3,
        max_tokens=300
    )
    
    return response.choices[0].message.content

# Usage
problem = "If a train travels 120 km in 2 hours, what's its average speed?"
solution = solve_problem_cot(problem)
print(solution)
```

### Example 4: RAG-Style Prompt Template

```python
def rag_prompt_template(context_chunks, question):
    """Create a RAG-style prompt with multiple context chunks"""
    context_text = "\n\n".join([
        f"[Document {i+1}]\n{chunk}"
        for i, chunk in enumerate(context_chunks)
    ])
    
    prompt = f"""You are a helpful assistant that answers questions based on provided documents.

Documents:
{context_text}

Question: {question}

Instructions:
1. Answer the question using information from the documents above
2. If multiple documents are relevant, synthesize information from all of them
3. Cite which document(s) you used (e.g., "According to Document 1...")
4. If the documents don't contain enough information, say so clearly
5. Be specific and accurate

Answer:"""
    
    return prompt

# Usage
chunks = [
    "Python is a programming language created in 1991.",
    "RAG stands for Retrieval-Augmented Generation."
]
question = "What is Python?"
prompt = rag_prompt_template(chunks, question)
# Use this prompt with OpenAI API
```

---

## 4. Student Practice Tasks

### Task 1: Basic Prompt Improvement
Take these vague prompts and rewrite them to be specific and clear:
- "Tell me about AI"
- "Explain this code"
- "What should I do?"

### Task 2: Role-Playing Prompts
Create prompts that assign different roles to the AI:
- A coding tutor
- A business consultant
- A creative writer
- A data analyst

### Task 3: Few-Shot Examples
Create a few-shot prompt for:
- Classifying emails as spam/not spam
- Converting text to a specific format
- Extracting key information

### Task 4: Chain-of-Thought
Write a CoT prompt for:
- Solving a math word problem
- Debugging code
- Making a decision

### Task 5: RAG Prompt Template
Create a reusable RAG prompt template function that:
- Takes context and question
- Includes instructions
- Specifies output format
- Handles cases where context is insufficient

### Task 6: Prompt Comparison
Test the same question with:
- A basic prompt
- An improved prompt with context
- A prompt with examples
Compare the quality of responses.

---

## 5. Summary / Key Takeaways

- **Be specific**: Clear, detailed prompts get better results
- **Use roles**: Assigning roles helps guide AI behavior
- **Few-shot learning**: Examples teach the AI what you want
- **Chain-of-thought**: Encourages step-by-step reasoning
- **Format instructions**: Specify the output format you need
- **Context matters**: In RAG, always include relevant context
- **Temperature settings**: Lower for factual, higher for creative
- **Iterate**: Prompt engineering is iterative—refine based on results
- **Test variations**: Try different phrasings to find what works best

---

## 6. Further Reading (Optional)

- OpenAI Prompt Engineering Guide
- "Prompt Engineering for LLMs" by Lilian Weng
- LangChain Prompt Templates documentation
- Anthropic's Prompt Engineering resources

---

**Next up:** Day 4 will teach you how to extract and chunk data from various sources!

