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

### Using Docker Compose with `score-compose`

```bash
make compose-up
```

```bash
score-compose provisioners list
```

```none
+-----------+-------+--------+---------------------+------------------------------------------------------------------------------------+
|   TYPE    | CLASS | PARAMS |       OUTPUTS       |                     DESCRIPTION                                                    |
+-----------+-------+--------+---------------------+------------------------------------------------------------------------------------+
| llm-model | (any) | model  | api-key, model, url | Generates the LLM model service via the Docker Model Runner (DMR) provider.        |
+-----------+-------+--------+---------------------+------------------------------------------------------------------------------------+
| llm-model | (any) | model  | api-key, model, url | Generates a curl service downloading the model with the Docker Model Runner (DMR). |
+-----------+-------+--------+---------------------+------------------------------------------------------------------------------------+
| llm-model | (any) | model  | api-key, model, url | Runs curl to download the model with the Docker Model Runner (DMR).                |
+-----------+-------+--------+---------------------+------------------------------------------------------------------------------------+
```

### Without Docker

```bash
export LLM_BASE_URL=http://your-llm-api
export LLM_MODEL_NAME=your-model
make go-up
```
