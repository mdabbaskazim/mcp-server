# MCP Server Python

A lightweight Python-based Model Context Protocol (MCP) server that searches official documentation, fetches relevant pages, cleans the HTML/text, and returns structured results for a given library or topic.

This project is designed to help AI tools answer questions using current documentation from trusted sources instead of relying only on static training data.

## Features

- Search the web using Serper
- Fetch official documentation pages
- Clean and summarize web content with an LLM
- Expose a reusable `get_docs` MCP tool
- Support multiple frameworks and docs sources
- Includes a sample client script to test server interactions

## Supported libraries

The server currently supports:

- `langchain`
- `llama-index`
- `openai`
- `uv`

## Project structure

```text
.
├── client.py              # Example MCP client that calls the server
├── mcp_server.py          # MCP server implementation
├── utils.py               # LLM and HTML cleanup helpers
├── pyproject.toml         # Project metadata and dependencies
├── .env.example           # Optional example environment file
├── README.md              # Project documentation
├── uv.lock                # Lockfile for uv
└── .python-version        # Python version pin
```

## Requirements

- Python 3.11+
- `uv` package manager
- API keys for:
  - Serper (`SERPER_API_KEY`)
  - Groq (`GROQ_API_KEY`)

## Setup

1. Clone the repository
2. Create a `.env` file in the project root
3. Add your environment variables:

```bash
SERPER_API_KEY=your_serper_api_key
GROQ_API_KEY=your_groq_api_key
```

4. Install dependencies:

```bash
uv sync
```

## Run the server

```bash
uv run mcp_server.py
```

This starts the MCP server in stdio mode.

## Example client usage

The repository includes a sample client in `client.py` that connects to the server and calls the `get_docs` tool.

Run:

```bash
uv run python client.py
```

This script will:

- initialize the MCP server session
- list available tools
- call `get_docs` with a sample query
- pass the returned docs text to an LLM for a final answer

## MCP tool

### `get_docs(query: str, library: str)`

Searches official documentation relevant to the provided query and library.

Example:

```python
query = "How to publish a package with uv on GitLab"
library = "uv"
```

The tool will:

1. Build a search query limited to the library documentation domain
2. Search the web using Serper
3. Fetch the most relevant pages
4. Clean the content
5. Return the resulting documentation excerpts with source links

## Example output

The tool returns content shaped like this:

```text
SOURCE: https://docs.astral.sh/uv/... 
...

SOURCE: https://docs.astral.sh/uv/... 
...
```

The client then formats the content into a more readable answer for the user.

## Notes

- This project is a simple research-oriented MCP example rather than a production-ready full documentation system.
- The server depends on external APIs and internet access for live documentation fetches.
- LLM-based cleaning can improve readability, but output quality depends on the selected model and provider configuration.

## Future improvements

- Add more documentation sources
- Improve result ranking and filtering
- Support additional MCP tools beyond docs lookup
- Add better error handling for empty or failed searches
- Add tests for integration and parsing behavior

## License

This project does not currently include a license file. If you plan to publish it publicly, consider adding one such as MIT or Apache 2.0.
