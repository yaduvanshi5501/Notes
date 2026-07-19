# 📝 Summary

Spring AI implements **Modular RAG**, inspired by the research paper *Modular RAG: Transforming RAG Systems into LEGO-like Reconfigurable Frameworks*. Instead of treating Retrieval-Augmented Generation as a single fixed pipeline, Spring AI divides it into independent modules that can be combined, replaced, or extended based on application requirements.

Unlike traditional RAG, where retrieval and generation are tightly coupled, Modular RAG separates each concern into its own component. This makes the pipeline flexible, reusable, and suitable for enterprise applications.

---

# 🏗️ Modular RAG Architecture

The complete pipeline consists of four independent modules.

```text
User Query
      ↓
Pre-Retrieval
      ↓
Retrieval
      ↓
Post-Retrieval
      ↓
Generation
      ↓
Grounded Answer
```

Only **Retrieval** and **Generation** are mandatory.

Pre-Retrieval and Post-Retrieval are optional optimization stages that improve retrieval quality and answer accuracy.

---

# 📌 Module 1 — Pre-Retrieval

The purpose of Pre-Retrieval is to improve the user's query before searching the VectorStore.

Instead of searching with the raw query, the pipeline can

- rewrite it,
- compress it,
- translate it,
- or expand it into multiple queries.

```text
Raw Query
      ↓
Transform / Expand
      ↓
Improved Query
      ↓
Vector Search
```

Spring AI provides two categories of preprocessing.

### Query Transformation

Transforms one query into a better query.

Supported transformers include

- `CompressionQueryTransformer`
- `RewriteQueryTransformer`
- `TranslationQueryTransformer`

Each transformer internally calls an LLM, so `chatClient.mutate()` is used to isolate those internal calls from the main advisor chain.

---

### Query Expansion

Instead of one query, multiple semantically different queries are generated.

```text
Original Query
      ↓
Query Expansion
      ↓
Query 1
Query 2
Query 3
      ↓
Multiple Vector Searches
```

`MultiQueryExpander` improves recall by searching the knowledge base from multiple perspectives.

---

# 🔍 Module 2 — Retrieval

After preprocessing, the query reaches the retrieval stage.

Spring AI uses

```text
VectorStoreDocumentRetriever
```

to

- generate query embeddings,
- search the VectorStore,
- apply metadata filters,
- remove low-score results,
- return the Top-K documents.

```text
Query
      ↓
Embedding
      ↓
Vector Search
      ↓
Threshold
      ↓
Top-K
      ↓
Relevant Documents
```

This is the core of every RAG application.

---

# 📄 Module 3 — Post-Retrieval

Retrieved documents are not always ideal for the LLM.

Post-processing improves document quality before prompt augmentation.

Typical operations include

- removing duplicate chunks,
- removing table-of-contents pages,
- filtering very small chunks,
- compressing documents,
- re-ranking relevance,
- limiting context size.

Example pipeline

```text
Retrieved Chunks
      ↓
Remove Noise
      ↓
Remove Duplicates
      ↓
Re-rank
      ↓
Top Documents
```

This stage reduces token usage while improving answer quality.

---

# 🤖 Module 4 — Generation

The final stage prepares the prompt for the language model.

Spring AI uses

```text
ContextualQueryAugmenter
```

to combine

- user question
- retrieved documents

into one augmented prompt.

```text
Question
      +
Retrieved Context
      ↓
Augmented Prompt
      ↓
LLM
      ↓
Answer
```

When no documents are found

```text
allowEmptyContext(false)
```

prevents hallucinations by instructing the model not to answer.

Setting

```text
allowEmptyContext(true)
```

allows fallback to the LLM's pretrained knowledge.

---

# 🔄 Complete Modular Pipeline

```text
User Query
      ↓
Compression / Rewrite / Translation
      ↓
MultiQuery Expansion
      ↓
VectorStoreDocumentRetriever
      ↓
Metadata Filtering
      ↓
Similarity Search
      ↓
Top-K Selection
      ↓
Document Post Processing
      ↓
ContextualQueryAugmenter
      ↓
Chat Model
      ↓
Grounded Answer
```

Each stage is optional except retrieval and generation, allowing developers to build lightweight or highly sophisticated RAG pipelines.

---

# ⚖️ Choosing the Right Components

| Requirement | Recommended Component |
|-------------|-----------------------|
| Multi-turn conversations | CompressionQueryTransformer |
| Verbose user queries | RewriteQueryTransformer |
| Multilingual users | TranslationQueryTransformer |
| Increase recall | MultiQueryExpander |
| Semantic document search | VectorStoreDocumentRetriever |
| Remove duplicate chunks | DocumentPostProcessor |
| Prevent hallucinations | ContextualQueryAugmenter |
| Metadata filtering | FilterExpressionBuilder |

---

# 🚀 Key Takeaways

- Spring AI implements a **Modular RAG** architecture where every stage is an independent component.
- The pipeline consists of **Pre-Retrieval**, **Retrieval**, **Post-Retrieval**, and **Generation** modules.
- **Retrieval** and **Generation** are mandatory; the other stages are optional optimizations.
- `CompressionQueryTransformer`, `RewriteQueryTransformer`, and `TranslationQueryTransformer` improve search quality before retrieval.
- `MultiQueryExpander` increases recall by generating multiple query variations.
- `VectorStoreDocumentRetriever` performs semantic similarity search using embeddings.
- `DocumentPostProcessor` cleans, deduplicates, re-ranks, and trims retrieved documents before they reach the LLM.
- `ContextualQueryAugmenter` combines the user query and retrieved context into the final prompt.
- `allowEmptyContext(false)` prevents hallucinations by refusing to answer when no supporting documents are found.
- Components that internally invoke an LLM (query transformers and query expanders) should use `chatClient.mutate()` to isolate those calls from the main advisor pipeline.
- Start with **Naive RAG (Retrieval + Generation)** and add Pre-Retrieval or Post-Retrieval modules only when higher retrieval quality or answer accuracy is required.