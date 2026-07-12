# 🖼️ Multimodality in Spring AI

Up to this point, we have learned how Spring AI helps developers:

- ✅ Prompts – Define what the AI should do.
- ✅ Prompt Templates – Create reusable prompts.
- ✅ Prompt Libraries – Organize prompts across the application.
- ✅ Model Parameters – Control creativity, randomness, and response length.
- ✅ Structured Output – Convert AI responses into Java objects.

The next major capability of modern AI systems is **Multimodality**.

---

# 📌 What is Multimodality?

**Multimodality** is the ability of an AI model to understand **multiple types of input together**.

Instead of processing only text, modern AI models can understand:

- 📝 Text + Image
- 📝 Text + Audio
- 🖼️ Image + Question
- 🎙️ Audio + Question
- 🖼️ Image + Text + Audio

This enables AI to reason more like humans.

---

# 🤔 Why Do We Need Multimodality?

Traditional AI models were designed for only one type of data.

| Traditional Model | Supports |
|-------------------|----------|
| NLP Model | Text |
| Vision Model | Images |
| Speech Model | Audio |

If an application needed both text and image understanding, developers had to combine multiple models.

Modern multimodal LLMs remove this limitation.

A single model can understand:

- Text
- Images
- Audio
- Video (some models)

---

# 🚀 Popular Multimodal Models

- OpenAI GPT-4o
- OpenAI GPT-4.1
- Google Gemini 1.5
- Anthropic Claude 3
- Llama 3 Vision
- LLaVA

Spring AI can integrate with all of these using a consistent programming model.

---

# 🏗️ Spring AI Multimodal Architecture

```text
            User
              │
              ▼
      ┌────────────────┐
      │   ChatClient   │
      └────────────────┘
              │
              ▼
      ┌────────────────┐
      │    Prompt      │
      └────────────────┘
              │
     ┌────────┴────────┐
     ▼                 ▼
 Text Message      Media Object
                      │
              ┌───────┴────────┐
              ▼                ▼
          MIME Type        Resource(File)
              │
              ▼
      Multimodal LLM
              │
              ▼
         AI Response
```

---

# 🧩 Core Components

## 1️⃣ ChatClient

The main interface used to communicate with the AI model.

Features:

- Text prompts
- Image prompts
- Audio prompts
- Streaming
- Structured Output

---

## 2️⃣ Prompt

A Prompt contains everything sent to the AI.

```text
Prompt
│
├── User Message
├── Image
└── Audio
```

---

## 3️⃣ Media

A Media object represents a media file.

```java
Media media = new Media(
    MimeTypeUtils.parseMimeType("image/png"),
    resource
);
```

It contains:

- MIME Type
- Resource

---

# 📂 MIME Types

| MIME Type | Meaning |
|-----------|---------|
| image/png | PNG Image |
| image/jpeg | JPEG Image |
| audio/wav | WAV Audio |
| audio/mp3 | MP3 Audio |
| application/pdf | PDF Document |

---

# 📄 Resource

A Resource contains the actual file.

```java
ByteArrayResource resource =
        new ByteArrayResource(image.getBytes());
```

---

# 📩 Sending an Image to the AI

```java
UserMessage userMessage = UserMessage.builder()
        .text("Describe this image")
        .media(media)
        .build();
```

The request contains both:

- Question
- Image

These are sent together to the model.

---

# ⚠️ Important Constraint

| Message Type | Supports Media? |
|--------------|-----------------|
| UserMessage | ✅ Yes |
| SystemMessage | ❌ No |
| AssistantMessage | ❌ No |

Only **UserMessage** can contain images or audio.

---

# 🎯 Real-World Use Cases

### 🖼️ Image Analysis

> "What objects are present in this photo?"

---

### ❓ Visual Question Answering

> "Why is this Spring Boot application failing?"

(with screenshot)

---

### 📄 Document Understanding

Upload a PDF or scanned document and ask:

> "Summarize this invoice."

---

### 📊 Chart Analysis

Upload a graph and ask:

> "Explain the sales trend."

---

### 🩺 Medical Image Assistance

Upload an X-ray and ask:

> "What abnormalities do you notice?"

---

### 🧾 OCR + Reasoning

Upload a receipt and ask:

> "Calculate the total GST."

---

# 🏆 Advantages

- Richer user interaction
- Better contextual understanding
- Less manual preprocessing
- One model handles multiple modalities
- Cleaner application architecture

---

# 💡 Remember

```
Prompts
      ↓
Prompt Templates
      ↓
Prompt Libraries
      ↓
Model Parameters
      ↓
Structured Output
      ↓
⭐ Multimodality ⭐
```

Spring AI combines all these capabilities to build intelligent, production-ready AI applications capable of understanding text, images, and audio in a single request.