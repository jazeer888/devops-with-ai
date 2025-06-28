# 🛠️ Building and Running Azure MCP Server Locally in Docker

## 1. Clone the Azure MCP Repository

First, clone the Azure MCP Server repository from GitHub:

```sh
git clone https://github.com/Azure/azure-mcp.git
cd azure-mcp
```

## 2. Build the Docker Image

Build the Docker image using the following command:

```sh
docker build -t azuremcp:1 .
```

#### For Mac (Apple Silicon)

```sh
docker build --platform=linux/amd64 -t azuremcp:1 .
```

This command builds the Docker image and tags it as `azuremcp:1`.

## 3. Create an Environment File with Azure Credentials

Create a `.env` file (e.g., `azure.env`) in the project directory with your Azure credentials:

```env
AZURE_TENANT_ID=xxxxxxxxxxxxxxxxxxxxxxx
AZURE_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxx
AZURE_CLIENT_SECRET=xxxxxxxxxxxxxxxxxxxxxxx
AZURE_SUBSCRIPTION_ID=xxxxxxxxxxxxxxxxxxxxxxx
AZURE_CREDENTIALS={"clientId":"xxxxxxxxxxxxxxxxxxxxxxx","clientSecret":"xxxxxxxxxxxxxxxxxxxxxxx","tenantId":"xxxxxxxxxxxxxxxxxxxxxxx"}
```

Replace the placeholder values above with your actual Azure service principal credentials.

## 4. Run the Docker Container

Run the Docker container with the following command:

```sh
docker run --rm -it \
  -p 8080:5008 \
  --env-file ./azure.env \
  azuremcp:1 \
  server start --transport sse
```

#### For Mac (Apple Silicon)

```sh
docker run --platform=linux/amd64 -d -it --rm --env-file .env -p 5008:5008 azuremcp64:local --transport sse
```

**What these commands do:**
- `--rm -it`: Runs the container interactively and removes it after it stops.
- `-p 8080:5008`: Maps port 5008 inside the container to port 8080 on your host machine.
- `--env-file ./azure.env`: Loads environment variables from the `azure.env` file.
- `azuremcp:1`: Specifies the Docker image to use.
- `server start --transport sse`: Starts the MCP server with Server-Sent Events (SSE) transport.

## 5. Connect n8n MCP Client to the Server

In your locally hosted n8n instance, configure the MCP Client node to connect to the Azure MCP Server:

- **SSE Endpoint:** `http://localhost:8080/sse`
- **Authentication:** If authentication is required, configure it accordingly. For local testing, you might not need authentication.

This setup allows your n8n MCP Client to communicate with the Azure MCP Server running in the Docker container on your local machine.

---

## Reference

For more detailed information, you can refer to the [Azure MCP Server GitHub repository](https://github.com/Azure/azure-mcp).

---


