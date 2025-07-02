# MCP Server Integrations

This repository contains a comprehensive list of all the MCP (Model Context Protocol) servers I have experimented with. Each server is designed to perform a specific function, and is tested for compatibility within the MCP framework.

---

##  Tested MCP Servers

### 1. Brave Search MCP
- **Purpose**: Perform real-time search queries using Brave’s search engine.
- **Status**:  Working
- **Use Case**: General-purpose web search, especially for privacy-focused results.
- **Notes**:
  - Fast responses.
  - Sometimes limited by request quotas.

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

### 2. Tavily Search MCP
- **Purpose**: Use the Tavily API for structured and concise web search.
- **Status**:  Working
- **Use Case**: Helpful in retrieval-augmented generation (RAG) pipelines.
- **Notes**:
  - More structured than Brave.
  - Good for summarizing answers directly.

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

### 3. Fetch MCP
- **Purpose**: Generic HTTP GET/POST-based fetching of data from external URLs or APIs.
- **Status**:  Working
- **Use Case**: Useful for scraping or calling arbitrary REST APIs.
- **Notes**:
  - Flexible but needs request config.
  - Requires validation for rate limits and headers.

#### 🔧 Fetch Server Configuration

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

### 4. Google Maps MCP
- **Purpose**: Fetch geolocation, directions, or place data from Google Maps API.
- **Status**:  Working (API key required)
- **Use Case**: Location-based queries, directions, nearby places.
- **Notes**:
  - Requires Google Cloud billing & API key.
  - Quotas may apply.

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

### 5. Sequential Thinking MCP
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

### 7. IP Info MCP

This is a simple MCP server that uses the [ipinfo.io](https://ipinfo.io) API to get detailed information about an IP address.

- **Purpose**: Retrieve geolocation and network information based on an IP address using the IPInfo API.
- **Status**:  Working
- **Use Case**: Useful for IP address lookup, security monitoring, analytics, and location-based services.
- **Notes**:
  - Requires a valid IPInfo API token.
  - Useful in dashboards or any tool needing IP metadata.

#### IP Info Server Configuration

```json
"mcpServers": {
  "ipinfo": {
    "command": "uvx",
    "args": [
      "--from",
      "git+https://github.com/briandconnelly/mcp-server-ipinfo.git",
      "mcp-server-ipinfo"
    ],
    "env": {
      "IPINFO_API_TOKEN": "<YOUR TOKEN HERE>"
    }
  }
}
```

> **Note**: You'll need to create a token to use the IPInfo API. Sign up for a free account at [ipinfo.io/signup](https://ipinfo.io/signup).

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
