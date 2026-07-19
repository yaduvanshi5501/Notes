# 🤖 Introduction to Artificial Intelligence (AI)

Artificial Intelligence (AI) enables machines to perform tasks that normally require human intelligence, such as learning, reasoning, decision-making, and problem-solving. Today, AI powers applications like ChatGPT, self-driving cars, recommendation systems, and medical diagnosis.

---

# 🧠 AI Hierarchy

```
Artificial Intelligence (AI)
        │
        └── Machine Learning (ML)
                │
                └── Deep Learning (DL)
                        │
                        └── Generative AI
```

---

# 🤖 Machine Learning (ML)

Machine Learning is a subset of AI where computers learn from **data** instead of being explicitly programmed.

### Types of ML

- **Supervised Learning** – Learns from labeled data (e.g., spam detection).
- **Unsupervised Learning** – Finds hidden patterns in unlabeled data (e.g., customer segmentation).
- **Reinforcement Learning** – Learns through rewards and penalties (e.g., robotics, games).

---

# 🧠 Deep Learning (DL)

Deep Learning is a specialized type of Machine Learning that uses **neural networks** with multiple layers to learn complex patterns from large datasets.

### Applications

- Computer Vision
- Natural Language Processing (NLP)
- Speech Recognition
- Autonomous Vehicles

---

# ✨ Generative AI

Generative AI creates **new content** such as text, images, audio, and videos by learning patterns from existing data.

Examples:

- ChatGPT
- DALL·E
- Gemini
- Claude

---

# 🏗️ Foundation Models (FM)

Foundation Models are large pre-trained AI models capable of performing multiple tasks.

Examples:

- GPT
- Gemini
- Claude

They can be customized for specific tasks using different techniques.

---

# 📝 LLM vs Foundation Model

- **Foundation Model (FM):** A broad AI model trained on large datasets for various tasks.
- **Large Language Model (LLM):** A type of Foundation Model specifically trained on text and code.

> **Every LLM is a Foundation Model, but not every Foundation Model is an LLM.**

---

# 💬 Prompt Engineering

Prompt Engineering is the practice of writing effective prompts to guide AI models toward better and more accurate responses.

---

# 🎯 Fine-Tuning

Fine-tuning improves a pre-trained Foundation Model by training it on a smaller, task-specific dataset.

---

# 📄 Prompt Stuffing

Prompt Stuffing means providing additional relevant information directly in the prompt so the AI can generate more accurate responses.

---

# 📚 Retrieval-Augmented Generation (RAG)

RAG enhances AI responses by retrieving relevant documents or external knowledge before generating an answer.

Unlike Fine-tuning, **RAG does not modify the model's weights**.

---

# 🔧 Tool Calling

Tool Calling allows AI models to interact with external tools or APIs to access real-time information or perform actions.

**Spring AI** simplifies Tool Calling using the `@Tool` annotation.

---

# 🤖 Agentic AI

Agentic AI goes beyond simple question-answering by planning, reasoning, using tools, and completing multi-step tasks with minimal human intervention.

### Key Components

- 🧠 Reasoning & Planning
- 🔧 Tool Usage
- 💾 Memory
- 🌍 Environment Interaction

### Example

Instead of only suggesting flights, an Agentic AI can:

- Search flights
- Book hotels
- Check company policies
- Add events to your calendar
- Send a confirmation email

—all automatically.

---
