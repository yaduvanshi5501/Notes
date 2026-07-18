# 📄 PDF Content Summarization using Spring AI

## Overview

Spring AI enables **PDF Content Summarization** by extracting text from PDF documents and sending it to an LLM (Azure OpenAI, OpenAI, Hugging Face, etc.) for summarization or question answering.

This allows enterprise applications to quickly understand lengthy documents without reading them manually.

---

## Key Features

- 📄 Upload PDF documents
- 🤖 AI-powered summarization
- 💬 Ask questions about PDF content
- 📚 Supports multi-page PDFs
- 🌐 REST API based
- ☁️ Works with Azure OpenAI and other LLM providers
- 🚀 Built using Spring Boot + Spring AI

---

## Use Cases

- Legal document analysis
- Research paper summarization
- Academic notes
- Compliance documents
- Corporate reports
- Policy documents

---

# Architecture

```text
User
   │
Upload PDF + Prompt
   │
   ▼
PdfReaderController
   │
   ▼
PagePdfDocumentReader
   │
Extract Text
   │
   ▼
Combine
(User Prompt + PDF Content)
   │
   ▼
ChatClient
   │
   ▼
Azure OpenAI
   │
   ▼
Summary / Answer
```

---

# Project Structure

```text
src
├── main
│
├── java
│   └── controller
│       └── PdfReaderController.java
│
├── resources
│   ├── application.properties
│   ├── system-prompt.st
│   └── static
│       └── index.html
```

---

# Required Dependency

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-pdf-document-reader</artifactId>
</dependency>
```

Used to read and extract text from PDF documents.

---

# Frontend (Demo)

The UI contains:

- Textbox for prompt
- PDF upload
- Submit button
- Loading indicator
- AI response section

It sends a **multipart/form-data** request to

```
POST /api/upload-pdf
```

containing

- `userPrompt`
- `file`

---

# Backend Flow

### 1. Upload PDF

```java
MultipartFile file
```

Receives the uploaded PDF.

---

### 2. Read PDF

```java
PagePdfDocumentReader pdfReader =
        new PagePdfDocumentReader(file.getResource());

String pdfContent =
        pdfReader.read().toString();
```

Extracts all text from the PDF.

---

### 3. Build Prompt

```java
String fullUserPrompt =
        userPrompt +
        "\n\nPDF Content:\n" +
        pdfContent;
```

Appends PDF content to the user's prompt.

---

### 4. Call AI

```java
String aiResponse =
        chatClient
            .prompt()
            .system(systemPrompt)
            .user(fullUserPrompt)
            .call()
            .content();
```

Sends the complete prompt to Azure OpenAI.

---

### 5. Return Response

```java
return ResponseEntity.ok(aiResponse);
```

Returns the AI-generated summary or answer.

---

# Controller

```java
// Keep your complete PdfReaderController.java here (copy-paste ready)
```

---

# Frontend

```html
<!-- Keep complete index.html here (copy-paste ready) -->
```

---

# Endpoint

## Upload PDF

```
POST /api/upload-pdf
```

### Request

- userPrompt
- file (PDF)

### Response

```
AI Summary / AI Answer
```

---

# Important Classes

| Class | Purpose |
|--------|----------|
| `PagePdfDocumentReader` | Reads PDF content |
| `MultipartFile` | Uploaded PDF |
| `ChatClient` | Sends prompt to LLM |
| `Resource` | Loads system prompt |
| `ResponseEntity` | Returns response |

---

# Flow Summary

```text
Upload PDF
      │
      ▼
Read PDF
      │
      ▼
Extract Text
      │
      ▼
Append User Prompt
      │
      ▼
ChatClient
      │
      ▼
Azure OpenAI
      │
      ▼
Summary
```

---

# Quick Revision

- `MultipartFile` → Receives uploaded PDF
- `PagePdfDocumentReader` → Extracts PDF text
- `ChatClient` → Sends prompt to AI
- `systemPrompt` → Defines AI behavior
- `userPrompt` → User question
- `fullUserPrompt` → Prompt + PDF content
- `ResponseEntity` → Returns AI response

---

# Summary

Spring AI's **PagePdfDocumentReader** extracts text from uploaded PDF documents. The extracted content is combined with the user's prompt and sent through **ChatClient** to an LLM, which returns a concise summary or answers questions about the document.

```
PDFReaderController.java:

package com.infy.controller;
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.reader.pdf.PagePdfDocumentReader;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.core.io.Resource;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RequestPart;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;
/**
REST Controller for PDF upload and AI analysis.
POSTMAN: POST /api/upload-pdf → Body → form-data → Add: systemPrompt (Text),
userPrompt (Text), file (File) 
SWAGGER: Navigate to /swagger-ui.html → POST
/api/upload-pdf → Try it out → Fill parameters → Execute */
@CrossOrigin
@RequestMapping("/api")
@RestController
public class PdfReaderController {
@Value("classpath:system-prompt.st")
Resource systemPrompt;
private final ChatClient chatClient;
public PdfReaderController(ChatClient.Builder chatClientBuilder) {
 this.chatClient = chatClientBuilder.build();
}
/**
Uploads PDF and processes with Azure OpenAI.
@param systemPrompt - AI behavior (e.g., "You are a helpful assistant")
@param userPrompt - Query about PDF (e.g., "Summarize this document")
@param file - PDF file to analyze
@return AI-generated response
*/
@PostMapping(value = "/upload-pdf", consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
public ResponseEntity handlePdfUpload(
 @RequestParam(defaultValue = "Summarize the main points of this document.") String userPrompt,
 @RequestPart("file") MultipartFile file) {
try {
// Extract text from PDF using PagePdfDocumentReader
PagePdfDocumentReader pdfReader = new PagePdfDocumentReader(file.getResource());
String pdfContent = pdfReader.read().toString();
 // Append PDF content to userPrompt
 String fullUserPrompt = userPrompt + "\n\nPDF Content:\n" + pdfContent;
 // Send to LLM
 String aiResponse = chatClient.prompt()
 		.system(systemPrompt)
 		.user(fullUserPrompt)
 		.call()
 		.content();
 return ResponseEntity.ok(aiResponse);
} catch (Exception e) {
return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Error: " + e.getMessage());
}
}
}
How the PDF Reader Works
The PDF reader functionality is implemented in the handlePdfUpload method of the PdfReaderController class. Here is a step-by-step explanation of how it operates:

File Upload: The user uploads a PDF file along with a prompt (e.g., "Summarize this document"). The file is received as a MultipartFile object.
Text Extraction: The PagePdfDocumentReader class is instantiated with the uploaded file's resource. It reads the content of the PDF and converts it into a string format.
Prompt Preparation: The extracted PDF content is appended to the user-provided prompt to create a comprehensive query for the AI model.
AI Processing: The combined prompt and PDF content are sent to the AI model using the ChatClient. The AI processes the input and generates a response.
Response Delivery: The AI-generated response is returned to the user as an HTTP response.
```