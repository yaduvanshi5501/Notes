# 🚀 Spring AI Handbook

# 02 - Project Setup

> **Goal:** Learn how to set up a Spring AI project from scratch, understand all required dependencies, project structure, configuration files, environment variables, and how everything works together.

---

# 📖 Table of Contents

1. Prerequisites
2. Project Creation
3. Project Structure
4. Maven Configuration
5. Spring AI BOM
6. Required Dependencies
7. Configuration Files
8. Environment Variables
9. How Spring Boot Loads Configuration
10. Bean Creation
11. Complete Startup Flow
12. API Key Management
13. Dependency Flow
14. Best Practices
15. Common Mistakes
16. Interview Questions
17. Quick Revision
18. Mind Map

---

# 1️⃣ Prerequisites

Before starting Spring AI, you should know:

- Java 21+
- Spring Boot
- Maven
- REST APIs
- Dependency Injection
- Spring Beans
- Basic AI Concepts

---

# 2️⃣ Create Spring Boot Project

Create a Spring Boot project using:

- Spring Initializr
- IntelliJ IDEA
- Eclipse
- VS Code

---

## Recommended Settings

| Property | Value |
|-----------|-------|
| Project | Maven |
| Language | Java |
| Java | 21 |
| Spring Boot | 3.5.x |
| Packaging | Jar |

---

# 3️⃣ Project Structure

```text
spring-ai-demo/

│

├── src
│   ├── main
│   │
│   ├── java
│   │     │
│   │     └── com.example.demo
│   │              │
│   │              ├── DemoApplication
│   │              ├── controller
│   │              ├── service
│   │              ├── config
│   │              └── dto
│   │
│   └── resources
│          │
│          ├── application.properties
│          ├── application.yml
│          └── static
│
├── pom.xml
│
└── README.md
```

---

# 4️⃣ Maven Configuration

Every Spring Boot project contains

```text
pom.xml
```

It tells Maven

- Which dependencies to download
- Java version
- Spring Boot version
- Plugins
- Build information

---

# 5️⃣ Spring AI BOM

## What is BOM?

BOM = **Bill Of Materials**

It manages versions of all Spring AI dependencies.

Instead of writing versions repeatedly

```xml
dependency A -> version
dependency B -> version
dependency C -> version
```

You define the version only once.

---

## Without BOM

```xml
dependency A
version

dependency B
version

dependency C
version
```

Problem

Different versions may conflict.

---

## With BOM

```text
Spring AI BOM

        │

        ├── Chat Dependency

        ├── Azure Dependency

        ├── Ollama Dependency

        ├── Image Dependency

        └── Audio Dependency
```

Everything uses compatible versions.

---

# 6️⃣ Required Dependencies

A typical Spring AI project contains:

## Spring Boot Starter Web

Purpose

- REST APIs
- Controllers
- JSON

---

## Spring Boot DevTools

Purpose

- Auto Restart
- Faster Development

---

## Spring AI Starter

Purpose

- ChatClient
- Prompt
- ChatModel
- AI Integration

---

## Provider Dependency

Choose one

- Azure OpenAI
- OpenAI
- Ollama

Only one provider is needed for most projects.

---

## Swagger (Optional)

Purpose

API Documentation

---

## Spring Boot Test

Purpose

Testing

---

# Dependency Relationship

```text
Spring Boot

     │

     ▼

Spring AI Starter

     │

     ▼

ChatClient

     │

     ▼

ChatModel

     │

     ▼

Provider
```

---

# 7️⃣ Configuration Files

Spring Boot automatically loads

```text
application.properties
```

or

```text
application.yml
```

These files contain

- API Keys
- Model Name
- Temperature
- URLs
- Timeouts

---

## Example

```properties
spring.application.name=spring-ai-demo

server.port=8080
```

---

# 8️⃣ Environment Variables

Never hardcode API keys.

Bad

```text
API Key

↓

Source Code

↓

GitHub

↓

Security Risk
```

Good

```text
Environment Variable

↓

Spring Boot

↓

application.properties

↓

Application
```

---

## Why?

- Secure
- Easy Deployment
- Different Keys
- No GitHub Exposure

---

# 9️⃣ Spring Boot Startup Process

```text
Run Application

↓

SpringApplication.run()

↓

Create IoC Container

↓

Read pom.xml

↓

Load Dependencies

↓

Load application.properties

↓

Create Beans

↓

Dependency Injection

↓

Start Embedded Tomcat

↓

Application Ready
```

---

# 🔟 Bean Creation

Spring Boot scans

```text
@Component

@Service

@Repository

@Controller

@Configuration
```

Each becomes a Bean.

---

## Bean Lifecycle

```text
Spring Boot

↓

Component Scan

↓

Create Bean

↓

Store in IoC Container

↓

Inject Dependencies

↓

Ready to Use
```

---

# 1️⃣1️⃣ ChatClient Bean

Internally

```text
Provider

↓

ChatModel Bean

↓

ChatClient Builder

↓

ChatClient Bean

↓

Injected into Controller
```

So you simply write

```java
@Autowired
ChatClient chatClient;
```

---

# 1️⃣2️⃣ Configuration Loading

```text
Application Starts

↓

application.properties

↓

Spring Environment

↓

@ConfigurationProperties

↓

Spring AI Auto Configuration

↓

ChatClient

↓

Ready
```

---

# 1️⃣3️⃣ Dependency Flow

```text
pom.xml

↓

Maven

↓

Downloads JAR Files

↓

Local Repository

↓

Project Build

↓

Spring Boot Starts

↓

Auto Configuration

↓

Beans Created
```

---

# 1️⃣4️⃣ Best Practices

✅ Use Java 21+

✅ Keep dependencies updated

✅ Use BOM

✅ Store API keys in Environment Variables

✅ Use one provider unless required

✅ Keep configuration separate

✅ Use DevTools only for development

---

# 1️⃣5️⃣ Common Mistakes

❌ Hardcoding API Keys

❌ Adding multiple providers unnecessarily

❌ Missing BOM

❌ Wrong Java Version

❌ Wrong Spring Boot Version

❌ Missing Starter Dependency

❌ Forgetting to reload Maven

---

# 1️⃣6️⃣ Interview Questions

## Why use Spring AI BOM?

To manage compatible dependency versions automatically.

---

## What is Maven?

A build and dependency management tool.

---

## Why use Environment Variables?

To keep secrets secure.

---

## Difference between application.properties and application.yml?

Both configure Spring Boot.

- `.properties` uses key=value.
- `.yml` uses hierarchical YAML syntax.

---

## What is Auto Configuration?

Spring Boot automatically creates Beans based on available dependencies and configuration.

---

# 1️⃣7️⃣ Quick Revision

```text
Create Project

↓

Add Dependencies

↓

Import BOM

↓

Configure API Key

↓

Spring Boot Starts

↓

Beans Created

↓

ChatClient Ready

↓

Create Controller

↓

Call AI
```

---

# 1️⃣8️⃣ Master Mind Map

```text
Project Setup
│
├── Spring Boot
│
├── Maven
│   ├── pom.xml
│   ├── Plugins
│   └── Dependencies
│
├── Spring AI BOM
│
├── Starter Dependencies
│   ├── Web
│   ├── AI
│   ├── Test
│   └── DevTools
│
├── Configuration
│   ├── application.properties
│   ├── application.yml
│   └── Environment Variables
│
├── Auto Configuration
│
├── IoC Container
│
├── Beans
│   ├── ChatClient
│   ├── ChatModel
│   └── Controllers
│
└── Ready Application
```

---

# 📝 Summary

After completing this chapter, you should be able to:

- Create a Spring AI project from scratch.
- Understand the purpose of every dependency.
- Explain what the Spring AI BOM does.
- Configure environment variables securely.
- Understand Spring Boot startup.
- Explain how ChatClient is created automatically.
- Understand the complete dependency and bean creation flow.

> **Next File:** [📂 03-ChatClient-ChatModel.md](./03-ChatClient-ChatModel.md)