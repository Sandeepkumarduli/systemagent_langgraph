# 🤖 LangGraph OS Agent Builder


⚠️ I didnt keep any layer to Stop running Dangerous and Malicious commands in Your machine So Be careful While Running this 

A collection of LangGraph-powered AI agents that can interact with your operating system via shell commands. Built with OpenAI + LangChain + LangGraph.

---

## 📁 Project Structure

```
windows-agent/
├── agentbuilder.ipynb      # Agent with a single cross-platform shell tool
├── agentbuilding.ipynb     # Agent with separate OS-specific tools + Human-in-the-Loop
├── practise.ipynb          # Beginner notebook — agent with a read-only OS info tool
├── app/                    # FastAPI app (optional)
├── requirements.txt
└── .env                    # Your API keys (create this manually)
```

---

## ⚙️ Environment Setup

### 1. Install dependencies

```bash
pip install -r requirements.txt
```

### 2. Create a `.env` file

Create a `.env` file in the project root with the following keys:

```env
OPENAI_API_KEY=sk-...          # Your OpenAI API key (required by all notebooks)
MODEL_NAME=gpt-4o              # Model to use, e.g. gpt-4o, gpt-4o-mini, gpt-3.5-turbo
```

> **Note:** `practise.ipynb` only needs `OPENAI_API_KEY` — it hardcodes `gpt-4o` and does not use `MODEL_NAME`.

---

## 📓 Notebooks

---

### 1. `agentbuilder.ipynb` — Single Cross-Platform Shell Tool

**Tool used:** `run_shell_command`

This notebook builds an agent with a **single unified shell tool** that automatically detects your OS at runtime:
- On **macOS / Linux** → runs `zsh` commands
- On **Windows** → runs `PowerShell` commands

No Human-in-the-Loop — the agent runs commands directly after deciding what to do.

**`.env` keys required:**
```env
OPENAI_API_KEY=sk-...
MODEL_NAME=gpt-4o
```

**How to run:**
1. Open `agentbuilder.ipynb` in Jupyter or VS Code.
2. Make sure `.env` is present in the same directory.
3. Run all cells top-to-bottom (Kernel → Restart & Run All).
4. At the bottom, call `chat("your question")` to interact with the agent.

**Example queries:**
```python
chat("What are the specifications of this machine?")
chat("Create a folder called test_agent on the Desktop")
chat("What is the current date and time?")
```

---

### 2. `agentbuilding.ipynb` — Separate OS-Specific Tools + Human-in-the-Loop ✋

**Tools used:** `run_shell_command_mac` and `run_shell_command_windows`

This notebook builds a more advanced agent with **two separate OS-specific tools**:
- `run_shell_command_mac` — only runs on macOS (uses `zsh`)
- `run_shell_command_windows` — only runs on Windows (uses `PowerShell`)

The LLM automatically picks the right tool based on the detected OS.

🔐 **Key feature — Human-in-the-Loop:** Before every shell command is executed, the agent **pauses and asks for your approval**:
```
============================================================
Planned command(s):
   Tool    : run_shell_command_mac
   Command : system_profiler SPHardwareDataType
============================================================
Run this command? (yes / no):
```
Type `yes` to allow or `no` to cancel. This makes it safe to use for sensitive system operations.

**`.env` keys required:**
```env
OPENAI_API_KEY=sk-...
MODEL_NAME=gpt-4o
```

**How to run:**
1. Open `agentbuilding.ipynb` in Jupyter or VS Code.
2. Make sure `.env` is present in the same directory.
3. Run all cells top-to-bottom (Kernel → Restart & Run All).
4. Use `chat("your question")` — you will be prompted in the terminal/output to approve each command.

**Example queries:**
```python
chat("What are the specifications of this machine?")
chat("Can you create a folder named agentic_folder on the Desktop?")
chat("How many files are in my Documents folder?")
```

---

### 3. `practise.ipynb` — Beginner Agent with Read-Only OS Info Tool

**Tool used:** `get_os_details`

This is a **practice/learning notebook** — great for understanding LangGraph basics. It uses a simple Python function as a tool instead of running shell commands:
- `get_os_details()` — returns OS info (system, machine, Python version, architecture, etc.) without executing any shell commands. Safe and read-only.

The agent uses `gpt-4o` (hardcoded) and only needs `OPENAI_API_KEY`.

**`.env` keys required:**
```env
OPENAI_API_KEY=sk-...
```

> `MODEL_NAME` is **not needed** for this notebook.

**How to run:**
1. Open `practise.ipynb` in Jupyter or VS Code.
2. Make sure `.env` is present in the same directory.
3. Run all cells top-to-bottom.
4. Interact using `graph.invoke({"messages": "your question"})`.

**Example queries:**
```python
graph.invoke({"messages": "What is my OS details?"})
graph.invoke({"messages": "Hi, what is the capital city of France?"})
```

---

## 🔑 `.env` Quick Reference

| Variable        | Used In                                        | Example value       |
|-----------------|------------------------------------------------|---------------------|
| `OPENAI_API_KEY`| All three notebooks                            | `sk-abc123...`      |
| `MODEL_NAME`    | `agentbuilder.ipynb` & `agentbuilding.ipynb`   | `gpt-4o`            |

---

## 🧩 Notebook Comparison

| Feature                        | `agentbuilder.ipynb` | `agentbuilding.ipynb` | `practise.ipynb`    |
|-------------------------------|----------------------|-----------------------|---------------------|
| Shell tool                     | ✅ Single unified    | ✅ Two OS-specific    | ❌ No shell access  |
| OS auto-detection              | ✅ Yes               | ✅ Yes                | ✅ Python only      |
| Human-in-the-Loop approval     | ❌ No                | ✅ Yes                | ❌ No               |
| macOS support                  | ✅ Yes               | ✅ Yes                | ✅ Yes              |
| Windows support                | ✅ Yes               | ✅ Yes                | ✅ Yes              |
| Needs `MODEL_NAME` in `.env`   | ✅ Yes               | ✅ Yes                | ❌ No               |
| Good for beginners             | ⚡ Intermediate      | 🔐 Advanced           | ✅ Beginner         |

---

## 🚀 Quickstart

```bash
# 1. Clone / navigate to the project
cd "windows-agent"

# 2. Install dependencies
pip install -r requirements.txt

# 3. Create .env
echo "OPENAI_API_KEY=sk-your-key-here" > .env
echo "MODEL_NAME=gpt-4o" >> .env

# 4. Launch Jupyter
jupyter notebook
```

Then open any of the three notebooks and run all cells.
