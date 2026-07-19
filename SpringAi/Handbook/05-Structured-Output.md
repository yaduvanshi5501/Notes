# 🚀 Spring AI Handbook

# 05 - Structured Output

> **Goal:** Learn how to convert AI responses directly into Java Objects (POJOs) instead of manually parsing text. Understand Structured Output, `entity()`, `BeanOutputConverter`, JSON generation, response mapping, and best practices.

---

# 📖 Table of Contents

1. What is Structured Output?
2. Why Structured Output?
3. Unstructured vs Structured Response
4. How Structured Output Works
5. Response Conversion Flow
6. BeanOutputConverter
7. entity()
8. JSON Output
9. Supported Data Types
10. Nested Objects
11. Collections
12. Error Handling
13. Validation
14. Best Practices
15. Common Mistakes
16. Interview Questions
17. Quick Revision
18. Master Mind Map

---

# 1️⃣ What is Structured Output?

Normally, AI returns **plain text**.

Example

```text
Java is a programming language developed by Sun Microsystems.
```

If you need a Java object, you must manually parse the text.

Structured Output eliminates this problem.

Instead of plain text, Spring AI converts the response directly into a Java object.

---

# 2️⃣ Why Structured Output?

Without Structured Output:

```text
AI

↓

Text

↓

Manual Parsing

↓

Java Object
```

Problems

- Manual parsing
- Error-prone
- Difficult to maintain
- Fragile if AI changes wording

---

With Structured Output

```text
AI

↓

JSON

↓

Spring AI

↓

Java Object
```

Advantages

- Type-safe
- Cleaner code
- Automatic mapping
- Easier maintenance
- Better readability

---

# 3️⃣ Unstructured vs Structured Response

## Unstructured Response

```text
AI

↓

"The capital of India is New Delhi."
```

Java receives

```java
String
```

---

## Structured Response

AI returns

```json
{
  "country": "India",
  "capital": "New Delhi"
}
```

Spring AI converts it into

```java
CountryInfo
```

No manual parsing required.

---

# 4️⃣ Internal Working

```text
User Prompt

↓

ChatClient

↓

LLM

↓

JSON Response

↓

BeanOutputConverter

↓

Java Object

↓

Application
```

---

# 5️⃣ Complete Response Conversion Flow

```text
Controller

↓

ChatClient

↓

Prompt

↓

LLM

↓

Generated JSON

↓

BeanOutputConverter

↓

Java POJO

↓

Controller Response
```

---

# 6️⃣ BeanOutputConverter

## What is BeanOutputConverter?

BeanOutputConverter is a Spring AI utility that converts AI-generated JSON into Java objects.

Think of it as a bridge between:

```text
JSON

↓

Java Object
```

---

## Responsibilities

- Read JSON
- Match fields
- Create Object
- Populate values
- Return POJO

---

## Internal Flow

```text
JSON

↓

Field Mapping

↓

Reflection

↓

Object Creation

↓

Return POJO
```

---

# 7️⃣ entity()

`entity()` is the easiest way to obtain structured output.

Instead of receiving a String,

you receive a Java object.

Flow

```text
Prompt

↓

LLM

↓

JSON

↓

entity()

↓

Java Object
```

Advantages

- Cleaner code
- Automatic conversion
- Less parsing
- Strong typing

---

# 8️⃣ JSON Generation

The AI must generate valid JSON.

Example

```json
{
  "name": "Satish",
  "age": 25,
  "city": "Patna"
}
```

Spring AI reads this JSON and maps it automatically.

---

## JSON Mapping Flow

```text
AI

↓

JSON

↓

Field Names

↓

Java Fields

↓

POJO
```

---

# 9️⃣ Supported Data Types

Structured Output supports

```text
Primitive Types

↓

String

Integer

Long

Double

Boolean

Enum
```

---

Also supports

```text
Objects

↓

Nested Objects

↓

Collections

↓

Lists

↓

Maps
```

---

# 🔟 Nested Objects

Example Structure

```text
Student

├── Name

├── Age

└── Address
      │
      ├── City
      ├── State
      └── Country
```

Spring AI can automatically populate nested objects.

---

# 1️⃣1️⃣ Collections

AI can generate lists.

Example

```text
Books

↓

Book

Book

Book

↓

Java List<Book>
```

Also supports

- Arrays
- Sets
- Maps

---

# 1️⃣2️⃣ Error Handling

Possible Problems

```text
Invalid JSON

↓

Conversion Failed
```

---

```text
Missing Fields

↓

Null Values
```

---

```text
Wrong Data Type

↓

Mapping Exception
```

---

Always validate responses.

---

# 1️⃣3️⃣ Validation

Before using AI output,

check

- Required fields
- Null values
- Invalid numbers
- Invalid enums
- Business rules

Flow

```text
AI

↓

POJO

↓

Validation

↓

Business Logic
```

---

# 1️⃣4️⃣ Best Practices

✅ Use Structured Output whenever possible.

✅ Keep object models simple.

✅ Validate generated data.

✅ Use meaningful field names.

✅ Keep prompts explicit.

Example

Instead of

```text
Tell me about Java.
```

Use

```text
Return JSON with

name

creator

releaseYear

latestVersion
```

---

# 1️⃣5️⃣ Common Mistakes

❌ Returning plain text when object is expected.

❌ Ignoring validation.

❌ Deep nested objects.

❌ Ambiguous prompts.

❌ Expecting AI to always generate perfect JSON.

❌ Not handling conversion failures.

---

# 1️⃣6️⃣ Interview Questions

## What is Structured Output?

Structured Output converts AI responses directly into Java objects.

---

## Why use Structured Output?

Because manual parsing becomes unnecessary.

---

## What is BeanOutputConverter?

A Spring AI class that converts generated JSON into Java objects.

---

## What does entity() return?

A Java object instead of plain text.

---

## Why validate AI responses?

AI may generate incomplete or invalid JSON.

Validation prevents application errors.

---

# 1️⃣7️⃣ Quick Revision

```text
Prompt

↓

LLM

↓

JSON

↓

BeanOutputConverter

↓

entity()

↓

Java Object

↓

Validation

↓

Business Logic
```

---

# 🌳 Master Mind Map

```text
Structured Output
│
├── ChatClient
│
├── entity()
│
├── BeanOutputConverter
│   ├── Read JSON
│   ├── Map Fields
│   ├── Create Object
│   └── Return POJO
│
├── JSON
│   ├── Primitive Types
│   ├── Nested Objects
│   ├── Collections
│   └── Maps
│
├── Validation
│
├── Error Handling
│
├── Best Practices
│
└── Business Logic
```

---

# 📌 Real-World Use Cases

| Use Case               | Structured Output |
| ---------------------- | ----------------- |
| Resume Parser          | Candidate Object  |
| Invoice Extraction     | Invoice DTO       |
| Customer Support       | Ticket Object     |
| Product Recommendation | Product DTO       |
| Travel Planner         | Trip Plan Object  |
| Student Information    | Student DTO       |
| Weather Report         | Weather DTO       |
| Movie Recommendation   | Movie Object      |

---

# 📚 Summary

After completing this chapter, you should be able to:

- Explain what Structured Output is.
- Describe why it is better than plain text.
- Understand how `entity()` works.
- Explain the role of `BeanOutputConverter`.
- Convert AI-generated JSON into Java objects.
- Handle nested objects and collections.
- Validate AI-generated data before using it.
- Apply Structured Output in real-world Spring AI applications.

---

## 🧠 Chapter Checklist

- ✅ Understand Structured Output
- ✅ Understand JSON Mapping
- ✅ Know `entity()`
- ✅ Know `BeanOutputConverter`
- ✅ Understand Nested Objects
- ✅ Understand Collections
- ✅ Learn Validation
- ✅ Learn Error Handling
- ✅ Ready for Real Projects

> **Next File:** [📂 06-Advisors-Streaming.md](./06-Advisors-Streaming.md)
