# Agent with MCP (Model Context Protocol)

This project demonstrates how to build an AI agent powered by Azure AI that integrates with Model Context Protocol (MCP) servers to extend its capabilities. The example includes an inventory management agent that queries inventory levels and sales data through MCP tools.

## Overview

The project showcases two approaches to using MCP with Azure AI agents:

1. **Direct MCP Server Integration** (`client.py`) - Runs a local MCP server and connects it directly to an Azure AI agent with function calling.
2. **Remote MCP Tool Integration** (`agent.py`) - Uses a publicly available MCP server endpoint (Microsoft Learn API specs).

## Project Structure

- `server.py` - MCP server implementation with inventory management tools
- `client.py` - Azure AI agent client that connects to the local MCP server
- `agent.py` - Example showing how to use remote MCP tools with an Azure AI agent
- `requirements.txt` - Python dependencies
- `labenv/` - Python virtual environment

## Prerequisites

- Python 3.13 or higher
- Azure AI Project with:
  - A valid project endpoint
  - A deployed model (e.g., gpt-4o)
  - Appropriate Azure credentials (using DefaultAzureCredential)

## Setup

1. **Clone the repository** and navigate to the project directory:
   ```bash
   cd /path/to/agent-with-mcp
   ```

2. **Create a virtual environment** (if not already created):
   ```bash
   python -m venv labenv
   source labenv/bin/activate  # On Windows: labenv\Scripts\activate
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment variables**:
   Create a `.env` file in the project root:
   ```env
   PROJECT_ENDPOINT=https://<your-project>.ai.azure.com
   MODEL_DEPLOYMENT_NAME=<your-model-deployment>
   ```

## Usage

### Running the Inventory Agent (Direct MCP Integration)

This example runs a local MCP server and connects an AI agent to it:

```bash
python client.py
```

The agent will:
1. Start the local MCP server
2. Connect to your Azure AI project
3. Enter a chat loop where you can ask questions about inventory

Example prompts:
- "What products should we restock?"
- "Which items should we consider for clearance?"
- "Show me the current inventory levels"

### Running the Remote MCP Agent

This example demonstrates using a remote MCP server:

```bash
python agent.py
```

The agent will query the Microsoft Learn API MCP server and respond with Azure CLI commands.

## Features

### Inventory Agent (client.py)

The MCP server provides two tools:

- **`get_inventory_levels()`** - Returns current stock levels for all products
- **`get_weekly_sales()`** - Returns units sold in the last week

The agent uses these tools to:
- Recommend restocking if inventory < 10 and weekly sales > 15
- Recommend clearance if inventory > 20 and weekly sales < 5

### Agent Implementation

The agent is built using:
- **Azure AI Projects SDK** - For agent creation and management
- **MCP (Model Context Protocol)** - For tool integration
- **OpenAI Conversations API** - For multi-turn conversations

## How It Works

1. **MCP Server Setup**: The server exposes inventory data through standardized MCP tools
2. **Tool Registration**: The agent discovers available MCP tools and registers them
3. **Function Calling**: When the agent needs to access inventory data, it calls the appropriate tool
4. **Response Processing**: Tool outputs are passed back to the model for final response generation

## Dependencies

Key packages (see `requirements.txt` for full list):
- `azure-ai-projects` - Azure AI agent framework
- `fastmcp` - MCP server implementation
- `azure-identity` - Azure authentication
- `python-dotenv` - Environment variable management
- `openai` - OpenAI API client

## Architecture

```
User Input
    ↓
Azure AI Agent
    ↓
Function Calling Layer
    ↓
MCP Client ← → MCP Server (local or remote)
    ↓
Tool Execution (get_inventory_levels, get_weekly_sales)
    ↓
Response Generation
    ↓
User Output
```

## Authentication

This project uses Azure's `DefaultAzureCredential`, which attempts authentication in the following order:
1. Environment variables (AZURE_CLIENT_ID, AZURE_CLIENT_SECRET, AZURE_TENANT_ID)
2. Azure CLI cached credentials
3. Managed Identity (if running on Azure)
4. Interactive browser login

Ensure you're authenticated with Azure before running the scripts.

## Troubleshooting

### Connection Issues
- Verify your `.env` file contains the correct `PROJECT_ENDPOINT` and `MODEL_DEPLOYMENT_NAME`
- Ensure you have Azure credentials configured

### MCP Server Errors
- Check that the Python path in `client.py` correctly points to your virtual environment's Python executable
- Verify the `server.py` file is in the same directory or the correct path is specified

### Import Errors
- Make sure all dependencies are installed: `pip install -r requirements.txt`
- Verify you're using the correct virtual environment

## License

This project is provided as an example for educational purposes.

## Additional Resources

- [Model Context Protocol Documentation](https://modelcontextprotocol.io/)
- [Azure AI Projects SDK](https://learn.microsoft.com/azure/ai-services/agents/)
- [FastMCP Documentation](https://github.com/jlowell/fastmcp)
