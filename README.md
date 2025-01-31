# deepseek-chat-service
🚀 A Dockerized chat UI powered by MongoDB, Ollama, and DeepSeek LLM. Easily deploy and run an AI-driven chat service with minimal setup.
# Chat UI with MongoDB and Ollama

This project sets up a chat UI service using MongoDB, Ollama, and a Chat UI container. The services are orchestrated using Docker Compose.

## Prerequisites

Ensure you have the following installed on your system:

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Setup and Execution

### 1. Clone the Repository

```sh
git clone <your-repository-url>
cd <your-repository-folder>
```

### 2. Create Required Directories and Files

Ensure the following directories exist:

```sh
mkdir -p ollama db .env.local
```

Create a `.env.local/.env.local` file (if it does not exist) with necessary environment variables:

#### `.env.local/.env.local` Example:
```ini
MONGODB_URL="mongodb://mongodb:27017"

HF_TOKEN="abc"

MODELS="[
  {
    \"name\": \"Ollama DeepSeek\",
    \"parameters\": {
      \"temperature\": 0.1,
      \"top_p\": 0.95,
      \"repetition_penalty\": 1.2,
      \"top_k\": 50,
      \"truncate\": 3072,
      \"max_new_tokens\": 1024,
      \"stop\": [\"</s>\"]
    },
    \"endpoints\": [
      {
        \"type\": \"ollama\",
        \"url\": \"http://ollama-service:11434\",
        \"ollamaName\": \"deepseek-r1:7b\"
      }
    ]
  }
]"
```

### 3. Start the Services

Run the following command to start all containers:

```sh
docker compose up -d
```

Once the containers are running, download the DeepSeek LLM using:

```sh
docker exec -it deepseek-ollama-service-1 ollama pull deepseek-r1:7b
```

Run the following command to start all containers:

```sh
docker compose up -d
```

This will start the following services:
- **MongoDB** (Port: `27017`)
- **Ollama Service** (Port: `11434`)
- **Chat UI** (Port: `5000`)

### 4. Verify Running Containers

To check if all containers are running:

```sh
docker ps
```

### 5. Stop the Services

To stop all running containers:

```sh
docker compose down
```

## Configuration Details

### MongoDB
- Image: `mongo:4.4.6`
- Exposes port `27017`
- Connected to `chat-ui` network

### Ollama Service
- Image: `ollama/ollama`
- Exposes port `11434`
- Stores data in `./ollama`
- Connected to `chat-ui` network

### Chat UI
- Image: `ghcr.io/huggingface/chat-ui-db:latest`
- Exposes port `5000`
- Uses `.env.local/.env.local` for environment variables
- Persists data in `./db`
- Depends on MongoDB
- Connected to `chat-ui` network

## Troubleshooting

- If a service fails to start, check logs with:
  ```sh
  docker compose logs -f <service-name>
  ```
- Ensure your `.env.local/.env.local` file is properly configured.
- Check if required directories exist (`db`, `ollama`, `.env.local`).
- Make sure no other services are running on the specified ports (`27017`, `11434`, `5000`).



