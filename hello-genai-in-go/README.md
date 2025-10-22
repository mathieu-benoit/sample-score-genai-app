# Hello-GenAI Go Application

A Go-powered GenAI app you can run locally using your favorite LLM — just follow the guide to get started.

This code is coming from https://github.com/docker/hello-genai.

## Environment Variables

- `PORT`: The port to run the server on (default: 8080)
- `LLM_BASE_URL`: The base URL of the LLM API (required)
- `LLM_MODEL_NAME`: The model name to use for API requests (required)
- `LOG_LEVEL`: The logging level (default: INFO)

## API Endpoints

- `GET /`: Main chat interface
- `POST /api/chat`: Chat API endpoint
- `GET /health`: Health check endpoint with detailed system information
- `GET /example`: Example of structured formatting
- `GET /api/docs`: Swagger UI for API documentation

## Running the Application

### Using Docker Compose

```bash
make compose-up
```

### Without Docker

```bash
export LLM_BASE_URL=http://your-llm-api
export LLM_MODEL_NAME=your-model
make go-up
```
