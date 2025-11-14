# Day 2 — Generative AI & LLM Basics

## 1. Beginner-Friendly Introduction

Welcome to the world of **Generative AI**! Today, you'll learn what Large Language Models (LLMs) are, how they work, and why they're the foundation of RAG systems.

**What is Generative AI?**
Generative AI refers to artificial intelligence systems that can create new content—text, images, code, etc.—based on patterns they've learned from training data. Unlike traditional AI that classifies or predicts, generative AI produces original outputs.

**Why this matters for RAG:**
- RAG systems use LLMs to generate answers
- Understanding LLMs helps you craft better prompts
- You'll interact with LLMs via APIs (like OpenAI)
- LLMs have limitations that RAG solves (hallucination, outdated knowledge)

**Real-world context:**
Think of an LLM as a very knowledgeable assistant who has read millions of books but can't remember specific sources. RAG gives this assistant access to a "library" (your documents) so it can provide accurate, sourced answers.

---

## 2. Deep-Dive Explanation

### 2.1 What are Large Language Models (LLMs)?

LLMs are neural networks trained on vast amounts of text data. They learn patterns, relationships, and language structure.

**Key characteristics:**
- **Large**: Billions of parameters (weights)
- **Language**: Understand and generate human language
- **Models**: Mathematical representations of language patterns

**How they work (simplified):**
```
Input Text → Neural Network → Probability Distribution → Generated Text
```

### 2.2 Popular LLMs

**OpenAI Models:**
- **GPT-3.5/GPT-4**: General-purpose, powerful
- **GPT-4 Turbo**: Faster, more efficient
- **Embedding models**: Convert text to vectors

**Other Models:**
- **Claude** (Anthropic): Strong reasoning
- **Llama** (Meta): Open-source alternative
- **Gemini** (Google): Multimodal capabilities

### 2.3 Understanding Tokens

LLMs process text in **tokens**, not words:
- 1 token ≈ 4 characters (roughly)
- "Hello world" = 2 tokens
- "RAG system" = 3 tokens

**Why it matters:**
- API pricing is often per token
- Models have token limits (context windows)
- You need to manage token usage efficiently

### 2.4 The OpenAI API

**Basic API Structure:**
```
Your Code → HTTP Request → OpenAI API → Response (JSON) → Your Code
```

**Key Components:**
- **API Key**: Authentication
- **Endpoint**: URL for the API
- **Model**: Which LLM to use (e.g., "gpt-4")
- **Messages**: Conversation format
- **Parameters**: Temperature, max_tokens, etc.

### 2.5 API Parameters Explained

**Temperature** (0-2):
- Lower (0-0.3): More deterministic, focused
- Higher (0.7-2): More creative, varied
- Default: 0.7

**Max Tokens:**
- Maximum length of the response
- Set based on your needs
- Be careful not to exceed model limits

**Top P** (0-1):
- Nucleus sampling
- Controls diversity
- Alternative to temperature

### 2.6 Model Capabilities and Limitations

**What LLMs are good at:**
- Understanding context
- Generating coherent text
- Following instructions
- Summarizing content
- Answering questions (if trained on the topic)

**What LLMs struggle with:**
- **Hallucination**: Making up facts
- **Outdated information**: Training data cutoff
- **Specific knowledge**: Not trained on your documents
- **Math/Logic**: Can make errors
- **Real-time data**: No access to current events

**This is why RAG exists!** RAG solves the "specific knowledge" and "outdated information" problems.

---

## 3. Instructor Examples

### Example 1: Basic OpenAI API Call

```python
import openai
import os

# Set your API key (use environment variable in production!)
openai.api_key = os.getenv("OPENAI_API_KEY")

def simple_chat(prompt):
    """Send a simple prompt to GPT-3.5"""
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "user", "content": prompt}
        ],
        temperature=0.7,
        max_tokens=150
    )
    
    return response.choices[0].message.content

# Usage
answer = simple_chat("What is RAG?")
print(answer)
```

### Example 2: Conversation with Context

```python
def chat_with_context(messages, user_message):
    """Maintain conversation context"""
    messages.append({"role": "user", "content": user_message})
    
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=messages,
        temperature=0.7
    )
    
    assistant_message = response.choices[0].message.content
    messages.append({"role": "assistant", "content": assistant_message})
    
    return assistant_message, messages

# Usage
conversation = []
response, conversation = chat_with_context(
    conversation, 
    "My name is Alice"
)
response, conversation = chat_with_context(
    conversation,
    "What's my name?"  # Model remembers context!
)
```

### Example 3: Using Different Models

```python
def compare_models(prompt):
    """Compare responses from different models"""
    models = ["gpt-3.5-turbo", "gpt-4"]
    results = {}
    
    for model in models:
        response = openai.ChatCompletion.create(
            model=model,
            messages=[{"role": "user", "content": prompt}],
            temperature=0.7,
            max_tokens=200
        )
        results[model] = response.choices[0].message.content
    
    return results

# Usage
prompt = "Explain quantum computing in simple terms"
results = compare_models(prompt)
for model, answer in results.items():
    print(f"\n{model}:\n{answer}")
```

### Example 4: Controlling Output with Parameters

```python
def generate_with_settings(prompt, temperature=0.7, max_tokens=100):
    """Generate text with custom parameters"""
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": prompt}],
        temperature=temperature,
        max_tokens=max_tokens,
        top_p=0.9
    )
    
    return {
        "content": response.choices[0].message.content,
        "tokens_used": response.usage.total_tokens,
        "model": response.model
    }

# Usage - Creative writing (high temperature)
creative = generate_with_settings(
    "Write a short story about a robot",
    temperature=1.2,
    max_tokens=200
)

# Usage - Factual answer (low temperature)
factual = generate_with_settings(
    "What is the capital of France?",
    temperature=0.2,
    max_tokens=50
)
```

---

## 4. Student Practice Tasks

### Task 1: Basic API Setup
Set up your OpenAI API key and make your first API call. Print the response and the number of tokens used.

### Task 2: Temperature Experiment
Send the same prompt to the API with different temperature values (0.1, 0.7, 1.5). Observe how the responses differ. What do you notice?

### Task 3: Token Counting
Write a function that estimates token count for a given text. Compare your estimate with the actual token count from the API response.

### Task 4: Conversation Memory
Create a simple chatbot that maintains conversation history. The bot should remember what was discussed earlier in the conversation.

### Task 5: Model Comparison
Compare responses from `gpt-3.5-turbo` and `gpt-4` for the same prompt. What differences do you observe in quality, detail, and token usage?

### Task 6: Error Handling
Write a robust API wrapper that handles:
- API key errors
- Rate limiting
- Network errors
- Invalid model names

---

## 5. Summary / Key Takeaways

- **LLMs** are neural networks trained on vast text data to understand and generate language
- **Tokens** are the units LLMs process (not words); manage them carefully
- **OpenAI API** provides access to powerful models via simple HTTP requests
- **Temperature** controls creativity (low = focused, high = creative)
- **Max tokens** limits response length
- **LLMs have limitations**: hallucination, outdated info, no access to your documents
- **RAG solves LLM limitations** by providing external knowledge
- **Context matters**: LLMs use conversation history to maintain coherence
- **Different models** have different capabilities and costs

---

## 6. Further Reading (Optional)

- OpenAI API Documentation: [platform.openai.com/docs](https://platform.openai.com/docs)
- "Attention Is All You Need" - The transformer paper (advanced)
- OpenAI Cookbook: Examples and best practices
- Token counting tools: tiktoken library

---

**Next up:** Day 3 will teach you how to craft effective prompts!

