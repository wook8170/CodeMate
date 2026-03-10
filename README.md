<div align="center"><sub>
English | Español | Deutsch | 日本語 | 简体中文 | 繁體中文 | 한국어
</sub></div>

# CodeMate

<p align="center">
  <img src="assets/docs/demo.gif" width="100%" />
</p>

<div align="center">
<table>
<tbody>
<td align="center">
<strong>CodeMate: Autonomous coding agent right in your IDE</strong>
</td>
</tbody>
</table>
</div>

Meet CodeMate, an AI assistant that can use your **CLI** and **Editor**.

Thanks to agentic coding capabilities, CodeMate can handle complex software development tasks step-by-step. With tools that let him create & edit files, explore large projects, use the browser, and execute terminal commands (after you grant permission), he can assist you in ways that go beyond code completion or tech support. CodeMate can even use the Model Context Protocol (MCP) to create new tools and extend his own capabilities. While autonomous AI scripts traditionally run in sandboxed environments, this extension provides a human-in-the-loop GUI to approve every file change and terminal command, providing a safe and accessible way to explore the potential of agentic AI.

1. Enter your task and add images to convert mockups into functional apps or fix bugs with screenshots.
2. CodeMate starts by analyzing your file structure & source code ASTs, running regex searches, and reading relevant files to get up to speed in existing projects. By carefully managing what information is added to context, CodeMate can provide valuable assistance even for large, complex projects without overwhelming the context window.
3. Once CodeMate has the information he needs, he can:
    - Create and edit files + monitor linter/compiler errors along the way, letting him proactively fix issues like missing imports and syntax errors on his own.
    - Execute commands directly in your terminal and monitor their output as he works, letting him e.g., react to dev server issues after editing a file.
    - For web development tasks, CodeMate can launch the site in a headless browser, click, type, scroll, and capture screenshots + console logs, allowing him to fix runtime errors and visual bugs.
4. When a task is completed, CodeMate will present the result to you.

> [!TIP]
> Follow VSCode guides to open CodeMate in the sidebar. This lets you use CodeMate side-by-side with your file explorer.

---

### Use any API and Model

CodeMate supports API providers like OpenRouter, Anthropic, OpenAI, Google Gemini, AWS Bedrock, Azure, GCP Vertex, Cerebras and Groq. You can also configure any OpenAI compatible API, or use a local model through LM Studio/Ollama.

### Run Commands in Terminal

CodeMate can execute commands directly in your terminal and receive the output. This allows him to perform a wide range of tasks, from installing packages and running build scripts to deploying applications, managing databases, and executing tests.

### Create and Edit Files

CodeMate can create and edit files directly in your editor, presenting you a diff view of the changes. You can edit or revert CodeMate's changes directly in the diff view editor.

### Use the Browser

CodeMate can launch a browser, click elements, type text, and scroll, capturing screenshots and console logs at each step. This allows for interactive debugging, end-to-end testing, and even general web use!

### Checkpoints: Compare and Restore

As CodeMate works through a task, the extension takes a snapshot of your workspace at each step. You can use the 'Compare' button to see a diff between the snapshot and your current workspace, and the 'Restore' button to roll back to that point.

## License

[Apache 2.0 © 2026 CodeMate Bot Inc.](./LICENSE)
