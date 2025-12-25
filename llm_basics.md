# How Large Language Models (LLMs) Generate Text

These notes summarize how Large Language Models (LLMs) generate text, based on my learning from the DeepLearning.AI RAG course and further exploration.

---

## High-Level Overview

A Large Language Model (LLM) is fundamentally a **next-token prediction system**.  
Given a sequence of tokens as input, it predicts the most probable next token and appends it to the sequence. This process is repeated iteratively until a complete response is formed.

LLMs do **not**:
- Look up words in a dictionary at runtime
- Search the internet by default
- Reason like humans

Instead, they rely on **statistical patterns learned during training**.

---

## Two Core Components of an LLM

### 1. Training Data
- LLMs are trained on massive amounts of text (books, articles, websites, code, etc.).
- During training, the model learns statistical relationships between tokens.
- The model does **not memorize exact sentences**, but learns generalizable patterns of language.

Example learned pattern:
> After “the sun is”, words like *shining*, *bright*, or *hot* are likely.

These patterns are stored implicitly in the model’s **parameters (weights)**.

---

### 2. Tokenizer and Vocabulary

Before training begins, every LLM is assigned a **tokenizer**.

The tokenizer:
- Splits text into **tokens** (sub-word units, not necessarily full words)
- Maps each token to a numeric ID
- Defines a **fixed vocabulary** (e.g., 20,000–100,000 tokens)

Important properties:
- The vocabulary is **fixed at training time**
- The model can only generate tokens from this vocabulary
- Different models have different tokenizers and vocabularies

Example:
