# Connecting Claude Code to Chrome Browser via MCP (Playwright)

## Overview

Claude Code connects to a Chrome (and other browsers) using the **Model Context Protocol (MCP)** with the **Playwright MCP server**. This gives Claude the ability to navigate, interact with, and inspect web pages in real time.

---

## How It Works

```
Claude Code (AI Agent)
       │
       │  MCP Protocol (JSON-RPC over stdio/socket)
       ▼
Playwright MCP Server  (@playwright/mcp or custom)
       │
       │  Playwright API
       ▼
Browser (Chromium / Firefox / WebKit)
       │
       ▼
   Web Page
```

- Claude sends **tool calls** like `browser_navigate`, `browser_click`, `browser_snapshot`
- The **MCP server** translates these into **Playwright API** calls
- Playwright controls the actual browser via the **Chrome DevTools Protocol (CDP)**

---

## Prerequisites

### 1. Node.js and npm
```bash
node --version   # v18+ recommended
npm --version
```

### 2. Install Playwright MCP Server
```bash
npm install -g @playwright/mcp
# or locally in your project
npm install @playwright/mcp
```

### 3. Install Playwright Browsers
```bash
npx playwright install chromium
# or all browsers
npx playwright install
```

---

## Configuration

### Option A — Claude Desktop App (`claude_desktop_config.json`)

Located at:
- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "playwright-chrome": {
      "command": "npx",
      "args": ["@playwright/mcp", "--browser", "chromium"],
      "env": {}
    },
    "playwright-firefox": {
      "command": "npx",
      "args": ["@playwright/mcp", "--browser", "firefox"],
      "env": {}
    },
    "playwright-webkit": {
      "command": "npx",
      "args": ["@playwright/mcp", "--browser", "webkit"],
      "env": {}
    }
  }
}
```

### Option B — Claude Code CLI (`.claude/settings.json`)

Located at the root of your project or `~/.claude/settings.json` for global config:

```json
{
  "mcpServers": {
    "playwright-chrome": {
      "command": "npx",
      "args": ["@playwright/mcp", "--browser", "chromium", "--headless"]
    }
  }
}
```

> Remove `--headless` if you want to **see** the browser window during automation.

---

## Available MCP Browser Tools

Once connected, Claude can use these tools:

| Tool | Description |
|------|-------------|
| `browser_navigate` | Go to a URL |
| `browser_snapshot` | Capture accessibility tree (better than screenshot for actions) |
| `browser_take_screenshot` | Take a visual screenshot |
| `browser_click` | Click an element |
| `browser_type` | Type text into a field |
| `browser_fill_form` | Fill multiple form fields at once |
| `browser_select_option` | Select a dropdown option |
| `browser_hover` | Hover over an element |
| `browser_drag` | Drag and drop between elements |
| `browser_press_key` | Press a keyboard key |
| `browser_scroll` | Scroll the page |
| `browser_wait_for` | Wait for text to appear/disappear |
| `browser_evaluate` | Run JavaScript on the page |
| `browser_network_requests` | Inspect all network requests |
| `browser_console_messages` | Get browser console output |
| `browser_tabs` | List, create, close, or switch tabs |
| `browser_navigate_back` | Go back in browser history |
| `browser_resize` | Resize the browser window |
| `browser_handle_dialog` | Accept or dismiss dialogs |
| `browser_file_upload` | Upload files via file input |
| `browser_run_code` | Run full Playwright JS code snippets |
| `browser_close` | Close the browser |

---

## Example Usage in Claude Code

Once the MCP server is configured and running, you can ask Claude:

```
"Navigate to https://example.com and take a screenshot"
"Click the Login button and type my credentials"
"Find all network requests made to /api/users"
"Fill out the form on the page and submit it"
```

Claude will automatically use the appropriate `browser_*` MCP tools.

---

## Verifying the Connection

### Check MCP Servers are running
In Claude Code CLI, type:
```
/mcp
```
This lists all connected MCP servers and their status.

### Test Browser Connection
Ask Claude:
```
"Navigate to https://example.com and give me the page title"
```
If the browser is connected, Claude will return the page title from the live browser.

---

## Headless vs Headed Mode

| Mode | Flag | Use Case |
|------|------|----------|
| **Headless** (default) | `--headless` | CI/CD, automation, no GUI needed |
| **Headed** (visible) | *(omit `--headless`)* | Debugging, watching what Claude does |

To run headed (see the browser window):
```json
"args": ["@playwright/mcp", "--browser", "chromium"]
```

---

## Connecting to an Existing Chrome Instance

If you want Claude to connect to an **already-running Chrome** (with your existing session/cookies):

### Step 1: Launch Chrome with Remote Debugging
```bash
# macOS
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome \
  --remote-debugging-port=9222 \
  --user-data-dir=/tmp/chrome-debug

# Linux
google-chrome --remote-debugging-port=9222

# Windows
chrome.exe --remote-debugging-port=9222
```

### Step 2: Configure MCP to Connect to It
```json
{
  "mcpServers": {
    "playwright-chrome": {
      "command": "npx",
      "args": [
        "@playwright/mcp",
        "--browser", "chromium",
        "--cdp-endpoint", "http://localhost:9222"
      ]
    }
  }
}
```

---

## Troubleshooting

### MCP server not connecting
```bash
# Test the MCP server manually
npx @playwright/mcp --browser chromium

# Should output JSON-RPC ready message
```

### Browser not launching
```bash
# Reinstall browsers
npx playwright install chromium --force
```

### Permission issues (macOS)
```bash
# Allow Chrome to be controlled
sudo xattr -d com.apple.quarantine $(which chromium) 2>/dev/null || true
```

### View MCP Logs
```bash
# Claude Code logs location (macOS)
tail -f ~/Library/Logs/Claude/mcp*.log
```

---

## Architecture Summary

```
┌─────────────────────────────────────────────────────┐
│                   Claude Code (AI)                  │
│  - Sends tool_use blocks: browser_navigate, etc.    │
└────────────────────────┬────────────────────────────┘
                         │ MCP (JSON-RPC over stdio)
┌────────────────────────▼────────────────────────────┐
│            @playwright/mcp Server                   │
│  - Receives tool calls                              │
│  - Translates to Playwright API                     │
└────────────────────────┬────────────────────────────┘
                         │ Playwright Node.js API
┌────────────────────────▼────────────────────────────┐
│         Chromium / Firefox / WebKit                 │
│  - Controlled via Chrome DevTools Protocol (CDP)    │
│  - Renders pages, executes JS, captures DOM         │
└─────────────────────────────────────────────────────┘
```

---

## References

- [Playwright MCP GitHub](https://github.com/microsoft/playwright-mcp)
- [Model Context Protocol Docs](https://modelcontextprotocol.io)
- [Claude Code MCP Configuration](https://docs.anthropic.com/claude-code/mcp)
- [Playwright Docs](https://playwright.dev)
