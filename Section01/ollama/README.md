
# Spring AI Ollama Integration

This project is a demonstration of how to integrate Spring AI with Ollama, a tool for running large language models locally.

## Overview

The application is a simple Spring Boot web application that exposes a single REST endpoint to interact with an Ollama-hosted language model. It uses the Spring AI `ChatClient` to send prompts to the model and receive responses.

## Prerequisites

*   Java 21 or later
*   Maven 3.6 or later
*   An Ollama instance running. You can download it from [https://ollama.com/](https://ollama.com/).

## Getting Started

1.  **Clone the repository:**

    ```bash
    git clone <repository-url>
    cd ollama
    ```

2.  **Run Ollama:**

    Ensure that your Ollama instance is running. You can pull a model (e.g., `llama2`) by running:

    ```bash
    ollama pull llama2
    ```

3.  **Configure the application:**

    Open the `src/main/resources/application.properties` file and configure the Ollama settings:

    ```properties
    # The base URL of your Ollama instance
    spring.ai.ollama.base-url=http://localhost:11434

    # The model to use for chat
    spring.ai.ollama.chat.options.model=llama2
    ```

    You can replace `llama2` with any other model you have available in Ollama.

4.  **Run the application:**

    ```bash
    ./mvnw spring-boot:run
    ```

    The application will start on port 8080.

## Usage

You can interact with the application by sending a GET request to the `/ollama-api/chat` endpoint with a `message` parameter.

Here's an example using `curl`:

```bash
curl "http://localhost:8080/ollama-api/chat?message=Tell%20me%20a%20joke"
```

The application will return a response from the language model.

## Testing

To run the tests, execute the following command:

```bash
./mvnw test
```

## Q&A

**Q: How can I use a different model?**

**A:** You can change the `spring.ai.ollama.chat.options.model` property in `application.properties` to the name of any model you have downloaded in Ollama.

**Q: How can I connect to a remote Ollama instance?**

**A:** You can change the `spring.ai.ollama.base-url` property in `application.properties` to the URL of your remote Ollama instance.

**Q: What if I get a connection error?**

**A:** Make sure your Ollama instance is running and that the `spring.ai.ollama.base-url` is correct. You can check the status of your Ollama instance by visiting the base URL in your browser.
