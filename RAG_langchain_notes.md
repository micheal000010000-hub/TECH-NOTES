# RAG & LangChain Notes: What I Learned

## 1. Embeddings

* **Embeddings** convert text into high-dimensional vectors that capture semantic meaning.
* Text must be **chunked** before passing into embedding models because models have a **token limit**.
* Example: `RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)`.
* **Vector store** stores these embeddings along with metadata (source, chunk number, etc.) for retrieval.
* OpenAI embeddings require an **API key**.
* Alternatives to OpenAI embeddings (no API key required): HuggingFace models like `sentence-transformers/all-MiniLM-L6-v2`.

## 2. Vector Store (Chroma)

* Stores **text chunks, embeddings, and metadata**.
* Logical structure:

```json
{
  "id": "uuid",
  "embedding": [0.012, -1.23, ...],
  "document": "text content",
  "metadata": {"source": "url", "chunk": 3}
}
```

* Retrieval process:

  1. Embed query
  2. Search vector index (e.g., HNSW) for nearest neighbors
  3. Return top document chunks

* `.as_retriever()` wraps vector store for easy query interface.

## 3. `format_docs()` and `.join()`

* `format_docs(docs)`:

```python
def format_docs(docs):
    return "\n\n".join(doc.page_content for doc in docs)
```

* `.join()`:

  * Takes an iterable of strings and concatenates them with the separator **between elements**.
  * Example: `"\n\n".join(["a", "b", "c"])` →

```
a

b

c
```

* Does **not** add separators at the start or end.
* Purpose: Flatten retrieved documents into readable text for the LLM.

## 4. RAG Chain

* A **pipeline** combining retrieval, prompt formatting, LLM, and post-processing.
* Structure:

```python
rag_chain = (
    {"context": retriever | format_docs, "question": RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)
```

### Step-by-step flow:

| Step            | Input              | Output                            |
| --------------- | ------------------ | --------------------------------- |
| Retriever       | question string    | list[Document]                    |
| format_docs     | list[Document]     | str                               |
| Dictionary      | question & context | {"context": str, "question": str} |
| PromptTemplate  | dict               | formatted string                  |
| LLM             | formatted string   | AIMessage                         |
| StrOutputParser | AIMessage          | str                               |

### Key points:

* `prompt` is a **PromptTemplate**: converts structured inputs (`context`, `question`) into a **single formatted string**.
* `StrOutputParser` extracts `.content` from the LLM response to get plain text.
* `.invoke()` runs the chain and passes outputs from one step to the next.

## 5. LLMs

* OpenAI LLMs: `ChatOpenAI(model_name="gpt-3.5-turbo")`
* Gemini LLMs: `ChatGoogleGenerativeAI(model="gemini-2.5-flash")`
* Switching LLM requires:

  * Changing the LLM class
  * Updating the model name
  * Setting API key (`OPENAI_API_KEY` for OpenAI, `GOOGLE_API_KEY` for Gemini)
* Example:

```python
from langchain_google_genai import ChatGoogleGenerativeAI

llm = ChatGoogleGenerativeAI(
    model="gemini-2.5-flash",
    temperature=0
)
```

* The rest of the chain remains unchanged.

## 6. Prompt & PromptTemplate

* A **PromptTemplate** is **not raw text**, it’s a callable that takes inputs and outputs **formatted text**.
* Example:

```python
prompt_text = prompt.format(context="retrieved docs", question="What is Task Decomposition?")
```

* Output is **a single string**, ready to feed into LLM.
* Does **not** return a dictionary; all placeholders are replaced in the text.

## 7. Retrieval-Augmented Generation (RAG) Concept

1. **Retrieve** relevant chunks from vector store using embeddings.
2. **Format** retrieved chunks into a single context string.
3. **Prompt** the LLM with context + user question.
4. **Parse** the output for clean final answer.

### Mental model

* Retrieval: structured knowledge
* Prompt: converts structure → natural language
* LLM: generates answer in language
* Parser: extracts clean string

## 8. Notes / Tips

* Always chunk documents before embedding.
* `format_docs` uses `\n\n` to separate paragraphs for LLM clarity.
* Output parser is crucial for getting usable strings from LLM responses.
* You can switch embeddings (OpenAI/HuggingFace) independently of the LLM.
* Gemini-Flash requires `ChatGoogleGenerativeAI` and Google API key.
* `.invoke()` triggers the full RAG pipeline.

---

*These notes summarize everything learned during the conversation about embeddings, vector stores, retrieval, RAG chains, prompt formatting, LLM integration, and output parsing.*
