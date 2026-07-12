# 📝 Prompt Templates in Spring AI

## 🤔 Why Use Prompt Templates?

In real-world applications, prompts are **dynamic**.

Instead of hardcoding prompts, values like:

- 👤 User Name
- 💼 Role
- ❓ Question
- 📄 Context

change at runtime.

Hardcoding prompts leads to:

- ❌ Duplicate code
- ❌ Difficult maintenance
- ❌ Inconsistent prompts

Prompt Templates solve these problems.

---

# 🚀 What is a PromptTemplate?

A **PromptTemplate** is a reusable prompt containing **placeholders**.

Those placeholders are replaced with actual values at runtime.

### Example

```text
You are a {role}.

Answer:

{question}
```

At runtime

```text
role = Java Expert

question = What is Dependency Injection?
```

becomes

```text
You are a Java Expert.

Answer:

What is Dependency Injection?
```

---

# 🏗️ How PromptTemplate Works

```text
Prompt Template

↓

Replace Variables

↓

Generate Prompt

↓

ChatClient

↓

LLM

↓

AI Response
```

---

# 💻 Basic Example

## Step 1 : Create Template

```java
PromptTemplate template =
new PromptTemplate(
    "You are a {role}. Answer: {question}"
);
```

---

## Step 2 : Replace Variables

```java
Prompt prompt =
template.create(
    Map.of(
        "role","Java Expert",
        "question","What is Spring AI?"
    )
);
```

---

## Step 3 : Send to AI

```java
String response =
chatClient.prompt(prompt)
          .call()
          .content();
```

---

# 🔄 What `create()` Does

When

```java
template.create(...)
```

is called,

Spring AI automatically:

✅ Replaces placeholders

↓

✅ Creates a `UserMessage`

↓

✅ Wraps it inside a `Prompt`

↓

✅ Ready to send to AI

---

# 🎨 render() vs create()

## render()

```java
String text =
template.render(
    Map.of("name","Alice")
);
```

Returns

```text
Hello Alice!
```

> Returns only a **String**.

---

## create()

```java
Prompt prompt =
template.create(...);
```

Returns

```text
Prompt
```

which can be sent directly to

```java
chatClient
```

---

# 💡 Benefits of PromptTemplate

- ♻️ Reusable prompts
- 📖 Cleaner code
- 🛠️ Easy maintenance
- 🎯 Consistent AI responses
- ⚡ Dynamic prompt generation

---

# 📄 External Prompt Templates

Instead of writing prompts inside Java,

store them inside

```text
resources/prompts/
```

Example

```text
email-template.st
```

---

# 📂 Project Structure

```text
src
└── main
    └── resources
          └── prompts
                └── email-template.st
```

---

# 📄 email-template.st

```text
Subject:
Feedback Request for {subject}

Dear {recipient},

Please share your feedback about

{subject}
```

---

# 📥 Loading Template

```java
@Value("classpath:prompts/email-template.st")
Resource emailTemplate;
```

Spring automatically loads the template file.

---

# 🏗️ Creating Prompt

```java
PromptTemplate promptTemplate =
new PromptTemplate(emailTemplate);

Prompt prompt =
promptTemplate.create(
    variables
);
```

---

# 🔄 Complete Flow

```text
email-template.st

↓

PromptTemplate

↓

Replace Variables

↓

Prompt

↓

ChatClient

↓

Azure OpenAI

↓

AI Response
```

---

# ⚙️ Custom Delimiters

## Default Placeholder

```text
{topic}
```

Works well for normal prompts.

---

## Problem

Suppose your prompt contains JSON.

```json
{
   "name":"John"
}
```

Spring AI may think

```text
{name}
```

is a placeholder.

---

# ✅ Solution

Use custom delimiters.

Instead of

```text
{topic}
```

use

```text
<topic>
```

---

## Example

```java
PromptTemplate.builder()
    .renderer(
        StTemplateRenderer.builder()
            .startDelimiterToken('<')
            .endDelimiterToken('>')
            .build()
    )
```

Template

```text
Tell me about

<topic>
```

---

# Why Use Custom Delimiters?

They prevent conflicts with:

- JSON
- YAML
- XML
- Java Code
- Configuration Files

---

# 📊 Prompt Storage Comparison

| Inside Java | External `.st` File |
|--------------|--------------------|
| Hard to maintain | Easy to edit |
| Requires recompilation | No code changes |
| Mixed with Java code | Clean separation |
| Less reusable | Highly reusable |

---

# 📋 Best Practices

✅ Store prompts in `.st` files.

✅ Use placeholders instead of string concatenation.

✅ Use `create()` to generate Prompt objects.

✅ Use `render()` only when a String is needed.

✅ Use custom delimiters when prompts contain JSON or code.

---

# 📝 Quick Revision

- **PromptTemplate** → Reusable prompt with placeholders.
- **create()** → Returns a `Prompt`.
- **render()** → Returns a rendered `String`.
- **Resource** → Loads external `.st` template.
- **.st file** → Keeps prompts outside Java code.
- **Custom Delimiters (`< >`)** → Avoid conflicts with JSON and code.



<br><br><br>
# 💻 PromptTemplate Demo

## 🎯 Use Case

This demo demonstrates different ways to build **dynamic prompts** using **Spring AI PromptTemplate**.

Instead of hardcoding prompts, placeholders are replaced with actual values at runtime, making prompts reusable, maintainable, and easier to manage.

The demo covers four scenarios:

1. 📝 Inline PromptTemplate
2. 📂 External PromptTemplate (`.st` file)
3. ⚙️ PromptTemplate with Custom Delimiters (`< >`)
4. 📖 Structured Prompt Engineering using PromptTemplate

---

# 🏗️ Application Flow

```text
                User
                  │
                  ▼
          Spring Controller
                  │
      ┌───────────┼──────────────┬────────────────┬─────────────────┐
      ▼           ▼              ▼                ▼
 Inline      Resource       Custom Delimiter    Structured Blog
 Template     Template          Template          Template
      │           │              │                │
      └───────────┴──────────────┴────────────────┘
                  │
                  ▼
           PromptTemplate
                  │
                  ▼
      Replace Placeholders
                  │
                  ▼
               Prompt
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

# 1️⃣ Inline PromptTemplate

### Endpoint

```http
POST /email/prompt-template
```

### Code

```java
PromptTemplate template =
new PromptTemplate("""
Send an email to {recipient}
requesting feedback on {subject}.
""");
```

Replace placeholders

```java
Prompt prompt =
template.create(
    Map.of(
        "recipient", recipient,
        "subject", subject
    )
);
```

Send to AI

```java
chatClient.prompt(prompt)
          .call()
          .content();
```

### Flow

```text
Template

↓

Replace Variables

↓

Prompt

↓

AI
```

---

# 2️⃣ External PromptTemplate

### Endpoint

```http
POST /email/resource-template
```

Instead of writing prompts inside Java,

the prompt is stored in

```text
email-template.st
```

### email-template.st

```text
Subject:
Feedback Request for {subject}

Dear {recipient},

Please share your feedback.
```

Load template

```java
@Value("classpath:prompts/email-template.st")
Resource emailTemplate;
```

Create Prompt

```java
PromptTemplate template =
new PromptTemplate(emailTemplate);
```

### Benefits

✅ Cleaner Java Code

✅ Easy Prompt Updates

✅ Reusable

---

# 3️⃣ Custom Delimiter PromptTemplate

### Endpoint

```http
POST /email/custom-delimiter
```

Problem

JSON uses

```json
{
   "service":""
}
```

Default placeholders

```text
{service}
```

conflict with JSON braces.

---

### Solution

Use

```text
<service>
```

instead.

Configuration

```java
PromptTemplate.builder()
    .renderer(
        StTemplateRenderer.builder()
            .startDelimiterToken('<')
            .endDelimiterToken('>')
            .build()
    )
```

Template

```text
Generate JSON

for

<service>

in

<region>
```

### Flow

```text
Template

↓

<service>

↓

payments

↓

Prompt

↓

AI
```

### Best For

- JSON
- XML
- YAML
- Java Code
- Configuration Files

---

# 4️⃣ Structured Prompt Engineering

### Endpoint

```http
POST /blog/structured-blog
```

Loads

```text
blog-template.st
```

instead of writing prompts inside Java.

---

### blog-template.st

```text
Role:
{role}

Audience:
{audience}

Context:
{context}

Instructions:
{steps}

Constraints:
{constraints}

Response Format:
{responseFormat}

Write a blog about

{topic}
```

Variables

```java
Map<String,Object> variables =
Map.of(
    "role",role,
    "topic",topic,
    "audience",audience
);
```

Create Prompt

```java
PromptTemplate template =
new PromptTemplate(blogTemplate);

Prompt prompt =
template.create(variables);
```

Send to AI

```java
chatClient.prompt(prompt)
          .call()
          .content();
```

---

# Why Structured Templates?

Instead of writing

```text
Write a blog about Java.
```

we provide

- Role
- Audience
- Context
- Instructions
- Constraints
- Output Format

Result:

✅ Better responses

✅ More consistent output

✅ Easy to reuse

---

# 📊 PromptTemplate Comparison

| Demo | Purpose |
|-------|---------|
| 📝 Inline Template | Small reusable prompts |
| 📂 Resource Template | Store prompts in `.st` files |
| ⚙️ Custom Delimiter | Avoid conflicts with JSON/Code |
| 📖 Structured Template | Professional prompt engineering |

---



````
package com.infy.prompttemplate.controller;

import java.util.Map;

import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.chat.prompt.Prompt;
import org.springframework.ai.chat.prompt.PromptTemplate;
import org.springframework.ai.template.st.StTemplateRenderer;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.core.io.Resource;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/email")
public class EmailPromptController {

    private final ChatClient chatClient;

    @Value("classpath:prompts/email-template.st")
    private Resource emailTemplate;

    public EmailPromptController(ChatClient.Builder chatClientBuilder) {
        this.chatClient = chatClientBuilder.build();
    }

    @PostMapping("/prompt-template")
    public ResponseEntity<String> generateEmail(
            @RequestParam(defaultValue = "Jack") String recipient,
            @RequestParam(defaultValue = "Q2 project deliverables") String subject) {

        PromptTemplate promptTemplate = new PromptTemplate("""
                Send an email to {recipient} requesting feedback on {subject}.
                """);

        Prompt prompt = promptTemplate.create(
                Map.of(
                        "recipient", recipient,
                        "subject", subject));

        System.out.println(promptTemplate.render(
                Map.of(
                        "recipient", recipient,
                        "subject", subject)));

        String answer = chatClient.prompt(prompt)
                .call()
                .content();

        return ResponseEntity.ok(answer);
    }

    @PostMapping("/resource-template")
    public ResponseEntity<String> resourceTemplatePrompt(
            @RequestParam(defaultValue = "Jack") String recipient,
            @RequestParam(defaultValue = "recent training workshop") String subject) {

        Map<String, Object> variables = Map.of(
                "recipient", recipient,
                "subject", subject);

        PromptTemplate promptTemplate = new PromptTemplate(emailTemplate);

        String answer = chatClient.prompt(promptTemplate.create(variables))
                .call()
                .content();

        return ResponseEntity.ok(answer);
    }

    @PostMapping("/custom-delimiter")
    public ResponseEntity<String> customDelimiterPrompt(
            @RequestParam(defaultValue = "payments") String service,
            @RequestParam(defaultValue = "us-east-1") String region) {

        String template = """
                You are a cloud infrastructure assistant.
                Generate a production-ready JSON config for the <service>
                in the <region>. Choose sensible values for port, replicas,
                and a health-check path.

                Return ONLY valid JSON:

                {
                  "service": "",
                  "region": "",
                  "port": "",
                  "replicas": "",
                  "healthCheck": ""
                }
                """;

        Map<String, Object> variables = Map.of(
                "service", service,
                "region", region);

        PromptTemplate promptTemplate = PromptTemplate.builder()
                .template(template)
                .renderer(
                        StTemplateRenderer.builder()
                                .startDelimiterToken('<')
                                .endDelimiterToken('>')
                                .build())
                .build();

        System.out.println(promptTemplate.render(variables));

        String response = chatClient.prompt(promptTemplate.create(variables))
                .call()
                .content();

        return ResponseEntity.ok(response);
    }
}

````



# 📝 Quick Revision

- **Inline PromptTemplate** → Prompt inside Java.
- **Resource PromptTemplate** → Prompt stored in `.st` file.
- **Custom Delimiter** → Uses `< >` instead of `{ }`.
- **Structured Prompt** → Uses Role, Context, Constraints, and Output Format.
- **create()** → Replaces placeholders and creates a `Prompt`.
- **render()** → Only renders the final prompt as a `String`.