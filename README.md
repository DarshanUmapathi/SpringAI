# SpringAI

This repository contains sections for the Spring AI Framework with various model. Just use this for learning purpose and not production grade application code.

## About Spring AI

Spring AI is a framework that simplifies the development of applications that incorporate artificial intelligence functionalities. It is inspired by popular Python libraries like LangChain and LlamaIndex, and it aims to bring a similar experience to Java developers.

### Key Features

*   **Portable API:** Provides a consistent API across different AI providers (OpenAI, Azure, Hugging Face, etc.).
*   **Multiple Model Types:** Supports various model types like Chat, Text-to-Image, and Embedding models.
*   **Vector Stores:** Integrates with popular vector databases (Chroma, Milvus, Pinecone, etc.).
*   **Function Calling:** Allows the AI to invoke external functions.

### Where to Find It

*   **Official Website:** [https://spring.io/projects/spring-ai](https://spring.io/projects/spring-ai)
*   **Reference Documentation:** [https://docs.spring.io/spring-ai/reference/](https://docs.spring.io/spring-ai/reference/)

## Project Ideas

*   **Chatbot:** Build a customer support chatbot that can answer frequently asked questions.
*   **Content Generation:** Create a tool that generates blog posts or social media updates.
*   **Image Generation:** Develop an application that creates images from textual descriptions.
*   **RAG (Retrieval-Augmented Generation):** Build a question-answering system over your own documents.

## Sections

*   [Section 1: OpenAI](./Section01/openai)
*   Section 2: Coming Soon
*   Section 3: Coming Soon

## FAQ: Spring AI Basics

**Q1: What is Spring AI?**

A1: Spring AI is an open-source framework designed to simplify the development of AI-powered applications using the Spring ecosystem. It provides abstractions for various AI models (chat, embeddings, text-to-image) and integrates with different AI providers and vector databases.

**Q2: Why use Spring AI instead of direct API calls?**

A2: Spring AI offers a consistent API across multiple AI providers, reducing vendor lock-in and making it easier to switch between models or providers. It also provides Spring-native features like auto-configuration, simplified dependency management, and integration with other Spring projects, streamlining AI application development.

**Q3: What types of AI models does Spring AI support?**

A3: Spring AI supports various model types including:
*   **Chat Models:** For conversational AI and generating human-like text.
*   **Embedding Models:** For creating vector representations of text, useful for semantic search and recommendation systems.
*   **Text-to-Image Models:** For generating images from text descriptions.
*   **Audio Transcription:** For converting speech to text.

**Q4: Does Spring AI support different AI providers?**

A4: Yes, Spring AI is designed to be provider-agnostic. It supports popular providers like OpenAI, Azure OpenAI, Hugging Face, Google (Vertex AI), and more, allowing developers to choose their preferred service.

**Q5: What are Vector Databases and why are they important for Spring AI?**

A5: Vector databases store high-dimensional vectors, which are numerical representations of data (like text embeddings). They are crucial for applications like Retrieval-Augmented Generation (RAG), enabling AI models to retrieve relevant information from a knowledge base before generating a response, thereby improving accuracy and reducing hallucinations. Spring AI provides integrations with several vector databases.
