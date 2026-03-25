# 📘 RAG Concepts — Deep Dive Explanation (Every Single Concept)

> Every concept explained like you're 10 years old, then shown with real code.

---

# PART 1: THE BIG PICTURE

---

## 1. What is RAG?

### The Simplest Explanation

**RAG** stands for **Retrieval-Augmented Generation**.

Let's break down each word:

| Word | Meaning | Analogy |
|------|---------|---------|
| **Retrieval** | Finding/fetching relevant information | Looking up answers in a textbook |
| **Augmented** | Enhanced / made better with extra info | Adding cheat notes to your exam |
| **Generation** | Creating a response using AI | Writing your answer on the paper |

### Real-Life Analogy

Imagine you're a **new employee** at a company. On your first day, a customer asks:

> "What's your company's refund policy?"

**Without RAG (like a normal ChatGPT):**
You panic. You were never trained on this company's policies. You either say "I don't know" or worse — you **guess** and give wrong information.

**With RAG:**
You walk to the filing cabinet → find the "Refund Policy" document → read it → then answer the customer accurately.

```
RAG is exactly this:

Step 1: Customer asks a question
Step 2: System SEARCHES your company documents (the filing cabinet)
Step 3: System FINDS the relevant document
Step 4: System READS it and generates an accurate answer
```

### Why Not Just Train the AI on Our Data?

| Approach | Cost | Time | Update Data | Privacy |
|----------|------|------|-------------|---------|
| **Fine-tuning** (retraining) | $$$$ | Days/Weeks | Retrain from scratch | Data sent to provider |
| **RAG** | $ | Minutes | Just update documents | Data stays local |

### Code: Simplest RAG in 10 Lines

```python
# The SIMPLEST possible RAG — no libraries, just the concept

# Step 1: Your "knowledge base" (in real life, these come from documents)
knowledge_base = [
    "Our refund policy allows returns within 30 days.",
    "Express shipping costs $12.99.",
    "The CEO of Acme Corp is Alice Johnson."
]

# Step 2: User asks a question
question = "What is the refund policy?"

# Step 3: RETRIEVE — Find the most relevant piece of info
#   (In real RAG, this uses embeddings. Here we use simple keyword matching.)
relevant_doc = None
for doc in knowledge_base:
    if "refund" in doc.lower():  # Simple keyword search
        relevant_doc = doc
        break

# Step 4: AUGMENT — Add the retrieved info to the question
prompt = f"""Based on this information: "{relevant_doc}"
Answer this question: {question}"""

# Step 5: GENERATE — Send to an LLM
# (In real life, you'd call OpenAI API here)
print(prompt)
# Output:
# Based on this information: "Our refund policy allows returns within 30 days."
# Answer this question: What is the refund policy?
```

---

## 2. Why Use RAG?

### Problem 1: Knowledge Cutoff

LLMs like ChatGPT were trained on data up to a certain date. They don't know anything after that.

```
You: "Who won the 2025 Cricket World Cup?"
ChatGPT (without RAG): "I don't have information after my training cutoff."
ChatGPT (with RAG): Searches the internet/your docs → Gives accurate answer
```

### Problem 2: No Access to Private Data

ChatGPT has never seen YOUR company's data.

```
You: "What's the leave policy at my company?"

Without RAG:
  → AI has NO IDEA. It was trained on public internet data only.

With RAG:
  → AI searches your HR documents → finds the leave policy → answers accurately.
```

### Problem 3: Hallucinations

When AI doesn't know something, it sometimes **makes up** answers confidently.

```
You: "What medication should I take for headaches?"

Without RAG (Hallucination Risk):
  → "Take 500mg of Xylomethicone every 4 hours."  ← MADE UP DRUG!

With RAG (Grounded in docs):
  → Retrieves actual medical FAQ → "Based on our medical guide,
     over-the-counter acetaminophen or ibuprofen is commonly recommended.
     Please consult your doctor."
```

### Problem 4: Fine-Tuning is Expensive

```
Fine-Tuning:
  - Cost: $1,000 - $100,000+
  - Time: Hours to days of GPU training
  - Update: Need to retrain every time data changes
  - Risk: Model might "forget" old knowledge

RAG:
  - Cost: ~$0.01 per query (API calls)
  - Time: Minutes to set up
  - Update: Just add new documents
  - Risk: None — original model unchanged
```

### When Should You Use RAG?

```
USE RAG WHEN:
  ✅ You have private/company documents
  ✅ Your data changes frequently
  ✅ You need factual, grounded answers
  ✅ You want to cite sources
  ✅ Budget is limited

DON'T USE RAG WHEN:
  ❌ General conversation (no specific knowledge needed)
  ❌ Creative writing (no facts to retrieve)
  ❌ You need the model to learn a new SKILL (use fine-tuning)
  ❌ Your data is only a few sentences (just put it in the prompt)
```

---

## 3. How RAG Works

### The Two Phases

RAG has TWO phases. Think of it like a library:

**Phase 1: INDEXING (Setting up the library)**
You do this ONCE. It's like organizing all books and creating a catalog.

**Phase 2: QUERYING (Using the library)**
You do this EVERY TIME a user asks a question.

```
PHASE 1: INDEXING (One-time setup)
═══════════════════════════════════════════════
  Your Documents
       │
       ▼
  ┌─────────────┐
  │ Load Files  │  ← Read PDFs, TXT, HTML, etc.
  └──────┬──────┘
         ▼
  ┌─────────────┐
  │   Clean     │  ← Remove noise, fix formatting
  └──────┬──────┘
         ▼
  ┌─────────────┐
  │   Chunk     │  ← Break into small pieces (500 chars each)
  └──────┬──────┘
         ▼
  ┌─────────────┐
  │   Embed     │  ← Convert text → numbers (vectors)
  └──────┬──────┘
         ▼
  ┌─────────────┐
  │   Store     │  ← Save vectors in a Vector Database
  └─────────────┘


PHASE 2: QUERYING (Every user question)
═══════════════════════════════════════════════
  User: "What is the refund policy?"
       │
       ▼
  ┌──────────────┐
  │ Embed Query  │  ← Convert question → vector (same model!)
  └──────┬───────┘
         ▼
  ┌──────────────┐
  │  Search DB   │  ← Find most similar document chunks
  └──────┬───────┘
         ▼
  ┌──────────────┐
  │ Build Prompt │  ← Combine: system prompt + retrieved chunks + question
  └──────┬───────┘
         ▼
  ┌──────────────┐
  │ LLM Answers  │  ← AI reads the context and generates an answer
  └──────┬───────┘
         ▼
  "You can return products within 30 days for a full refund."
```

### Code: See the Full Flow

```python
# Let's trace through each step with print statements

# ── PHASE 1: INDEXING ──
print("=== PHASE 1: INDEXING ===\n")

# Step 1: Load
raw_text = "Refund policy: Returns accepted within 30 days. Products must be unused."
print(f"1. LOADED: '{raw_text}'\n")

# Step 2: Chunk
chunks = ["Refund policy: Returns accepted within 30 days.",
          "Products must be unused."]
print(f"2. CHUNKED into {len(chunks)} pieces:")
for i, c in enumerate(chunks):
    print(f"   Chunk {i}: '{c}'")

# Step 3: Embed (simplified — real embeddings are 1536 numbers)
fake_embeddings = {
    chunks[0]: [0.8, 0.1, 0.9],  # "refund" concept
    chunks[1]: [0.7, 0.2, 0.8],  # "product condition" concept
}
print(f"\n3. EMBEDDED each chunk into vectors:")
for text, vec in fake_embeddings.items():
    print(f"   '{text[:40]}...' → {vec}")

# Step 4: Store
vector_db = fake_embeddings.copy()
print(f"\n4. STORED {len(vector_db)} vectors in database")

# ── PHASE 2: QUERYING ──
print("\n\n=== PHASE 2: QUERYING ===\n")

query = "How do I return a product?"
print(f"5. USER QUERY: '{query}'")

query_vector = [0.75, 0.15, 0.85]
print(f"6. QUERY EMBEDDED: {query_vector}")

# Find most similar (simplified)
print(f"7. SEARCHING vector database...")
print(f"   Best match: '{chunks[0]}' (similarity: 0.95)")

# Build prompt
context = chunks[0]
prompt = f"Context: {context}\nQuestion: {query}\nAnswer:"
print(f"\n8. PROMPT BUILT:\n   {prompt}")

# Generate (simplified)
answer = "You can return products within 30 days of purchase."
print(f"\n9. LLM GENERATED: '{answer}'")
```

---

## 4. Components of RAG

Every RAG system has **6 building blocks**. Let me explain each:

```
┌─────────────────────────────────────────────────────┐
│                  RAG COMPONENTS                      │
│                                                      │
│  ┌──────────┐  ┌──────────┐  ┌────────────┐        │
│  │ 1. Data  │→ │ 2. Chunk │→ │ 3. Embed   │        │
│  │  Source   │  │  Engine  │  │   Model    │        │
│  └──────────┘  └──────────┘  └─────┬──────┘        │
│                                     │                │
│  ┌──────────┐  ┌──────────┐  ┌─────▼──────┐        │
│  │ 6. LLM   │← │5. Retrie │← │ 4. Vector  │        │
│  │Generator │  │   ver    │  │    DB      │        │
│  └──────────┘  └──────────┘  └────────────┘        │
└─────────────────────────────────────────────────────┘
```

### Component 1: Data Source
**What:** Where your information lives.
**Examples:** PDF files, text files, web pages, databases, emails, Slack messages.
**Analogy:** The raw ingredients in a kitchen.

### Component 2: Chunking Engine
**What:** Breaks large documents into small pieces.
**Why:** LLMs can only process limited text at once, and small pieces are easier to search.
**Analogy:** Chopping vegetables into bite-sized pieces.

### Component 3: Embedding Model
**What:** Converts text into numbers (vectors) that capture meaning.
**Why:** Computers can't understand text, but they can compare numbers.
**Analogy:** Translating words into GPS coordinates — similar meanings get nearby coordinates.

### Component 4: Vector Database
**What:** A special database that stores vectors and finds similar ones quickly.
**Why:** Normal databases search by exact match. Vector DBs search by meaning similarity.
**Analogy:** An organized library with a smart catalog system.

### Component 5: Retriever
**What:** Takes a user's question and finds the most relevant chunks.
**Why:** The user's question needs to be matched against stored knowledge.
**Analogy:** The librarian who finds the right books for you.

### Component 6: LLM (Generator)
**What:** The AI model that reads the retrieved context and writes an answer.
**Why:** Raw document chunks aren't a nice answer — the LLM summarizes and explains.
**Analogy:** The chef who turns raw ingredients into a finished dish.

---

## 5. Types of RAG

### Type 1: Naive RAG (The Basics)

This is the simplest version — exactly what we've been discussing.

```
User Question → Embed → Search DB → Get Chunks → LLM → Answer
```

**How it works:**
1. User asks a question
2. Embed the question
3. Search the vector database
4. Get the top K chunks
5. Feed them to the LLM
6. Return the answer

**Problems with Naive RAG:**
```
Problem 1: BAD QUERIES
  User types: "ugh my thing broke what do I do"
  This vague query gets poor search results.

Problem 2: IRRELEVANT CHUNKS
  The top 3 chunks might not all be relevant.
  Chunk 1: Relevant ✅
  Chunk 2: Somewhat relevant 🟡
  Chunk 3: Irrelevant ❌  ← Wasting space in the prompt

Problem 3: NO VERIFICATION
  The LLM might still hallucinate even with context.
  No check to verify the answer is actually grounded.
```

### Type 2: Advanced RAG (Smarter)

Adds **pre-retrieval** and **post-retrieval** improvements.

```
User Question
     │
     ▼
┌────────────────┐
│ QUERY REWRITE  │  ← Make the question better before searching
│ "ugh broke"    │
│  → "How to fix │
│  a broken      │
│  product"      │
└───────┬────────┘
        ▼
┌────────────────┐
│   RETRIEVE     │  ← Search the vector DB
│   Top 10 chunks│
└───────┬────────┘
        ▼
┌────────────────┐
│   RE-RANK      │  ← Score and reorder by true relevance
│   Keep top 3   │  ← Remove irrelevant chunks
└───────┬────────┘
        ▼
┌────────────────┐
│   LLM GENERATE │  ← Generate answer from best chunks
└───────┬────────┘
        ▼
     Answer
```

**What's New:**

| Improvement | What It Does | Example |
|-------------|-------------|---------|
| **Query Rewriting** | Rephrases the user's question for better retrieval | "it broke" → "How to troubleshoot product malfunction" |
| **Re-Ranking** | Scores chunks by relevance AFTER retrieval | Uses a cross-encoder model to re-score |
| **Hybrid Search** | Combines keyword + semantic search | Catches both exact terms AND similar meanings |

```python
# Example: Query Rewriting with an LLM
from openai import OpenAI
client = OpenAI()

def rewrite_query(original_query: str) -> str:
    """Use an LLM to rewrite a vague query into a clear one."""
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[{
            "role": "user",
            "content": f"""Rewrite this user question to be more specific 
and better for searching a knowledge base. 
Return ONLY the rewritten question, nothing else.

Original: {original_query}
Rewritten:"""
        }],
        temperature=0.0,
        max_tokens=100
    )
    return response.choices[0].message.content.strip()

# Test it
vague = "ugh my thing broke what do"
clear = rewrite_query(vague)
print(f"Original: '{vague}'")
print(f"Rewritten: '{clear}'")
# Rewritten: "How do I troubleshoot or repair a broken product?"
```

```python
# Example: Re-Ranking with a Cross-Encoder
from sentence_transformers import CrossEncoder

reranker = CrossEncoder("cross-encoder/ms-marco-MiniLM-L-6-v2")

query = "What is the refund policy?"
chunks = [
    "Our CEO is Alice Johnson.",           # Irrelevant
    "Returns accepted within 30 days.",     # Relevant!
    "We have offices in 5 countries.",       # Irrelevant
    "Products must be unused for returns.",  # Relevant!
]

# Score each chunk against the query
pairs = [[query, chunk] for chunk in chunks]
scores = reranker.predict(pairs)

# Sort by score (highest first)
ranked = sorted(zip(scores, chunks), reverse=True)
for score, chunk in ranked:
    print(f"  Score {score:.4f}: {chunk}")

# Output:
#   Score 8.2341: Returns accepted within 30 days.
#   Score 7.1523: Products must be unused for returns.
#   Score 0.1234: Our CEO is Alice Johnson.
#   Score 0.0567: We have offices in 5 countries.
```

### Type 3: Modular RAG (Most Flexible)

Think of this as **LEGO blocks** — you can add, remove, or swap any component.

```
                    ┌──────────┐
                    │  ROUTER  │  ← Decides which path to take
                    └────┬─────┘
                         │
           ┌─────────────┼─────────────┐
           ▼             ▼             ▼
    ┌────────────┐ ┌──────────┐ ┌──────────┐
    │  Web       │ │ Vector   │ │ SQL      │
    │  Search    │ │ DB       │ │ Database │
    └─────┬──────┘ └────┬─────┘ └────┬─────┘
          │              │            │
          └──────────────┼────────────┘
                         ▼
                  ┌────────────┐
                  │  RE-RANKER │
                  └─────┬──────┘
                        ▼
                  ┌────────────┐
                  │    LLM     │
                  └─────┬──────┘
                        ▼
                  ┌────────────┐
                  │   JUDGE    │  ← Checks if the answer is good
                  └─────┬──────┘
                   Good? │ Bad?
                    │       │
                    ▼       ▼
                 Answer   Loop back and try again
```

**Modular features:**

| Module | Purpose | Example |
|--------|---------|---------|
| **Router** | Sends the query to the right retriever | "weather?" → Web search / "refund?" → Vector DB |
| **Multiple Retrievers** | Different knowledge sources | Web + local docs + SQL |
| **Judge** | Validates if the answer is correct | "Is this answer grounded in the context?" |
| **Iterative Retrieval** | Loops back if answer isn't good enough | "Not confident? Search again with different terms." |

---

# PART 2: DATA PIPELINE

---

## 6. Data Collection

### 6.1 Sources — Where Does Data Come From?

Your RAG system is only as good as its data. Here are common sources:

```
INTERNAL SOURCES:                    EXTERNAL SOURCES:
┌─────────────────────┐             ┌─────────────────────┐
│ • Company docs (PDF)│             │ • Web pages (HTML)   │
│ • Internal wikis    │             │ • Public APIs        │
│ • Slack messages    │             │ • Research papers    │
│ • Emails            │             │ • Wikipedia          │
│ • Notion pages      │             │ • News articles      │
│ • Google Drive      │             │ • Government data    │
│ • Database records  │             │ • Stack Overflow     │
└─────────────────────┘             └─────────────────────┘
```

```python
# Example: Loading from different sources

# ── Source 1: Text File ──
with open("policy.txt", "r") as f:
    text_data = f.read()
print(f"Text file: {len(text_data)} characters")

# ── Source 2: PDF File ──
from langchain_community.document_loaders import PyPDFLoader
loader = PyPDFLoader("manual.pdf")
pdf_pages = loader.load()
print(f"PDF: {len(pdf_pages)} pages")

# ── Source 3: Web Page ──
from langchain_community.document_loaders import WebBaseLoader
loader = WebBaseLoader("https://example.com/faq")
web_docs = loader.load()
print(f"Web page: {len(web_docs)} documents")

# ── Source 4: CSV / Spreadsheet ──
from langchain_community.document_loaders import CSVLoader
loader = CSVLoader("products.csv")
csv_docs = loader.load()
print(f"CSV: {len(csv_docs)} rows")
```

### 6.2 Format — What Shape is the Data?

| Format | Extension | How to Read | Library |
|--------|-----------|-------------|---------|
| Plain Text | `.txt` | `open()` | Built-in Python |
| PDF | `.pdf` | `PyPDFLoader` | `pypdf`, `langchain` |
| Word Doc | `.docx` | `Docx2txtLoader` | `docx2txt` |
| CSV | `.csv` | `CSVLoader` | `pandas`, `langchain` |
| JSON | `.json` | `json.load()` | Built-in Python |
| HTML | `.html` | `BeautifulSoup` | `bs4` |
| Markdown | `.md` | `UnstructuredMarkdownLoader` | `langchain` |

```python
# Loading each format:

# TXT
with open("data.txt", "r", encoding="utf-8") as f:
    text = f.read()

# PDF
from langchain_community.document_loaders import PyPDFLoader
pages = PyPDFLoader("data.pdf").load()  # Each page = one document

# CSV
import csv
with open("data.csv", "r") as f:
    reader = csv.DictReader(f)
    rows = [row for row in reader]

# JSON
import json
with open("data.json", "r") as f:
    data = json.load(f)

# HTML
from bs4 import BeautifulSoup
with open("page.html", "r") as f:
    soup = BeautifulSoup(f.read(), "html.parser")
    text = soup.get_text()
```

### 6.3 Structured vs Unstructured Data

This is a crucial concept. Let me explain clearly:

**STRUCTURED DATA** = Data organized in a predictable format with rows and columns.

```
┌──────────┬─────────┬────────┬───────────┐
│ order_id │ product │ price  │ date      │
├──────────┼─────────┼────────┼───────────┤
│ 1001     │ Laptop  │ 999.99 │ 2024-01-15│
│ 1002     │ Mouse   │ 29.99  │ 2024-01-16│
│ 1003     │ Keyboard│ 79.99  │ 2024-01-16│
└──────────┴─────────┴────────┴───────────┘

Examples: SQL databases, CSV files, Excel spreadsheets, JSON with fixed schema
How to query: SELECT * FROM orders WHERE price > 50
```

**UNSTRUCTURED DATA** = Free-form text, no fixed format.

```
"Hey team, just wanted to let everyone know that the new refund
policy went into effect last Monday. Customers now have 30 days
instead of 14 to return products. Also, we're offering free
return shipping for orders over $100. Please update your scripts
accordingly. Thanks! - Alice"

Examples: Emails, PDFs, chat messages, meeting notes, web pages
How to query: You CAN'T query this with SQL → This is where RAG shines!
```

**SEMI-STRUCTURED DATA** = Has some organization but not fully tabular.

```json
{
  "name": "Alice",
  "messages": [
    "The refund policy changed to 30 days",
    "Free return shipping over $100"
  ],
  "tags": ["policy", "returns"]
}

Examples: JSON, XML, HTML, YAML
```

**RAG primarily handles UNSTRUCTURED data** — this is its superpower.

### 6.4 Data Cleaning — Removing the Mess

Raw data is NEVER clean. Here's what you'll encounter and how to fix it:

```python
# ── Problem 1: Extra Whitespace ──
dirty = "Hello    world\n\n\n\nHow   are   you?"
clean = " ".join(dirty.split())
print(clean)  # "Hello world How are you?"


# ── Problem 2: Headers/Footers that Repeat on Every Page ──
dirty = "CONFIDENTIAL - Acme Corp\nThe refund policy...\nPage 3 of 50"
import re
clean = re.sub(r'CONFIDENTIAL.*?\n', '', dirty)
clean = re.sub(r'Page \d+ of \d+', '', clean)
print(clean)  # "The refund policy..."


# ── Problem 3: Special Characters / Encoding Issues ──
dirty = "Itâ€™s a beautifulÂ day"  # Encoding mess
clean = dirty.encode('ascii', 'ignore').decode('ascii')
print(clean)  # "Its a beautiful day"


# ── Problem 4: HTML Tags ──
dirty = "<p>The <b>refund policy</b> allows returns.</p>"
from bs4 import BeautifulSoup
clean = BeautifulSoup(dirty, "html.parser").get_text()
print(clean)  # "The refund policy allows returns."


# ── Problem 5: URLs and Email Addresses (sometimes noise) ──
dirty = "Contact us at https://example.com/help or email info@example.com"
clean = re.sub(r'https?://\S+', '[URL]', dirty)
clean = re.sub(r'\S+@\S+', '[EMAIL]', clean)
print(clean)  # "Contact us at [URL] or email [EMAIL]"
```

### 6.5 Noise Removal — The Complete Pipeline

```python
import re
from typing import Optional


def clean_document(text: str, 
                   remove_urls: bool = False,
                   remove_emails: bool = False,
                   custom_patterns: Optional[list[str]] = None) -> str:
    """
    Complete text cleaning pipeline for RAG.
    
    Args:
        text: Raw text to clean
        remove_urls: Whether to strip URLs
        remove_emails: Whether to strip email addresses
        custom_patterns: List of regex patterns to remove
    
    Returns:
        Cleaned text
    """
    # Step 1: Fix encoding issues
    text = text.encode('utf-8', errors='ignore').decode('utf-8')
    
    # Step 2: Remove null bytes and control characters
    text = re.sub(r'[\x00-\x08\x0b\x0c\x0e-\x1f\x7f]', '', text)
    
    # Step 3: Normalize line endings
    text = text.replace('\r\n', '\n').replace('\r', '\n')
    
    # Step 4: Remove page numbers (common in PDFs)
    text = re.sub(r'\n\s*\d+\s*\n', '\n', text)          # Standalone page numbers
    text = re.sub(r'Page\s+\d+\s*(of\s+\d+)?', '', text)  # "Page X of Y"
    
    # Step 5: Remove headers/footers (customize for your docs)
    text = re.sub(r'CONFIDENTIAL.*?\n', '', text, flags=re.IGNORECASE)
    text = re.sub(r'©.*?\n', '', text)  # Copyright lines
    
    # Step 6: Optional — Remove URLs
    if remove_urls:
        text = re.sub(r'https?://\S+', '', text)
    
    # Step 7: Optional — Remove emails
    if remove_emails:
        text = re.sub(r'\S+@\S+\.\S+', '', text)
    
    # Step 8: Custom patterns
    if custom_patterns:
        for pattern in custom_patterns:
            text = re.sub(pattern, '', text)
    
    # Step 9: Fix multiple newlines (keep max 2)
    text = re.sub(r'\n{3,}', '\n\n', text)
    
    # Step 10: Fix multiple spaces
    text = re.sub(r' {2,}', ' ', text)
    
    # Step 11: Strip each line
    text = '\n'.join(line.strip() for line in text.split('\n'))
    
    # Step 12: Final strip
    text = text.strip()
    
    return text


# ── Usage ──
raw_text = """
    CONFIDENTIAL - Internal Use Only
    
    Page 1 of 5
    
    Refund    Policy
    
    Customers may return   products within 30 days.
    Visit https://acme.com/returns for details.
    Contact support@acme.com for help.
    
    © 2024 Acme Corporation
    
    Page 2 of 5
"""

cleaned = clean_document(raw_text, remove_urls=True)
print(cleaned)
# Output:
# Refund Policy
#
# Customers may return products within 30 days.
# Contact support@acme.com for help.
```

---

## 7. Chunking

### 7.1 What is Chunking and Why Do We Need It?

**Chunking** = Splitting a large document into smaller pieces.

**Why?** Three critical reasons:

```
REASON 1: LLMs Have a Context Window Limit
═══════════════════════════════════════════
GPT-4o-mini can process ~128K tokens.
But sending a 500-page book is wasteful and slow.
You only need the RELEVANT section, not the whole book.

REASON 2: Embeddings Work Better on Focused Text
═══════════════════════════════════════════════════
A vector for "the entire company handbook" is too vague.
A vector for "30-day refund policy" is specific and searchable.

REASON 3: More Precise Retrieval
═══════════════════════════════════════════════════
Small chunks → you retrieve EXACTLY what's relevant.
Whole documents → you retrieve lots of irrelevant text too.
```

### Visual Explanation:

```
BEFORE CHUNKING:
┌──────────────────────────────────────────────────────┐
│ "Chapter 1: Refund Policy. Customers may return      │
│ products within 30 days. Products must be unused.     │
│ Chapter 2: Shipping. We offer free shipping over $50. │
│ Express shipping costs $12.99..."                     │
│                   (2000 characters)                   │
└──────────────────────────────────────────────────────┘
     ↓ One big embedding → vague, covers everything

AFTER CHUNKING:
┌──────────────────────┐  ┌──────────────────────┐
│ "Refund Policy.      │  │ "Shipping. We offer  │
│ Customers may return │  │ free shipping over    │
│ within 30 days.      │  │ $50. Express shipping │
│ Products must be     │  │ costs $12.99..."      │
│ unused."             │  │                       │
│    (Chunk 1)         │  │    (Chunk 2)          │
└──────────────────────┘  └──────────────────────┘
     ↓ Focused embedding       ↓ Focused embedding
   About refunds only!       About shipping only!
```

Now when a user asks "What's the refund policy?", the system finds **Chunk 1** specifically.

### 7.2 Chunk Size — How Big Should Each Piece Be?

```
SMALL CHUNKS (100-256 tokens):
  "Returns accepted within 30 days."
  
  ✅ Very precise — great for specific factual questions
  ✅ Fast to embed and search
  ❌ Might lose context ("30 days of WHAT? For WHOM?")
  ❌ Creates LOTS of chunks → more storage
  Best for: FAQ databases, short factual answers


MEDIUM CHUNKS (256-512 tokens):  ← RECOMMENDED FOR MOST CASES
  "Refund Policy: Customers may return any product 
   within 30 days of purchase for a full refund. 
   Products must be unused and in original packaging."
  
  ✅ Good balance of specificity and context
  ✅ Enough context to understand the topic
  Best for: Most RAG applications


LARGE CHUNKS (512-1024 tokens):
  "Chapter 3: Refund Policy
   Our refund policy was established in 2020 to ensure 
   customer satisfaction. Customers may return any product
   within 30 days... [full chapter]"
  
  ✅ Maximum context — good for complex topics
  ❌ May include irrelevant information
  ❌ Fewer chunks fit in the LLM's context window
  Best for: Complex topics needing full paragraphs
```

### 7.3 Fixed-Size Chunking vs Dynamic Chunking

#### Fixed-Size Chunking (Dumb but simple)

Cuts text every N characters — no regard for sentences or paragraphs.

```python
def fixed_size_chunk(text: str, size: int = 100) -> list[str]:
    """Split text into fixed-size chunks. Simple but can break sentences."""
    chunks = []
    for i in range(0, len(text), size):
        chunks.append(text[i:i + size])
    return chunks


text = "The refund policy allows returns within 30 days. Products must be in original packaging. Contact support for help."

chunks = fixed_size_chunk(text, size=50)
for i, c in enumerate(chunks):
    print(f"Chunk {i}: '{c}'")

# Output:
# Chunk 0: 'The refund policy allows returns within 30 da'  ← CUT mid-word!
# Chunk 1: 'ys. Products must be in original packaging. Co'  ← CUT mid-word!
# Chunk 2: 'ntact support for help.'
```

**Problem:** "30 da" and "ys" are split across chunks. The meaning is broken!

#### Dynamic / Recursive Chunking (Smart) ✅ RECOMMENDED

Splits at natural boundaries (paragraphs → sentences → words).

```python
from langchain.text_splitter import RecursiveCharacterTextSplitter

# The splitter tries these separators IN ORDER:
# 1. "\n\n" — Try to split at paragraph breaks first
# 2. "\n"   — If paragraph break isn't possible, try line breaks
# 3. ". "   — Then try sentence endings
# 4. " "    — Then try word boundaries
# 5. ""     — Last resort: split anywhere

splitter = RecursiveCharacterTextSplitter(
    chunk_size=100,          # Max characters per chunk
    chunk_overlap=20,        # Characters shared between chunks
    separators=["\n\n", "\n", ". ", " ", ""]
)

text = """The refund policy allows returns within 30 days. Products must be in original packaging.

Contact support for any questions about returns. Our team is available 24/7."""

chunks = splitter.split_text(text)
for i, c in enumerate(chunks):
    print(f"Chunk {i}: '{c}'\n")

# Output:
# Chunk 0: 'The refund policy allows returns within 30 days. Products must be in original packaging.'
# Chunk 1: 'Contact support for any questions about returns. Our team is available 24/7.'
# 
# Clean splits at paragraph and sentence boundaries!
```

#### Other Chunking Strategies

```python
# ── Sentence-Based Chunking ──
# Groups N sentences together

import re

def sentence_chunk(text: str, sentences_per_chunk: int = 3) -> list[str]:
    """Group sentences into chunks."""
    sentences = re.split(r'(?<=[.!?])\s+', text)
    chunks = []
    for i in range(0, len(sentences), sentences_per_chunk):
        chunk = " ".join(sentences[i:i + sentences_per_chunk])
        chunks.append(chunk)
    return chunks


# ── Semantic Chunking ──
# Splits when the TOPIC changes (uses embeddings to detect topic shifts)

# Concept:
# Sentence 1: "Refund policy allows returns." → embedding A
# Sentence 2: "Products must be unused."      → embedding B  (similar to A)
# Sentence 3: "Our CEO is Alice."             → embedding C  (DIFFERENT!)
#
# Split between sentence 2 and 3 because the topic changed!

# LangChain has this built in:
# from langchain_experimental.text_splitter import SemanticChunker
```

### 7.4 Chunk Overlapping — Why and How

**Problem:** When you split text, information at the boundary gets separated.

```
WITHOUT OVERLAP:
─────────────────────────────────────────────
Chunk 1: "The refund policy allows returns within"
Chunk 2: "30 days of purchase. Products must be unused."
─────────────────────────────────────────────

User asks: "How many days for refund?"
Search finds Chunk 2 (has "30 days")
But Chunk 2 starts with "30 days of purchase" — 
  it lost the context that this is about "refund policy"!
```

```
WITH 20-CHARACTER OVERLAP:
─────────────────────────────────────────────
Chunk 1: "The refund policy allows returns within"
Chunk 2: "allows returns within 30 days of purchase. Products must be unused."
                              ↑
                    Overlap region — shared text!
─────────────────────────────────────────────

Now Chunk 2 has "returns within 30 days" — full context preserved!
```

```python
# Practical example:

from langchain.text_splitter import RecursiveCharacterTextSplitter

text = "A B C D E F G H I J K L M N O P Q R S T U V W X Y Z"

# WITHOUT overlap
splitter_no_overlap = RecursiveCharacterTextSplitter(
    chunk_size=20, chunk_overlap=0, separators=[" "]
)
chunks = splitter_no_overlap.split_text(text)
print("No overlap:")
for c in chunks:
    print(f"  '{c}'")
# 'A B C D E F G'
# 'H I J K L M N'
# 'O P Q R S T U'
# 'V W X Y Z'

# WITH overlap of 6 characters
splitter_overlap = RecursiveCharacterTextSplitter(
    chunk_size=20, chunk_overlap=6, separators=[" "]
)
chunks = splitter_overlap.split_text(text)
print("\nWith overlap:")
for c in chunks:
    print(f"  '{c}'")
# 'A B C D E F G'
# 'F G H I J K L'    ← "F G" appears in both chunks!
# 'K L M N O P Q'
# 'P Q R S T U V'
# 'U V W X Y Z'
```

**Rule of thumb for overlap:**
```
chunk_size = 500 → chunk_overlap = 50-100  (10-20%)
chunk_size = 1000 → chunk_overlap = 100-200 (10-20%)
```

---

## 8. Embedding Generation

### 8.1 What is an Embedding?

An **embedding** is a list of decimal numbers that captures the **meaning** of text.

```
Think of it like GPS coordinates for MEANING:

"I love dogs"        → [0.82, 0.15, 0.91, 0.33, ...]  ← Location A
"I adore puppies"    → [0.80, 0.17, 0.89, 0.31, ...]  ← Very close to A!
"The weather is nice" → [0.12, 0.67, 0.23, 0.88, ...]  ← Far from A

Just like GPS:
  Delhi and Noida are close → similar coordinates
  Delhi and New York are far → very different coordinates

In embedding space:
  "dogs" and "puppies" are close → similar numbers
  "dogs" and "weather" are far → very different numbers
```

### Why Do We Need Embeddings?

```
Computers don't understand words. They understand NUMBERS.

The word "refund" means nothing to a computer.
But [0.82, 0.15, 0.91, ...] is something it can CALCULATE with.

With embeddings, the computer can answer:
  "How similar is 'refund' to 'return'?"
  → Compare their vectors → cosine similarity = 0.92 (very similar!)
  
  "How similar is 'refund' to 'banana'?"
  → Compare their vectors → cosine similarity = 0.05 (not similar!)
```

```python
# Let's actually SEE embeddings

from openai import OpenAI
client = OpenAI()

# Get the embedding for a sentence
response = client.embeddings.create(
    model="text-embedding-3-small",
    input="I love dogs"
)

embedding = response.data[0].embedding

print(f"Total numbers: {len(embedding)}")  # 1536
print(f"First 10 numbers: {embedding[:10]}")
# [-0.0012, 0.0345, -0.0678, 0.0123, -0.0456, 0.0789, ...]

# These 1536 numbers capture the MEANING of "I love dogs"
```

### 8.2 Embedding vs Token — What's the Difference?

```
TOKENIZATION:
═════════════
"I love dogs" → ["I", " love", " dogs"]  (3 tokens)

Tokens are the BUILDING BLOCKS — individual pieces the model processes.
Think of tokens as individual LEGO bricks.


EMBEDDING:
═════════════
"I love dogs" → [0.82, 0.15, 0.91, ..., 0.33]  (1536 numbers)

An embedding is a SINGLE VECTOR that captures the meaning of the ENTIRE text.
Think of an embedding as a PHOTOGRAPH of the assembled LEGO model.
```

```python
# Seeing the difference:

# ── TOKENIZATION ──
import tiktoken
encoder = tiktoken.encoding_for_model("gpt-4o-mini")

text = "I love dogs"
tokens = encoder.encode(text)
print(f"Tokens: {tokens}")                    # [40, 3021, 12875]
print(f"Token count: {len(tokens)}")           # 3
print(f"Decoded: {[encoder.decode([t]) for t in tokens]}")  # ['I', ' love', ' dogs']

# ── EMBEDDING ──
from openai import OpenAI
client = OpenAI()

response = client.embeddings.create(
    model="text-embedding-3-small",
    input="I love dogs"
)
embedding = response.data[0].embedding
print(f"\nEmbedding dimensions: {len(embedding)}")  # 1536
print(f"First 5 values: {embedding[:5]}")
# [-0.0012, 0.0345, -0.0678, 0.0123, -0.0456]
```

### 8.3 Embedding Models — Which One to Choose?

```
FREE MODELS (Run on your machine):
═══════════════════════════════════
┌────────────────────────────┬──────┬────────────┬─────────┐
│ Model                      │ Dims │ Max Tokens │ Quality │
├────────────────────────────┼──────┼────────────┼─────────┤
│ all-MiniLM-L6-v2           │ 384  │ 256        │ ⭐⭐⭐     │
│ all-mpnet-base-v2          │ 768  │ 512        │ ⭐⭐⭐⭐    │
│ bge-large-en-v1.5          │ 1024 │ 512        │ ⭐⭐⭐⭐    │
│ nomic-embed-text-v1.5      │ 768  │ 8192       │ ⭐⭐⭐⭐    │
└────────────────────────────┴──────┴────────────┴─────────┘
Best for: Learning, prototyping, limited budget

PAID MODELS (API calls):
═══════════════════════════════════
┌────────────────────────────┬──────┬────────────┬─────────┐
│ Model                      │ Dims │ Max Tokens │ Quality │
├────────────────────────────┼──────┼────────────┼─────────┤
│ text-embedding-3-small     │ 1536 │ 8191       │ ⭐⭐⭐⭐    │
│ text-embedding-3-large     │ 3072 │ 8191       │ ⭐⭐⭐⭐⭐   │
│ voyage-3                   │ 1024 │ 32000      │ ⭐⭐⭐⭐⭐   │
└────────────────────────────┴──────┴────────────┴─────────┘
Best for: Production, high quality needed
```

```python
# ── Using a FREE model (HuggingFace) ──
from sentence_transformers import SentenceTransformer

model = SentenceTransformer("all-MiniLM-L6-v2")

sentences = ["I love dogs", "I adore puppies", "The stock market crashed"]
embeddings = model.encode(sentences)

print(f"Shape: {embeddings.shape}")  # (3, 384) → 3 sentences, 384 dimensions each

# Check similarity
from sklearn.metrics.pairwise import cosine_similarity
sim = cosine_similarity([embeddings[0]], [embeddings[1]])
print(f"'dogs' vs 'puppies': {sim[0][0]:.4f}")  # ~0.83 (very similar!)

sim = cosine_similarity([embeddings[0]], [embeddings[2]])
print(f"'dogs' vs 'stock market': {sim[0][0]:.4f}")  # ~0.05 (not similar!)


# ── Using a PAID model (OpenAI) ──
from openai import OpenAI
client = OpenAI()

response = client.embeddings.create(
    model="text-embedding-3-small",
    input=["I love dogs", "I adore puppies"]
)

emb1 = response.data[0].embedding  # 1536 dimensions
emb2 = response.data[1].embedding  # 1536 dimensions
```

### 8.4 Dimension Size — What Does It Mean?

```
Dimensions = How many numbers are in the vector.

ANALOGY: Describing a person.

3 dimensions (very basic):
  [tall, male, old]
  → You can tell people apart, but many will look the same.

384 dimensions (good):
  [height, weight, hair_color, eye_color, ..., shoe_size]
  → Much better differentiation!

1536 dimensions (great):
  [height, weight, hair_color, eye_color, ..., favorite_food, walking_speed, ...]
  → Very detailed, almost unique descriptions!

3072 dimensions (best):
  Even more detailed → Even better at distinguishing similar texts!


TRADEOFF:
  More dimensions → Better quality BUT more storage and slower search
  Fewer dimensions → Less quality BUT faster and cheaper
```

```python
# Comparison of dimension sizes:

# 384 dimensions (all-MiniLM-L6-v2)
import sys
import numpy as np

vec_384 = np.random.rand(384)
vec_1536 = np.random.rand(1536)
vec_3072 = np.random.rand(3072)

print(f"384-dim vector size:  {vec_384.nbytes:,} bytes")   # 3,072 bytes
print(f"1536-dim vector size: {vec_1536.nbytes:,} bytes")  # 12,288 bytes
print(f"3072-dim vector size: {vec_3072.nbytes:,} bytes")  # 24,576 bytes

# For 1 million documents:
print(f"\n1M docs with 384-dim:  {384 * 4 * 1_000_000 / 1e9:.2f} GB")   # 1.54 GB
print(f"1M docs with 1536-dim: {1536 * 4 * 1_000_000 / 1e9:.2f} GB")  # 6.14 GB
print(f"1M docs with 3072-dim: {3072 * 4 * 1_000_000 / 1e9:.2f} GB")  # 12.29 GB
```

### 8.5 Max Tokens Your Model Can Embed

Every embedding model has a **maximum input length**. If you exceed it, the extra text is **silently truncated** (cut off).

```
MODEL: all-MiniLM-L6-v2 (max 256 tokens)

INPUT: "This is a very long document about refund policies 
that goes on and on for 300 tokens worth of text..."
         ├── First 256 tokens EMBEDDED ──┤← TRUNCATED (ignored!) ─┤

The model only embeds the first 256 tokens.
Everything after that is LOST — silently!
You won't get an error, just a bad embedding.
```

**This is why chunk size matters!**

```python
# Make sure your chunks fit within the model's max tokens

import tiktoken

def check_chunk_fits(chunk: str, max_tokens: int, model: str = "gpt-4o-mini") -> bool:
    """Check if a chunk fits within the embedding model's token limit."""
    encoder = tiktoken.encoding_for_model(model)
    token_count = len(encoder.encode(chunk))
    fits = token_count <= max_tokens
    
    if not fits:
        print(f"WARNING: Chunk has {token_count} tokens but max is {max_tokens}!")
    return fits

# Example:
chunk = "This is a test chunk. " * 100  # Long chunk
check_chunk_fits(chunk, max_tokens=256)
# WARNING: Chunk has 700 tokens but max is 256!

# SOLUTION: Make chunk_size smaller than model's max tokens
# If model max = 256 tokens ≈ 1000 characters → set chunk_size < 1000
```

---

## 9. Vector DB / Storage

### 9.1 What is a Vector Database?

A **vector database** is a specialized database designed to:
1. **Store** high-dimensional vectors (those lists of numbers)
2. **Search** for the most similar vectors FAST

```
REGULAR DATABASE:
  "Find all users WHERE name = 'Alice'"
  → Exact match search. Either it matches or it doesn't.

VECTOR DATABASE:
  "Find the 3 vectors MOST SIMILAR to this query vector"
  → Similarity search. Returns results ranked by closeness.
```

### 9.2 SQL vs NoSQL vs Vector DB — A Clear Comparison

```
SQL DATABASE (PostgreSQL, MySQL)
════════════════════════════════
What it stores:  Structured tables with rows and columns
How you query:   SELECT * FROM users WHERE age > 25
Best for:        User accounts, transactions, inventory
Similarity:      ❌ Not designed for it

Example:
  ┌─────┬───────┬─────┐
  │ id  │ name  │ age │
  ├─────┼───────┼─────┤
  │ 1   │ Alice │ 30  │
  │ 2   │ Bob   │ 25  │
  └─────┴───────┴─────┘


NoSQL DATABASE (MongoDB, DynamoDB)
════════════════════════════════════
What it stores:  Flexible JSON-like documents
How you query:   db.users.find({"age": {"$gt": 25}})
Best for:        Flexible schemas, nested data, rapid prototyping
Similarity:      ❌ Not designed for it

Example:
  {
    "name": "Alice",
    "age": 30,
    "hobbies": ["reading", "hiking"]
  }


VECTOR DATABASE (ChromaDB, Pinecone, FAISS)
════════════════════════════════════════════
What it stores:  Vectors (lists of numbers) + metadata
How you query:   "Find 5 nearest neighbors to this vector"
Best for:        Semantic search, RAG, recommendation systems
Similarity:      ✅ Built exactly for this!

Example:
  {
    "id": "chunk_1",
    "text": "Refund policy allows returns within 30 days.",
    "vector": [0.12, -0.45, 0.78, ..., 0.33],  # 1536 dimensions
    "metadata": {"source": "policy.pdf", "page": 3}
  }
```

### 9.3 Why Indexing? What is it?

When you have millions of vectors, you can't compare each one to the query — it's too slow.

**Indexing** creates a data structure that makes searching much faster.

```
WITHOUT INDEX (Brute Force):
════════════════════════════
Query vector → Compare with vector 1
             → Compare with vector 2
             → Compare with vector 3
             → ...
             → Compare with vector 1,000,000
Time: O(n) = 1,000,000 comparisons 🐌 SLOW


WITH HNSW INDEX:
════════════════════════════
Vectors are organized in a graph structure.
The search jumps through "layers" to quickly zoom in.

Layer 3 (top):     [A] ─────── [B]          ← Few nodes, big jumps
Layer 2:        [A] ── [C] ── [B] ── [D]    ← More nodes
Layer 1:     [A]─[E]─[C]─[F]─[B]─[G]─[D]   ← Even more nodes
Layer 0:  [A][H][E][I][C][J][F][K][B][L][G][M][D]  ← All nodes

Search: Start at top → jump to closest → drop down → jump → drop → found!
Time: O(log n) = ~20 comparisons for 1 million vectors ⚡ FAST
```

```python
# ChromaDB handles indexing automatically!
import chromadb

client = chromadb.PersistentClient(path="./chroma_db")
collection = client.create_collection(
    name="my_docs",
    metadata={
        "hnsw:space": "cosine",       # Distance metric
        "hnsw:M": 16,                 # Max connections per node
        "hnsw:ef_construction": 200   # Build-time accuracy
    }
)
# HNSW index is created automatically when you add vectors!
```

### 9.4 Popular Vector Databases Compared

```
FOR LEARNING / PROTOTYPING:
  ChromaDB    → Easiest to use, runs locally, no setup
  FAISS       → Facebook's library, fastest local search

FOR PRODUCTION:
  Pinecone    → Fully managed cloud, easiest production setup
  Weaviate    → Feature-rich, good for complex queries
  Qdrant      → Great filtering, Rust-based (fast)
  Milvus      → Scalable, handles billions of vectors

ALREADY USING POSTGRESQL?
  pgvector    → Extension for PostgreSQL, no new database needed
```

```python
# ── Example: ChromaDB (Recommended for beginners) ──

import chromadb

# Create persistent database (saves to disk)
client = chromadb.PersistentClient(path="./my_chroma_db")

# Create a collection (like a table)
collection = client.get_or_create_collection("documents")

# Add documents (ChromaDB auto-generates embeddings!)
collection.add(
    documents=[
        "Refund policy: Returns within 30 days.",
        "Shipping: Free over $50, Express $12.99.",
        "Warranty: 1-year limited coverage.",
        "CEO: Alice Johnson, founded 2010."
    ],
    metadatas=[
        {"topic": "refund", "page": 1},
        {"topic": "shipping", "page": 2},
        {"topic": "warranty", "page": 3},
        {"topic": "company", "page": 4}
    ],
    ids=["doc1", "doc2", "doc3", "doc4"]
)

print(f"Stored {collection.count()} documents")

# Query
results = collection.query(
    query_texts=["How much does shipping cost?"],
    n_results=2  # Top 2 results
)

print("\nQuery: 'How much does shipping cost?'")
for doc, dist in zip(results["documents"][0], results["distances"][0]):
    print(f"  [{dist:.4f}] {doc}")

# Output:
#   [0.2134] Shipping: Free over $50, Express $12.99.
#   [0.8921] Refund policy: Returns within 30 days.


# ── Example: FAISS (For speed) ──

import numpy as np
import faiss

# Create some random embeddings (in real life, use an embedding model)
dimension = 384
num_vectors = 10000
vectors = np.random.rand(num_vectors, dimension).astype('float32')

# Normalize vectors (required for cosine similarity)
faiss.normalize_L2(vectors)

# Create FAISS index
index = faiss.IndexFlatIP(dimension)  # IP = Inner Product (cosine after normalization)
index.add(vectors)

print(f"FAISS index has {index.ntotal} vectors")

# Search
query = np.random.rand(1, dimension).astype('float32')
faiss.normalize_L2(query)

distances, indices = index.search(query, k=5)  # Top 5
print(f"Top 5 matches: {indices[0]}")
print(f"Scores: {distances[0]}")
```

---

## 10. Query Embedding

### 10.1 What Happens When a User Asks a Question?

The user's question must be converted into a vector so we can search the vector database.

```
User: "How do I return a product?"
              │
              ▼
   ┌────────────────────┐
   │  EMBEDDING MODEL   │  ← Same model used for documents!
   │ text-embedding-3-  │
   │ small              │
   └─────────┬──────────┘
             ▼
   [0.13, -0.44, 0.77, 0.34, ..., 0.55]
             │
             ▼
   ┌────────────────────┐
   │  VECTOR DATABASE   │  ← Search for similar vectors
   │  (ChromaDB/FAISS)  │
   └────────────────────┘
```

### 10.2 Why Must We Use the SAME Model?

This is one of the most common beginner mistakes. Let me explain clearly:

```
Different models produce vectors in DIFFERENT "languages."

MODEL A (OpenAI text-embedding-3-small):
  "dogs"  → [0.82, 0.15, 0.91, ...]    ← These numbers are in "Language A"
  
MODEL B (all-MiniLM-L6-v2):
  "dogs"  → [0.34, -0.67, 0.12, ...]   ← These numbers are in "Language B"

They represent the SAME word, but the numbers are COMPLETELY DIFFERENT!
It's like converting Celsius to Fahrenheit — you can't mix them.
```

```python
# PROOF: Same text, different models, different embeddings

from sentence_transformers import SentenceTransformer
from openai import OpenAI

text = "I love dogs"

# Model A: MiniLM
model_a = SentenceTransformer("all-MiniLM-L6-v2")
embedding_a = model_a.encode([text])[0]

# Model B: mpnet
model_b = SentenceTransformer("all-mpnet-base-v2")
embedding_b = model_b.encode([text])[0]

print(f"Model A dimensions: {len(embedding_a)}")    # 384
print(f"Model B dimensions: {len(embedding_b)}")    # 768
print(f"Model A first 5: {embedding_a[:5]}")
print(f"Model B first 5: {embedding_b[:5]}")

# Completely different numbers! You can't compare them!
# Even the DIMENSIONS are different (384 vs 768)!
```

**Rule:** Whatever model you used to embed your documents, use the EXACT SAME model to embed queries.

```python
# CORRECT APPROACH:

EMBEDDING_MODEL = "text-embedding-3-small"  # Define once, use everywhere

# During INDEXING (embedding documents):
doc_embedding = get_embedding("Refund policy...", model=EMBEDDING_MODEL)

# During QUERYING (embedding user question):
query_embedding = get_embedding("What is the refund policy?", model=EMBEDDING_MODEL)
# ↑ Same model! Now the vectors are comparable.
```

---

## 11. Retrieval

### 11.1 How Does Retrieval Work?

After embedding the query, we search the vector database for the **closest** chunks.

```
SIMPLIFIED VIEW:
═══════════════

Your query vector:  [0.82, 0.15, 0.91]

Vector Database:
  Chunk A: [0.80, 0.17, 0.89]  → Distance: 0.03 (VERY CLOSE!) ← Retrieved
  Chunk B: [0.78, 0.20, 0.85]  → Distance: 0.08 (Close)       ← Retrieved
  Chunk C: [0.10, 0.85, 0.05]  → Distance: 0.95 (FAR)         ← Not retrieved
  Chunk D: [0.75, 0.22, 0.80]  → Distance: 0.12 (Moderate)    ← Retrieved
  Chunk E: [0.05, 0.90, 0.10]  → Distance: 0.98 (VERY FAR)    ← Not retrieved

Top K=3: Return chunks A, B, D
```

### 11.2 Retrieval Methods

#### Method 1: Similarity Search (Semantic/Vector Search)

This is the most common method. It finds chunks with **similar meaning**, even if the words are different.

```
Query: "How do I return an item?"
Found: "Our refund policy allows returns within 30 days."

Notice: The query says "return an item" and the document says "refund policy"
  and "returns." No exact word match, but the MEANING is similar!
  This is the magic of semantic search.
```

```python
# Semantic search with ChromaDB
results = collection.query(
    query_texts=["How do I return an item?"],
    n_results=3
)

# The database converts the query to a vector and finds
# the 3 most similar document vectors
```

#### Method 2: Keyword Search (BM25)

Traditional text search — matches **exact words**.

```
Query: "refund policy"
Searches for documents containing the words "refund" AND "policy"

Found: "The refund policy allows returns within 30 days."
  ↑ Contains both "refund" and "policy"

NOT Found: "You can return items for a full money-back guarantee."
  ↑ Same meaning, but doesn't contain the exact words "refund" or "policy"
```

```python
# Keyword search with BM25 (a popular algorithm)
# pip install rank-bm25

from rank_bm25 import BM25Okapi

# Your document chunks
documents = [
    "The refund policy allows returns within 30 days.",
    "Products must be in original packaging for returns.",
    "Our CEO Alice founded the company in 2010.",
    "Express shipping costs $12.99 for 2-3 day delivery."
]

# Tokenize (split into words)
tokenized_docs = [doc.lower().split() for doc in documents]

# Create BM25 index
bm25 = BM25Okapi(tokenized_docs)

# Search
query = "refund policy"
query_tokens = query.lower().split()
scores = bm25.get_scores(query_tokens)

# Show results sorted by score
for score, doc in sorted(zip(scores, documents), reverse=True):
    print(f"  Score {score:.2f}: {doc}")

# Output:
#   Score 1.47: The refund policy allows returns within 30 days.
#   Score 0.00: Products must be in original packaging for returns.
#   Score 0.00: Our CEO Alice founded the company in 2010.
#   Score 0.00: Express shipping costs $12.99 for 2-3 day delivery.
```

#### Method 3: Hybrid Search (Recommended!) ✅

Combines **semantic search** + **keyword search** for the best of both worlds.

```
WHY HYBRID?

Semantic search ALONE fails when:
  Query: "What is error code E-4021?"
  Semantic search might find generic "error handling" docs
  but miss the specific code "E-4021" ← Keywords matter here!

Keyword search ALONE fails when:
  Query: "How to return items?"
  Keyword search looks for "return" and "items"
  but misses: "Our refund policy allows sending products back."
  ← No exact word match, but same meaning!

HYBRID catches both:
  Semantic: Finds docs with similar MEANING
  Keyword: Finds docs with exact TERMS
  Combine both → Better results!
```

```python
# Hybrid Search Implementation

import numpy as np
from rank_bm25 import BM25Okapi


def hybrid_search(query: str,
                  documents: list[str],
                  embeddings: np.ndarray,
                  query_embedding: np.ndarray,
                  alpha: float = 0.5,
                  top_k: int = 3) -> list[dict]:
    """
    Combine semantic and keyword search.
    
    Args:
        query: User's question
        documents: List of document chunks
        embeddings: Pre-computed document embeddings (numpy array)
        query_embedding: Query embedding (numpy array)
        alpha: Weight for semantic vs keyword (1.0 = all semantic, 0.0 = all keyword)
        top_k: Number of results to return
    """
    # ── SEMANTIC SCORES ──
    # Cosine similarity between query and each document
    from numpy.linalg import norm
    semantic_scores = np.array([
        np.dot(query_embedding, doc_emb) / (norm(query_embedding) * norm(doc_emb))
        for doc_emb in embeddings
    ])
    
    # Normalize to 0-1 range
    sem_min, sem_max = semantic_scores.min(), semantic_scores.max()
    if sem_max > sem_min:
        semantic_scores = (semantic_scores - sem_min) / (sem_max - sem_min)
    
    # ── KEYWORD SCORES (BM25) ──
    tokenized = [doc.lower().split() for doc in documents]
    bm25 = BM25Okapi(tokenized)
    keyword_scores = bm25.get_scores(query.lower().split())
    
    # Normalize to 0-1 range
    kw_min, kw_max = keyword_scores.min(), keyword_scores.max()
    if kw_max > kw_min:
        keyword_scores = (keyword_scores - kw_min) / (kw_max - kw_min)
    
    # ── COMBINE ──
    hybrid_scores = alpha * semantic_scores + (1 - alpha) * keyword_scores
    
    # ── RANK ──
    top_indices = np.argsort(hybrid_scores)[::-1][:top_k]
    
    results = []
    for idx in top_indices:
        results.append({
            "document": documents[idx],
            "hybrid_score": hybrid_scores[idx],
            "semantic_score": semantic_scores[idx],
            "keyword_score": keyword_scores[idx]
        })
    
    return results


# Usage example:
# results = hybrid_search(
#     query="error code E-4021",
#     documents=all_chunks,
#     embeddings=chunk_embeddings,
#     query_embedding=query_emb,
#     alpha=0.6,  # Slightly favor semantic
#     top_k=3
# )
```

### 11.3 Top K — How Many Chunks to Retrieve?

```
Top K = The number of chunks you retrieve from the database.

K=1: Just the single best match
  ✅ Most focused
  ❌ Might miss important context in other chunks

K=3: Three best matches (RECOMMENDED for most cases)
  ✅ Good balance between relevance and coverage
  ✅ Usually captures the full context needed

K=5: Five best matches
  ✅ More context for complex questions
  ❌ Risk of including less relevant chunks

K=10+: Many matches
  ✅ Maximum coverage
  ❌ Dilutes the good results with mediocre ones
  ❌ Uses up more of the LLM's context window
  ❌ More expensive (more tokens in the prompt)
```

```python
# The effect of different K values:

results_k1 = collection.query(query_texts=["refund policy"], n_results=1)
results_k3 = collection.query(query_texts=["refund policy"], n_results=3)
results_k10 = collection.query(query_texts=["refund policy"], n_results=10)

# K=1:  ["Refund policy allows returns within 30 days."]
# K=3:  ["Refund policy allows...", "Products must be unused...", "Contact support..."]
# K=10: [... plus 7 less-relevant chunks about shipping, warranty, etc.]
```

### 11.4 Distance Metrics — How to Measure Similarity

When comparing two vectors, we need a way to measure "how close" they are.

#### Cosine Similarity (Most Popular for Text) ✅

Measures the **angle** between two vectors, ignoring their length.

```
Think of two arrows:
  → →   Pointing same direction = Similar (cosine ≈ 1.0)
  → ↑   Perpendicular = Unrelated (cosine ≈ 0.0)
  → ←   Pointing opposite = Opposite meaning (cosine ≈ -1.0)

It only cares about DIRECTION, not LENGTH.
A short arrow and a long arrow pointing the same way are still similar.
```

```python
import numpy as np

def cosine_similarity(a, b):
    """Calculate cosine similarity between two vectors."""
    return np.dot(a, b) / (np.linalg.norm(a) * np.linalg.norm(b))

# Similar meaning
v1 = np.array([0.8, 0.1, 0.9])  # "I love dogs"
v2 = np.array([0.7, 0.2, 0.85]) # "I adore puppies"
print(f"dogs vs puppies: {cosine_similarity(v1, v2):.4f}")  # ~0.99 (very similar!)

# Different meaning
v3 = np.array([-0.5, 0.8, -0.3]) # "The stock market crashed"
print(f"dogs vs stocks: {cosine_similarity(v1, v3):.4f}")   # ~-0.14 (not similar!)
```

#### Euclidean Distance (L2)

Measures the **straight-line distance** between two points.

```
Think of two points on a map:
  Point A (2, 3) and Point B (5, 7)
  Distance = √((5-2)² + (7-3)²) = √(9 + 16) = √25 = 5

  0 = identical points
  Small number = close together (similar)
  Large number = far apart (different)
```

```python
def euclidean_distance(a, b):
    """Calculate Euclidean distance between two vectors."""
    return np.linalg.norm(np.array(a) - np.array(b))

v1 = [0.8, 0.1, 0.9]
v2 = [0.7, 0.2, 0.85]
v3 = [-0.5, 0.8, -0.3]

print(f"dogs vs puppies: {euclidean_distance(v1, v2):.4f}")  # ~0.17 (close!)
print(f"dogs vs stocks:  {euclidean_distance(v1, v3):.4f}")  # ~1.83 (far!)
```

#### Dot Product

Measures both **direction AND magnitude** (length).

```python
def dot_product(a, b):
    """Calculate dot product of two vectors."""
    return np.dot(a, b)

v1 = np.array([0.8, 0.1, 0.9])
v2 = np.array([0.7, 0.2, 0.85])

print(f"Dot product: {dot_product(v1, v2):.4f}")  # 1.345
```

#### When to Use Which?

```
┌───────────────────┬──────────────────────────────────────┐
│ Metric            │ When to Use                          │
├───────────────────┼──────────────────────────────────────┤
│ Cosine Similarity │ DEFAULT for text. ✅                  │
│                   │ Works best when vectors are NOT       │
│                   │ normalized. Most vector DBs use this. │
├───────────────────┼──────────────────────────────────────┤
│ Euclidean (L2)    │ When the MAGNITUDE (length) of        │
│                   │ vectors matters (e.g., image features)│
├───────────────────┼──────────────────────────────────────┤
│ Dot Product       │ When vectors are already NORMALIZED.  │
│                   │ Fastest to compute.                   │
└───────────────────┴──────────────────────────────────────┘

For RAG with text: Just use COSINE SIMILARITY. It's the standard.
```

---

## 12. Prompt Construction

### 12.1 What is Prompt Construction?

After retrieving relevant chunks, we need to **assemble a prompt** that tells the LLM:
1. **Who you are** (system prompt)
2. **What context to use** (retrieved chunks)
3. **What to answer** (user's question)

```
THE ANATOMY OF A RAG PROMPT:
════════════════════════════

┌─────────────────────────────────────────────┐
│ SYSTEM PROMPT (Sets behavior)               │
│ "You are a helpful customer support agent.  │
│  Answer ONLY from the given context."       │
├─────────────────────────────────────────────┤
│ CONTEXT (Retrieved chunks)                  │
│ "Chunk 1: Refund policy: 30 days..."        │
│ "Chunk 2: Products must be unused..."       │
│ "Chunk 3: Contact support@acme.com..."      │
├─────────────────────────────────────────────┤
│ QUESTION (User's query)                     │
│ "What is the refund policy?"                │
├─────────────────────────────────────────────┤
│ INSTRUCTION (How to answer)                 │
│ "Answer concisely based on the context."    │
└─────────────────────────────────────────────┘
```

### 12.2 Prompt Template

```python
# A well-designed prompt template:

PROMPT_TEMPLATE = """Use the following pieces of context to answer the question.
If you don't know the answer based on the context, say "I don't have enough 
information to answer that question." Don't try to make up an answer.

Context:
---
{context}
---

Question: {question}

Helpful Answer:"""


def build_prompt(question: str, chunks: list[str]) -> str:
    """Build the final prompt from retrieved chunks and the question."""
    
    # Combine chunks with separators
    context = "\n\n".join([
        f"[Source {i+1}]: {chunk}" 
        for i, chunk in enumerate(chunks)
    ])
    
    return PROMPT_TEMPLATE.format(
        context=context,
        question=question
    )


# Example:
chunks = [
    "Refund policy: Returns accepted within 30 days of purchase.",
    "Products must be unused and in original packaging.",
    "Contact support@acme.com to initiate returns."
]

prompt = build_prompt("How do I return a product?", chunks)
print(prompt)
```

**Output:**
```
Use the following pieces of context to answer the question.
If you don't know the answer based on the context, say "I don't have enough 
information to answer that question." Don't try to make up an answer.

Context:
---
[Source 1]: Refund policy: Returns accepted within 30 days of purchase.

[Source 2]: Products must be unused and in original packaging.

[Source 3]: Contact support@acme.com to initiate returns.
---

Question: How do I return a product?

Helpful Answer:
```

### 12.3 Context Injection — How Many Chunks?

The number of chunks depends on your LLM's **context window** (max input size):

```
TOKEN BUDGET CALCULATION:
═════════════════════════

Total context window:    4,096 tokens (GPT-3.5)
System prompt:          -  100 tokens
Prompt template:        -   50 tokens
User question:          -   30 tokens
Reserved for answer:    -  500 tokens
─────────────────────────────────────
Available for chunks:    3,416 tokens

If each chunk ≈ 500 tokens:
  3,416 / 500 = ~6 chunks maximum

PRACTICAL RECOMMENDATIONS:
  GPT-3.5 (4K):     2-3 chunks
  GPT-4 (8K):       3-5 chunks
  GPT-4o (128K):    5-15 chunks
  Claude (200K):    10-20 chunks
```

```python
# Smart chunk selection based on token budget

import tiktoken

def select_chunks_within_budget(chunks: list[str],
                                 max_context_tokens: int = 3000,
                                 model: str = "gpt-4o-mini") -> list[str]:
    """Select as many chunks as fit within the token budget."""
    encoder = tiktoken.encoding_for_model(model)
    
    selected = []
    total_tokens = 0
    
    for chunk in chunks:  # Assumes chunks are already sorted by relevance
        chunk_tokens = len(encoder.encode(chunk))
        
        if total_tokens + chunk_tokens > max_context_tokens:
            break  # No more room
            
        selected.append(chunk)
        total_tokens += chunk_tokens
    
    print(f"Selected {len(selected)}/{len(chunks)} chunks ({total_tokens} tokens)")
    return selected
```

### 12.4 System Prompt — Setting the AI's Behavior

```python
# ── BASIC system prompt ──
basic_system = "You are a helpful assistant. Answer based on the provided context."


# ── DETAILED system prompt (RECOMMENDED) ──
detailed_system = """You are an expert customer support assistant for Acme Corporation.

YOUR ROLE:
- Answer customer questions accurately using ONLY the provided context.
- Be friendly, professional, and concise.

RULES:
1. ONLY use information from the provided context to answer.
2. If the context doesn't contain the answer, say:
   "I don't have that information. Please contact support@acme.com or call 1-800-ACME-HELP."
3. NEVER make up or assume information not in the context.
4. If the question is ambiguous, ask for clarification.
5. Include specific details (dates, prices, email addresses) when available.
6. Keep answers under 3 sentences unless more detail is needed.

FORMATTING:
- Use bullet points for lists.
- Bold important information like deadlines and prices.
"""


# ── ADVANCED system prompt with few-shot examples ──
advanced_system = """You are an expert customer support assistant for Acme Corporation.
Answer ONLY based on the provided context.

Example 1:
Context: "Returns accepted within 30 days."
Question: "Can I return after 45 days?"
Answer: "Our policy allows returns within 30 days of purchase. Unfortunately, 
a return after 45 days would not be eligible. Please contact support@acme.com 
to discuss your specific situation."

Example 2:
Context: "Express shipping costs $12.99."
Question: "What's the cheapest shipping option?"
Answer: "I have information about Express shipping at $12.99, but I don't have 
details about other shipping tiers in my current context. Please check our 
shipping page or contact support for a complete list of options."

Now answer the user's question following these examples.
"""
```

---

## 13. LLM Generation

### 13.1 What Happens in the Generation Step?

The LLM receives the full prompt (system prompt + context + question) and generates a response.

```
INPUT TO LLM:
┌──────────────────────────────────────────┐
│ System: "You are a helpful assistant..." │
│                                          │
│ Context: "Refund policy: 30 days..."     │
│                                          │
│ Question: "What is the refund policy?"   │
└──────────────────────────────────────────┘
                    │
                    ▼
           ┌───────────────┐
           │     LLM       │
           │  (GPT-4, etc) │
           │               │
           │ Reads context  │
           │ Understands    │
           │ Generates      │
           └───────┬───────┘
                   │
                   ▼
OUTPUT FROM LLM:
"You can return products within 30 days of purchase
 for a full refund. Products must be unused and in
 their original packaging."
```

### 13.2 Model Selection — Which LLM?

```
CHEAP & FAST:
  gpt-4o-mini       → Best value, good for most RAG tasks
  gpt-3.5-turbo     → Cheapest OpenAI, decent quality
  
SMART & POWERFUL:
  gpt-4o            → Best overall quality
  claude-3.5-sonnet → Great reasoning, 200K context
  
FREE & LOCAL:
  llama-3-8b        → Good quality, runs on your machine
  mistral-7b        → Fast, efficient
  phi-3-mini        → Small but capable

RECOMMENDATION FOR RAG:
  Start with gpt-4o-mini → Upgrade to gpt-4o if quality isn't enough
```

```python
from openai import OpenAI
client = OpenAI()

def generate(prompt: str, 
             system: str,
             model: str = "gpt-4o-mini",
             temperature: float = 0.2,
             max_tokens: int = 500) -> str:
    """Generate a response from the LLM."""
    
    response = client.chat.completions.create(
        model=model,
        messages=[
            {"role": "system", "content": system},
            {"role": "user", "content": prompt}
        ],
        temperature=temperature,
        max_tokens=max_tokens
    )
    
    return response.choices[0].message.content
```

### 13.3 Temperature — Controlling Creativity

```
Temperature controls HOW the LLM picks the next word.

temperature = 0.0:
  ALWAYS picks the most likely next word.
  Same input → Same output every time.
  Best for: RAG (factual answers) ✅

  "The refund policy is" → "30" → "days" → "."
  Deterministic, predictable, factual.

temperature = 0.3:
  MOSTLY picks the most likely word, with slight variation.
  Best for: RAG with some natural variation ✅

temperature = 0.7:
  Picks from a wider range of likely words.
  Best for: Creative writing, brainstorming.

  "The refund policy is" → "generous" → "," → "allowing" → ...
  More creative, but might add info not in the context!

temperature = 1.0:
  Almost random word selection.
  Best for: Poetry, maximum creativity.
  NEVER use for RAG!
```

```python
# Demonstration: Same prompt, different temperatures

prompt = "Explain the refund policy based on this context: 'Returns within 30 days.'"

# Temperature 0.0 — Deterministic
answer_0 = generate(prompt, system_prompt, temperature=0.0)
# "The refund policy allows returns within 30 days of purchase."

# Temperature 0.7 — Creative
answer_07 = generate(prompt, system_prompt, temperature=0.7)
# "Our generous refund policy gives you a full 30 days to decide
#  if you're happy with your purchase!"
# ↑ Added "generous" and "happy" — not in the original context!

# Temperature 1.0 — Very Creative
answer_10 = generate(prompt, system_prompt, temperature=1.0)
# "Think of our 30-day return window as a safety net for your
#  shopping adventures! We believe in giving you breathing room."
# ↑ Almost entirely made up! Bad for RAG!
```

### 13.4 Max Tokens — Controlling Output Length

```
max_tokens controls the MAXIMUM length of the LLM's response.

max_tokens = 50:   ~37 words   → Very short answers
max_tokens = 100:  ~75 words   → Brief answers
max_tokens = 250:  ~187 words  → Medium answers
max_tokens = 500:  ~375 words  → Detailed answers ✅ RECOMMENDED
max_tokens = 1000: ~750 words  → Very detailed
max_tokens = 4096: ~3000 words → Maximum (for most models)

NOTE: This is the MAXIMUM. The LLM may finish earlier if the answer
is naturally shorter. You're not paying for unused tokens.
```

### 13.5 Streaming — Real-Time Response

```python
# WITHOUT STREAMING:
# User waits 3-5 seconds... then sees the ENTIRE answer at once.
# Bad user experience for long answers.

response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": prompt}],
    stream=False  # Default
)
print(response.choices[0].message.content)
# [3 second wait] → "Full answer appears all at once."


# WITH STREAMING:
# User sees words appear ONE BY ONE (like ChatGPT typing).
# Much better user experience!

response = client.chat.completions.create(
    model="gpt-4o-mini",
    messages=[{"role": "user", "content": prompt}],
    stream=True  # Enable streaming
)

full_answer = ""
for chunk in response:
    token = chunk.choices[0].delta.content
    if token:
        print(token, end="", flush=True)  # Print each token immediately
        full_answer += token

print()  # New line at the end
```

### 13.6 Hallucination — When AI Makes Things Up

```
HALLUCINATION = The LLM generates information that is NOT in the context
                and is NOT true. It MAKES THINGS UP confidently.

CONTEXT PROVIDED:
  "The refund policy allows returns within 30 days."

USER QUESTION:
  "Can I get a refund after 60 days?"

HALLUCINATED ANSWER ❌:
  "Yes! We offer a special 60-day extended return window for 
   premium members. Just call our VIP line at 1-800-555-GOLD."
   ↑ COMPLETELY MADE UP. Not in context. No VIP line exists.

CORRECT ANSWER ✅:
  "Based on the available information, our refund policy allows 
   returns within 30 days of purchase. I don't have information 
   about exceptions to this policy. Please contact support for 
   more details."
```

**Why does hallucination happen?**

```
LLMs are trained to ALWAYS produce helpful-sounding text.
When they don't have the answer, instead of saying "I don't know,"
they sometimes FABRICATE an answer that SOUNDS plausible.

It's like a student who makes up an answer on an exam
rather than leaving it blank.
```

**How to REDUCE hallucinations in RAG:**

```python
# METHOD 1: Low temperature
temperature = 0.0  # Stick to the most likely (factual) response

# METHOD 2: Strong system prompt instructions
system_prompt = """Answer ONLY based on the provided context.
If the context doesn't contain the answer, say exactly:
"I don't have that information in my knowledge base."
NEVER make up information. NEVER assume. NEVER extrapolate."""

# METHOD 3: Prompt grounding
prompt = f"""Context (ONLY use this to answer):
{context}

Question: {question}

Important: If the answer is NOT in the context above, 
say "This information is not available." Do NOT guess."""

# METHOD 4: Post-generation check
def check_for_hallucination(answer: str, context: str) -> bool:
    """Simple check: does the answer mention things not in context?"""
    # Use another LLM call to verify
    check_prompt = f"""Given this context:
{context}

And this answer:
{answer}

Is the answer fully supported by the context? Reply YES or NO."""
    
    result = generate(check_prompt, "You are a fact checker.", temperature=0.0)
    return "YES" in result.upper()
```

### 13.7 Retrieval Failure — When No Good Chunks Are Found

Sometimes the vector database doesn't contain information relevant to the question.

```
USER: "What is the company's policy on bringing pets to the office?"

VECTOR DB SEARCH:
  Chunk 1: "Refund policy: 30 days" → Distance: 0.85 (not relevant)
  Chunk 2: "Shipping: Free over $50" → Distance: 0.90 (not relevant)
  Chunk 3: "Warranty: 1 year"       → Distance: 0.92 (not relevant)

ALL chunks are far from the query → RETRIEVAL FAILURE!
There is NO pet policy in the knowledge base.
```

```python
def handle_retrieval_failure(query: str, 
                              results: dict, 
                              threshold: float = 0.5) -> str:
    """Handle cases where retrieval finds nothing relevant."""
    
    distances = results["distances"][0]
    chunks = results["documents"][0]
    
    # Check if best result is above the distance threshold
    # (Remember: for cosine DISTANCE, lower = more similar)
    best_distance = distances[0] if distances else float('inf')
    
    if best_distance > threshold:
        # Retrieval failed — no relevant chunks found
        return (
            "I couldn't find information about that in my knowledge base. "
            "This topic may not be covered in our current documents. "
            "Please contact support@acme.com or call 1-800-ACME-HELP "
            "for assistance."
        )
    
    # Retrieval succeeded — proceed with generation
    prompt = build_prompt(query, chunks)
    return generate(prompt, system_prompt)
```

---

## 14. Evaluation

### 14.1 Why Evaluate?

Building a RAG system is easy. Building a **GOOD** one is hard. Evaluation tells you:
- Are you retrieving the right chunks?
- Is the LLM answering correctly?
- Is the system hallucinating?
- Where should you improve?

### 14.2 Key Metrics Explained

```
FAITHFULNESS (Is the answer supported by the context?)
══════════════════════════════════════════════════════
Context: "Returns within 30 days."
Answer: "You can return items within 30 days." → Faithful ✅
Answer: "You can return items within 60 days." → NOT Faithful ❌


ANSWER RELEVANCY (Does the answer address the question?)
══════════════════════════════════════════════════════════
Question: "What is the refund policy?"
Answer: "You can return products within 30 days." → Relevant ✅
Answer: "Our CEO is Alice Johnson." → NOT Relevant ❌


CONTEXT PRECISION (Are the retrieved chunks relevant?)
═══════════════════════════════════════════════════════
Question: "What is the refund policy?"
Chunk 1: "Refund policy: 30 days." → Relevant ✅
Chunk 2: "Our CEO is Alice."      → NOT Relevant ❌
Precision = 1/2 = 50%


CONTEXT RECALL (Did we find ALL relevant chunks?)
══════════════════════════════════════════════════
Your DB has 3 chunks about refund policy.
You retrieved 2 of them.
Recall = 2/3 = 67%
```

### 14.3 Evaluation with RAGAS

```python
# pip install ragas datasets

from ragas import evaluate
from ragas.metrics import (
    faithfulness,
    answer_relevancy, 
    context_precision,
    context_recall
)
from datasets import Dataset

# Prepare your test data
eval_data = {
    "question": [
        "What is the refund policy?",
        "How much does express shipping cost?",
        "Who is the CEO?",
    ],
    "answer": [  # Your RAG system's answers
        "Customers can return products within 30 days for a full refund.",
        "Express shipping costs $12.99 and takes 2-3 business days.",
        "The CEO is Alice Johnson, who founded the company in 2010.",
    ],
    "contexts": [  # The chunks your system retrieved
        ["Refund policy allows returns within 30 days of purchase."],
        ["Express shipping (2-3 business days) is available for $12.99."],
        ["Our CEO is Alice Johnson. She founded Acme Corp in 2010."],
    ],
    "ground_truth": [  # The correct answers (human-written)
        "Products can be returned within 30 days for a full refund.",
        "Express shipping is $12.99.",
        "Alice Johnson is the CEO.",
    ]
}

# Convert to HuggingFace Dataset
dataset = Dataset.from_dict(eval_data)

# Run evaluation
results = evaluate(
    dataset,
    metrics=[
        faithfulness,       # Is answer grounded in context?
        answer_relevancy,   # Does answer match the question?
        context_precision,  # Are retrieved chunks relevant?
        context_recall      # Did we find all relevant chunks?
    ]
)

print(results)
# {
#   'faithfulness': 0.95,        → 95% of claims are supported by context
#   'answer_relevancy': 0.92,    → 92% of answers address the question
#   'context_precision': 0.88,   → 88% of retrieved chunks are relevant
#   'context_recall': 0.85       → 85% of relevant chunks were found
# }
```

### 14.4 Simple Manual Evaluation (No Extra Libraries)

```python
def evaluate_rag_system(rag_function, test_cases: list[dict]) -> dict:
    """
    Simple evaluation framework for RAG.
    
    Each test case should have:
    - question: The query
    - expected_keywords: Words that should appear in the answer
    - expected_not_keywords: Words that should NOT appear (hallucination check)
    """
    results = []
    
    for i, test in enumerate(test_cases):
        answer = rag_function(test["question"])
        
        # Check for expected keywords (relevance)
        expected = test.get("expected_keywords", [])
        found = [kw for kw in expected if kw.lower() in answer.lower()]
        relevance_score = len(found) / len(expected) if expected else 1.0
        
        # Check for unexpected keywords (hallucination)
        unexpected = test.get("expected_not_keywords", [])
        hallucinated = [kw for kw in unexpected if kw.lower() in answer.lower()]
        hallucination_score = 1.0 - (len(hallucinated) / len(unexpected)) if unexpected else 1.0
        
        # Combined score
        combined = (relevance_score + hallucination_score) / 2
        
        results.append({
            "question": test["question"],
            "answer": answer[:100],
            "relevance": relevance_score,
            "no_hallucination": hallucination_score,
            "combined": combined
        })
        
        status = "✅" if combined >= 0.7 else "❌"
        print(f"{status} Test {i+1}: {test['question']}")
        print(f"   Relevance: {relevance_score:.0%} | "
              f"No Hallucination: {hallucination_score:.0%} | "
              f"Combined: {combined:.0%}")
    
    # Overall
    avg = sum(r["combined"] for r in results) / len(results)
    print(f"\n{'='*50}")
    print(f"Overall Score: {avg:.0%}")
    print(f"Passed: {sum(1 for r in results if r['combined'] >= 0.7)}/{len(results)}")
    
    return {"results": results, "overall_score": avg}


# Usage:
test_cases = [
    {
        "question": "What is the refund policy?",
        "expected_keywords": ["30 days", "return", "refund"],
        "expected_not_keywords": ["60 days", "no refunds"]
    },
    {
        "question": "How much is express shipping?",
        "expected_keywords": ["12.99"],
        "expected_not_keywords": ["free"]
    },
    {
        "question": "What color is the sky on Mars?",
        "expected_keywords": ["don't have", "not available"],
        "expected_not_keywords": []  # Should admit it doesn't know
    }
]

# evaluate_rag_system(my_rag_function, test_cases)
```

### 14.5 Evaluation Tools Overview

```
TOOL           │ WHAT IT DOES                        │ BEST FOR
───────────────┼─────────────────────────────────────┼──────────────────
RAGAS          │ Automated RAG metrics                │ Quick quality check
LangSmith      │ Traces every LLM call, visual UI    │ Debugging & monitoring
DeepEval       │ Unit tests for LLMs                  │ CI/CD pipelines
TruLens        │ Feedback functions for RAG            │ Production monitoring
Phoenix/Arize  │ ML observability platform            │ Enterprise monitoring
```

### What to Do with Evaluation Results

```
LOW FAITHFULNESS (answer not grounded):
  → Lower temperature (0.0 - 0.1)
  → Strengthen system prompt ("ONLY answer from context")
  → Add hallucination check

LOW ANSWER RELEVANCY (answer doesn't match question):
  → Improve prompt template
  → Add few-shot examples to system prompt
  → Use a better LLM

LOW CONTEXT PRECISION (irrelevant chunks retrieved):
  → Adjust chunk size
  → Try a better embedding model
  → Add re-ranking step
  → Use hybrid search

LOW CONTEXT RECALL (missing relevant chunks):
  → Increase Top K
  → Use query rewriting
  → Use hybrid search
  → Check if chunking is splitting important info
```

---

## 🎯 Summary: The Complete RAG Mental Model

```
1. COLLECT data from your sources (PDFs, docs, web)
2. CLEAN the data (remove noise, fix formatting)
3. CHUNK into small pieces (500 chars, 50 overlap)
4. EMBED each chunk into a vector (text-embedding-3-small)
5. STORE vectors in a vector DB (ChromaDB)
   ─── Setup complete! ───
6. USER asks a question
7. EMBED the question (same model!)
8. RETRIEVE top K chunks (cosine similarity)
9. BUILD prompt (system prompt + context + question)
10. LLM GENERATES answer (gpt-4o-mini, temp=0.2)
11. EVALUATE regularly (RAGAS, manual tests)
12. IMPROVE based on metrics
```

---

*You now understand every concept in RAG from the ground up. Happy building!*
