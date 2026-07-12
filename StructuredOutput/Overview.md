# 📦 Structured Output in Spring AI

## 🤔 Why Do We Need Structured Output?

In the previous module, we learned about **Model Parameters** like:

- Temperature
- TopP
- MaxTokens

These parameters control **how the AI responds**, but **they do not control the format of the response**.

For example, if you ask the AI to return JSON:

```text
Return employee details in JSON format.
```

The model may return:

```json
{
  "name":"Satish",
  "age":25
}
```

Although it **looks like JSON**, it is still just a **String**.

```text
LLM

↓

"{ "name":"Satish","age":25 }"

↓

String
```

Your application still has to parse it manually.

---

# ❌ Problems Without Structured Output

Without Structured Output:

- AI returns plain text
- Manual JSON parsing
- Error-prone
- Difficult to maintain
- Inconsistent responses

Example

```java
String response = chatClient.prompt(prompt)
        .call()
        .content();
```

Now you must manually convert this string into

- Java Object
- List
- Map

---

# ✅ What is Structured Output?

Structured Output automatically converts AI responses into Java objects.

Instead of getting

```java
String
```

You directly receive

```java
Employee
```

or

```java
List<Employee>
```

or

```java
Map<String,Object>
```

No manual parsing required.

---

# Before vs After

| Without Structured Output | With Structured Output |
|---------------------------|-------------------------|
| Returns String | Returns Java Object |
| Manual Parsing | Automatic Conversion |
| Error-prone | Type Safe |
| More Code | Less Code |
| Hard to Maintain | Easy to Maintain |

---

# How Structured Output Works

```text
Prompt

↓

Spring AI

↓

LLM

↓

JSON String

↓

Structured Output Layer

↓

Java Object
```

Instead of working with raw text, Spring AI gives you typed Java objects.

---

# Two Techniques Used

Spring AI supports two techniques.

---

# 1️⃣ Output Converters

Works with almost every LLM.

Flow

```text
Prompt

↓

Spring AI adds formatting instructions

↓

LLM

↓

JSON String

↓

Output Converter

↓

Java Object
```

Spring AI tells the model

```text
Return only valid JSON.
```

The AI follows the instruction.

Spring AI then converts it into Java objects.

---

# Available Output Converters

| Converter | Purpose |
|-----------|---------|
| BeanOutputConverter | Java Object (POJO/Record) |
| ListOutputConverter | List<T> |
| MapOutputConverter | Map<String,Object> |

---

# 2️⃣ Native Structured Output

Supported by modern models like newer OpenAI models.

Flow

```text
Java Class

↓

JSON Schema

↓

OpenAI

↓

Schema Valid JSON

↓

Java Object
```

Instead of prompt instructions,

Spring AI sends an actual JSON Schema.

Advantages

- More reliable
- No extra markdown
- No invalid JSON
- Production ready

---

# Complete Structured Output Flow

```text
Java Class

↓

Spring AI

↓

JSON Schema / Format Instructions

↓

LLM

↓

JSON Response

↓

Validation

↓

Parsing

↓

Java Object
```

---

# Common Structured Output Types

| Return Type | entity() |
|-------------|----------|
| POJO / Record | `.entity(Employee.class)` |
| Array | `.entity(Employee[].class)` |
| List | `.entity(new ParameterizedTypeReference<List<Employee>>() {})` |
| Map | `.entity(new ParameterizedTypeReference<Map<String,String>>() {})` |

---

# Bean Output Example

Suppose we have

```java
public class Employee{

    private String name;

    private String department;

}
```

Then

```java
Employee employee =
chatClient.prompt("Generate an employee")
        .call()
        .entity(Employee.class);
```

Flow

```text
Prompt

↓

LLM

↓

JSON

↓

BeanOutputConverter

↓

Employee Object
```

No parsing required.

---

# List Output Example

Need multiple employees.

```java
Employee[] employees =
chatClient.prompt("Generate 5 employees")
        .call()
        .entity(Employee[].class);

List<Employee> list =
Arrays.asList(employees);
```

Flow

```text
Prompt

↓

LLM

↓

JSON Array

↓

Employee[]

↓

List<Employee>
```

---

# Map Output Example

Need key-value data.

```java
Map<String,String> capitals =
chatClient.prompt("Top countries and capitals")
        .call()
        .entity(
new ParameterizedTypeReference<Map<String,String>>() {});
```

Output

```text
India  → New Delhi

Japan  → Tokyo

USA    → Washington DC
```

Flow

```text
Prompt

↓

LLM

↓

JSON

↓

MapOutputConverter

↓

Map<String,String>
```

---

# Why ParameterizedTypeReference?

Java removes generic information at runtime.

This is called

```text
Type Erasure
```

So

```java
List<Employee>
```

becomes

```java
List
```

Spring AI cannot identify

```java
Employee
```

inside List.

Therefore use

```java
new ParameterizedTypeReference<List<Employee>>() {}
```

This preserves generic type information.

---

# entity() Method

Spring AI automatically chooses the correct converter.

| Type | Converter Used |
|------|----------------|
| `.entity(Employee.class)` | BeanOutputConverter |
| `.entity(Employee[].class)` | BeanOutputConverter |
| `.entity(List<T>)` | ListOutputConverter |
| `.entity(Map<K,V>)` | MapOutputConverter |

You never create these converters manually.

Spring AI does everything internally.

---

# How entity() Works

```text
Prompt

↓

ChatClient

↓

entity()

↓

Detect Target Type

↓

Choose Converter

↓

Generate JSON Instructions

↓

LLM

↓

Parse JSON

↓

Return Java Object
```

---

# When to Use Which?

| Scenario | Use |
|----------|------|
| One Object | `.entity(Employee.class)` |
| Multiple Objects | `.entity(Employee[].class)` |
| List | `ParameterizedTypeReference<List<T>>()` |
| Map | `ParameterizedTypeReference<Map<K,V>>()` |

---

# Advantages

✅ Automatic Parsing

✅ No Manual JSON Parsing

✅ Type Safe

✅ Cleaner Code

✅ Less Boilerplate

✅ Easy Integration

✅ Production Ready

---

# Real World Uses

- Employee Management
- Student Records
- Product Catalog
- Customer Details
- REST APIs
- Chatbots
- AI Workflow Automation
- Document Processing

---

# Quick Revision

| Concept | Purpose |
|----------|---------|
| Structured Output | Converts AI response into Java Objects |
| BeanOutputConverter | Converts JSON → POJO |
| ListOutputConverter | Converts JSON → List |
| MapOutputConverter | Converts JSON → Map |
| entity() | Automatically selects converter |
| ParameterizedTypeReference | Preserves generic types |

---

# Easy Way to Remember

```text
Prompt

↓

LLM

↓

JSON String

↓

Spring AI

↓

BeanOutputConverter
ListOutputConverter
MapOutputConverter

↓

Java Objects
```

---

# One-Line Interview Definitions

- **Structured Output** → Automatically converts AI-generated text into strongly typed Java objects.
- **BeanOutputConverter** → Converts JSON into a Java POJO or Record.
- **ListOutputConverter** → Converts JSON arrays into Java Lists.
- **MapOutputConverter** → Converts JSON objects into Java Maps.
- **ParameterizedTypeReference** → Preserves generic type information (like `List<T>` and `Map<K,V>`) at runtime.
- **entity()** → Automatically selects the appropriate converter and returns the required Java type.

---
<br><br><br>

<dependencies>

    <!-- Spring Boot Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- Spring AI Azure OpenAI -->
    <dependency>
        <groupId>org.springframework.ai</groupId>
        <artifactId>spring-ai-starter-model-azure-openai</artifactId>
    </dependency>

    <!-- Lombok -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <optional>true</optional>
    </dependency>

    <!-- Spring Boot Test -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

</dependencies>