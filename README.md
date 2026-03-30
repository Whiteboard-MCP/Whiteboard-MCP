# Whiteboard MCP

**Give your AI the power to draw.** Architecture diagrams, flowcharts, wireframes, sketches — generated from a single chat message.

Whiteboard MCP is a [Model Context Protocol](https://modelcontextprotocol.io/) server that gives AI agents like Claude a real canvas to draw on. Your AI describes what to draw, and you watch it appear in real-time.

[![npm version](https://img.shields.io/npm/v/whiteboard-mcp)](https://www.npmjs.com/package/whiteboard-mcp)
[![License: Proprietary](https://img.shields.io/badge/license-Proprietary-blue)](https://whiteboard-mcp.com)

---

## What is Whiteboard MCP?

Whiteboard MCP connects your AI coding agent to an interactive drawing canvas. Instead of describing diagrams in text or generating static images, the AI draws directly on a whiteboard that you can see, interact with, and export.

**One prompt. One diagram. Zero manual drawing.**

```
> Look at this codebase and draw me an architecture diagram

Claude: I'll analyze the codebase and create a diagram.
  ▸ open_canvas()
  ▸ create_flowchart(nodes: [...], edges: [...])
  ▸ take_screenshot()

Here's your architecture diagram showing the API Gateway
routing to Auth, Core, and Worker services.
```

The canvas opens in your browser. Shapes appear in real-time as the AI draws. You can pan, zoom, select, move, resize — it's a full whiteboard.

---

## Quick Start

### Install

Add Whiteboard MCP to your editor's MCP configuration:

<details>
<summary><strong>Claude Code</strong></summary>

Add to `.mcp.json` in your project root, or `~/.claude/settings.json` for global access:

```json
{
  "mcpServers": {
    "whiteboard": {
      "command": "npx",
      "args": ["-y", "whiteboard-mcp"]
    }
  }
}
```

Restart Claude Code. Then just ask:

> "Open the whiteboard and draw me a flowchart of the login process"

</details>

<details>
<summary><strong>Claude Desktop</strong></summary>

Edit your Claude Desktop config:

- **Mac:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "whiteboard": {
      "command": "npx",
      "args": ["-y", "whiteboard-mcp"]
    }
  }
}
```

</details>

<details>
<summary><strong>Cursor</strong></summary>

Add to `.cursor/mcp.json` in your project root:

```json
{
  "mcpServers": {
    "whiteboard": {
      "command": "npx",
      "args": ["-y", "whiteboard-mcp"]
    }
  }
}
```

</details>

<details>
<summary><strong>VS Code (Copilot)</strong></summary>

Add to `.vscode/mcp.json` in your project root:

```json
{
  "servers": {
    "whiteboard": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "whiteboard-mcp"]
    }
  }
}
```

</details>

<details>
<summary><strong>Windsurf</strong></summary>

Add to `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "whiteboard": {
      "command": "npx",
      "args": ["-y", "whiteboard-mcp"]
    }
  }
}
```

</details>

<details>
<summary><strong>Zed</strong></summary>

Add to your Zed `settings.json` under `context_servers`:

```json
{
  "context_servers": {
    "whiteboard": {
      "command": {
        "path": "npx",
        "args": ["-y", "whiteboard-mcp"]
      }
    }
  }
}
```

</details>

<details>
<summary><strong>Any MCP Client</strong></summary>

Run the server directly:

```bash
npx -y whiteboard-mcp
```

Or configure your MCP client to launch it with `command: "npx"` and `args: ["-y", "whiteboard-mcp"]` using stdio transport.

</details>

### Try it

After installing, tell your AI:

- *"Open the whiteboard and draw me an architecture diagram of this project"*
- *"Draw a flowchart showing the user registration process"*
- *"Sketch a dog sniffing some flowers in the sunshine"*
- *"Create a diagram with a web client, API gateway, auth service, and database"*

---

## Features

### 30+ Drawing Tools

| Category | Tools |
|----------|-------|
| **Shapes** | `draw_rectangle` `draw_ellipse` `draw_diamond` |
| **Lines** | `draw_line` `draw_arrow` `draw_freehand` |
| **Content** | `add_text` `add_image` |
| **Diagrams** | `add_labeled_box` `connect_shapes` `create_flowchart` |
| **Manipulation** | `move_element` `resize_element` `update_element` `delete_elements` `duplicate_elements` |
| **Layer** | `bring_to_front` `send_to_back` |
| **Canvas** | `open_canvas` `clear_canvas` `set_viewport` `set_canvas_background` `toggle_grid` |
| **Export** | `take_screenshot` `save_png` `save_svg` `save_json` `export_json` `export_svg` |
| **History** | `undo` `redo` |
| **Query** | `get_canvas_info` `get_elements` `find_elements` |

### One-Call Diagrams

The `create_flowchart` tool generates complete auto-laid-out diagrams from a simple graph definition:

```
create_flowchart({
  nodes: [
    { id: "client", label: "Web Client" },
    { id: "api", label: "API Gateway" },
    { id: "db", label: "PostgreSQL", type: "ellipse" }
  ],
  edges: [
    { from: "client", to: "api", label: "HTTPS" },
    { from: "api", to: "db", label: "SQL" }
  ],
  layout: "TB"
})
```

One tool call. Complete diagram. Auto-positioned nodes, labeled arrows, proper spacing.

### Smart Arrows

`connect_shapes` creates arrows that bind to shapes. When you move a shape, the arrow follows. The AI builds diagrams with connected components, not disconnected shapes.

### Interactive Canvas

The canvas isn't just for the AI — you can use it too:

- **Draw** — Rectangle, ellipse, diamond, line, arrow, freehand, text tools
- **Select & Move** — Click to select, drag to move
- **Resize** — Drag corner handles to resize any element
- **Pan & Zoom** — Scroll to pan, Ctrl+scroll to zoom, hand tool for drag-pan
- **Snap** — Snap to grid and snap to shapes for precise positioning
- **Keyboard shortcuts** — V (select), R (rectangle), O (ellipse), D (diamond), L (line), A (arrow), P (pen), T (text), H (hand), Delete, Ctrl+Z/Y

### Export

- **PNG** — Raster export for sharing and documentation
- **SVG** — Vector export for high-quality printing and editing
- **JSON** — Save and load your canvas state

### Preferences

- Canvas background color (including dark mode)
- Grid visibility toggle
- Snap to grid
- Snap to shapes

---

## How It Works

Whiteboard MCP runs as a local MCP server on your machine. When your AI agent calls `open_canvas`, it starts a lightweight web server and opens the canvas in your browser. All drawing happens through MCP tool calls — the AI sends commands, the canvas renders them in real-time via WebSocket.

```
AI Agent (Claude, Copilot, etc.)
    │
    │ MCP protocol (stdio)
    ▼
Whiteboard MCP Server
    │
    ├── HTTP server (serves canvas UI)
    ├── WebSocket (real-time state sync)
    │
    ▼
Browser Canvas (your whiteboard)
```

Everything runs locally. No cloud services, no data leaving your machine, no accounts to create.

---

## Requirements

- **Node.js 18+** (for npx)
- **Any modern browser** (Chrome, Firefox, Edge, Safari)
- **An MCP-compatible AI agent** (Claude Code, Claude Desktop, Cursor, VS Code, Windsurf, Zed, etc.)

---

## Pricing

Whiteboard MCP includes a **14-day free trial** — no credit card required. After the trial, purchase a license for a one-time payment of **$29**.

[Buy a license](https://buy.polar.sh/polar_cl_NUuC4w4fbvLkX8V2IGccJo6eVCEtrVWsVq6AB3PcZxu)

### Activating Your License

After purchasing, add your license key to your MCP config:

```json
{
  "mcpServers": {
    "whiteboard": {
      "command": "npx",
      "args": ["-y", "whiteboard-mcp"],
      "env": {
        "WHITEBOARD_MCP_LICENSE_KEY": "YOUR-KEY-HERE"
      }
    }
  }
}
```

Restart your editor. The trial banner changes to **PRO**.

---

## Support

- **Issues & Feature Requests:** [GitHub Issues](https://github.com/Whiteboard-MCP/Whiteboard-MCP/issues)
- **Email:** [whiteboard-mcp@fastmail.com](mailto:whiteboard-mcp@fastmail.com)
- **Discord:** Available to license holders — join link provided after purchase

---

## FAQ

<details>
<summary><strong>Does it work with Claude Code?</strong></summary>

Yes. Claude Code is the primary target. Add the MCP config and Claude can draw diagrams, flowcharts, and sketches directly.

</details>

<details>
<summary><strong>Does it work with Cursor / VS Code / Windsurf / Zed?</strong></summary>

Yes. Whiteboard MCP works with any editor or AI agent that supports the Model Context Protocol (MCP). See the install instructions above for editor-specific configuration.

</details>

<details>
<summary><strong>Do I need to install anything besides Node.js?</strong></summary>

No. `npx whiteboard-mcp` downloads and runs everything automatically. No global installs, no build steps, no dependencies to manage.

</details>

<details>
<summary><strong>Does it send data to the cloud?</strong></summary>

No. Everything runs locally on your machine. The only network call is license key validation (a single POST to verify your key). Your drawings, code, and data never leave your computer.

</details>

<details>
<summary><strong>Can I use it without an AI agent?</strong></summary>

The canvas has a full interactive UI — you can draw shapes, move elements, and export manually. But the real power is having an AI drive it. That's what Whiteboard MCP is built for.

</details>

<details>
<summary><strong>What happens after the trial?</strong></summary>

After 14 days, the tools still work but you'll see periodic reminders to purchase a license. Buy once for $29 and own it forever — lifetime updates included.

</details>

<details>
<summary><strong>Can I get a refund?</strong></summary>

Yes. If Whiteboard MCP doesn't work for you, email whiteboard-mcp@fastmail.com within 30 days for a full refund.

</details>

---

## Links

- [Website](https://whiteboard-mcp.com)
- [npm](https://www.npmjs.com/package/whiteboard-mcp)
- [Buy a License](https://buy.polar.sh/polar_cl_NUuC4w4fbvLkX8V2IGccJo6eVCEtrVWsVq6AB3PcZxu)
- [Report an Issue](https://github.com/Whiteboard-MCP/Whiteboard-MCP/issues)

---

*Built for developers who think visually.*
