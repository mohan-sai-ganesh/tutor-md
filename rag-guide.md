# 🚀 Complete RAG Guide — From Zero to Hero

> A beginner-friendly, in-depth guide to Retrieval-Augmented Generation (RAG)

---

## Table of Contents

1. [What is RAG?](#1-what-is-rag)
2. [Why Use RAG?](#2-why-use-rag)
3. [How RAG Works](#3-how-rag-works)
4. [Components of RAG](#4-components-of-rag)
5. [Types of RAG](#5-types-of-rag)
6. [Data Collection](#6-data-collection)
7. [Chunking](#7-chunking)
8. [Embedding Generation](#8-embedding-generation)
9. [Vector DB / Storage](#9-vector-db--storage)
10. [Query Embedding](#10-query-embedding)
11. [Retrieval](#11-retrieval)
12. [Prompt Construction](#12-prompt-construction)
13. [LLM Generation](#13-llm-generation)
14. [Evaluation](#14-evaluation)
15. [Complete RAG Project](#15-complete-rag-project)

---

## 1. What is RAG?

**RAG = Retrieval-Augmented Generation**

Imagine you're taking an open-book exam. Instead of memorizing everything, you:
1. **Look up** relevant pages in your textbook (Retrieval)
2. **Read** the relevant sections (Augmentation)
3. **Write** your answer using that information (Generation)

RAG does exactly this for AI. It's a technique that lets an LLM (like ChatGPT) **look up information** from your own documents before answering a question — instead of relying only on what it learned during training.

### Without RAG:
```
User: "What is our company's refund policy?"
LLM:  "I don't have information about your specific company's refund policy."
```

### With RAG:
```
User: "What is our company's refund policy?"
RAG System:
  1. Searches your company docs → Finds refund policy document
  2. Feeds that document to the LLM
  3. LLM reads it and answers accurately:
     "Your company offers a 30-day full refund for unused products..."
```

**In simple terms:** RAG = Search your data + Feed it to AI + Get accurate answers.

---

## 2. Why Use RAG?

### Problem: LLMs Have Limitations

| Problem | Explanation |
|---------|-------------|
| **Knowledge Cutoff** | ChatGPT was trained on data up to a certain date. It doesn't know what happened yesterday. |
| **No Private Data** | It has never seen your company documents, internal wikis, or personal files. |
| **Hallucinations** | When it doesn't know something, it sometimes makes up answers confidently. |
| **Expensive Fine-Tuning** | Retraining a model on your data costs thousands of dollars and takes days. |

### Solution: RAG Fixes All of These

| RAG Benefit | How |
|-------------|-----|
| **Always Up-to-Date** | Just update your documents — no retraining needed. |
| **Uses Your Data** | It searches YOUR files, databases, and documents. |
| **Reduces Hallucinations** | The LLM answers based on real retrieved text, not guesses. |
| **Cheap & Fast** | No GPU clusters needed — just a vector database and an API call. |

### Real-world Use Cases:
- **Customer support bots** that answer from your FAQ/docs
- **Legal assistants** that search through thousands of case files
- **Medical assistants** that reference latest research papers
- **Code assistants** that know your codebase

---

## 3. How RAG Works

Here's the full pipeline, step by step:

```
┌─────────────────────────────────────────────────────────┐
│                    RAG PIPELINE                         │
│                                                         │
│  INDEXING PHASE (One-time setup):                       │
│  ┌──────────┐  ┌──────────┐  ┌───────────┐  ┌───────┐ │
│  │ Collect  │→ │  Chunk   │→ │  Embed    │→ │ Store │ │
│  │  Data    │  │  Data    │  │  Chunks   │  │ in DB │ │
│  └──────────┘  └──────────┘  └───────────┘  └───────┘ │
│                                                         │
│  QUERY PHASE (Every user question):                     │
│  ┌──────────┐  ┌──────────┐  ┌───────────┐  ┌───────┐ │
│  │  User    │→ │  Embed   │→ │ Retrieve  │→ │ Build │ │
│  │  Query   │  │  Query   │  │ Top Chunks│  │Prompt │ │
│  └──────────┘  └──────────┘  └───────────┘  └───┬───┘ │
│                                                  │      │
│                              ┌───────────┐  ┌───▼───┐  │
│                              │  Return   │← │  LLM  │  │
│                              │  Answer   │  │Generate│  │
│                              └───────────┘  └───────┘  │
└─────────────────────────────────────────────────────────┘
```

### Step-by-Step:

1. **Collect Data** — Gather your documents (PDFs, text files, web pages, etc.)
2. **Chunk Data** — Break large documents into smaller pieces
3. **Embed Chunks** — Convert text chunks into numerical vectors (arrays of numbers)
4. **Store in Vector DB** — Save those vectors in a special database
5. **User Asks a Question** — "What is the refund policy?"
6. **Embed the Query** — Convert the question into a vector too
7. **Retrieve Top Matches** — Find the most similar chunks using vector math
8. **Build a Prompt** — Combine the retrieved text + the question into a prompt
9. **LLM Generates Answer** — The LLM reads the context and responds
10. **Return Answer** — Send the answer back to the user

---

## 4. Components of RAG

RAG has **6 core components**:

```
1. Data Source        → Where your knowledge comes from
2. Chunking Engine    → Breaks documents into pieces
3. Embedding Model    → Converts text → numbers (vectors)
4. Vector Database    → Stores and searches vectors efficiently
5. Retriever          → Finds the most relevant chunks
6. LLM (Generator)   → Produces the final answer
```

Think of it like a restaurant:
- **Data Source** = the ingredients (raw materials)
- **Chunking** = chopping vegetables into usable pieces
- **Embedding** = labeling each ingredient so you can find it
- **Vector DB** = the organized pantry
- **Retriever** = the chef finding the right ingredients
- **LLM** = the chef cooking the final dish

---

## 5. Types of RAG

### 5.1 Naive RAG (Basic)

The simplest form — the pipeline we described above.

```
Query → Retrieve → Generate → Answer
```

**Problem:** Sometimes retrieves irrelevant chunks or misses important ones.

### 5.2 Advanced RAG

Adds **pre-retrieval** and **post-retrieval** optimizations:

```
Query → [Query Rewriting] → Retrieve → [Re-Rank Results] → Generate → Answer
```

Improvements include:
- **Query Rewriting**: Rephrase the user's question for better search
- **Re-Ranking**: Score retrieved chunks by relevance before sending to LLM
- **Hybrid Search**: Combine keyword search + semantic search

### 5.3 Modular RAG

A flexible, plug-and-play architecture where each component can be swapped:

```
Query → [Router] → [Retriever A or B] → [Re-Ranker] → [Generator] → Answer
```

- Different retrievers for different question types
- Can add a **judge** module to check answer quality
- Can loop back if the answer isn't good enough (iterative retrieval)

### Comparison Table:

| Feature | Naive RAG | Advanced RAG | Modular RAG |
|---------|-----------|-------------|-------------|
| Complexity | Low | Medium | High |
| Quality | Basic | Good | Best |
| Query Rewriting | ❌ | ✅ | ✅ |
| Re-Ranking | ❌ | ✅ | ✅ |
| Iterative Retrieval | ❌ | ❌ | ✅ |
| Custom Routing | ❌ | ❌ | ✅ |

---

## 6. Data Collection

This is the **foundation** of RAG. Garbage in = garbage out.

### 6.1 Sources

Your data can come from anywhere:

```python
# Common data sources
sources = {
    "documents": ["PDF", "Word", "Text files", "Markdown"],
    "web":       ["Web pages", "APIs", "RSS feeds"],
    "databases": ["SQL databases", "NoSQL databases"],
    "media":     ["Transcribed audio", "OCR from images"],
    "apps":      ["Slack messages", "Emails", "Notion pages"]
}
```

### 6.2 Formats

| Format | Example | How to Load |
|--------|---------|-------------|
| PDF | Research papers | `PyPDFLoader` |
| TXT | Plain text files | `open("file.txt")` |
| CSV | Spreadsheets | `CSVLoader` |
| JSON | API responses | `json.load()` |
| HTML | Web pages | `BeautifulSoup` |
| DOCX | Word documents | `Docx2txtLoader` |

### 6.3 Structured vs Unstructured Data

```
STRUCTURED DATA (organized, predictable):
┌────────────┬───────┬────────┐
│ Name       │ Age   │ City   │
├────────────┼───────┼────────┤
│ Alice      │ 30    │ Delhi  │
│ Bob        │ 25    │ Mumbai │
└────────────┴───────┴────────┘
→ Think: SQL tables, CSV files, JSON with fixed keys

UNSTRUCTURED DATA (messy, varied):
"The company was founded in 2010 by Alice in Delhi.
 She later moved operations to Mumbai where Bob
 joined as CTO..."
→ Think: PDFs, emails, meeting notes, web pages
```

**RAG mostly deals with unstructured data** — that's where its power lies.

### 6.4 Data Cleaning & Noise Removal

Raw data is messy. You must clean it before processing.

```python
import re

def clean_text(text: str) -> str:
    """Clean raw text for RAG processing."""

    # 1. Remove extra whitespace
    text = re.sub(r'\s+', ' ', text)

    # 2. Remove special characters (keep basic punctuation)
    text = re.sub(r'[^\w\s.,!?;:\-\'\"()]', '', text)

    # 3. Remove page numbers like "Page 1 of 10"
    text = re.sub(r'Page \d+ of \d+', '', text)

    # 4. Remove headers/footers that repeat
    text = re.sub(r'CONFIDENTIAL', '', text, flags=re.IGNORECASE)

    # 5. Normalize unicode characters
    text = text.encode('ascii', 'ignore').decode('ascii')

    # 6. Strip leading/trailing whitespace
    text = text.strip()

    return text

# Example:
raw = "  Page 1 of 10  \n\n  Hello   World!!!  \n CONFIDENTIAL  "
clean = clean_text(raw)
print(clean)  # "Hello World!!!"
```

### Why Cleaning Matters:
```
DIRTY DATA:
"Page 3 of 50 | CONFIDENTIAL | © 2024 Acme Corp
 T h e  r e f u n d  p o l i c y  states..."
→ LLM gets confused by noise, gives bad answers.

CLEAN DATA:
"The refund policy states customers can return products
 within 30 days for a full refund."
→ LLM understands clearly, gives accurate answers.
```

---

## 7. Chunking

### 7.1 What is Chunking & Why?

Imagine you have a 500-page book. You can't feed the entire book to an LLM — it has a context window limit (e.g., 4096 tokens). And even if you could, the LLM would struggle to find the relevant part.

**Chunking** = Breaking a large document into smaller, digestible pieces.

```
BEFORE CHUNKING:
┌──────────────────────────────────────────────┐
│ 500-page document (too large for LLM)        │
└──────────────────────────────────────────────┘

AFTER CHUNKING:
┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐
│Chunk 1 │ │Chunk 2 │ │Chunk 3 │ │Chunk N │
│ 500    │ │ 500    │ │ 500    │ │ ...    │
│ chars  │ │ chars  │ │ chars  │ │        │
└────────┘ └────────┘ └────────┘ └────────┘
```

### 7.2 Chunk Size

This is one of the most important decisions in RAG.

```python
# Common chunk sizes
small_chunk = 256   # tokens (~200 words) — very specific
medium_chunk = 512  # tokens (~400 words) — balanced ✅ RECOMMENDED
large_chunk = 1024  # tokens (~800 words) — more context
```

### 7.3 Small Chunks vs Large Chunks

| | Small Chunks (256 tokens) | Large Chunks (1024 tokens) |
|---|---|---|
| **Precision** | ✅ Very specific matches | ❌ May include irrelevant text |
| **Context** | ❌ May miss surrounding info | ✅ Keeps full context |
| **Retrieval Speed** | ✅ Faster to search | ❌ Slower |
| **Number of Chunks** | Many (more storage) | Few (less storage) |
| **Best For** | FAQ, specific facts | Long explanations, stories |

### 7.4 Fixed vs Dynamic Chunking

```python
# ── FIXED-SIZE CHUNKING ──
# Splits every N characters regardless of content.
# Simple but can cut sentences in half.

def fixed_chunk(text: str, chunk_size: int = 500) -> list[str]:
    """Split text into fixed-size chunks."""
    return [text[i:i + chunk_size] for i in range(0, len(text), chunk_size)]

text = "Alice went to the market. She bought apples. Then she came home."
chunks = fixed_chunk(text, chunk_size=30)
print(chunks)
# ['Alice went to the market. She',   ← Sentence cut in half!
#  ' bought apples. Then she came',
#  ' home.']


# ── DYNAMIC / RECURSIVE CHUNKING ──
# Respects sentence/paragraph boundaries. Much better!

from langchain.text_splitter import RecursiveCharacterTextSplitter

splitter = RecursiveCharacterTextSplitter(
    chunk_size=500,        # Max characters per chunk
    chunk_overlap=50,      # Overlap between chunks (explained below)
    separators=["\n\n", "\n", ". ", " ", ""]  # Try these in order
)

text = """Chapter 1: Introduction
This is the introduction to our guide. It covers the basics.

Chapter 2: Advanced Topics
This chapter dives deep into advanced concepts."""

chunks = splitter.split_text(text)
for i, chunk in enumerate(chunks):
    print(f"Chunk {i}: {chunk}\n")
```

### 7.5 Chunk Overlapping

Without overlap, you can lose context at chunk boundaries.

```
WITHOUT OVERLAP:
Chunk 1: "...the refund policy allows returns within"
Chunk 2: "30 days of purchase. Items must be unused."
→ If user asks "how many days for refund?", Chunk 2 alone
  lacks context about "refund policy".

WITH OVERLAP (50 characters):
Chunk 1: "...the refund policy allows returns within"
Chunk 2: "policy allows returns within 30 days of purchase. Items must be unused."
→ Now Chunk 2 has the full sentence! Better retrieval.
```

```python
from langchain.text_splitter import RecursiveCharacterTextSplitter

# Overlap = 50 characters shared between consecutive chunks
splitter = RecursiveCharacterTextSplitter(
    chunk_size=200,
    chunk_overlap=50  # ← This is the overlap
)

text = "The refund policy allows returns within 30 days of purchase. Items must be unused and in original packaging. Contact support at help@example.com for returns."

chunks = splitter.split_text(text)
for i, chunk in enumerate(chunks):
    print(f"Chunk {i}: '{chunk}'\n")
```

**Rule of thumb:** Set overlap to **10-20% of chunk size**.

---

## 8. Embedding Generation

### 8.1 What Are Embeddings?

An **embedding** is a list of numbers (a vector) that represents the **meaning** of text.

```
"I love dogs" → [0.12, -0.45, 0.78, 0.33, ..., 0.56]  (1536 numbers)
"I adore puppies" → [0.13, -0.44, 0.77, 0.34, ..., 0.55]  (very similar!)
"The stock market crashed" → [-0.89, 0.23, -0.11, ..., 0.02]  (very different)
```

**Key insight:** Similar meanings → similar numbers. This is how RAG finds relevant chunks!

### 8.2 Embedding vs Token

```
TOKENS: Breaking text into pieces for the model to process.
"I love dogs" → ["I", " love", " dogs"]  (3 tokens)
→ Tokens are the INPUT UNITS of a model.

EMBEDDINGS: Converting text into a meaningful numerical vector.
"I love dogs" → [0.12, -0.45, 0.78, ...]  (a single vector)
→ Embeddings capture the MEANING of the entire text.
```

Think of it this way:
- **Tokens** = individual Lego bricks
- **Embeddings** = a photograph of the assembled Lego model (captures the whole picture)

### 8.3 Popular Embedding Models

| Model | Provider | Dimensions | Max Tokens | Quality | Cost |
|-------|----------|-----------|------------|---------|------|
| `text-embedding-3-small` | OpenAI | 1536 | 8191 | Good | $0.02/1M tokens |
| `text-embedding-3-large` | OpenAI | 3072 | 8191 | Better | $0.13/1M tokens |
| `all-MiniLM-L6-v2` | HuggingFace | 384 | 256 | Decent | Free |
| `all-mpnet-base-v2` | HuggingFace | 768 | 512 | Good | Free |
| `voyage-3` | Voyage AI | 1024 | 32000 | Excellent | Paid |

### 8.4 Dimension Size

```
Dimensions = How many numbers in the vector.

Low Dimensions (384):
[0.12, -0.45, 0.78, ..., 0.33]  ← 384 numbers
✅ Faster, less storage
❌ Less precise meaning capture

High Dimensions (3072):
[0.12, -0.45, 0.78, ..., 0.56, 0.89, -0.23, ...]  ← 3072 numbers
✅ More precise meaning capture
❌ Slower, more storage
```

**Analogy:** Describing a person with 5 words vs 50 words — more dimensions = more detail.

### 8.5 Max Tokens Your Model Can Embed

Each embedding model has a limit on input size:

```python
# If your chunk exceeds the model's max tokens, it gets TRUNCATED.
# This means the end of your chunk is silently ignored!

# WRONG: Chunk is too big for the model
chunk = "..." * 10000  # 10,000 tokens
# Model with 512 max tokens only embeds the first 512 → meaning is lost!

# RIGHT: Ensure chunks fit within the model's limit
from langchain.text_splitter import RecursiveCharacterTextSplitter
splitter = RecursiveCharacterTextSplitter(
    chunk_size=400,  # Stay under the model's max token limit
    chunk_overlap=50
)
```

### 8.6 Code: Generating Embeddings

```python
# ── Option 1: OpenAI Embeddings (paid, high quality) ──
from openai import OpenAI
client = OpenAI(api_key="your-key")

def get_openai_embedding(text: str) -> list[float]:
    """Get embedding vector from OpenAI."""
    response = client.embeddings.create(
        model="text-embedding-3-small",
        input=text
    )
    return response.data[0].embedding  # List of 1536 floats

embedding = get_openai_embedding("What is the refund policy?")
print(f"Dimensions: {len(embedding)}")  # 1536
print(f"First 5 values: {embedding[:5]}")
# [0.0123, -0.0456, 0.0789, 0.0234, -0.0567]


# ── Option 2: HuggingFace (free, runs locally) ──
from sentence_transformers import SentenceTransformer

model = SentenceTransformer("all-MiniLM-L6-v2")

texts = ["What is the refund policy?", "How do I return a product?"]
embeddings = model.encode(texts)

print(f"Shape: {embeddings.shape}")  # (2, 384) → 2 texts, 384 dimensions each
```

---

## 9. Vector DB / Storage

### 9.1 What is a Vector Database?

A **vector database** is a special database designed to store and search **vectors** (embeddings) efficiently.

```
NORMAL DATABASE (SQL):
┌────┬──────────┬──────┐
│ ID │ Name     │ Age  │     → Search by exact match: WHERE name = 'Alice'
├────┼──────────┼──────┤
│ 1  │ Alice    │ 30   │
│ 2  │ Bob      │ 25   │
└────┴──────────┴──────┘

VECTOR DATABASE:
┌────┬──────────────────────────┬─────────────────────┐
│ ID │ Text                     │ Vector               │
├────┼──────────────────────────┼─────────────────────┤
│ 1  │ "Refund policy is..."    │ [0.12, -0.45, ...]  │  → Search by SIMILARITY:
│ 2  │ "Returns accepted..."    │ [0.13, -0.44, ...]  │    "Find vectors closest
│ 3  │ "Our CEO is..."          │ [-0.89, 0.23, ...]  │     to my query vector"
└────┴──────────────────────────┴─────────────────────┘
```

### 9.2 Vector DB vs Normal DB

| Feature | Normal DB (PostgreSQL, MongoDB) | Vector DB (ChromaDB, Pinecone) |
|---------|-------------------------------|-------------------------------|
| **Search Type** | Exact match, range queries | Similarity/nearest neighbor |
| **Data Stored** | Strings, numbers, JSON | High-dimensional vectors |
| **Query** | `WHERE name = 'Alice'` | "Find 5 most similar vectors" |
| **Use Case** | User accounts, orders | Semantic search, RAG |
| **Indexing** | B-tree, Hash | HNSW, IVF, Annoy |

### 9.3 SQL vs NoSQL vs Vector DB

```
SQL (PostgreSQL, MySQL):
  → Structured data, tables, relationships
  → "Find all orders where amount > 100"

NoSQL (MongoDB, DynamoDB):
  → Flexible schemas, JSON documents
  → "Find all products in category 'electronics'"

Vector DB (ChromaDB, Pinecone, Weaviate):
  → Vector similarity search
  → "Find text chunks most similar to this question"
```

### 9.4 Why Indexing?

Without indexing, searching means comparing your query against **every single vector** — too slow for millions of documents.

```
WITHOUT INDEX (Brute Force):
Query vector compared with ALL vectors: O(n)
1 million vectors × 1536 dimensions = SLOW 🐌

WITH INDEX (HNSW, IVF):
Query vector compared with a small subset: O(log n)
Same 1 million vectors = FAST ⚡
```

Common index types:
- **HNSW** (Hierarchical Navigable Small World): Best for accuracy, used by most vector DBs
- **IVF** (Inverted File Index): Groups vectors into clusters, searches the nearest cluster
- **Flat**: No index, brute-force search — only for small datasets

### 9.5 Popular Vector Databases

| Database | Type | Free Tier | Best For |
|----------|------|-----------|----------|
| **ChromaDB** | Local / embedded | ✅ Fully free | Prototyping, learning |
| **FAISS** | Library (Meta) | ✅ Fully free | High-performance local search |
| **Pinecone** | Cloud managed | ✅ Free tier | Production apps |
| **Weaviate** | Cloud / self-hosted | ✅ Free tier | Enterprise RAG |
| **Qdrant** | Cloud / self-hosted | ✅ Free tier | Advanced filtering |
| **pgvector** | PostgreSQL extension | ✅ Free | If you already use PostgreSQL |

### 9.6 Code: Storing Vectors in ChromaDB

```python
# pip install chromadb

import chromadb

# 1. Create a client (stores data locally in ./chroma_db)
client = chromadb.PersistentClient(path="./chroma_db")

# 2. Create a collection (like a table)
collection = client.get_or_create_collection(
    name="my_documents",
    metadata={"hnsw:space": "cosine"}  # Use cosine similarity
)

# 3. Add documents (ChromaDB auto-embeds with its default model)
collection.add(
    documents=[
        "The refund policy allows returns within 30 days.",
        "Products must be unused and in original packaging.",
        "Contact support@example.com for help.",
        "Our CEO is Alice and she founded the company in 2010."
    ],
    ids=["doc1", "doc2", "doc3", "doc4"]  # Unique IDs
)

# 4. Query the collection
results = collection.query(
    query_texts=["How can I return a product?"],
    n_results=2  # Get top 2 matches
)

print(results["documents"])
# [['The refund policy allows returns within 30 days.',
#   'Products must be unused and in original packaging.']]
```

---

## 10. Query Embedding

### 10.1 What Model to Use?

When the user asks a question, we convert it to an embedding using the **same model** we used for document chunks.

### 10.2 Why the SAME Model?

```
SAME MODEL (Correct ✅):
Document embedding (OpenAI): "Refund policy" → [0.12, -0.45, 0.78, ...]
Query embedding (OpenAI):    "Return items"  → [0.13, -0.44, 0.77, ...]
→ Similar meanings = similar vectors → FOUND! ✅

DIFFERENT MODELS (Wrong ❌):
Document embedding (OpenAI):     "Refund policy" → [0.12, -0.45, 0.78, ...]
Query embedding (HuggingFace):   "Return items"  → [0.89, 0.23, -0.11, ...]
→ Different models produce INCOMPATIBLE vectors → NOT FOUND ❌
```

**Rule:** ALWAYS use the same embedding model for both documents and queries.

```python
from openai import OpenAI
client = OpenAI(api_key="your-key")

# SAME function used for both document chunks AND user queries
def embed(text: str) -> list[float]:
    response = client.embeddings.create(
        model="text-embedding-3-small",  # Must be the same model!
        input=text
    )
    return response.data[0].embedding

# During indexing:
doc_vector = embed("Refund policy allows returns within 30 days.")

# During querying:
query_vector = embed("How do I return a product?")
```

---

## 11. Retrieval

### 11.1 How Does Retrieval Work?

Once we have the query vector, we search the vector DB for the most similar document chunks.

```
Query: "How do I return a product?"
Query Vector: [0.13, -0.44, 0.77, ...]

Vector DB Search:
┌──────────────────────────┬──────────┐
│ Chunk                    │ Score    │
├──────────────────────────┼──────────┤
│ "Refund policy: 30 days" │ 0.95 ← Most similar
│ "Must be unused..."      │ 0.89
│ "Contact support..."     │ 0.72
│ "CEO is Alice..."        │ 0.15 ← Least similar
└──────────────────────────┴──────────┘

Return Top 2 → ["Refund policy: 30 days", "Must be unused..."]
```

### 11.2 Retrieval Methods

#### Similarity Search (Vector/Semantic Search)

Uses **vector distance** to find semantically similar text.

```python
# The query "How to return items?" finds "Refund policy allows returns..."
# even though they share almost no exact words!
# This is the POWER of semantic search.

results = collection.query(
    query_texts=["How to return items?"],
    n_results=3
)
```

#### Keyword Search (Traditional)

Uses **exact word matching** (like Google before AI).

```python
# BM25 is the most popular keyword search algorithm
from rank_bm25 import BM25Okapi

corpus = [
    "The refund policy allows returns within 30 days",
    "Products must be unused and in original packaging",
    "Our CEO Alice founded the company in 2010"
]

tokenized = [doc.lower().split() for doc in corpus]
bm25 = BM25Okapi(tokenized)

query = "refund policy"
scores = bm25.get_scores(query.lower().split())
print(scores)  # [high_score, low_score, low_score]
```

#### Hybrid Search (Best of Both Worlds) ✅ RECOMMENDED

Combines **semantic search** + **keyword search** for better results.

```
Query: "What is policy ABC-123?"

Semantic Search finds: "The return policy states..."  (understands meaning)
Keyword Search finds:  "Policy ABC-123 was created..." (matches exact keyword)

Hybrid combines both → Better coverage!
```

```python
# Hybrid search with weighted scores
def hybrid_search(query, chunks, alpha=0.5):
    """
    alpha=1.0 → 100% semantic search
    alpha=0.0 → 100% keyword search
    alpha=0.5 → 50/50 balanced
    """
    semantic_scores = get_semantic_scores(query, chunks)
    keyword_scores = get_keyword_scores(query, chunks)

    hybrid_scores = [
        alpha * sem + (1 - alpha) * kw
        for sem, kw in zip(semantic_scores, keyword_scores)
    ]
    return hybrid_scores
```

### 11.3 Top K

**Top K** = How many chunks to retrieve.

```python
# Top K = 3 → Return the 3 most similar chunks
results = collection.query(
    query_texts=["How to return items?"],
    n_results=3  # ← This is Top K
)
```

| Top K | Pros | Cons |
|-------|------|------|
| K=1 | Fast, focused | May miss important context |
| K=3 | Good balance ✅ | — |
| K=5 | More context | May include irrelevant chunks |
| K=10 | Maximum context | Slow, dilutes relevance, may exceed LLM context |

### 11.4 Distance Metrics

How do we measure "similarity" between two vectors?

```python
import numpy as np

a = np.array([1, 2, 3])
b = np.array([1, 2, 4])

# 1. COSINE SIMILARITY — Most popular for text ✅
# Measures the angle between vectors (ignores magnitude).
# 1.0 = identical, 0.0 = unrelated, -1.0 = opposite
cosine_sim = np.dot(a, b) / (np.linalg.norm(a) * np.linalg.norm(b))
print(f"Cosine Similarity: {cosine_sim:.4f}")  # 0.9914

# 2. EUCLIDEAN DISTANCE (L2) — Measures straight-line distance
# 0 = identical, larger = more different
euclidean = np.linalg.norm(a - b)
print(f"Euclidean Distance: {euclidean:.4f}")  # 1.0000

# 3. DOT PRODUCT — Measures both direction AND magnitude
dot = np.dot(a, b)
print(f"Dot Product: {dot}")  # 17
```

**When to use what:**

| Metric | Best For | Range |
|--------|----------|-------|
| **Cosine Similarity** | Text search (most common) | -1 to 1 |
| **Euclidean (L2)** | When magnitude matters | 0 to ∞ |
| **Dot Product** | Normalized vectors, speed | -∞ to ∞ |

---

## 12. Prompt Construction

After retrieval, we **build a prompt** that combines the retrieved chunks with the user's question.

### 12.1 Prompt Template

```python
PROMPT_TEMPLATE = """You are a helpful assistant. Answer the user's question
based ONLY on the following context. If the context doesn't contain the answer,
say "I don't have enough information to answer that."

Context:
{context}

Question: {question}

Answer:"""
```

### 12.2 Context Injection

```python
def build_prompt(query: str, retrieved_chunks: list[str]) -> str:
    """Inject retrieved chunks into the prompt template."""

    # Join all chunks with separators
    context = "\n\n---\n\n".join(retrieved_chunks)

    # Fill the template
    prompt = PROMPT_TEMPLATE.format(
        context=context,
        question=query
    )
    return prompt

# Example:
chunks = [
    "The refund policy allows returns within 30 days of purchase.",
    "Products must be unused and in their original packaging."
]

prompt = build_prompt("What is the return policy?", chunks)
print(prompt)
```

**Output:**
```
You are a helpful assistant. Answer the user's question
based ONLY on the following context. If the context doesn't contain the answer,
say "I don't have enough information to answer that."

Context:
The refund policy allows returns within 30 days of purchase.

---

Products must be unused and in their original packaging.

Question: What is the return policy?

Answer:
```

### 12.3 How Many Chunks to Include?

This depends on your LLM's context window:

| LLM | Context Window | Recommended Chunks |
|-----|---------------|-------------------|
| GPT-3.5 | 4K tokens | 2-3 chunks |
| GPT-4 | 8K-128K tokens | 3-10 chunks |
| Claude 3.5 | 200K tokens | 5-20 chunks |
| Llama 3 | 8K tokens | 2-4 chunks |

**Rule:** Leave room for the system prompt + question + the generated answer.

### 12.4 System Prompt

The **system prompt** sets the LLM's behavior:

```python
system_prompt = """You are an expert customer support assistant for Acme Corp.

Rules:
1. Only answer based on the provided context.
2. If you don't know, say "I'm not sure, let me connect you with a human agent."
3. Be concise and friendly.
4. If the user asks about pricing, always mention the current promotion.
5. Never make up information.
"""
```

---

## 13. LLM Generation

### 13.1 The Generation Step

This is where the LLM reads the prompt (with context) and generates an answer.

```python
from openai import OpenAI
client = OpenAI(api_key="your-key")

def generate_answer(prompt: str, system_prompt: str) -> str:
    """Send prompt to LLM and get an answer."""

    response = client.chat.completions.create(
        model="gpt-4o-mini",          # The LLM model
        messages=[
            {"role": "system", "content": system_prompt},
            {"role": "user", "content": prompt}
        ],
        temperature=0.3,              # Low = more focused
        max_tokens=500                # Max output length
    )
    return response.choices[0].message.content
```

### 13.2 Key Parameters

#### Temperature

Controls **randomness/creativity** of the output.

```python
# temperature=0.0 → Deterministic. Same input = same output. Best for facts.
# temperature=0.3 → Slightly creative. Good for RAG. ✅ RECOMMENDED
# temperature=0.7 → Creative. Good for writing, brainstorming.
# temperature=1.0 → Very creative / unpredictable. Not for RAG.

response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[...],
    temperature=0.3  # Low for factual RAG answers
)
```

#### Max Tokens

Controls the **maximum length** of the generated response.

```python
# max_tokens=100   → Short answers (~75 words)
# max_tokens=500   → Medium answers (~375 words) ✅
# max_tokens=2000  → Long, detailed answers
# max_tokens=4096  → Maximum for most models

response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[...],
    max_tokens=500
)
```

### 13.3 Streaming

Instead of waiting for the full response, you can **stream** tokens as they're generated:

```python
# Without streaming: Wait 5 seconds... then see full answer at once.
# With streaming: See words appear one by one (like ChatGPT's typing effect).

stream = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[
        {"role": "system", "content": "You are a helpful assistant."},
        {"role": "user", "content": prompt}
    ],
    stream=True  # ← Enable streaming
)

# Print tokens as they arrive
for chunk in stream:
    token = chunk.choices[0].delta.content
    if token:
        print(token, end="", flush=True)
```

### 13.4 Hallucination

**Hallucination** = When the LLM makes up information that isn't in the context.

```
CONTEXT: "Refund policy allows returns within 30 days."

QUESTION: "Can I return after 60 days?"

HALLUCINATED ANSWER ❌: "Yes, we offer a special 60-day extended return window
for premium members."  ← MADE UP! Not in context!

CORRECT ANSWER ✅: "Based on the information available, the refund policy only
allows returns within 30 days. I don't have information about exceptions."
```

**How to reduce hallucinations:**
1. Use low temperature (0.0 - 0.3)
2. Add "Only answer from the given context" in the system prompt
3. Add "If you don't know, say so" instruction
4. Validate the answer against the retrieved chunks

### 13.5 Retrieval Failure

Sometimes retrieval fails — no relevant chunks are found.

```python
def rag_answer(query: str, collection) -> str:
    """Complete RAG flow with failure handling."""

    # Step 1: Retrieve
    results = collection.query(query_texts=[query], n_results=3)
    chunks = results["documents"][0]
    distances = results["distances"][0]

    # Step 2: Check retrieval quality
    RELEVANCE_THRESHOLD = 0.7  # Cosine similarity threshold

    # If best match is below threshold, retrieval failed
    if not chunks or distances[0] > RELEVANCE_THRESHOLD:
        return ("I couldn't find relevant information in the knowledge base. "
                "Please try rephrasing your question or contact support.")

    # Step 3: Build prompt and generate
    prompt = build_prompt(query, chunks)
    answer = generate_answer(prompt, system_prompt)
    return answer
```

---

## 14. Evaluation

### 14.1 Why Evaluate?

You built a RAG system — but how do you know if it's actually good? Evaluation measures:
- Is the retriever finding the right chunks?
- Is the LLM generating correct answers?
- Is the system hallucinating?

### 14.2 Key Metrics

| Metric | What It Measures | Good Score |
|--------|-----------------|------------|
| **Faithfulness** | Is the answer supported by the context? | > 0.8 |
| **Answer Relevancy** | Does the answer address the question? | > 0.8 |
| **Context Precision** | Are the retrieved chunks relevant? | > 0.7 |
| **Context Recall** | Did we retrieve ALL the relevant chunks? | > 0.7 |

### 14.3 Evaluation Tools

#### RAGAS (Most Popular)

```python
# pip install ragas

from ragas import evaluate
from ragas.metrics import faithfulness, answer_relevancy, context_precision
from datasets import Dataset

# Prepare evaluation data
eval_data = {
    "question": [
        "What is the refund policy?",
        "Who is the CEO?"
    ],
    "answer": [
        "You can return products within 30 days for a full refund.",
        "The CEO is Alice, who founded the company in 2010."
    ],
    "contexts": [
        ["Refund policy allows returns within 30 days of purchase."],
        ["Our CEO is Alice and she founded the company in 2010."]
    ],
    "ground_truth": [
        "Customers can return products within 30 days.",
        "Alice is the CEO."
    ]
}

dataset = Dataset.from_dict(eval_data)

# Run evaluation
results = evaluate(
    dataset,
    metrics=[faithfulness, answer_relevancy, context_precision]
)

print(results)
# {'faithfulness': 0.95, 'answer_relevancy': 0.92, 'context_precision': 0.88}
```

#### Manual Evaluation

```python
# Simple accuracy check
test_cases = [
    {
        "question": "What is the refund policy?",
        "expected_keywords": ["30 days", "return", "refund"],
    },
    {
        "question": "Who is the CEO?",
        "expected_keywords": ["Alice"],
    }
]

def evaluate_rag(rag_function, test_cases):
    """Simple keyword-based evaluation."""
    results = []
    for case in test_cases:
        answer = rag_function(case["question"])
        # Check if expected keywords appear in the answer
        hits = sum(
            1 for kw in case["expected_keywords"]
            if kw.lower() in answer.lower()
        )
        score = hits / len(case["expected_keywords"])
        results.append({
            "question": case["question"],
            "answer": answer,
            "score": score
        })
        print(f"Q: {case['question']}")
        print(f"A: {answer}")
        print(f"Score: {score:.0%}\n")
    
    avg = sum(r["score"] for r in results) / len(results)
    print(f"Overall Accuracy: {avg:.0%}")
    return results
```

#### Other Evaluation Tools

| Tool | Description |
|------|-------------|
| **RAGAS** | Framework with built-in RAG metrics |
| **LangSmith** | LangChain's tracing and evaluation platform |
| **DeepEval** | Unit testing framework for LLMs |
| **TruLens** | Tracks and evaluates LLM apps |
| **Phoenix (Arize)** | Observability for LLM applications |

---

## 15. Complete RAG Project

Now let's put everything together into a **complete, working RAG system**.

### Project: "DocChat" — Chat with Your Documents

```
Architecture:
┌──────────────┐     ┌──────────┐     ┌──────────┐
│ PDF/TXT      │ ──→ │ Chunker  │ ──→ │ Embedder │
│ Documents    │     └──────────┘     └────┬─────┘
└──────────────┘                           │
                                           ▼
┌──────────────┐     ┌──────────┐     ┌──────────┐
│ Answer       │ ←── │ LLM      │ ←── │ ChromaDB │
└──────────────┘     └──────────┘     └──────────┘
        ▲                                  ▲
        │            ┌──────────┐          │
        └────────────│  Query   │──────────┘
                     │ Handler  │
                     └──────────┘
```

### Step 0: Installation

```bash
pip install openai chromadb langchain langchain-community \
            langchain-openai pypdf python-dotenv
```

### Step 1: Project Structure

```
rag_project/
├── .env                  # API keys
├── data/                 # Your documents go here
│   ├── company_policy.txt
│   └── product_guide.pdf
├── ingest.py             # Loads, chunks, embeds, stores documents
├── query.py              # Handles user queries
├── rag_pipeline.py       # Full RAG pipeline
├── evaluate.py           # Evaluation script
└── requirements.txt
```

### Step 2: Environment Setup (.env)

```env
OPENAI_API_KEY=sk-your-api-key-here
```

### Step 3: Sample Documents (data/company_policy.txt)

```text
Acme Corporation - Company Policies

Refund Policy:
Customers may return any product within 30 days of purchase for a full refund.
Products must be unused and in their original packaging. Opened software cannot
be returned. To initiate a return, contact our support team at support@acme.com
or call 1-800-ACME-HELP.

Shipping Policy:
We offer free standard shipping on orders over $50. Standard shipping takes 5-7
business days. Express shipping (2-3 business days) is available for $12.99.
Overnight shipping is available for $24.99. International shipping rates vary
by destination.

Warranty Policy:
All Acme products come with a 1-year limited warranty covering manufacturing
defects. Extended warranties of 2 or 3 years can be purchased at checkout.
Warranty does not cover damage from misuse, accidents, or unauthorized
modifications.

Employee Benefits:
Full-time employees receive health insurance, dental coverage, and a 401(k)
match of up to 4%. Employees also get 15 days of PTO per year, increasing to
20 days after 3 years of service. Remote work is available 2 days per week.

Contact Information:
Email: support@acme.com
Phone: 1-800-ACME-HELP (1-800-226-3435)
Hours: Monday-Friday, 8 AM - 6 PM EST
Headquarters: 123 Innovation Drive, San Francisco, CA 94105
```

### Step 4: Document Ingestion (ingest.py)

```python
"""
ingest.py — Load documents, chunk them, embed them, store in ChromaDB.
Run this ONCE to set up the knowledge base.
"""

import os
from dotenv import load_dotenv
from langchain_community.document_loaders import (
    TextLoader,
    PyPDFLoader,
    DirectoryLoader
)
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_openai import OpenAIEmbeddings
import chromadb

# Load API keys
load_dotenv()

# ─── CONFIG ───
DATA_DIR = "./data"
CHROMA_DIR = "./chroma_db"
COLLECTION_NAME = "company_docs"
CHUNK_SIZE = 500        # Characters per chunk
CHUNK_OVERLAP = 50      # Overlap between chunks


def load_documents(data_dir: str):
    """Load all documents from the data directory."""
    documents = []

    for filename in os.listdir(data_dir):
        filepath = os.path.join(data_dir, filename)

        if filename.endswith(".txt"):
            loader = TextLoader(filepath, encoding="utf-8")
            documents.extend(loader.load())
            print(f"  Loaded: {filename}")

        elif filename.endswith(".pdf"):
            loader = PyPDFLoader(filepath)
            documents.extend(loader.load())
            print(f"  Loaded: {filename}")

        else:
            print(f"  Skipped: {filename} (unsupported format)")

    return documents


def chunk_documents(documents, chunk_size=CHUNK_SIZE, overlap=CHUNK_OVERLAP):
    """Split documents into smaller chunks."""
    splitter = RecursiveCharacterTextSplitter(
        chunk_size=chunk_size,
        chunk_overlap=overlap,
        separators=["\n\n", "\n", ". ", " ", ""]
    )
    chunks = splitter.split_documents(documents)
    return chunks


def store_in_chroma(chunks):
    """Embed chunks and store in ChromaDB."""
    # Initialize embedding model
    embedding_model = OpenAIEmbeddings(model="text-embedding-3-small")

    # Initialize ChromaDB
    client = chromadb.PersistentClient(path=CHROMA_DIR)

    # Delete existing collection if it exists (fresh start)
    try:
        client.delete_collection(COLLECTION_NAME)
    except Exception:
        pass

    collection = client.create_collection(
        name=COLLECTION_NAME,
        metadata={"hnsw:space": "cosine"}
    )

    # Add chunks in batches
    batch_size = 50
    for i in range(0, len(chunks), batch_size):
        batch = chunks[i:i + batch_size]

        # Get text content
        texts = [chunk.page_content for chunk in batch]
        metadatas = [chunk.metadata for chunk in batch]
        ids = [f"chunk_{i + j}" for j in range(len(batch))]

        # Generate embeddings
        embeddings = embedding_model.embed_documents(texts)

        # Store in ChromaDB
        collection.add(
            documents=texts,
            embeddings=embeddings,
            metadatas=metadatas,
            ids=ids
        )
        print(f"  Stored batch {i // batch_size + 1}: {len(batch)} chunks")

    return collection


def main():
    print("=" * 50)
    print("RAG Document Ingestion Pipeline")
    print("=" * 50)

    # Step 1: Load documents
    print("\n📂 Loading documents...")
    documents = load_documents(DATA_DIR)
    print(f"   Total documents loaded: {len(documents)}")

    # Step 2: Chunk documents
    print("\n✂️  Chunking documents...")
    chunks = chunk_documents(documents)
    print(f"   Total chunks created: {len(chunks)}")

    # Preview first chunk
    print(f"\n   Sample chunk:\n   '{chunks[0].page_content[:100]}...'")

    # Step 3: Embed and store
    print("\n💾 Embedding and storing in ChromaDB...")
    collection = store_in_chroma(chunks)
    print(f"   Collection '{COLLECTION_NAME}' created with {collection.count()} chunks")

    print("\n✅ Ingestion complete! Ready for queries.")


if __name__ == "__main__":
    main()
```

### Step 5: Query Handler (query.py)

```python
"""
query.py — Handle user queries against the RAG knowledge base.
"""

import chromadb
from langchain_openai import OpenAIEmbeddings
from dotenv import load_dotenv

load_dotenv()

# ─── CONFIG ───
CHROMA_DIR = "./chroma_db"
COLLECTION_NAME = "company_docs"
TOP_K = 3
RELEVANCE_THRESHOLD = 1.0  # Max cosine distance (lower = more similar)


def get_collection():
    """Connect to ChromaDB collection."""
    client = chromadb.PersistentClient(path=CHROMA_DIR)
    return client.get_collection(name=COLLECTION_NAME)


def retrieve_chunks(query: str, top_k: int = TOP_K) -> dict:
    """Retrieve the most relevant chunks for a query."""
    collection = get_collection()
    embedding_model = OpenAIEmbeddings(model="text-embedding-3-small")

    # Embed the query using the SAME model
    query_embedding = embedding_model.embed_query(query)

    # Search ChromaDB
    results = collection.query(
        query_embeddings=[query_embedding],
        n_results=top_k
    )

    return {
        "chunks": results["documents"][0],
        "distances": results["distances"][0],
        "metadatas": results["metadatas"][0]
    }


def is_relevant(distances: list[float], threshold: float = RELEVANCE_THRESHOLD) -> bool:
    """Check if retrieved chunks are relevant enough."""
    return len(distances) > 0 and distances[0] < threshold


if __name__ == "__main__":
    # Quick test
    query = "What is the refund policy?"
    results = retrieve_chunks(query)
    print(f"Query: {query}\n")
    for i, (chunk, dist) in enumerate(zip(results["chunks"], results["distances"])):
        print(f"Chunk {i+1} (distance: {dist:.4f}):")
        print(f"  {chunk[:150]}...\n")
```

### Step 6: Full RAG Pipeline (rag_pipeline.py)

```python
"""
rag_pipeline.py — Complete RAG pipeline: Query → Retrieve → Generate.
"""

from openai import OpenAI
from dotenv import load_dotenv
from query import retrieve_chunks, is_relevant

load_dotenv()
client = OpenAI()

# ─── SYSTEM PROMPT ───
SYSTEM_PROMPT = """You are a helpful assistant for Acme Corporation.
Answer questions based ONLY on the provided context.

Rules:
1. If the context contains the answer, respond clearly and concisely.
2. If the context does NOT contain the answer, say:
   "I don't have that information in my knowledge base. Please contact support@acme.com."
3. Never make up information.
4. Be friendly and professional.
5. If applicable, include relevant details like phone numbers, emails, or deadlines.
"""

# ─── PROMPT TEMPLATE ───
PROMPT_TEMPLATE = """Answer the question based on the following context.

Context:
{context}

Question: {question}

Answer:"""


def build_prompt(query: str, chunks: list[str]) -> str:
    """Construct the final prompt with retrieved context."""
    context = "\n\n---\n\n".join(chunks)
    return PROMPT_TEMPLATE.format(context=context, question=query)


def generate_answer(prompt: str, stream: bool = False) -> str:
    """Generate answer using the LLM."""
    messages = [
        {"role": "system", "content": SYSTEM_PROMPT},
        {"role": "user", "content": prompt}
    ]

    if stream:
        # Streaming response
        response = client.chat.completions.create(
            model="gpt-4o-mini",
            messages=messages,
            temperature=0.2,
            max_tokens=500,
            stream=True
        )
        full_answer = ""
        for chunk in response:
            token = chunk.choices[0].delta.content
            if token:
                print(token, end="", flush=True)
                full_answer += token
        print()  # New line after streaming
        return full_answer
    else:
        # Non-streaming response
        response = client.chat.completions.create(
            model="gpt-4o-mini",
            messages=messages,
            temperature=0.2,
            max_tokens=500
        )
        return response.choices[0].message.content


def rag_query(question: str, stream: bool = False) -> dict:
    """
    Complete RAG pipeline.
    Returns the answer and metadata about the retrieval.
    """
    # Step 1: Retrieve relevant chunks
    retrieval = retrieve_chunks(question)

    # Step 2: Check if retrieval found relevant results
    if not is_relevant(retrieval["distances"]):
        return {
            "answer": "I couldn't find relevant information for your question. "
                      "Please try rephrasing or contact support@acme.com.",
            "chunks_used": [],
            "retrieval_scores": []
        }

    # Step 3: Build the prompt
    prompt = build_prompt(question, retrieval["chunks"])

    # Step 4: Generate the answer
    answer = generate_answer(prompt, stream=stream)

    return {
        "answer": answer,
        "chunks_used": retrieval["chunks"],
        "retrieval_scores": retrieval["distances"]
    }


def main():
    """Interactive RAG chatbot."""
    print("=" * 50)
    print("🤖 Acme Corp DocChat — Ask me anything!")
    print("   Type 'quit' to exit")
    print("=" * 50)

    while True:
        question = input("\n❓ You: ").strip()
        if question.lower() in ("quit", "exit", "q"):
            print("Goodbye!")
            break
        if not question:
            continue

        print("\n💬 Assistant: ", end="")
        result = rag_query(question, stream=True)

        # Show retrieval metadata
        print(f"\n📊 Retrieved {len(result['chunks_used'])} chunks "
              f"(best score: {result['retrieval_scores'][0]:.4f})"
              if result['retrieval_scores'] else "")


if __name__ == "__main__":
    main()
```

### Step 7: Evaluation (evaluate.py)

```python
"""
evaluate.py — Evaluate the RAG system's performance.
"""

from rag_pipeline import rag_query


# ─── TEST CASES ───
TEST_CASES = [
    {
        "question": "What is the refund policy?",
        "expected_keywords": ["30 days", "return", "unused", "original packaging"],
        "category": "refund"
    },
    {
        "question": "How much does express shipping cost?",
        "expected_keywords": ["12.99", "2-3 business days"],
        "category": "shipping"
    },
    {
        "question": "How many PTO days do employees get?",
        "expected_keywords": ["15 days", "20 days", "3 years"],
        "category": "employee"
    },
    {
        "question": "What is the warranty period?",
        "expected_keywords": ["1-year", "1 year", "manufacturing defects"],
        "category": "warranty"
    },
    {
        "question": "What is the company phone number?",
        "expected_keywords": ["1-800", "ACME-HELP"],
        "category": "contact"
    },
    {
        "question": "What is the meaning of life?",
        "expected_keywords": ["don't have", "not", "contact"],
        "category": "out_of_scope"
    }
]


def evaluate_rag():
    """Run evaluation on test cases."""
    print("=" * 60)
    print("RAG Evaluation Report")
    print("=" * 60)

    results = []
    total_score = 0

    for i, test in enumerate(TEST_CASES, 1):
        print(f"\n{'─' * 60}")
        print(f"Test {i}: {test['question']}")
        print(f"Category: {test['category']}")

        # Get RAG answer
        result = rag_query(test["question"])
        answer = result["answer"]

        # Calculate keyword match score
        matches = [
            kw for kw in test["expected_keywords"]
            if kw.lower() in answer.lower()
        ]
        score = len(matches) / len(test["expected_keywords"])
        total_score += score

        # Display results
        print(f"Answer: {answer[:200]}{'...' if len(answer) > 200 else ''}")
        print(f"Expected Keywords: {test['expected_keywords']}")
        print(f"Found Keywords: {matches}")
        print(f"Score: {score:.0%}")
        print(f"Chunks Retrieved: {len(result['chunks_used'])}")

        results.append({
            "question": test["question"],
            "score": score,
            "category": test["category"]
        })

    # Summary
    avg_score = total_score / len(TEST_CASES)
    print(f"\n{'=' * 60}")
    print(f"OVERALL SCORE: {avg_score:.0%}")
    print(f"Passed: {sum(1 for r in results if r['score'] >= 0.5)}/{len(TEST_CASES)}")
    print(f"{'=' * 60}")

    # Per-category breakdown
    categories = set(r["category"] for r in results)
    print("\nPer-Category Scores:")
    for cat in sorted(categories):
        cat_results = [r for r in results if r["category"] == cat]
        cat_avg = sum(r["score"] for r in cat_results) / len(cat_results)
        status = "✅" if cat_avg >= 0.5 else "❌"
        print(f"  {status} {cat}: {cat_avg:.0%}")


if __name__ == "__main__":
    evaluate_rag()
```

### Step 8: Requirements (requirements.txt)

```text
openai>=1.12.0
chromadb>=0.4.22
langchain>=0.1.0
langchain-community>=0.0.16
langchain-openai>=0.0.5
pypdf>=4.0.0
python-dotenv>=1.0.0
sentence-transformers>=2.5.0
rank-bm25>=0.2.2
ragas>=0.1.0
```

### Step 9: Run the Project

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Set your API key
echo "OPENAI_API_KEY=sk-your-key" > .env

# 3. Add documents to ./data/ folder

# 4. Run ingestion (one time)
python ingest.py

# 5. Start the chatbot
python rag_pipeline.py

# 6. Run evaluation
python evaluate.py
```

### Expected Output:

```
==================================================
🤖 Acme Corp DocChat — Ask me anything!
   Type 'quit' to exit
==================================================

❓ You: What is the refund policy?

💬 Assistant: Acme Corporation offers a 30-day refund policy.
Customers can return any product within 30 days of purchase for
a full refund, provided the product is unused and in its original
packaging. Opened software cannot be returned. To start a return,
you can contact support at support@acme.com or call 1-800-ACME-HELP.

📊 Retrieved 3 chunks (best score: 0.1823)

❓ You: How much is overnight shipping?

💬 Assistant: Overnight shipping is available for $24.99.

📊 Retrieved 3 chunks (best score: 0.2156)

❓ You: quit
Goodbye!
```

---

## 🎯 Quick Reference Cheat Sheet

```
RAG Pipeline At a Glance:
━━━━━━━━━━━━━━━━━━━━━━━━━
1. COLLECT   → Gather documents (PDF, TXT, etc.)
2. CLEAN     → Remove noise, fix formatting
3. CHUNK     → Split into 500-char pieces with 50-char overlap
4. EMBED     → Convert to vectors (text-embedding-3-small)
5. STORE     → Save in ChromaDB (cosine similarity)
6. QUERY     → Embed user question with SAME model
7. RETRIEVE  → Find top-3 similar chunks
8. PROMPT    → Inject chunks into prompt template
9. GENERATE  → LLM produces answer (temp=0.2)
10. EVALUATE → RAGAS / keyword matching

Key Decisions:
━━━━━━━━━━━━━
Chunk Size:     500 characters
Chunk Overlap:  50 characters (10%)
Embedding:      text-embedding-3-small (1536 dims)
Vector DB:      ChromaDB (cosine)
Top K:          3 chunks
Temperature:    0.2 (factual)
Max Tokens:     500
Distance:       Cosine similarity
```

---

*Built with ❤️ — Happy building!*
