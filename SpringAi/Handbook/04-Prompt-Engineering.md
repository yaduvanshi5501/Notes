# 🚀 Spring AI Handbook

# 04 - Prompt Engineering

> **Goal:** Learn how to write effective prompts for Large Language Models (LLMs). Understand prompt engineering techniques, prompt templates, message roles, variables, and best practices to get accurate, consistent, and high-quality AI responses.

---

# 📖 Table of Contents

1. What is Prompt Engineering?
2. Why Prompt Engineering?
3. Anatomy of a Prompt
4. Types of Prompts
5. Prompt Components
6. Prompt Templates
7. Prompt Variables
8. Message Roles
9. Few-shot Prompting
10. Zero-shot Prompting
11. Chain-of-Thought Prompting
12. Prompt Chaining
13. Context Management
14. Good vs Bad Prompts
15. Token Optimization
16. Best Practices
17. Common Mistakes
18. Interview Questions
19. Quick Revision
20. Master Mind Map

---

# 1️⃣ What is Prompt Engineering?

Prompt Engineering is the process of designing clear and effective instructions for an AI model so that it generates accurate, relevant, and useful responses.

A **prompt** is simply the input you provide to an AI model.

Think of it like giving instructions to a very intelligent assistant. The clearer your instructions, the better the result.

---

# 2️⃣ Why Prompt Engineering?

Poor prompts often lead to:

- Incorrect answers
- Incomplete responses
- Hallucinations
- Inconsistent output

Well-designed prompts provide:

- Better accuracy
- Consistent responses
- Reduced ambiguity
- Better formatting
- Improved reasoning

---

# 3️⃣ Anatomy of a Prompt

A well-structured prompt usually contains:

```text
Prompt

├── Role
├── Context
├── Task
├── Constraints
├── Format
└── Expected Output
```

---

## Example

```text
Role:
You are a Java Expert.

Context:
I am learning Spring Boot.

Task:
Explain Dependency Injection.

Constraints:
Use simple language.

Output:
Bullet points with examples.
```

---

# 4️⃣ Types of Prompts

## Simple Prompt

```text
Explain Spring Boot.
```

---

## Instruction Prompt

```text
Explain Spring Boot in simple English.
```

---

## Context Prompt

```text
I am a beginner learning Java.

Explain Spring Boot.
```

---

## Persona Prompt

```text
You are a senior Java architect.

Explain Spring Boot.
```

---

## Structured Prompt

```text
Explain Spring Boot.

Return:

Definition

Advantages

Disadvantages

Example

Summary
```

---

# 5️⃣ Prompt Components

```text
Prompt

│

├── Role

├── Context

├── Task

├── Constraints

└── Output Format
```

Each component helps reduce ambiguity.

---

# 6️⃣ Prompt Templates

Instead of writing prompts repeatedly, Spring AI supports reusable templates.

Example concept:

```text
Hello {name}

Explain {topic}

Using {language}
```

Variables are replaced at runtime.

---

## Why Prompt Templates?

- Reusable
- Cleaner code
- Dynamic prompts
- Easier maintenance
- Less duplication

---

## Template Flow

```text
Template

↓

Variables

↓

Generated Prompt

↓

LLM

↓

Response
```

---

# 7️⃣ Prompt Variables

Variables make prompts dynamic.

Example placeholders:

```text
{name}

{topic}

{language}

{level}
```

At runtime:

```text
{name}

↓

Satish

↓

Prompt Updated
```

---

## Benefits

- Reusable prompts
- Personalized responses
- Less hardcoding

---

# 8️⃣ Message Roles

A conversation consists of multiple messages.

```text
Conversation

├── System Message
├── User Message
├── Assistant Message
└── Tool Message
```

---

## System Message

Defines AI behavior.

Example:

```text
You are a Spring Boot Expert.
```

This remains active throughout the conversation.

---

## User Message

Contains the user's request.

Example:

```text
Explain Dependency Injection.
```

---

## Assistant Message

Previous AI responses.

Used for maintaining conversation history.

---

## Tool Message

Contains responses from external tools or functions.

---

# 9️⃣ Zero-Shot Prompting

No examples are provided.

Example:

```text
Translate this sentence into French.
```

The AI relies only on its knowledge.

---

## Advantages

- Fast
- Simple
- Less prompt length

---

# 🔟 One-Shot Prompting

One example is provided before asking the real question.

Example:

```text
Input:
Apple

Output:
Fruit

Now:

Input:
Carrot

Output:
?
```

---

# 1️⃣1️⃣ Few-Shot Prompting

Multiple examples teach the AI the expected format.

Example:

```text
Java -> Programming Language

Python -> Programming Language

MySQL -> Database

Spring Boot ->
```

The model learns from examples.

---

## Flow

```text
Examples

↓

Pattern Learning

↓

AI

↓

New Response
```

---

# 1️⃣2️⃣ Chain-of-Thought Prompting

Instead of asking only for the answer, encourage reasoning.

Example:

```text
Think step by step before answering.
```

Benefits:

- Better reasoning
- Improved accuracy
- Reduced logical errors

---

## Flow

```text
Question

↓

Reasoning

↓

Intermediate Steps

↓

Final Answer
```

---

# 1️⃣3️⃣ Prompt Chaining

Break a large task into multiple prompts.

Instead of:

```text
Build an entire Spring Boot project.
```

Use:

```text
Prompt 1

↓

Generate Architecture

↓

Prompt 2

↓

Generate Entities

↓

Prompt 3

↓

Generate Controllers

↓

Prompt 4

↓

Generate Services
```

Advantages:

- Better quality
- Easier debugging
- Modular responses

---

# 1️⃣4️⃣ Context Management

AI responses improve when relevant context is provided.

Poor:

```text
Explain it.
```

Good:

```text
I am a beginner learning Spring AI.

Explain ChatClient in simple language with examples.
```

---

# 1️⃣5️⃣ Good vs Bad Prompts

## ❌ Bad Prompt

```text
Explain Java.
```

Problems:

- Too broad
- No audience
- No format
- No context

---

## ✅ Good Prompt

```text
You are a Java Trainer.

Explain Java Collections to a beginner.

Use simple English.

Include examples.

Limit the explanation to 300 words.

Provide a summary.
```

Advantages:

- Clear objective
- Audience defined
- Output controlled

---

# 1️⃣6️⃣ Token Optimization

Every prompt consumes tokens.

Large prompts:

- Higher cost
- Slower responses
- More token usage

Keep prompts concise.

---

## Optimization Flow

```text
Long Prompt

↓

Remove Unnecessary Words

↓

Smaller Prompt

↓

Lower Tokens

↓

Faster Response
```

---

# 1️⃣7️⃣ Best Practices

- Clearly define the AI's role.
- Provide enough context.
- Ask one task at a time.
- Specify output format.
- Use prompt templates for reuse.
- Keep prompts concise.
- Use examples when necessary.
- Prefer structured outputs when possible.

---

# 1️⃣8️⃣ Common Mistakes

❌ Vague prompts

❌ Multiple unrelated questions

❌ Missing context

❌ No output format

❌ Extremely long prompts

❌ Assuming the AI knows your project details

---

# 1️⃣9️⃣ Interview Questions

### What is Prompt Engineering?

The process of designing prompts that produce accurate and consistent AI responses.

---

### Why is context important?

Context helps the AI understand the situation and generate more relevant responses.

---

### Difference between Zero-shot and Few-shot prompting?

| Zero-shot | Few-shot |
|------------|----------|
| No examples | Multiple examples |
| Faster | More accurate for specific formats |
| Simpler | Better pattern learning |

---

### What is a Prompt Template?

A reusable prompt containing placeholders that are replaced with values at runtime.

---

### Why use System Messages?

To define the AI's behavior and maintain consistent responses.

---

# 2️⃣0️⃣ Quick Revision

```text
Prompt Engineering

↓

Role

↓

Context

↓

Task

↓

Constraints

↓

Output Format

↓

Prompt Template

↓

Variables

↓

LLM

↓

Response
```

---

# 🌳 Master Mind Map

```text
Prompt Engineering
│
├── Prompt
│   ├── Role
│   ├── Context
│   ├── Task
│   ├── Constraints
│   └── Output Format
│
├── Prompt Templates
│   ├── Variables
│   ├── Dynamic Values
│   └── Reusability
│
├── Message Roles
│   ├── System
│   ├── User
│   ├── Assistant
│   └── Tool
│
├── Prompting Techniques
│   ├── Zero-shot
│   ├── One-shot
│   ├── Few-shot
│   ├── Chain-of-Thought
│   └── Prompt Chaining
│
├── Token Optimization
│
├── Best Practices
│
├── Common Mistakes
│
└── AI Response
```

---

# 📝 Summary

After completing this chapter, you should be able to:

- Explain what Prompt Engineering is.
- Design effective prompts.
- Understand prompt templates and variables.
- Differentiate between System, User, Assistant, and Tool messages.
- Choose between Zero-shot, One-shot, Few-shot, and Chain-of-Thought prompting.
- Optimize prompts for better AI performance.
- Apply best practices to generate accurate, consistent, and efficient AI responses.

> **Next File:** [📂 05-Structured-Output.md](./05-Structured-Output.md)