# oh-my-opencode-lite 🚀

A lightweight, powerful agent orchestration plugin for **OpenCode**. It transforms your AI assistant into a manager capable of delegating complex tasks to specialized sub-agents, running searches in the background, and managing multi-step workflows with ease.

## 🏗️ Architecture & Flow

The plugin follows a "Hub and Spoke" model:

1.  **The Orchestrator (Hub)**: The main entry point for user requests. It analyzes the task and decides which specialized agents to call.
2.  **Specialized Agents (Spokes)**: Domain-specific experts (e.g., UI/UX, Documentation, Architecture) that handle narrow tasks with high precision.
3.  **Background Manager**: A robust engine that allows the Orchestrator to "fire and forget" tasks (like deep codebase searches or documentation research) while continuing to work on other parts of the problem.

### The Flow of a Request
1. **User Prompt**: "Refactor the auth logic and update the docs."
2. **Orchestrator**: Creates a TODO list.
3. **Delegation**:
   - Launches an `@explore` background task to find all auth-related files.
   - Launches a `@librarian` task to check the latest documentation for the auth library used.
4. **Integration**: Once background results are ready, the Orchestrator performs the refactor.
5. **Finalization**: Passes the changes to `@document-writer` to update the README.

---

## 🤖 Meet the Agents

| Agent | Role | Best Used For... |
| :--- | :--- | :--- |
| **orchestrator** | Manager | Planning, task delegation, and overall coordination. |
| **oracle** | Architect | Complex debugging, architectural decisions, and code reviews. |
| **explore** | Searcher | Fast codebase grep, finding patterns, and locating definitions. |
| **librarian** | Researcher | External library docs, GitHub examples, and API research. |
| **frontend-ui-ux** | Designer | Visual changes, CSS/styling, and React/Vue component polish. |
| **document-writer** | Scribe | Technical documentation, READMEs, and inline code comments. |
| **multimodal-looker** | Visionary | Analyzing screenshots, wireframes, or UI designs. |

---

## 🛠️ Tools & Capabilities

### Background Tasks
The plugin provides three core tools to manage asynchronous work:

-   `background_task`: Launches an agent in a new session.
    -   `sync=true`: Blocks until the agent finishes (ideal for quick sub-tasks).
    -   `sync=false`: Runs in the background (ideal for long searches or research).
-   `background_output`: Fetches the result of a background task using its ID.
-   `background_cancel`: Aborts running tasks if they are no longer needed.

---

## ⚙️ Configuration

You can customize the behavior of the plugin via the OpenCode configuration.

### Customizing Agents
In your `.opencode/config.json` (or via the plugin's config hook), you can override models or prompts:

```json
{
  "oh-my-opencode-lite": {
    "agents": {
      "oracle": {
        "model": "claude-3-5-sonnet",
        "temperature": 0,
        "prompt_append": "Always prioritize security in your reviews."
      }
    },
    "disabled_agents": ["multimodal-looker"]
  }
}
```

---

## 👨‍💻 Development

### Getting Started
1.  **Install dependencies**:
    ```bash
    bun install
    ```
2.  **Run in development mode**:
    ```bash
    npm run dev
    ```
    This will build the plugin and launch OpenCode with the plugin active.

### Project Structure
-   `src/index.ts`: Plugin entry point and tool registration.
-   `src/agents/`: Definitions and system prompts for all sub-agents.
-   `src/features/background-manager.ts`: State management for async tasks.
-   `src/tools/background.ts`: Implementation of the `background_*` tools.
-   `src/config/`: Configuration schema and loading logic.

### Building
```bash
npm run build
```
Generates the `dist/` folder with bundled ESM code and TypeScript declarations.

---

## 📜 License
MIT
