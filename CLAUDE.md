# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

6-week Master AI Agentic Engineering course repo with Jupyter notebooks and Python examples across multiple agent frameworks:

- `1_foundations/` — Core concepts: OpenAI API basics, function calling, Gradio UI
- `2_openai/` — OpenAI SDK deep dives
- `3_crew/` — CrewAI multi-agent examples (debate, coder, stock_picker, financial_researcher, engineering_team)
- `4_langgraph/` — LangGraph stateful graph-based agents
- `5_autogen/` — AutoGen conversation-based agents
- `6_mcp/` — Model Context Protocol server implementations (20+ community projects)
- `guides/` — Numbered Jupyter guide notebooks (`01_intro.ipynb` through `12_starting_your_project.ipynb`)
- `setup/` — Platform-specific setup guides (Windows, Mac, Linux)

## Setup

Requires Python 3.12+. Uses `uv` for dependency management:

```bash
uv pip compile pyproject.toml -o requirements.txt
pip install -r requirements.txt
```

Requires `OPENAI_API_KEY`. Create a `.env` file in the repo root.

## Running

Launch Jupyter from the repo root:
```bash
jupyter lab
```

Some subprojects have standalone Gradio apps:
```bash
python app.py    # starts at http://localhost:7860
```

## Architecture Patterns

### Agentic Loop (function calling)
LLM receives a problem → responds with tool calls → tools are executed → results fed back → loop until done. Tools are defined as JSON schemas with separate handler functions.

### CrewAI Multi-Agent (`3_crew/`)
YAML-driven agents with defined roles, goals, and backstories. Tasks declare dependencies; CrewAI handles sequential or hierarchical execution.

### LangGraph (`4_langgraph/`)
Stateful agents modeled as directed graphs. Nodes are processing steps; edges define transitions. Supports cycles and conditional routing.

### MCP (`6_mcp/`)
Model Context Protocol tool servers. Each community project exposes tools consumable by any MCP-compatible client.

## Key Environment Variables

| Variable | Purpose |
|----------|---------|
| `OPENAI_API_KEY` | Required by all notebooks/scripts |
| `OPENAI_MODEL` | Model override (default: `gpt-4o-mini`) |
| `ANTHROPIC_API_KEY` | Used in some notebooks |

## Frameworks in Use

- **OpenAI API** — function calling, GPT-4o/4o-mini, OpenAI Agents framework
- **CrewAI** — multi-agent crews with YAML config (`3_crew/`)
- **LangGraph** — stateful graph-based agents (`4_langgraph/`)
- **AutoGen** — conversation-based agents (`5_autogen/`)
- **MCP (Model Context Protocol)** — tool servers (`6_mcp/`)
- **Gradio** — web UI for interactive demos (`1_foundations/`, some crew projects)
