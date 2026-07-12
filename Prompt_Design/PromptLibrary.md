# 📚 Prompt Library Demo

## 🎯 Use Case

This demo shows how to build a **Prompt Library** in Spring AI.

Instead of creating prompts inside Java classes, prompt templates are stored as external `.st` files and loaded automatically at application startup.

The application:

- 📂 Loads all prompt templates from the `resources/prompts` folder.
- 🔍 Selects a prompt based on the requested category.
- 🔄 Replaces placeholders with runtime values.
- 🤖 Sends the generated prompt to the LLM.
- 📝 Returns the AI-generated response.

---

# 🏗️ Application Flow

```text
                  User
                    │
                    ▼
          PromptLibraryController
                    │
                    ▼
          PromptLibraryService
                    │
                    ▼
        PromptLibraryConfig
                    │
     ┌──────────────┼───────────────┐
     ▼              ▼               ▼
 summarize.st   translate.st   sentiment_analysis.st
                    │
                    ▼
            PromptTemplate
                    │
                    ▼
         Replace Variables
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

# 📂 Project Structure

```text
src
└── main
    └── resources
         └── prompts
              ├── summarize.st
              ├── translate.st
              └── sentiment_analysis.st
```

---

# 📦 Components

| Component | Purpose |
|-----------|---------|
| PromptLibraryConfig | Loads all prompt templates |
| PromptLibraryService | Selects and executes prompt |
| PromptLibraryController | REST API |
| PromptTemplate | Creates Prompt |
| ChatClient | Sends Prompt to AI |

---

# 1️⃣ PromptLibraryConfig

Loads every `.st` file automatically.

```java
@Value("classpath:/prompts/*.st")
Resource[] resources;
```

Each template is stored in a Map.

```java
Map<String, PromptTemplate>
```

Example

```text
summarize

↓

summarize.st
```

---

# Flow

```text
resources/prompts

↓

Load all .st files

↓

PromptTemplate

↓

Store in HashMap
```

---

# 2️⃣ PromptLibraryService

Gets the correct prompt.

```java
PromptTemplate template =
promptLibrary.get(category);
```

Creates Prompt.

```java
Prompt prompt =
template.create(data);
```

Calls AI.

```java
chatClient.prompt(prompt)
          .call()
          .content();
```

---

# Service Flow

```text
Category

↓

Find PromptTemplate

↓

Replace Variables

↓

Prompt

↓

ChatClient

↓

LLM
```

---

# 3️⃣ PromptLibraryController

REST Endpoint

```http
POST /prompt-library/execute
```

Receives

```json
{
   "category":"summarize",
   "data":{
      "text":"Spring AI..."
   }
}
```

Calls

```java
service.execute(...)
```

Returns

```text
AI Response
```

---

# 📄 summarize.st

```text
You are a professional editor.

Summarize the text below in

approximately {length} words.

Text:

{text}
```

Used for:

- Article Summarization
- Notes
- Reports

---

# 🌍 translate.st

```text
You are a professional translator.

Translate the text below

into {language}.

{text}
```

Used for:

- Language Translation
- Localization

---

# 😊 sentiment_analysis.st

```text
Analyze the sentiment.

Overall Sentiment

Confidence

Key Emotional Indicators

{text}
```

Used for:

- Customer Reviews
- Feedback Analysis
- Social Media Monitoring

---

# 🔄 Complete Execution Flow

```text
User Request

↓

Category

↓

Prompt Library

↓

Prompt Template

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

# 🎯 Advantages

✅ Prompts stored outside Java

✅ Reusable Prompt Templates

✅ Centralized Prompt Management

✅ Easy to maintain

✅ Easy to add new prompts

---

# 📊 Prompt Library vs PromptTemplate

| PromptTemplate | Prompt Library |
|---------------|----------------|
| Single Template | Collection of Templates |
| Used directly | Selected dynamically |
| Best for small apps | Best for enterprise applications |
| Stored individually | Organized by category |

---

# 📝 Quick Revision

- **Prompt Library** → Collection of reusable PromptTemplates.
- **PromptLibraryConfig** → Loads all `.st` files.
- **PromptLibraryService** → Finds template and creates Prompt.
- **PromptLibraryController** → Executes prompt via REST API.
- **PromptTemplate** → Replaces placeholders.
- **ChatClient** → Sends Prompt to AI.
- Add a new prompt by simply creating a new `.st` file.