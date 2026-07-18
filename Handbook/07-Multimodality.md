# 🚀 Spring AI Handbook

# 07 - Multimodality

> **Goal:** Understand Multimodal AI, how Spring AI supports multiple input and output types (Text, Images, Audio, PDFs, Documents), how media is processed internally, and where multimodality is used in real-world applications.

---

# 📖 Table of Contents

1. What is Multimodality?
2. Why Multimodality?
3. Types of Modalities
4. Spring AI Multimodal Architecture
5. Media API
6. Image Input
7. Image Understanding
8. PDF Input
9. Audio Input
10. Multi-Input Prompt
11. Multimodal Request Lifecycle
12. Supported AI Models
13. Real World Use Cases
14. Best Practices
15. Common Mistakes
16. Interview Questions
17. Quick Revision
18. Master Mind Map

---

# 1️⃣ What is Multimodality?

A **modality** is a way of communicating information.

Examples

- Text
- Image
- Audio
- Video
- PDF
- Documents

A model that understands **more than one modality** is called a **Multimodal Model**.

---

## Example

Instead of asking

```text
Explain this image.
```

You upload the image and ask

```text
What is happening in this image?
```

The AI understands both

- Image
- Text

This is Multimodality.

---

# 2️⃣ Why Multimodality?

Traditional AI

```text
Text

↓

AI

↓

Text
```

Modern AI

```text
Image

↓

Audio

↓

PDF

↓

Text

↓

AI

↓

Text

↓

Image

↓

JSON
```

---

Advantages

- Better understanding
- Richer conversations
- Document analysis
- Image reasoning
- OCR
- Accessibility

---

# 3️⃣ Types of Modalities

```text
Modalities

├── Text

├── Image

├── Audio

├── PDF

├── Video

├── Documents

└── Structured Data
```

---

## Text

Example

```text
Explain Spring Boot.
```

---

## Image

Example

```text
Describe this image.
```

---

## Audio

Example

```text
Convert speech into text.
```

---

## PDF

Example

```text
Summarize this PDF.
```

---

## Document

Example

```text
Extract invoice information.
```

---

# 4️⃣ Spring AI Multimodal Architecture

```text
                 User

                  │

                  ▼

            Spring Boot

                  │

                  ▼

             ChatClient

                  │

                  ▼

             Prompt Builder

                  │

        ┌─────────┴──────────┐
        │                    │
     Text                Media Objects

        │                    │

        └─────────┬──────────┘

                  ▼

             ChatModel

                  │

                  ▼

          Multimodal LLM

                  │

                  ▼

            ChatResponse
```

---

# 5️⃣ Media API

Spring AI represents non-text content using **Media**.

Instead of sending only text,

you can attach

- Images
- PDFs
- Audio
- Files

---

## Media Flow

```text
Image

↓

Media Object

↓

Prompt

↓

ChatClient

↓

LLM
```

---

Advantages

- Unified API
- Easy attachment
- Multiple media types

---

# 6️⃣ Image Input

Images can be provided together with text instructions.

Example

```text
Prompt

↓

"What is in this picture?"

+

Image

↓

AI
```

---

## Image Processing Flow

```text
Image

↓

Media

↓

Prompt

↓

Vision Model

↓

AI Analysis

↓

Response
```

---

## Typical Questions

- Describe the image.
- Identify objects.
- Extract text.
- Detect colors.
- Explain charts.
- Read handwritten notes.

---

# 7️⃣ Image Understanding

Modern LLMs perform

```text
Image

↓

Object Detection

↓

Scene Understanding

↓

Reasoning

↓

Answer
```

The AI does not simply "see" pixels.

It understands

- Relationships
- Context
- Objects
- Text inside images
- Layout

---

# 8️⃣ PDF Input

Instead of copying text,

provide the PDF.

Flow

```text
PDF

↓

Media

↓

Prompt

↓

LLM

↓

Summary
```

---

Possible Tasks

- Summarization
- Question Answering
- Data Extraction
- Translation
- Report Generation

---

# 9️⃣ Audio Input

Audio can also be processed.

Example

```text
Speech

↓

AI

↓

Text
```

or

```text
Speech

↓

AI

↓

Summary
```

---

Applications

- Meeting Notes
- Voice Assistants
- Interviews
- Customer Calls

---

# 🔟 Multi-Input Prompt

Spring AI allows combining multiple inputs.

Example

```text
Prompt

↓

Image

↓

PDF

↓

Text

↓

LLM
```

The model receives everything together.

---

## Example

```text
Question

+

Invoice PDF

+

Company Logo

↓

Explain the invoice.
```

---

# 1️⃣1️⃣ Complete Request Lifecycle

```text
User

↓

Controller

↓

Prompt

↓

Text

+

Media

↓

ChatClient

↓

ChatModel

↓

Vision/LLM Model

↓

Response

↓

Browser
```

---

# 1️⃣2️⃣ Supported AI Models

Different providers support different modalities.

| Provider     | Text    | Image            | Audio   | PDF     |
| ------------ | ------- | ---------------- | ------- | ------- |
| OpenAI GPT   | ✅      | ✅               | ✅      | ✅      |
| Azure OpenAI | ✅      | ✅               | ✅      | ✅      |
| Ollama\*     | ✅      | Depends on Model | Depends | Depends |
| Hugging Face | Depends | Depends          | Depends | Depends |

> \*Capabilities depend on the specific model you choose.

---

# 1️⃣3️⃣ Real World Use Cases

## Resume Analyzer

```text
Resume PDF

↓

AI

↓

Candidate Summary
```

---

## Invoice Processing

```text
Invoice

↓

AI

↓

Invoice Object
```

---

## Medical Report

```text
Medical PDF

↓

AI

↓

Diagnosis Summary
```

---

## Image Captioning

```text
Image

↓

AI

↓

Description
```

---

## OCR

```text
Image

↓

Extract Text

↓

Return JSON
```

---

## Classroom Assistant

```text
Lecture PDF

↓

AI

↓

Notes

↓

Quiz
```

---

## Chat with Documents

```text
User Question

↓

PDF

↓

AI

↓

Answer
```

---

# 1️⃣4️⃣ Best Practices

✅ Compress large images.

✅ Use clear images.

✅ Keep PDFs readable.

✅ Ask specific questions.

✅ Use structured output for extracted information.

✅ Validate AI-generated answers.

✅ Choose a model that supports the required modality.

---

# 1️⃣5️⃣ Common Mistakes

❌ Uploading blurry images.

❌ Using unsupported file formats.

❌ Sending extremely large PDFs.

❌ Asking vague questions.

❌ Assuming every model supports images.

❌ Ignoring provider limitations.

---

# 1️⃣6️⃣ Interview Questions

## What is Multimodality?

The ability of an AI model to understand and process multiple types of input such as text, images, audio, and documents.

---

## What is a Media object?

A Spring AI abstraction representing non-text input (images, PDFs, audio, etc.) attached to a prompt.

---

## Can a prompt contain both text and an image?

Yes. A prompt can combine text instructions with one or more media objects.

---

## What are common multimodal use cases?

- Image analysis
- PDF summarization
- OCR
- Invoice extraction
- Resume analysis
- Voice assistants

---

## Does every AI model support multimodality?

No. Support depends on the specific provider and model.

---

# 1️⃣7️⃣ Quick Revision

```text
Text

+

Image

+

PDF

+

Audio

↓

Media

↓

Prompt

↓

ChatClient

↓

ChatModel

↓

Multimodal LLM

↓

Response
```

---

# 🌳 Master Mind Map

```text
Multimodality
│
├── Text
│
├── Images
│   ├── Object Detection
│   ├── OCR
│   ├── Charts
│   └── Scene Understanding
│
├── PDF
│   ├── Summary
│   ├── Extraction
│   └── Q&A
│
├── Audio
│   ├── Speech
│   ├── Transcription
│   └── Summary
│
├── Media API
│
├── Prompt
│
├── ChatClient
│
├── ChatModel
│
├── Vision Models
│
├── Structured Output
│
└── Real World Applications
```

---

# 📚 Summary

After completing this chapter, you should be able to:

- Explain what Multimodality is.
- Identify different types of modalities.
- Understand how Spring AI uses the Media API.
- Describe how text, images, PDFs, and audio are processed.
- Explain the complete multimodal request lifecycle.
- Choose the appropriate AI model for different media types.
- Apply multimodal features in real-world Spring AI applications.

---

## 🧠 Chapter Checklist

- ✅ Understand Multimodality
- ✅ Learn Media API
- ✅ Learn Image Processing
- ✅ Learn PDF Processing
- ✅ Learn Audio Processing
- ✅ Understand Multi-Input Prompts
- ✅ Know Supported Models
- ✅ Ready for Image & Document AI Applications

> **Next File:** [📂 08-Image-PDF-Speech.md](./08-Image-PDF-Speech.md)
