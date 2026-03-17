# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a collection of independent AI agent projects, each in its own directory:

- `agents_engg/` — Educational course (6-week Master AI Agentic Engineering) with Jupyter notebooks covering multiple frameworks (OpenAI, CrewAI, LangGraph, AutoGen, MCP)
- `agentic-ai-task-agent/` — Production-style Python library for autonomous task planning using OpenAI function calling
- `career_ai_assist_agent/` — Gradio chatbot that answers career questions using RAG + function calling
- `deep_research_agent/` — Multi-agent research system with web search, report writing, and email delivery
- `trendwise_stock_picker/` — CrewAI-based stock picker with YAML-driven agent configuration

These are independent projects — each has its own `requirements.txt` and `.env` setup.

## Common Setup

All projects require `OPENAI_API_KEY`. Copy `.env.example` to `.env` and fill in values where applicable.

```bash
pip install -r requirements.txt
```

The `agents_engg/` course project uses `pyproject.toml` with `uv`:
```bash
uv pip compile pyproject.toml -o requirements.txt
pip install -r requirements.txt
```

## Commands by Project

### agentic-ai-task-agent
```bash
pip install -e .                                        # editable install
pytest tests/                                           # run tests
pytest tests/ --cov=agentic --cov-report=html          # with coverage
python -m agentic.cli solve "your problem" --verbose   # CLI usage
python examples/interactive_demo.py                     # interactive demo
```

### career_ai_assist_agent
```bash
python run.py    # starts Gradio UI at http://localhost:7860
```

### deep_research_agent
```bash
python run.py    # starts Gradio UI at http://localhost:7860
```

### trendwise_stock_picker
```bash
python -m trendwise_stock_picker.main
```

## Architecture Patterns

### Agentic Loop (function calling)
Used in `agentic-ai-task-agent`, `career_ai_assist_agent`, `deep_research_agent`:
- Agent sends problem to LLM → LLM responds with tool calls → Agent executes tools → results fed back to LLM → loop until done
- Tools are defined as JSON schemas; handlers are separate functions

### Multi-Agent Orchestration
Used in `deep_research_agent` and `trendwise_stock_picker`:
- Specialized agents (planner, searcher, writer, email sender) with defined roles
- Context/output passes between agents sequentially
- `deep_research_agent` uses the OpenAI Agents framework with async processing
- `trendwise_stock_picker` uses CrewAI with hierarchical delegation

### YAML-Driven Agent Config (trendwise_stock_picker)
Agent roles, goals, backstories, and LLM assignments live in `config/agents.yaml`; task descriptions and dependencies in `config/tasks.yaml`. Runtime behavior changes by editing YAML, not Python.

### Memory Management
- `agentic-ai-task-agent`: `TodoMemory` tracks task state across the agentic loop
- `career_ai_assist_agent`: conversation history preserved for multi-turn context
- `trendwise_stock_picker`: CrewAI long-term, short-term, and entity memory

## Key Environment Variables

| Variable | Used By |
|----------|---------|
| `OPENAI_API_KEY` | All projects |
| `OPENAI_MODEL` | Most projects (default: `gpt-4o-mini`) |
| `SENDGRID_API_KEY` | `deep_research_agent` (email delivery) |
| `PUSHOVER_USER` / `PUSHOVER_TOKEN` | `trendwise_stock_picker` (push notifications) |
| `SERVER_HOST` / `SERVER_PORT` | Gradio-based projects |

## Frameworks in Use

- **OpenAI API** — function calling, GPT-4o/4o-mini, OpenAI Agents framework
- **CrewAI** — multi-agent crews with YAML config (`trendwise_stock_picker`, `agents_engg/3_crew/`)
- **LangGraph** — stateful graph-based agents (`agents_engg/4_langgraph/`)
- **AutoGen** — conversation-based agents (`agents_engg/5_autogen/`)
- **MCP (Model Context Protocol)** — tool servers (`agents_engg/6_mcp/`)
- **Gradio** — web UI for `career_ai_assist_agent` and `deep_research_agent`
