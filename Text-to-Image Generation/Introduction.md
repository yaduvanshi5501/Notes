# 🖼️ Text-to-Image Generation with Spring AI

## Overview

Spring AI supports **Text-to-Image Generation**, allowing developers to generate images from natural language prompts using AI image models such as **DALL·E 3**.

Instead of returning text, the AI creates an image based on the provided prompt.

Example:

> **Prompt:** _"An astronaut riding a horse in a futuristic city."_

↓

🖼️ AI Generated Image

---

# Why Text-to-Image?

Some common use cases include:

- 🎨 AI Art Generation
- 📢 Marketing & Advertisement
- 🛍️ Product Design
- 🎮 Game Asset Creation
- 📚 Educational Content
- 🤖 Creative Applications

---

# Spring AI Components

| Component       | Purpose                              |
| --------------- | ------------------------------------ |
| `ImageModel`    | Generates images from prompts        |
| `ImagePrompt`   | Wraps the text prompt                |
| `ImageResponse` | Contains generated image information |
| `ImageOutput`   | Provides the generated image URL     |

---

# Architecture

```text
User Prompt
      │
      ▼
ImagePrompt
      │
      ▼
ImageModel.call()
      │
      ▼
ImageResponse
      │
      ▼
Image URL
      │
 ┌────┴────┐
 ▼         ▼
Return URL Download Image
```

---

# Deploy DALL·E 3 on Azure

1. Open **Azure AI Foundry**
2. Go to **Deployments**
3. Click **Deploy Base Model**
4. Search **dall-e-3**
5. Select the model
6. Click **Deploy**
7. Save the deployment name

---

# Configuration

```properties
spring.application.name=image-generator

spring.ai.azure.openai.api-key=${AZURE_OPENAI_KEY}

spring.ai.azure.openai.endpoint=${AZURE_OPENAI_ENDPOINT}

spring.ai.azure.openai.image.options.deployment-name=dall-e-3
```

---

# Image Generation Flow

```java
ImagePrompt imagePrompt = new ImagePrompt(prompt);

ImageResponse response = imageModel.call(imagePrompt);

String imageUrl = response.getResult()
                          .getOutput()
                          .getUrl();
```

---

# Endpoints

## Generate Image URL

```
GET /image/generate-image-url
```

Example

```
http://localhost:8080/image/generate-image-url?prompt=Sunset over Himalayas
```

Returns

```
https://generated-image-url...
```

---

## Generate Image

```
GET /image/generate-image
```

Example

```
http://localhost:8080/image/generate-image?prompt=Sunset over Himalayas
```

Returns

```
PNG Image
```

---

# How It Works

### 1. Create Image Prompt

```java
ImagePrompt imagePrompt = new ImagePrompt(prompt);
```

Converts the user's text into an image prompt.

---

### 2. Call AI Model

```java
ImageResponse response = imageModel.call(imagePrompt);
```

Sends the prompt to DALL·E 3.

---

### 3. Get Image URL

```java
String imageUrl =
response.getResult()
        .getOutput()
        .getUrl();
```

Retrieves the generated image URL.

---

### 4. Download Image

```java
byte[] imageBytes =
new URL(imageUrl)
.openStream()
.readAllBytes();
```

Downloads the generated image.

---

# Key Classes

| Class                    | Purpose                             |
| ------------------------ | ----------------------------------- |
| `ImageModel`             | Main interface for image generation |
| `ImagePrompt`            | Represents text prompt              |
| `ImageResponse`          | Stores AI response                  |
| `ImageOutput`            | Contains image URL                  |
| `ResponseEntity<String>` | Returns image URL                   |
| `ResponseEntity<byte[]>` | Returns image bytes                 |

---

# Advantages

- ✅ Easy Spring Boot integration
- ✅ Provider-independent API
- ✅ Supports Azure OpenAI
- ✅ Minimal code
- ✅ Easy provider switching

---

# Quick Revision

- `ImageModel` → Generates images
- `ImagePrompt` → Holds prompt
- `ImageResponse` → AI response
- `imageModel.call()` → Calls DALL·E
- `getUrl()` → Gets image URL
- `ResponseEntity<String>` → Returns URL
- `ResponseEntity<byte[]>` → Returns image
- Deployment Name → `dall-e-3`

---

# Summary

Spring AI provides a simple abstraction for **Text-to-Image Generation** using `ImageModel`. The application converts a natural language prompt into an AI-generated image and can return either the **image URL** or the **image bytes**, making it easy to integrate image generation into Spring Boot applications.

```
// Controller for handling image generation requests using Spring AI's ImageModel.
// Provides endpoints to generate image URLs and serve image bytes.
package com.infy.controller;
import java.io.IOException;
import java.net.URL;
import org.springframework.ai.image.ImageModel;
import org.springframework.ai.image.ImagePrompt;
import org.springframework.ai.image.ImageResponse;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
@CrossOrigin
@RestController
@RequestMapping("/image")
public class ImageGenController {
// Using DALL·E model for image generation, as it can generate images from text
// prompts.
private final ImageModel imageModel;
// Constructor injection for ImageModel dependency.
public ImageGenController(ImageModel imageModel) {
	this.imageModel = imageModel;
}
/**
 * Endpoint to generate an image URL from a text prompt.
 *
 * @param prompt The text prompt for image generation.
 * @return ResponseEntity containing the image URL as a string.
 */
@GetMapping("/generate-image-url")
public ResponseEntity<String> generateImage(
		@RequestParam(defaultValue = "An astronaut riding a horse in a futuristic city, digital art") String prompt) {
	// Create an image prompt from the user input.
	ImagePrompt imagePrompt = new ImagePrompt(prompt);
	// Call the image model to generate a response.
	ImageResponse response = imageModel.call(imagePrompt);
	// Extract the image URL from the response.
	String imageUrl = response.getResult().getOutput().getUrl();
	return ResponseEntity.ok(imageUrl);
}
/**
 * Endpoint to generate and serve the image bytes directly from a text prompt.
 *
 * @param prompt The text prompt for image generation.
 * @return ResponseEntity containing the image bytes with PNG content type.
 * @throws IOException if image download fails.
 */
@GetMapping(value = "/generate-image", produces = MediaType.IMAGE_PNG_VALUE)
public ResponseEntity<byte[]> generateImage1(
		@RequestParam(defaultValue = "An astronaut riding a horse in a futuristic city, digital art") String prompt)
		throws IOException {
	// Create an image prompt from the user input.
	ImagePrompt imagePrompt = new ImagePrompt(prompt);
	// Call the image model to generate a response.
	ImageResponse response = imageModel.call(imagePrompt);
	// Extract the image URL from the response.
	String imageUrl = response.getResult().getOutput().getUrl();
	// Download the image bytes from the URL.
	byte[] imageBytes = new URL(imageUrl).openStream().readAllBytes();
	return new ResponseEntity<>(imageBytes, HttpStatus.OK);
}
}

```
