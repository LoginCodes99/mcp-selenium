# Gemini Context: mcp-selenium

## Project Overview

This project is a Node.js application that implements a Model Context Protocol (MCP) server for Selenium WebDriver. Its purpose is to expose browser automation capabilities (such as launching browsers, navigating, clicking elements, and taking screenshots) as a set of tools that can be called by an MCP-compatible client, like an AI agent or a chatbot.

The server is built using the `@modelcontextprotocol/sdk` and uses `selenium-webdriver` to interact with web browsers (Chrome, Firefox, Edge). It uses `zod` for runtime schema validation of tool parameters. The server communicates over standard I/O, making it easy to integrate with various parent processes.

The core logic resides in `src/lib/server.js`, which defines the available tools and their implementation. The `bin/mcp-selenium.js` script acts as an executable wrapper to launch the server.

## Building and Running

### Prerequisites
- Node.js (v18 or later recommended, as per Dockerfile)
- npm

### Installation

To install project dependencies, run:
```bash
npm install
```

### Running the Server

There are multiple ways to run the server:

**1. Directly with Node.js (for development):**
```bash
node src/lib/server.js
```

**2. As an Executable (if installed globally or via npx):**
The `package.json` file defines a `bin` entry, allowing the server to be run as a command.
```bash
# Using npx without installation
npx @angiejones/mcp-selenium

# Or, if installed globally with `npm install -g .`
mcp-selenium
```

**3. Using Docker:**
The project includes a `Dockerfile` to containerize the application. The container includes the Chromium browser.

- **Build the image:**
  ```bash
  docker build -t mcp-selenium .
  ```
- **Run the container:**
  The server listens on standard I/O, so you need to run the container with the `-i` (interactive) flag.
  ```bash
  docker run -i mcp-selenium
  ```

## Development Conventions

- **Code Style:** The codebase uses ES Modules (`import`/`export`). It is not configured with a linter or automatic formatter, but follows a consistent style.
- **Tooling:** The server defines a comprehensive suite of tools for browser automation, detailed in `README.md`. Each tool's input is validated using a `zod` schema.
- **Testing:** The `package.json` file does not contain a functional test script. The `test` script currently returns an error: `"Error: no test specified"`.
- **Entrypoint:** The main entry point for execution is `src/lib/server.js`. A wrapper script in `bin/mcp-selenium.js` is provided for easier command-line execution.
- **State Management:** The server maintains a simple in-memory state in `src/lib/server.js` to track active WebDriver sessions.
