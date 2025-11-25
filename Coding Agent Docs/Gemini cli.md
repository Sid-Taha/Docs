# Gemini CLI Learning Notes

A comprehensive guide and reference for using the Gemini CLI coding agent.

## 📦 Installation & Setup

**Install via NPM:**

```bash
npm install -g @google/gemini-cli
```

**Verify Installation:**

```bash
gemini -v
```

**Authentication:**
To link your account and set up credentials:

```bash
gemini /auth
```

-----

## 🚀 Basic Usage

**Start the Agent:**
Run the following command in any terminal window to start an interactive session:

```bash
gemini
```

**Get Help:**
To view a list of available commands and documentation within the tool:

```bash
/help
```

**Select a Specific Model:**
You can specify the model version using the `--model` flag:

```bash
gemini --model gemini-2.5-flash
```

### Modes of Operation

| Flag / Shortcut | Mode | Description |
| :--- | :--- | :--- |
| `--yolo` or `Ctrl+Y` | **Autonomous (YOLO)** | "You Only Look Once." The agent runs fully autonomously, executing commands and editing files without asking for permission every time. |
| `--debug` | **Debug Mode** | Starts the CLI with verbose logging, useful for troubleshooting agent behavior and errors. |

-----

## 💻 Core Concepts

### The Kernel & The Agent

The **Kernel** is the core program of the operating system that controls everything inside the computer. It manages system resources like CPU and RAM, handles file I/O (Read/Write), and controls peripheral devices (Keyboard, Mouse, Network).

**How Gemini CLI uses this:**
The Gemini CLI acts as a user-space application that interfaces with the Kernel. It can:

1.  **Create/Edit Files:** Writing code directly into your project.
2.  **Execute Commands:** Running shell commands to install packages or test code.
3.  **Manage Directories:** Organizing project structure.

### Command Line Interactions

| Command | Action |
| :--- | :--- |
| `/tools` | View the active tools available to the agent (e.g., file writer, terminal). |
| `!` | **Manual Shell Command.** Type this before a command to run it in your terminal immediately (e.g., `!npm install`). |
| `@` | **Context Selection.** Use this to explicitly add a file to the agent's context window (e.g., `@server.js`). |

-----

## 💾 State Management & Memory

### Checkpoints

The CLI allows you to save and resume your work using checkpoints.

**Starting with Checkpoints:**

```bash
gemini -m gemini-2.5-flash -c
```

*The `-c` flag enables checkpointing.*

**Restoring a Session:**
If you started with the `-c` flag, you can return to a previous state:

```bash
/restore
```

**Checkpoint Storage Location:**
Checkpoints are stored in your root configuration folder:
`~/.gemini/tmp/<long-string-id>/checkpoints/`

### Memory

To inspect what the agent currently "knows" or has retained in context:

  * `/memory show` - Displays current memory context.
  * `/memory list` - Lists available memory segments.

### Performance Stats

To view consumption and usage statistics:

  * `/stats model` - View token usage and model performance.
  * `/stats tool` - View tool execution frequency and results.

-----

## ⚙️ Configuration & Customization

The Gemini CLI is highly configurable via Markdown files for prompts and JSON files for settings.

### 1\. System Prompts (`GEMINI.md`)

This file tells the agent how to behave, what coding style to use, or specific project rules.

  * **Global Prompt:** `~/.gemini/GEMINI.md` (Applies to *all* projects).
  * **Project Prompt:** Create a `GEMINI.md` file in your specific project folder. The agent will prioritize this when working in that directory.
  * **Folder Specific:** You can place a `GEMINI.md` inside any sub-folder to give specific instructions for that module.

### 2\. Settings (`settings.json`)

Used to configure technical aspects like MCP servers and context paths.

  * **Global Settings:** `~/.gemini/settings.json`
  * **Project Settings:** Create a `settings.json` in your project root to override global settings.

#### Example: Defining Context

You can pre-load specific files into the agent's context automatically.

```json
{
  "context": {
    "files": ["plan.md", "GEMINI.md"]
  }
}
```

-----

## 🔌 Model Context Protocol (MCP)

You can extend Gemini CLI's capabilities by connecting it to MCP Servers (e.g., GitHub, Postgres, etc.).

**Configuration:**
Add the `mcpServers` block to your `settings.json` file:

```json
{
  "mcpServers": {
    "github": {
      "httpUrl": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "Authorization": "<your_access_token>"
      },
      "timeout": 5001
    }
  }
}
```

-----

