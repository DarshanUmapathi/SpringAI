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
