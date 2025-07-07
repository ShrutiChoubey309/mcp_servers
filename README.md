# MCP Server Integrations

This repository contains a comprehensive list of all the MCP (Model Context Protocol) servers I have experimented with. Each server is designed to perform a specific function, and is tested for compatibility within the MCP framework.

---
## Index

1. [Brave Search MCP Server](#1-brave-search-mcp-server)
2. [Tavily Search MCP Server](#2-tavily-search-mcp-server)
3. [Fetch MCP Server](#3-fetch-mcp-server)
4. [Google Maps MCP Server](#4-google-maps-mcp-server)
5. [Sequential Thinking MCP Server](#5-sequential-thinking-mcp-server)
6. [Weather MCP](#6-weather-mcp)
7. [Slack MCP Server](#8-slack-mcp-server)

---
##  Tested MCP Servers

### 1. Brave Search MCP Server
- **Purpose**: Perform real-time search queries using Brave’s search engine.
- **Status**:  Working
- **Use Case**: General-purpose web search, especially for privacy-focused results.
- **Notes**:
  - Fast responses.
  - Sometimes limited by request quotas.
  - You need to provide the API key.

**Get your Brave Search API key from:**  
- Sign up for a [https://brave.com/search/api/](https://brave.com/search/api/)
- Choose a plan (Free tier available with 2,000 queries/month)
- Generate your API key from the developer dashboard

#### Brave Search Server Configuration
Here is how the Brave Search MCP server is configured in the `mcpServers` JSON block:

```json
"mcpServers": {
  "brave-search": {
    "command": "npx",
    "args": [
      "-y",
      "@modelcontextprotocol/server-brave-search"
    ],
    "env": {
      "BRAVE_API_KEY": "API KEY"
    }
  }
}
```
---

### 2. Tavily Search MCP Server
- **Purpose**: Use the Tavily API for structured and concise web search.
- **Status**:  Working
- **Use Case**: Helpful in retrieval-augmented generation (RAG) pipelines.
- **Notes**:
  - More structured than Brave.
  - Good for summarizing answers directly.
  - You need to provide the API key.


**Get your Tavily Search API key from:**  
- Sign up for a [https://www.tavily.com/](https://www.tavily.com/)
- Your API Key will be visible on the dashboard under API Access

#### Tavily Server Configuration

```json
"mcpServers": {
  "tavily-mcp": {
    "command": "npx",
    "args": ["-y", "tavily-mcp@0.1.2"],
    "env": {
      "TAVILY_API_KEY": "API KEY"
    }
  }
}
```
---

### 3. Fetch MCP Server
- **Purpose**: Generic HTTP GET/POST-based fetching of data from external URLs or APIs.
- **Status**:  Working
- **Use Case**: Useful for scraping or calling arbitrary REST APIs.
- **Notes**:
  - Flexible but needs request config.
  - Requires validation for rate limits and headers.

#### Fetch Server Configuration

```json
"mcpServers": {
  "fetch": {
    "command": "python",
    "args": ["-m", "mcp_server_fetch"]
  }
}
```

> **Note**: You must install the Fetch MCP server before running it.

#### Installation

Install the server using pip:

```bash
pip install mcp-server-fetch
```

---

### 4. Google Maps MCP Server
- **Purpose**: Fetch geolocation, directions, or place data from Google Maps API.
- **Status**:  Working (API key required)
- **Use Case**: Location-based queries, directions, nearby places.
- **Notes**:
  - Requires Google Cloud billing & API key.
  - Quotas may apply.
  - You need to provide the API key.

**Get your Google Maps API key from:**  
- Go to the [https://console.cloud.google.com/](https://console.cloud.google.com/), create a project, and enable the Maps APIs you need (like Maps JavaScript API, Geocoding API, etc.).
- Go to APIs & Services → Credentials, click "Create Credentials" → "API Key", and copy the generated key.
- (Optional) Set up billing and restrict the key for security (recommended).

#### Google Maps Server Configuration

```json
"mcpServers": {
  "google-maps": {
    "command": "npx",
    "args": [
      "-y",
      "@modelcontextprotocol/server-google-maps"
    ],
    "env": {
      "GOOGLE_MAPS_API_KEY": "API KEY"
    }
  }
}
```

---

### 5. Sequential Thinking MCP Server
- **Purpose**: Enables chain-of-thought or multistep reasoning via subagent execution.
- **Status**:  Working
- **Use Case**: Tasks requiring intermediate reasoning or tool calling.
- **Notes**:
  - Works well with nested plans.
  - Supports recursive tool calling.

#### Sequential Thinking Server Configuration

```json
"mcpServers": {
  "sequential-thinking": {
    "command": "npx",
    "args": [
      "-y",
      "@modelcontextprotocol/server-sequential-thinking"
    ]
  }
}
```

---

### 6. Weather MCP

This is a simple MCP server that uses the [weather.gov](https://www.weather.gov/) API to fetch current weather conditions and forecasts for locations across the United States.

- **Purpose**: Fetch current weather and forecasts using external weather APIs.
- **Status**:  Working
- **Use Case**: Daily weather updates, travel planning, etc.
- **Notes**:
  - Requires city name or lat/lon.
  - May need API key for extended data.

#### Weather Server Configuration

```json
"mcpServers": {
  "weather": {
    "command": "uv",
    "args": [
      "run",
      "--with",
      "mcp[cli]",
      "mcp",
      "run",
      "path/to/weather_server.py"
    ]
  }
}
```

---

### 7. Slack MCP Server
- **Purpose**: Send messages, alerts, or updates to Slack channels or users using the Slack Web API.
- **Status**:  Working
- **Use Case**: Ideal for notifications, bot-based responses, and workflow automation via RAG pipelines or external triggers..
- **Notes**:
  - Can be used to send status updates from other MCP tools or agents.
  - Supports channel names or user IDs as message targets..

## Setup

1. Create a Slack App:
   - Visit the [Slack Apps page](https://api.slack.com/apps)
   - Click "Create New App"
   - Choose "From scratch"
   - Name your app and select your workspace

2. Configure Bot Token Scopes:
   Navigate to "OAuth & Permissions" and add these scopes:
   - `channels:history` - View messages and other content in public channels
   - `channels:read` - View basic channel information
   - `chat:write` - Send messages as the app
   - `reactions:write` - Add emoji reactions to messages
   - `users:read` - View users and their basic information
   - `users.profile:read` - View detailed profiles about users

4. Install App to Workspace:
   - Click "Install to Workspace" and authorize the app
   - Save the "Bot User OAuth Token" that starts with `xoxb-`

5. Get your Team ID (starts with a `T`) by following [this guidance](https://slack.com/help/articles/221769328-Locate-your-Slack-URL-or-ID#find-your-workspace-or-org-id)

#### Slack Server Configuration

```json
"mcpServers": {
    "slack": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-slack"
      ],
      "env": {
        "SLACK_BOT_TOKEN": "xoxb-your-bot-token",
        "SLACK_TEAM_ID": "T01234567",
        "SLACK_CHANNEL_IDS": "C01234567, C76543210"
      }
    }
  }
```
---


## MCP Server Setup & Integration Notes

- I have developed an **MCP Tool** that serves as the client layer to invoke these servers and retrieve structured responses.
- All servers have been tested using one of the following commands: `uv`, `npx`, or `python`, depending on the server's implementation.
- Output from each server is validated against the expected schema.

---

## Future Integrations

We plan to integrate additional MCP servers, like- Email, Calender, PDF/Document parsing MCP, etc.

---

## Testing Strategy

I have created an internal **MCP Tool** that automates testing and execution of MCP servers. It accepts:

- A **server configuration JSON** file
- A **user-defined task** as input

This tool uses the `mcp_use` module with a custom `MCPClient` and `MCPAgent` to communicate with MCP servers.

### Example Usage

```python
from mcp_use import MCPAgent, MCPClient

# Load MCP server configuration
client = MCPClient.from_config_file(config_file) # e.g., config_file = "brave_search_server.json"

# Initialize the MCP Agent with any supported LLM
agent = MCPAgent(
    llm=llm,  # e.g., llm = AzureChatOpenAI()
    client=client,
    max_steps=15,
    memory_enabled=True  # Enables built-in conversational memory
)

# Run the agent with a user input
response = await agent.run(user_input)
```

---

## Contributions

Feel free to contribute additional MCP wrappers or suggestions via pull requests!

---

## License

MIT License
