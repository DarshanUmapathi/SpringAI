# Spring AI OpenAI Demo

This is a demonstration project for using Spring AI with OpenAI. The application provides a simple RESTful API to interact with an AI chat model.

## Description

The core of this application is a `ChatController` that exposes a single endpoint:

-   `GET /api/chat`

This endpoint accepts a `message` query parameter and uses the Spring AI `ChatClient` to send this message to an OpenAI chat model. The model's response is then returned as a string.

## Prerequisites

-   Java 21
-   Maven
-   An OpenAI API key

## Configuration

1.  The application requires your OpenAI API key to be available as an environment variable. Set it before running the application:

    ```bash
    export OPENAI_API_KEY="your-openai-api-key"
    ```
    On Windows, use:
    ```powershell
    $env:OPENAI_API_KEY="your-openai-api-key"
    ```

2.  The application runs on port `8081` by default, as configured in `src/main/resources/application.properties`.

## How to Run

1.  Clone the repository.
2.  Open a terminal in the project's root directory.
3.  Make sure your `OPENAI_API_KEY` is set.
4.  Run the application using the Maven wrapper:

    ```bash
    ./mvnw spring-boot:run
    ```

## How to Use

Once the application is running, you can send requests to the chat endpoint using a tool like `curl` or your web browser.

**Example using cURL:**

```bash
curl "http://localhost:8081/api/chat?message=Tell%20me%20a%20joke"
```

The API will return the AI's response as a plain text string.

**Example using Postman:**

1.  Open Postman.
2.  Create a new GET request.
3.  Enter the request URL: `http://localhost:8081/api/chat?message=Tell%20me%20a%20joke`
4.  Click "Send".

The API will return the AI's response as a plain text string.

## ChatModel: The Core Abstraction

The `ChatModel` interface is a lower-level abstraction in Spring AI that provides a simple and consistent way to interact with various AI chat models. It defines the contract for communication, allowing you to send prompts and receive generated responses without being tied to a specific AI provider's API.

### Importance of ChatModel

-   **Abstraction:** It hides the complexity of interacting with different AI models, each with its own unique API and data format.
-   **Portability:** By coding to the `ChatModel` interface, you can easily switch between different AI models (e.g., OpenAI, Google Gemini, Anthropic Claude) with minimal code changes.
-   **Consistency:** It provides a unified API for sending prompts and receiving responses, making your code cleaner and more maintainable.

### How it Defines Contracts

The `ChatModel` interface defines a single method:

```java
ChatResponse call(Prompt prompt);
```

-   `Prompt`: This is an object that encapsulates the message or series of messages you want to send to the AI model.
-   `ChatResponse`: This object contains the AI's response, including the generated message and any associated metadata.

### Example Usage

Here's a simple example of how to use the `ChatModel` interface directly:

```java
import org.springframework.ai.chat.model.ChatModel;
import org.springframework.ai.chat.model.ChatResponse;
import org.springframework.ai.chat.prompt.Prompt;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class ChatService {

    private final ChatModel chatModel;

    @Autowired
    public ChatService(ChatModel chatModel) {
        this.chatModel = chatModel;
    }

    public String getAiResponse(String message) {
        Prompt prompt = new Prompt(message);
        ChatResponse response = chatModel.call(prompt);
        return response.getResult().getOutput().getContent();
    }
}
```

In this example, the `ChatService` takes a `ChatModel` as a dependency. The `getAiResponse` method creates a `Prompt` object, calls the `chatModel.call()` method, and extracts the generated content from the `ChatResponse`.

### Interview Questions and Answers

**Q1: What is the primary purpose of the `ChatModel` interface in Spring AI?**

**A1:** The `ChatModel` interface serves as a low-level abstraction for interacting with various AI chat models. Its primary purpose is to provide a consistent and portable API for sending prompts and receiving responses, decoupling the application from the specifics of any particular AI provider's API.

**Q2: How does `ChatModel` promote portability in an application?**

**A2:** By coding against the `ChatModel` interface, developers can easily switch between different AI models (e.g., from OpenAI to Google Gemini) by simply changing the configuration and providing a different implementation of the `ChatModel`. This avoids vendor lock-in and allows for greater flexibility in choosing the best model for a given task.

**Q3: What are the main components of the `ChatModel` contract?**

**A3:** The contract is primarily defined by the `call(Prompt prompt)` method. The two main components are:
-   `Prompt`: An object representing the input to the model, which can be a single message or a conversation history.
-   `ChatResponse`: An object representing the model's output, containing the generated message and other metadata like usage statistics.

**Q4: When would you use the `ChatModel` interface directly instead of the higher-level `ChatClient`?**

**A4:** You would typically use the `ChatModel` directly when you need more control over the interaction with the AI model, such as when implementing custom retry logic, handling specific error conditions, or when you need to work with the raw `ChatResponse` object to access metadata not exposed by the `ChatClient`. The `ChatClient` is a higher-level abstraction that uses the `ChatModel` internally and provides a more fluent and convenient API for common use cases.

## ChatClient: Higher-Level Interaction

While `ChatModel` provides the core, lower-level contract for AI chat interactions, the `ChatClient` is a higher-level abstraction built on top of `ChatModel`. It offers a more fluent and convenient API for common use cases, simplifying prompt construction and response extraction.

### Importance of ChatClient

-   **Simplicity:** `ChatClient` streamlines common chat interactions, requiring less boilerplate code compared to direct `ChatModel` usage.
-   **Fluent API:** It provides a builder-pattern-like interface for constructing prompts, making the code more readable and expressive.
-   **Convenience:** It handles the creation of `Prompt` objects and the extraction of content from `ChatResponse` objects, reducing the cognitive load for developers.

### How it Works

`ChatClient` internally uses a `ChatModel` to perform the actual call to the AI provider. It abstracts away the direct manipulation of `Prompt` and `ChatResponse` objects for simpler scenarios.

### Example Usage

Here's how to use `ChatClient` in a typical Spring AI application:

```java
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/api")
public class ChatController {

    private final ChatClient chatClient;

    public ChatController(ChatClient.Builder chatClientBuilder) {
        this.chatClient = chatClientBuilder.build();
    }

    @GetMapping("/chat")
    public String chat(@RequestParam("message") String  message ) {
        return chatClient.prompt(message).call().content();
    }
}
```

In this example, the `ChatController` uses `ChatClient` to send a message and directly retrieve the content of the AI's response with a single line of code.

### Interview Questions and Answers (ChatClient)

**Q1: What is `ChatClient` in Spring AI, and how does it relate to `ChatModel`?**

**A1:** `ChatClient` is a higher-level, more convenient abstraction in Spring AI designed for common chat interactions. It is built on top of `ChatModel`, which is the lower-level interface defining the core contract for AI chat. `ChatClient` simplifies prompt creation and response handling by internally utilizing a `ChatModel` instance.

**Q2: What advantages does `ChatClient` offer over directly using `ChatModel`?**

**A2:** `ChatClient` offers several advantages:
-   **Simplicity and reduced boilerplate:** It provides a fluent API that makes common chat operations more concise.
-   **Readability:** The builder-pattern approach for constructing prompts enhances code readability.
-   **Convenience:** It abstracts away the direct creation of `Prompt` objects and the parsing of `ChatResponse`, simplifying development for typical use cases.

**Q3: When would you prefer to use `ChatClient`?**

**A3:** You would prefer `ChatClient` for most standard chat interactions where you need to send a simple message or a series of messages and receive the generated content. It's ideal for rapid development and scenarios where the advanced features and metadata of `ChatResponse` are not immediately required.

**Q4: Can you still access `ChatModel`'s capabilities when using `ChatClient`?**

**A4:** Yes, `ChatClient` is built upon `ChatModel`. While `ChatClient` provides a simplified interface, if you need the granular control offered by `ChatModel` (e.g., access to `ChatResponse` metadata, custom retry logic), you would either inject and use `ChatModel` directly or configure `ChatClient` with specific `ChatModel` implementations.

## Observability

This application is configured with observability features using Micrometer, Prometheus, and Grafana. This allows you to monitor the application's performance, AI model usage, and other key metrics.

### Components

-   **Micrometer:** Collects metrics from the application (e.g., HTTP requests, JVM stats, Spring AI metrics).
-   **Prometheus:** A time-series database that scrapes metrics from the application's `/actuator/prometheus` endpoint.
-   **Grafana:** A visualization tool that queries Prometheus and displays metrics in dashboards.

### Accessing the Dashboards

When the application is running (which also starts the Docker containers for Prometheus and Grafana via Docker Compose), you can access the following:

1.  **Prometheus UI:** `http://localhost:9090`
    -   You can verify that the application is being scraped by going to **Status -> Targets**. You should see `openai-app` (host.docker.internal:8081) as UP.
    -   You can query metrics directly, such as `gen_ai_client_token_usage_total`.

2.  **Grafana UI:** `http://localhost:3000`
    -   **Login:**
        -   Username: `openai`
        -   Password: `openai`
    -   **Dashboard:** Navigate to **Dashboards** and select **Spring AI Metrics Dashboard**.
    -   This dashboard provides insights into:
        -   Total AI Requests
        -   Average Response Time
        -   Token Usage (Prompt vs. Completion)
        -   Error Rates
        -   System Resources (CPU, Memory)

### Key Metrics

Some of the key metrics exposed by Spring AI include:

-   `gen_ai_client_token_usage_total`: Tracks the number of tokens used (input and output).
-   `gen_ai_client_operation_seconds`: Tracks the duration and count of AI operations.
-   `http_server_requests_seconds`: Tracks HTTP request duration and count.

### Troubleshooting

-   **"Prometheus DataSource not connected" in Grafana:** Ensure that the Prometheus container is running and accessible from the Grafana container. The default configuration uses `http://prometheus:9090`.
-   **No data in dashboards:**
    -   Ensure the application is running and reachable by Prometheus.
    -   Make some requests to the `/api/chat` endpoint to generate traffic and metrics.
    -   Check the Prometheus Targets page (`http://localhost:9090/targets`) to ensure the scrape target is UP.
