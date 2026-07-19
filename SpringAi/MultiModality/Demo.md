# 🖼️ Spring AI Multimodality

Up to this point, we have learned how Spring AI helps developers with:

- ✅ Prompts
- ✅ Prompt Templates
- ✅ Prompt Libraries
- ✅ Model Parameters
- ✅ Structured Output

The next major capability of modern AI systems is **Multimodality**.

Unlike traditional LLMs that only understand text, multimodal models can understand **multiple types of input simultaneously**, such as text, images, and audio.

---

# 📖 What is Multimodality?

**Multimodality** is the ability of an AI model to process and understand multiple data types together.

Instead of accepting only text, modern AI models can process combinations like:

- 📝 Text + Image
- 📝 Text + Audio
- 🖼️ Image + Question
- 🎙️ Audio + Question
- 🖼️ Image + Text + Audio

This allows AI to reason much more like humans.

---

# 🤔 Why Do We Need Multimodality?

Traditional AI models were designed for only one type of data.

| Traditional Model | Supports |
|-------------------|----------|
| NLP Models | Text |
| Vision Models | Images |
| Speech Models | Audio |

If an application needed both text and image understanding, developers had to combine multiple AI models.

Modern LLMs remove this limitation.

A single model can now understand:

- Text
- Images
- Audio
- Video (supported by some models)

---

# 🚀 Popular Multimodal Models

- GPT-4o
- GPT-4.1
- Google Gemini 1.5
- Claude 3
- Llama 3 Vision
- LLaVA

Spring AI provides a unified API for interacting with these models.

---

# 🛠 Deploy GPT-4o on Azure

## Step 1

Open **Azure AI Foundry**

Navigate to

```
Deployments
```

Click

```
Deploy Model
```

Then

```
Deploy Base Model
```

---

## Step 2

Search for

```
gpt-4o
```

Select

```
GPT-4o
```

Click

```
Confirm
```

---

## Step 3

Click

```
Deploy
```

Azure provisions the model.

---

## Step 4

Save the deployment name.

Example

```
gpt-4o
```

This value will be required inside **application.properties**.

---

# ⚙ application.properties

```properties
spring.ai.azure.openai.api-key=${AZURE_OPENAI_KEY}

spring.ai.azure.openai.endpoint=${AZURE_OPENAI_ENDPOINT}

spring.ai.azure.openai.chat.options.deployment-name=${DEPLOYMENT_NAME}
```

---

# 📁 Project Structure

```text
src
 ├── main
 │
 ├── java
 │     └── com.infy.controller
 │             └── MultiModalController.java
 │
 └── resources
       ├── application.properties
       └── error.png
```

---

# 🏗 Multimodal Architecture

```text
                 User
                   │
                   ▼
        Text + Image Upload
                   │
                   ▼
        MultiModalController
                   │
                   ▼
             UserMessage
         (Text + Media)
                   │
                   ▼
                Prompt
                   │
                   ▼
              ChatClient
                   │
                   ▼
        Azure OpenAI GPT-4o
                   │
                   ▼
          AI Generated Response
```

---

# 🧩 Core Components

## ChatClient

The primary interface for communicating with GPT-4o.

Features

- Text prompts
- Image prompts
- Audio prompts
- Streaming
- Structured Output

---

## Prompt

A Prompt contains one or more messages sent to the model.

```text
Prompt
│
├── UserMessage
└── Media
```

---

## UserMessage

A UserMessage can contain:

- Text
- Images
- Audio

Example

```java
UserMessage userMessage = UserMessage.builder()
        .text("Describe this image")
        .media(media)
        .build();
```

---

## Media

A Media object wraps the uploaded file.

```java
Media media = new Media(
        MimeTypeUtils.parseMimeType("image/png"),
        resource
);
```

It contains

- MIME Type
- Resource

---

## Resource

Uploaded files are represented as Spring Resources.

Example

```java
ByteArrayResource resource =
        new ByteArrayResource(image.getBytes());
```

---

# 📌 Endpoint 1 — Fix Programming Error

```http
POST /api/fix-error
```

This endpoint loads an image named

```
error.png
```

from

```
src/main/resources
```

and asks GPT-4o to analyze the screenshot.

---

## Flow

```text
error.png
      │
      ▼
 Resource
      │
      ▼
  Media Object
      │
      ▼
 UserMessage
(Text + Image)
      │
      ▼
    Prompt
      │
      ▼
  ChatClient
      │
      ▼
 Azure GPT-4o
      │
      ▼
 Error Solution
```

---

## Example Request

```http
POST /api/fix-error
```

Parameter

```
userPrompt

Fix the Java error in this screenshot.
```

---

## Example Response

```
The error occurs because the variable
"name" has not been initialized.

Initialize the variable before using it.

Example

String name = "Spring AI";
```

---

# 📌 Endpoint 2 — Describe Uploaded Image

```http
POST /api/describe
```

Consumes

```
multipart/form-data
```

---

## Flow

```text
Image Upload
      │
      ▼
MultipartFile
      │
      ▼
ByteArrayResource
      │
      ▼
Media
      │
      ▼
UserMessage
(Text + Image)
      │
      ▼
Prompt
      │
      ▼
ChatClient
      │
      ▼
GPT-4o
      │
      ▼
Description
```

---

## Example Request

```http
POST /api/describe
```

Form Data

```
userPrompt

Describe this image.
```

```
image

cat.png
```

---

## Example Response

```
The image shows a white cat sitting
on a wooden table.

A laptop and coffee mug are visible
in the background.

The cat appears relaxed.
```

---

# 🔍 Why ByteArrayResource?

Uploaded images are received as

```
MultipartFile
```

Spring AI expects

```
Resource
```

Therefore

```text
MultipartFile
        │
        ▼
ByteArrayResource
        │
        ▼
     Resource
        │
        ▼
      Media
```

---

# 🏷 Why MIME Type?

The model needs to know what type of file it is receiving.

Examples

| MIME Type | Meaning |
|-----------|----------|
| image/png | PNG Image |
| image/jpeg | JPEG Image |
| audio/wav | WAV Audio |
| audio/mp3 | MP3 Audio |
| application/pdf | PDF |

Without the MIME type, the model cannot correctly interpret the uploaded media.

---

# ⚠ Important Constraint

Only **UserMessage** supports media.

| Message Type | Media Support |
|--------------|---------------|
| UserMessage | ✅ Yes |
| SystemMessage | ❌ No |
| AssistantMessage | ❌ No |

---

# 🌍 Real-World Use Cases

- 📸 Image Captioning
- 🐞 Error Screenshot Analysis
- 📄 OCR & Document Understanding
- 📊 Chart Analysis
- 🧾 Invoice Processing
- 🛍 Product Recognition
- 🏥 Medical Image Analysis
- 🚗 Traffic Sign Detection
- 🛰 Satellite Image Interpretation
- 📑 Resume & Form Analysis

---

# ✅ Key Takeaways

- GPT-4o is a **multimodal** model that understands text and images together.
- Spring AI uses **ChatClient** to communicate with the model.
- Images are wrapped inside a **Media** object.
- A **UserMessage** combines both text and media.
- The **Prompt** is sent to GPT-4o using **ChatClient**.
- GPT-4o analyzes both the image and the text simultaneously to produce context-aware responses.
- Multimodality enables powerful applications such as image analysis, OCR, screenshot debugging, document understanding, and visual question answering.


```
package com.infy.controller;

import java.io.IOException;
import java.util.List;

import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.chat.messages.UserMessage;
import org.springframework.ai.chat.prompt.Prompt;
import org.springframework.ai.content.Media;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.core.io.ByteArrayResource;
import org.springframework.core.io.Resource;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.util.MimeTypeUtils;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RequestPart;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.multipart.MultipartFile;

@CrossOrigin
@RequestMapping("/api")
@RestController
public class MultiModalController {

    private final ChatClient chatClient;

    @Value("classpath:error.png")
    private Resource errorImage;

    public MultiModalController(ChatClient.Builder chatClientBuilder) {
        this.chatClient = chatClientBuilder.build();
    }

    @PostMapping("/fix-error")
    public ResponseEntity<String> fixErrorInImage(
            @RequestParam(defaultValue = "Provide a solution for the error depicted in this image")
            String userPrompt) {

        try {
            Media media = new Media(
                    MimeTypeUtils.parseMimeType("image/png"),
                    errorImage
            );

            UserMessage userMessage = UserMessage.builder()
                    .text(userPrompt)
                    .media(media)
                    .build();

            String response = chatClient
                    .prompt(new Prompt(List.of(userMessage)))
                    .call()
                    .content();

            return ResponseEntity.ok(response);

        } catch (Exception e) {
            return ResponseEntity.internalServerError()
                    .body("Failed to process request: " + e.getMessage());
        }
    }

    @PostMapping(
            value = "/describe",
            consumes = MediaType.MULTIPART_FORM_DATA_VALUE
    )
    public ResponseEntity<String> describeImage(
            @RequestParam(defaultValue = "Describe this image")
            String userPrompt,
            @RequestPart("image")
            MultipartFile image) {

        try {
            ByteArrayResource resource = new ByteArrayResource(image.getBytes());

            Media media = new Media(
                    MimeTypeUtils.parseMimeType(image.getContentType()),
                    resource
            );

            UserMessage userMessage = UserMessage.builder()
                    .text(userPrompt)
                    .media(media)
                    .build();

            String response = chatClient
                    .prompt(new Prompt(List.of(userMessage)))
                    .call()
                    .content();

            return ResponseEntity.ok(response);

        } catch (IOException e) {
            return ResponseEntity.internalServerError()
                    .body("Failed to process image: " + e.getMessage());

        } catch (Exception e) {
            return ResponseEntity.internalServerError()
                    .body("Failed to process request: " + e.getMessage());
        }
    }
}

```