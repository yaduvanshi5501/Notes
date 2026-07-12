# 🎯 Prompt Engineering in Spring AI

## 🤔 Why Are Good Prompts Important?

The quality of an AI response depends largely on the quality of the prompt.

A well-designed prompt helps the AI generate:

- ✅ Accurate responses
- 💡 Useful answers
- 🎨 Creative outputs
- 📋 Consistent results

> **Rule:** Better Prompt ➜ Better AI Response

---

# 🛠️ What is Prompt Engineering?

**Prompt Engineering** is the process of designing and refining prompts so that an AI model generates accurate, relevant, and structured responses.

### Goal

Guide the AI to produce responses with the desired:

- Tone
- Format
- Detail
- Accuracy

---

# 🌱 Why is Prompt Engineering Important?

AI models continue to evolve, so prompt-writing techniques also keep improving.

Good prompt engineering helps you:

- Improve response quality
- Reduce incorrect answers
- Generate structured outputs
- Build reliable AI applications

---

# 📦 Spring AI Prompt Components

| Component | Purpose |
|----------|----------|
| `Prompt` | Complete input sent to AI |
| `PromptTemplate` | Reusable prompt with placeholders |
| `SystemMessage` | Defines AI behavior |
| `UserMessage` | User's question |
| `AssistantMessage` | Previous AI response (conversation history) |

---

# 💬 Simple Prompt

```java
chatClient.prompt()
          .user(question)
          .call()
          .content();
```

### Best For

- Simple questions
- Quick AI interactions

---

# 👨‍💻 Persona Engineering

A **SystemMessage** defines the AI's role.

```java
.system("You are a Senior Java Developer.")
.user(code)
```

### Example Roles

- Java Expert
- Doctor
- Teacher
- Chef
- Interviewer

---

# 📄 Output Constrained Prompting

Force the AI to return data in a specific format.

Example:

```text
Return valid JSON only.
```

### Benefits

- Easy API integration
- Structured output
- Easier parsing into Java Objects

---

# 🚀 Advanced Prompting Techniques

## 1️⃣ Zero-Shot Prompting

### What?

Ask the AI directly without giving any examples.

```text
Translate:
Hello
```

### Best For

Simple tasks.

---

## 2️⃣ Few-Shot Prompting

### What?

Provide a few examples before asking the AI.

```text
Hello → Hola

Good Morning → Buenos días

Thank You →
```

### Best For

Teaching the AI the expected format.

---

## 3️⃣ Chain-of-Thought (CoT)

### What?

Ask the AI to think step by step before answering.

```text
Solve step by step.
```

### Best For

- Mathematics
- Logical reasoning
- Complex problems

---

## 4️⃣ Prompt Chaining

### What?

Break a large task into multiple smaller prompts.

Example

```text
Article

↓

Extract Key Points

↓

Generate Summary

↓

Create Bullet Points
```

### Best For

Complex workflows.

---

# 📊 Prompting Comparison

| Technique | Best Use Case |
|------------|---------------|
| Zero-Shot | Simple tasks |
| Few-Shot | Pattern learning |
| Chain-of-Thought | Logical reasoning |
| Prompt Chaining | Multi-step workflows |

---

# 💡 Best Practices

- ✅ Write clear prompts.
- ✅ Assign an AI role using `SystemMessage`.
- ✅ Use `PromptTemplate` for reusable prompts.
- ✅ Request structured outputs (JSON, POJO).
- ✅ Use advanced prompting for complex tasks.

---

# 🔄 Prompt Engineering Flow

```text
User Request
      │
      ▼
Prompt Engineering
      │
      ▼
Spring AI Prompt
      │
      ▼
LLM
      │
      ▼
Better AI Response
```

---

# 📝 Quick Revision

- **Prompt Engineering** → Designing better prompts for better AI responses.
- **Prompt** → Complete input sent to AI.
- **PromptTemplate** → Reusable prompt with variables.
- **SystemMessage** → Defines AI behavior.
- **Zero-Shot** → No examples.
- **Few-Shot** → Few examples.
- **Chain-of-Thought** → Step-by-step reasoning.
- **Prompt Chaining** → Multi-step AI workflow.
- Better prompts lead to more accurate, consistent, and reliable AI responses.  


<br><br><br>

# 💻 Prompt Engineering Demo Code

This demo demonstrates **four Prompt Engineering techniques** in Spring AI.

---

# 1️⃣ Zero-Shot Prompting

### Endpoint

```http
POST /promptengineering/zeroshot
```

### Code

```java
String response = chatClient
        .prompt(prompt)
        .call()
        .content();
```

### How it Works

```text
User Prompt
      │
      ▼
ChatClient
      │
      ▼
LLM
      │
      ▼
Response
```

### Explanation

- Sends only a user instruction.
- No examples are provided.
- AI relies completely on its pre-trained knowledge.

**Example**

```text
Prompt:
What is the sentiment of this review?

↓

Response:
POSITIVE
```

---

# 2️⃣ Few-Shot Prompting

### Endpoint

```http
POST /promptengineering/fewshot
```

### Code

```java
String response = chatClient
        .prompt()
        .system(fewShotSystemPrompt)
        .user(userMessage)
        .call()
        .content();
```

### How it Works

```text
Few-Shot Examples
        │
        ▼
System Prompt
        │
        ▼
User Prompt
        │
        ▼
LLM
        │
        ▼
Response
```

### Explanation

- Loads examples from `few-shot-system.st`.
- User provides only the review.
- AI follows the example pattern.

**Example**

```text
Review:
Amazing Laptop

↓

POSITIVE
```

---

# 3️⃣ Chain-of-Thought Prompting

### Endpoint

```http
POST /promptengineering/chainofthought
```

### Code

```java
String response = chatClient
        .prompt()
        .system(systemPrompt)
        .user(prompt)
        .call()
        .content();
```

### System Prompt

```text
Solve the problem
step by step.
```

### How it Works

```text
Question

↓

Reason Step 1

↓

Reason Step 2

↓

Final Answer
```

### Explanation

The System Prompt tells the AI to explain its reasoning before giving the final answer.

---

# 4️⃣ Prompt Chaining

### Endpoint

```http
POST /promptengineering/promptchaining
```

## Step 1

```java
String keyPoints = chatClient.prompt()
        .system("Extract 3 key points.")
        .user(text)
        .call()
        .content();
```

---

## Step 2

```java
String summary = chatClient.prompt()
        .system("Write one sentence summary.")
        .user(keyPoints)
        .call()
        .content();
```

---

## Step 3

```java
String questions = chatClient.prompt()
        .system("Generate two questions.")
        .user(summary)
        .call()
        .content();
```

---

### Flow

```text
Article
   │
   ▼
Extract Key Points
   │
   ▼
Summary
   │
   ▼
Generate Questions
```

### Explanation

Instead of asking one complex question,

the application breaks it into multiple AI calls.

Each response becomes the input for the next prompt.

---

# 📄 Few-Shot Prompt File

**few-shot-system.st**

```text
You are a sentiment classifier.

Review:
Best purchase ever.

Sentiment:
POSITIVE

Review:
Stopped working.

Sentiment:
NEGATIVE

Review:
It is okay.

Sentiment:
NEUTRAL
```

### Why Use a `.st` File?

- Keeps Java code clean.
- Easy to modify prompts.
- Reusable across multiple APIs.

---

# 🔄 Complete Flow

```text
                User
                  │
                  ▼
      PromptEngineeringController
                  │
      ┌───────────┼─────────────┬──────────────┐
      ▼           ▼             ▼              ▼
 Zero-Shot    Few-Shot   Chain-of-Thought  Prompt Chaining
      │           │             │              │
      └───────────┴─────────────┴──────────────┘
                  │
                  ▼
             ChatClient
                  │
                  ▼
             Azure OpenAI
                  │
                  ▼
             AI Response
```

---

# 📝 Quick Revision

| Technique | Code |
|-----------|------|
| Zero-Shot | `prompt(prompt)` |
| Few-Shot | `system(...).user(...)` |
| Chain-of-Thought | `system(reasoning).user(...)` |
| Prompt Chaining | Multiple sequential `chatClient.prompt()` calls |