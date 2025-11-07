# MCP

{% embed url="https://www.youtube.com/watch?v=5B__zNXrFmg" %}



{% embed url="https://www.youtube.com/watch?v=DosHnyq78xY" %}

**Model Context Protocol (MCP): Complete Overview**

The **Model Context Protocol (MCP)** is an open standard developed by Anthropic to **standardize how AI applications (like chatbots, IDE assistants, or custom agents) connect with external tools, data sources, and systems**[1](https://www.philschmid.de/mcp-introduction)[2](https://en.wikipedia.org/wiki/Model_Context_Protocol)[3](https://www.keyvalue.systems/blog/mcp-explained-the-model-context-protocol-thats-powering-smarter-ai/). It acts as a "universal interface"—much like USB for hardware—allowing AI models to interact with external APIs, databases, files, and more, in a consistent, secure, and modular way.

### Key Concepts and Architecture

* **Client-Server Model:**
  * **Host:** The application the user interacts with (e.g., Claude Desktop, IDEs, custom agents).
  * **Client:** Lives within the host, manages a 1:1 connection to a specific MCP server.
  * **Server:** Exposes tools, resources, and prompts to the AI model via a standard API[1](https://www.philschmid.de/mcp-introduction)[3](https://www.keyvalue.systems/blog/mcp-explained-the-model-context-protocol-thats-powering-smarter-ai/)[4](https://auth0.com/blog/an-introduction-to-mcp-and-authorization/).
*   **Core MCP Primitives:**

    | Primitive | Control                | Description                                       | Example Use                  |
    | --------- | ---------------------- | ------------------------------------------------- | ---------------------------- |
    | Prompts   | User-controlled        | Interactive templates invoked by user choice      | Slash commands, menu options |
    | Resources | Application-controlled | Contextual data managed by the client application | File contents, API responses |
    | Tools     | Model-controlled       | Functions exposed to the LLM to take actions      | API calls, data updates      |
* **Lifecycle:**
  1. **Initialization:** Host creates MCP clients, which handshake with servers to exchange capabilities[1](https://www.philschmid.de/mcp-introduction)[4](https://auth0.com/blog/an-introduction-to-mcp-and-authorization/).
  2. **Discovery:** Clients query servers for available tools, resources, and prompts[1](https://www.philschmid.de/mcp-introduction)[4](https://auth0.com/blog/an-introduction-to-mcp-and-authorization/).
  3. **Invocation:** LLM (via the host) requests tool/resource/prompt execution[1](https://www.philschmid.de/mcp-introduction)[4](https://auth0.com/blog/an-introduction-to-mcp-and-authorization/).
  4. **Execution & Response:** Server processes the request and returns results to the client, which relays them to the host and LLM context[1](https://www.philschmid.de/mcp-introduction)[4](https://auth0.com/blog/an-introduction-to-mcp-and-authorization/).

### Applications of MCP

* **Desktop Assistants:** Securely access system tools and files (e.g., Claude Desktop).
* **Enterprise Automation:** Integrate with internal CRMs, knowledge bases, or proprietary databases.
* **Multi-tool Agent Workflows:** Coordinate actions across multiple tools (e.g., document lookup + messaging).
* **Natural Language Data Access:** Bridge LLMs with structured databases for plain-language queries.
* **Software Development:** IDEs and coding platforms use MCP to give coding assistants real-time project context[2](https://en.wikipedia.org/wiki/Model_Context_Protocol).

### Python-Based Example: Building an MCP Server

Below is a **minimal MCP server in Python** that exposes a tool to fetch the top chatters from a SQLite database. This demonstrates how you can make external data available to an LLM via MCP[6](https://www.digitalocean.com/community/tutorials/mcp-server-python)[5](https://github.com/ruslanmv/Simple-MCP-Server-with-Python).

### 1. Set Up Your Environment

```
bashpython -m venv mcp-env
source mcp-env/bin/activate  # On Windows: mcp-env\Scripts\activate
pip install mcp
```

### 2. Prepare the Database

* Download or create a `community.db` SQLite database with a `chatters` table containing columns `name` and `messages`.

### 3. Write the MCP Server

```
python# sqlite-server.py

from mcp.server.fastmcp import FastMCP
import sqlite3

# Initialize the MCP server with a friendly name
mcp = FastMCP("Community Chatters")

# Define a tool to fetch the top chatters from the SQLite database
@mcp.tool()
def get_top_chatters():
    """Retrieve the top chatters sorted by number of messages."""
    conn = sqlite3.connect('community.db')
    cursor = conn.cursor()
    cursor.execute("SELECT name, messages FROM chatters ORDER BY messages DESC")
    results = cursor.fetchall()
    conn.close()
    # Format the results as a list of dictionaries
    chatters = [{"name": name, "messages": messages} for name, messages in results]
    return chatters

# Run the MCP server locally
if __name__ == '__main__':
    mcp.run()
```

> This script exposes the `get_top_chatters` tool via MCP, allowing any compatible LLM host (like Claude Desktop or Cursor IDE) to invoke it and retrieve live data from your database[6](https://www.digitalocean.com/community/tutorials/mcp-server-python)[5](https://github.com/ruslanmv/Simple-MCP-Server-with-Python).

### Summary of How It Works

* **MCP Host** (e.g., Claude Desktop) connects to your MCP server.
* **LLM** receives a user prompt (e.g., "Show me the top chatters").
* **Host** detects the relevant tool (`get_top_chatters`) and invokes it via the MCP client.
* **Server** executes the tool, fetches data, and returns results.
* **LLM** incorporates the fresh data into its response to the user.

**MCP** thus enables **secure, modular, and standardized integration** between AI models and external systems, dramatically simplifying how AI applications interact with the world beyond their initial training data



1. [https://www.philschmid.de/mcp-introduction](https://www.philschmid.de/mcp-introduction)
2. [https://en.wikipedia.org/wiki/Model\_Context\_Protocol](https://en.wikipedia.org/wiki/Model_Context_Protocol)
3. [https://www.keyvalue.systems/blog/mcp-explained-the-model-context-protocol-thats-powering-smarter-ai/](https://www.keyvalue.systems/blog/mcp-explained-the-model-context-protocol-thats-powering-smarter-ai/)
4. [https://auth0.com/blog/an-introduction-to-mcp-and-authorization/](https://auth0.com/blog/an-introduction-to-mcp-and-authorization/)
5. [https://github.com/ruslanmv/Simple-MCP-Server-with-Python](https://github.com/ruslanmv/Simple-MCP-Server-with-Python)
6. [https://www.digitalocean.com/community/tutorials/mcp-server-python](https://www.digitalocean.com/community/tutorials/mcp-server-python)
7. [https://www.aalpha.net/blog/what-is-mcp-in-ai/](https://www.aalpha.net/blog/what-is-mcp-in-ai/)
8. [https://www.getzep.com/ai-agents/developer-guide-to-mcp](https://www.getzep.com/ai-agents/developer-guide-to-mcp)
9. [https://docs.unstructured.io/examplecode/tools/mcp](https://docs.unstructured.io/examplecode/tools/mcp)
10. [https://www.youtube.com/watch?v=mhdGVbJBswA](https://www.youtube.com/watch?v=mhdGVbJBswA)
11. [https://modelcontextprotocol.io/introduction](https://modelcontextprotocol.io/introduction)
12. [https://www.anthropic.com/news/model-context-protocol](https://www.anthropic.com/news/model-context-protocol)
13. [https://docs.mindsdb.com/mcp/overview](https://docs.mindsdb.com/mcp/overview)
14. [https://www.digitalocean.com/community/tutorials/model-context-protocol](https://www.digitalocean.com/community/tutorials/model-context-protocol)
15. [https://github.com/modelcontextprotocol/python-sdk](https://github.com/modelcontextprotocol/python-sdk)
16. [https://modelcontextprotocol.io/quickstart/client](https://modelcontextprotocol.io/quickstart/client)
17. [https://www.datacamp.com/tutorial/mcp-model-context-protocol](https://www.datacamp.com/tutorial/mcp-model-context-protocol)
18. [https://ai.pydantic.dev/mcp/run-python/](https://ai.pydantic.dev/mcp/run-python/)
19. [https://www.linkedin.com/pulse/build-custom-mcp-client-server-from-scratch-using-python-tavargere-odq2c](https://www.linkedin.com/pulse/build-custom-mcp-client-server-from-scratch-using-python-tavargere-odq2c)
20. [https://scrapfly.io/blog/how-to-build-an-mcp-server-in-python-a-complete-guide/](https://scrapfly.io/blog/how-to-build-an-mcp-server-in-python-a-complete-guide/)
