# 📚 Retrieval-Augmented Generation (RAG) with Spring AI

> **Course Goal:** Learn how to build enterprise-grade AI applications using Spring AI by integrating Large Language Models (LLMs) with Retrieval-Augmented Generation (RAG), embeddings, vector databases, advisors, and ETL pipelines.

---

# 🎯 What You Will Learn

This module teaches how to build intelligent Java/Spring Boot applications that can answer questions using your own documents instead of relying only on the LLM's built-in knowledge.

By the end of this module, you will understand:

- How Spring AI integrates with LLMs
- How Advisors modify AI requests and responses
- How Chat Memory works
- How Embeddings are generated
- How Vector Databases store knowledge
- How Retrieval-Augmented Generation (RAG) works
- How to build an ETL pipeline for enterprise AI applications
- How to process documents and retrieve relevant information
- How to build production-ready AI applications

---

# 📖 Prerequisites

Before learning RAG, you should already understand:

- Generative AI
- Large Language Models (LLMs)
- Spring AI
- Azure OpenAI or GitHub Models
- ChatClient
- ChatModel
- Prompt Engineering
- Prompt Templates
- Prompt Library
- Model Parameters
- Structured Outputs
- Multimodal AI concepts

These topics form the foundation upon which RAG is built.

---

# 🧠 Why RAG?

Imagine asking ChatGPT:

> "What is my company's leave policy?"

The LLM does **not** know your company's internal documents.

Without RAG:

```
User
   │
   ▼
LLM
   │
   ▼
"I don't know."
```

With RAG:

```
User
   │
   ▼
Retriever
   │
Search Company Documents
   │
Relevant Documents
   │
   ▼
LLM
   │
   ▼
Accurate Answer
```

Instead of guessing, the model searches your documents and uses that information to generate a reliable answer.

---

# 🏗️ Spring AI Architecture for RAG

```text
User
   │
   ▼
ChatClient
   │
   ▼
Advisors
   │
   ▼
Retriever
   │
   ▼
Vector Store
   │
Relevant Documents
   │
   ▼
Prompt Augmentation
   │
   ▼
LLM
   │
   ▼
Response
```

Each component has a specific responsibility that together creates a scalable, enterprise-ready AI application.

---

# 🧩 Major Topics Covered

This course is divided into several important sections.

## 1. Spring AI Advisors

Advisors are middleware components in Spring AI.

They execute before or after the LLM call and can inspect, modify, or enhance requests and responses.

Think of Advisors like filters or interceptors in Spring Boot.

They help implement:

- Logging
- Chat Memory
- Prompt modification
- Safety checks
- Authentication
- Response filtering
- Custom business logic

They are essential for building production-ready AI systems.

---

## 2. Chat Memory

The course introduces **In-Memory Chat Memory**.

Instead of every request being independent, previous conversations can be remembered.

Example:

User:

> My name is Satish.

Later:

> What is my name?

Without memory:

```
I don't know.
```

With memory:

```
Your name is Satish.
```

This is achieved using Spring AI Memory Advisors.

---

## 3. Embeddings

The course explains how text is converted into numerical vectors.

Example:

```
"I love Java"

↓

[0.23, 0.54, -0.11, ...]
```

These vectors capture the semantic meaning of the text instead of only the words.

Embeddings allow AI systems to compare meaning rather than exact keywords.

---

## 4. Vector Stores

Once embeddings are created, they are stored in a Vector Database.

Instead of searching documents using keywords, Vector Stores search based on meaning.

Example:

User asks:

> How do I reset my password?

Stored document:

> Steps for changing account credentials.

Although the wording is different, the embedding similarity identifies the correct document.

Vector Stores form the knowledge base for RAG.

---

## 5. Retrieval-Augmented Generation (RAG)

This is the core topic of the course.

RAG combines:

- Retrieval
- Context Augmentation
- LLM Generation

Instead of directly asking the LLM, relevant documents are first retrieved and added to the prompt.

The LLM then answers using those documents.

This improves:

- Accuracy
- Reliability
- Freshness of information
- Domain-specific knowledge

---

## 6. ETL Pipeline

A major portion of the course focuses on building a complete ETL pipeline.

ETL stands for:

- **Extract**
- **Transform**
- **Load**

This pipeline prepares documents for retrieval.

---

### Extract

Documents are read from multiple sources.

Examples include:

- PDF
- Word
- Text
- HTML
- Markdown

Spring AI provides **DocumentReaders** for this purpose.

---

### Transform

The extracted documents are processed before storage.

This includes:

- Cleaning text
- Splitting large documents into chunks
- Creating embeddings

Spring AI uses TextSplitters for chunking.

---

### Load

Generated embeddings are stored inside Vector Stores.

Once stored, they become searchable by similarity search.

---

# 🔄 Complete RAG Workflow

```text
User Question
      │
      ▼
Question Processing
      │
      ▼
Query Rewriting
      │
      ▼
Query Expansion
      │
      ▼
Retriever
      │
      ▼
Vector Store
      │
Relevant Chunks
      │
      ▼
Prompt Augmentation
      │
      ▼
LLM
      │
      ▼
Generated Answer
```

---

# 🛠 Important Spring AI Components

## Advisors

Responsible for:

- Request interception
- Response interception
- Logging
- Prompt modification
- Memory
- Safety
- Custom business rules

Examples:

- MessageChatMemoryAdvisor
- SafeGuardAdvisor
- QuestionAnswerAdvisor
- RetrievalAugmentationAdvisor

---

## Prompt Templates

Allow reusable prompts with placeholders.

Instead of writing prompts repeatedly, templates improve maintainability.

---

## Embedding Models

Convert text into vectors.

Used before storing data inside Vector Stores.

---

## Vector Stores

Store embeddings for similarity search.

They become the searchable knowledge base for RAG applications.

---

## DocumentReaders

Read documents from multiple file formats.

---

## TextSplitters

Break large documents into smaller chunks.

Chunking improves retrieval quality.

---

# 📚 Learning Outcomes Explained

By completing this course, you should be able to:

## Understand Advisors

- Explain their purpose
- Describe how they intercept requests
- Chain multiple advisors together
- Apply default and per-request advisors

---

## Use Built-in Advisors

Implement advisors such as:

- MessageChatMemoryAdvisor
- SafeGuardAdvisor

to improve chat interactions.

---

## Create Custom Advisors

Develop custom middleware that can:

- Add system instructions
- Enrich prompts
- Validate user input
- Remove sensitive data
- Post-process AI responses

---

## Understand Embeddings

Explain:

- What embeddings are
- Why they are needed
- How they enable semantic search

---

## Understand Vector Stores

Explain:

- How embeddings are stored
- How similarity search works
- Why vector databases are essential for RAG

---

## Analyze a Complete RAG Pipeline

Understand how Spring AI performs:

- Retrieval
- Prompt augmentation
- Response generation

using advisors like:

- QuestionAnswerAdvisor
- RetrievalAugmentationAdvisor

---

## Build an ETL Pipeline

Design a complete workflow that:

1. Reads documents
2. Splits documents
3. Generates embeddings
4. Stores embeddings
5. Enables semantic search

---

# 🌟 Why This Module Is Important

Traditional LLMs only know what they were trained on.

Enterprise applications require AI to answer questions about:

- Company policies
- Internal documents
- Research papers
- Product manuals
- Customer support articles
- Knowledge bases

RAG solves this problem by connecting LLMs with external knowledge sources.

This makes AI:

- More accurate
- More reliable
- More up-to-date
- Suitable for enterprise applications

---

# 📝 Quick Revision

- Spring AI provides tools to build AI-powered Java applications.
- Advisors act as middleware around LLM calls.
- Chat Memory enables conversational context.
- Embeddings convert text into vectors.
- Vector Stores store embeddings for semantic search.
- Similarity Search retrieves documents based on meaning.
- RAG combines retrieval with LLM generation.
- ETL pipelines prepare documents for retrieval.
- DocumentReaders extract data from files.
- TextSplitters create smaller chunks for better retrieval.
- Embedding Models generate vector representations.
- QuestionAnswerAdvisor and RetrievalAugmentationAdvisor simplify RAG implementation.
- Production AI applications rely on Advisors, Memory, Vector Stores, and ETL pipelines working together.

---

# 🧠 Mind Map

```text
RAG with Spring AI
│
├── Spring AI
│   ├── ChatClient
│   ├── ChatModel
│   ├── Prompt Templates
│   └── Structured Outputs
│
├── Advisors
│   ├── Logging
│   ├── Memory
│   ├── Prompt Modification
│   ├── Safety
│   └── Custom Logic
│
├── Embeddings
│
├── Vector Stores
│
├── Similarity Search
│
├── RAG
│   ├── Retrieve
│   ├── Augment
│   └── Generate
│
└── ETL
    ├── Extract
    ├── Transform
    └── Load
```

---

# 🔥 Summary

The **RAG with Spring AI** module focuses on building enterprise-grade AI applications that combine the reasoning power of Large Language Models with external knowledge sources. It introduces Spring AI Advisors for managing and customizing AI interactions, explains embeddings and vector stores for semantic search, and demonstrates how Retrieval-Augmented Generation retrieves relevant information before generating responses. The course also covers building a complete ETL pipeline to process documents, create embeddings, and populate vector databases, enabling AI systems to provide accurate, context-aware, and domain-specific answers suitable for real-world enterprise applications.