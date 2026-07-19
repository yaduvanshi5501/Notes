# 🎤 Speech-to-Text (STT) with Spring AI

## Overview

Speech-to-Text (STT) converts **spoken audio into written text** using AI transcription models.

Spring AI provides a unified API to integrate transcription models such as:

- Azure OpenAI
- OpenAI Whisper
- Hugging Face

This enables developers to build voice-enabled applications without dealing with provider-specific APIs.

---

# What is Speech-to-Text?

```text
Audio File
     │
     ▼
Speech-to-Text Model
     │
     ▼
Transcribed Text
```

Unlike Chat Models that generate text, STT models **listen to audio and return text**.

---

# Features

- 🎤 Audio → Text conversion
- 📄 MP3, WAV, FLAC support
- 🌍 Automatic language detection
- ⚡ Real-time transcription
- ☁️ Multi-provider support
- 🚀 Spring Boot integration

---

# Use Cases

- Voice assistants
- Meeting transcription
- Lecture notes
- Podcast transcription
- Video subtitles
- Accessibility applications
- Voice-driven enterprise applications

---

# Spring AI Capability Stack

```text
Prompts
      │
Prompt Templates
      │
Model Parameters
      │
Structured Output
      │
Multimodality
      │
├── Text → Image
├── PDF Summarization
└── Speech → Text
```

---

# Deploy Transcription Model (Azure)

1. Open **Azure AI Foundry**
2. Go to **Deployments**
3. Click **Deploy Base Model**
4. Search **gpt-4o-mini-transcribe**
5. Select the model
6. Click **Deploy**
7. Save the deployment name

---

# application.properties

```properties
spring.application.name=audio-to-text

spring.ai.azure.openai.audio.transcription.options.deployment-name=gpt-4o-mini-transcribe

spring.ai.azure.openai.api-key=${AZURE_OPENAI_KEY}

spring.ai.azure.openai.endpoint=${AZURE_OPENAI_ENDPOINT}

spring.servlet.multipart.max-file-size=50MB
spring.servlet.multipart.max-request-size=50MB
```

---

# Required Dependency

```xml
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-starter-model-azure-openai</artifactId>
</dependency>
```

Provides Azure OpenAI integration for audio transcription.

---

# Architecture

```text
User
   │
Upload Audio
   │
   ▼
SampleController
   │
   ▼
AudioTranscriptionPrompt
   │
   ▼
AzureOpenAiAudioTranscriptionModel
   │
   ▼
Azure OpenAI
   │
   ▼
Transcribed Text
```

---

# Processing Flow

```text
Audio File
     │
     ▼
MultipartFile
     │
     ▼
AudioTranscriptionPrompt
     │
     ▼
AzureOpenAiAudioTranscriptionModel
     │
     ▼
call(prompt)
     │
     ▼
Output Text
```

---

# Demo Code

```java
@RestController
public class SampleController {

    @Autowired
    AzureOpenAiAudioTranscriptionModel audioTranscriptionModel;

    @PostMapping(
        value = "/process-audio",
        consumes = MediaType.MULTIPART_FORM_DATA_VALUE
    )
    public ResponseEntity<String> processAudio(
            @RequestPart("audio") MultipartFile audio)
            throws IOException {

        try {

            var prompt = new AudioTranscriptionPrompt(
                    audio.getResource(),
                    AzureOpenAiAudioTranscriptionOptions
                            .builder()
                            .build()
            );

            String text = audioTranscriptionModel
                    .call(prompt)
                    .getResult()
                    .getOutput();

            return ResponseEntity.ok(text);

        } catch (Exception e) {

            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                    .body("Error: " + e.getMessage());
        }
    }
}
```

---

# Endpoint

## Convert Audio to Text

```http
POST /process-audio
```

### Request

- Multipart File (`audio`)

### Response

```text
Transcribed text from uploaded audio
```

---

# Important Classes

| Class | Purpose |
|--------|----------|
| `AzureOpenAiAudioTranscriptionModel` | Azure transcription model |
| `AudioTranscriptionPrompt` | Creates transcription request |
| `AzureOpenAiAudioTranscriptionOptions` | Configures transcription |
| `MultipartFile` | Uploaded audio file |
| `ResponseEntity` | Returns transcription |

---

# Important Methods

Create Prompt

```java
var prompt = new AudioTranscriptionPrompt(
        audio.getResource(),
        AzureOpenAiAudioTranscriptionOptions
                .builder()
                .build()
);
```

Call Model

```java
String text = audioTranscriptionModel
        .call(prompt)
        .getResult()
        .getOutput();
```

Return Response

```java
return ResponseEntity.ok(text);
```

---

# Flow Summary

```text
Upload Audio
      │
      ▼
MultipartFile
      │
      ▼
AudioTranscriptionPrompt
      │
      ▼
AzureOpenAiAudioTranscriptionModel
      │
      ▼
Speech-to-Text Model
      │
      ▼
Transcribed Text
```

---

# Quick Revision

- `Speech-to-Text` → Audio → Text
- `AzureOpenAiAudioTranscriptionModel` → Calls transcription model
- `AudioTranscriptionPrompt` → Wraps audio file
- `audio.getResource()` → Converts uploaded file into Resource
- `call(prompt)` → Sends audio to Azure OpenAI
- `getOutput()` → Returns transcribed text
- Endpoint → `/process-audio`
- Deployment Model → `gpt-4o-mini-transcribe`

---

# Summary

Spring AI Speech-to-Text uses **AzureOpenAiAudioTranscriptionModel** to convert uploaded audio files into text. The audio is wrapped inside an **AudioTranscriptionPrompt**, sent to the Azure OpenAI transcription model, and the generated text is returned through a REST API.

```


@RestController
public class SampleController
{
    @Autowired
    AzureOpenAiAudioTranscriptionModel audioTranscriptionModel;
    
    /**
	 * Uploads an mvw audio file and processes with Azure OpenAI.
	 *
	 * @param systemPrompt - AI behavior (e.g., "You are a helpful assistant")
	 * @param userPrompt   - Query about audio file (e.g., "Convert audio content to text")
	 * @param file         - audio file to analyze
	 * @return AI-generated response
	 */
     
    @PostMapping(value = "/process-audio", consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
    public ResponseEntity<String> processAudio(
    		@Parameter(description = "Audio to text conversion") @RequestPart("audio") MultipartFile audio) throws IOException        
    {
      try {
    	// Create a transcription prompt using the audio resource
    	// and configure Azure OpenAI transcription options
    	var prompt = new AudioTranscriptionPrompt(
    	        audio.getResource(),
    	        AzureOpenAiAudioTranscriptionOptions.builder()
    	            	            .build()
    	);
    	// Call the audio transcription model and extract the transcribed text
    	String text = audioTranscriptionModel
    	        .call(prompt)
    	        .getResult()
    	        .getOutput();
				
        return ResponseEntity.ok(text);
      	}
      catch(Exception e) {
    	  return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("Error: " + e.getMessage());
      }
      
    }
}

 ``` 