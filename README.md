<div align="left">
 <h1> <img src="logo.ico" alt="CrawlLama Logo" width="64" height="64"> CrawlLama</h1>
</div>

![Python Version](https://img.shields.io/badge/python-3.10%2B-blue)
![Platform](https://img.shields.io/badge/platform-Windows-lightblue)
![Platform](https://img.shields.io/badge/platform-Linux-lightgrey)
![License](https://img.shields.io/badge/license-Non--Commercial-orange)
![Status](https://img.shields.io/badge/status-Active-success)
[![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/arn-c0de/Crawllama)

[Documentation](docs/README.md) | [Quickstart](docs/getting-started/QUICKSTART.md) | [API Guide](docs/guides/API_USAGE.md) | [Adaptive Hops](docs/getting-started/ADAPTIVE_HOPS_QUICKSTART.md) | [Security](SECURITY.md) | [Changelog](CHANGELOG.md)

[Project Website](https://arn-c0de.github.io/Crawllama/)

**AI Research Agent with OSINT and Multi-Hop Reasoning** 
Current Version: `1.4.11` (Refactor & Code Quality)
New in `1.4.8`: [Company Intelligence Developer Documentation](docs/osint/COMPANY_INTELLIGENCE.md)

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Images](#images)
- [Quickstart](#quickstart)
- [Installation](#installation)
- [Usage](#usage)
- [REST API](#rest-api)
- [CLI Commands and Options](#cli-commands-and-options)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [Testing](#testing)
- [Plugin Development](#plugin-development)
- [Technology Stack](#technology-stack)
- [Documentation](#documentation)
- [Roadmap](#roadmap)
- [Performance](#performance)
- [Legal Notices](#legal-notices)
- [Troubleshooting](#troubleshooting)
- [Support and Community](#support-and-community)
- [Contributing](#contributing)
- [License](#license)
- [Credits](#credits)
- [Further Documentation](#further-documentation)

---

## Overview

A locally-capable AI research agent with advanced intelligence features (local processing requires a local LLM backend such as Ollama; cloud-based LLM APIs like Claude or OpenAI are **not** local):
- OSINT module: email, phone, and IP intelligence; social media analysis; advanced search operators
- Multi-hop reasoning using LangGraph for complex queries
- Adaptive agent selection based on query complexity (low/mid/high)
- REST API with FastAPI for integration
- Plugin system for extensibility
- Performance optimizations with large context support and asynchronous execution

## Features

### Core features

- **Adaptive agent hopping system** – Automatic agent selection based on query complexity (low/mid/high), confidence-based escalation, and resource-aware adaptation (NEW v1.4.4)
- **UI settings for adaptive report** – Toggle the Adaptive Intelligence Report directly from the interactive settings menu (NEW v1.4.4)
- **Multi-hop reasoning** – LangGraph-based agent with a multi-step workflow (Router → Search → Analyze → Follow-Up → Synthesize → Critique)
- **Restart command** – Restart the agent without exiting the program
- **Parallelization** – Multi-aspect searches using thread pools for improved performance
- **Performance optimizations** – 16k context support for RTX 3080; async and parallel processing
- **Multi-source web search** – DuckDuckGo, Brave Search, Serper API with fallback
- **Wikipedia integration** – Dedicated Wikipedia search (German/English)
- **Advanced RAG system** – Batch processing, multi-query and hybrid search ([RAG Analysis](docs/guides/RAG_ANALYSIS.md))
- **Intelligent caching** – TTL-based with hash keys, LRU eviction, and a configurable max size (500MB)
- **Tool orchestration** – Automatic tool selection via LLM
- **Interactive settings menu** – Live configuration of LLM, search, RAG and OSINT
- **Context usage tracker** – Real-time token usage monitoring using tiktoken
- **Health monitoring dashboard** – Interactive system monitoring with a rich UI
- **Lazy-loading** – On-demand loading for tools and plugins
- **Async operations** – Parallel HTTP requests with aiohttp
- **Resource monitoring** – RAM usage, performance tracking, and automated garbage collection
- **FastAPI REST API** – 40+ endpoints with auto-documentation (`/query`, `/plugins`, `/stats`, `/health`) (see `app.py`)
- **Plugin system** – Dynamic loading and unloading of plugins
- **Enhanced CLI** – Rich formatting and Markdown output
- **Setup scripts** – `setup.bat`, `setup.sh` with auto-configuration
- Optional cloud LLM support (note: using cloud APIs such as Claude or OpenAI means data is sent to external servers and is **not** processed locally)

### OSINT features

- **Advanced search operators** – `site:`, `inurl:`, `intext:`, `filetype:`, `email:`, `phone:`, `ip:`
- **Email intelligence** – Validation, MX records, disposable detection, and variations
- **Phone intelligence** – Validation, carrier lookup, country detection, and formatting
- **Persistent memory store** – Survives `/clear`; stores emails, phones, IPs, usernames, domains and notes
- **Memory store CRUD** – Full CRUD functionality with `forget` command
- **Batch processing** – Analyze multiple emails or phones simultaneously with summary statistics
- **IP intelligence** – IPv4/IPv6 analysis, geolocation, ISP info, security reputation and VPN detection
- **Social intelligence** – Supports 12 platforms (GitHub, LinkedIn, Twitter, Instagram, Facebook, YouTube, Reddit, Pinterest, TikTok, Twitch, Telegram, Discord)
- **AI query enhancement** – Query variations, operator suggestions, entity detection and auto-type detection
- **Compliance module** – Rate limiting, terms of use, audit logging and robots.txt compliance
- **Privacy protection** – Ethical scraping, usage tracking; no API keys required
- **Safesearch quality filter** – Configurable result quality (off/moderate/strict)

### Security & performance

- **Code quality** – Refactored, focused methods for better maintainability
- **Accurate token counting** – tiktoken integration for precise token counts
- **Intelligent retry logic** – Tenacity-based retries with exponential backoff
- **Rate limiting** – 1 request/second and robots.txt checks
- **Fallback system** – Automatic fallbacks for API failures
- **Secure config** – Encrypted API key storage
- **Output validation** – Sanitization of LLM outputs
- **Domain blacklist** – Protection against unwanted domains
- **RTX 3080 optimization** – 16k context support (qwen3:8b), increased cache sizes
- **Windows console compatibility** – ASCII output and UTF-8 encoding for robust CLI experience (NEW v1.4.4)
- **Clear-all command** – Instantly reset session, cache, and memory from the CLI (NEW v1.4.4)
- **Tor mode** – Optionally route all outbound web traffic (crawling, search, OSINT, cloud LLM calls) through a Tor SOCKS5 proxy with a fail-fast startup check and DNS-leak prevention (see [Tor Mode](#tor-mode-anonymous-fetching))
- **Local operation** – LLM processing happens locally **only when using a local backend such as Ollama**. When using cloud-based APIs (e.g., Claude, OpenAI), queries are sent to external servers and are **not** processed locally. Web searches and data fetching always require internet access regardless of the LLM backend. For enhanced privacy, we recommend using a VPN or proxy.

## Images

### Health Dashboard - Live System Monitor
Real-time monitoring with rich terminal UI displaying system metrics, component health, and performance tracking.

![Health Dashboard](images/screenshots/health.png)

### Interactive CLI Interface
CrawlLama's adaptive intelligence system with automatic agent selection and interactive commands.

![CLI Interface](images/screenshots/main.png)

### Test Dashboard GUI
Tkinter-based test management interface with automatic test detection and real-time progress tracking.

![Test Dashboard](images/screenshots/test-dashboard.png)

## Quickstart

## Installation

> **Important — Keep dependencies up to date:**
> Dependencies are managed with [uv](https://docs.astral.sh/uv/) via [`pyproject.toml`](pyproject.toml) and [`uv.lock`](uv.lock). If you are using a pre-built / downloaded package release (zip/tar), always install dependencies from the **latest `pyproject.toml`/`uv.lock` in this repository** rather than any bundled copy.
> Package versions are updated regularly to address security vulnerabilities and compatibility issues.
> ```bash
> uv sync
> ```
> Always fetch the current files from the repository before installing to ensure you have the latest, secure package versions.

**Windows:**
1. Download Crawllama
2. Extract to any folder (e.g., `C:\Crawllama`)
3. Install Ollama from [ollama.ai/download](https://ollama.ai/download)
4. Start Ollama and load model:
 ```cmd
 ollama serve
 ollama pull qwen3:8b
 ```
5. In the Crawllama folder:
 ```cmd
 setup.bat
 run.bat
 ```

**Linux/macOS:**
1. Download and extract:
 ```bash
 wget https://github.com/arn-c0de/Crawllama/archive/refs/heads/main.zip
 unzip main.zip
 cd Crawllama-main
 ```
2. Install Ollama:
 ```bash
 curl -fsSL https://ollama.ai/install.sh | sh
 ollama serve &
 ollama pull qwen3:8b
 ```
3. Setup and start:
 ```bash
 chmod +x setup.sh run.sh
 ./setup.sh
 ./run.sh
 ```

---

### Option 1: Setup Scripts (Recommended for Git Installation)

**Windows:**
```cmd
setup.bat
```

**Linux/macOS:**
```bash
chmod +x setup.sh
./setup.sh
```

Note: After the initial setup, you must select at least one LLM model during setup. If a model is already installed, you can skip this step—otherwise, selection is required to avoid errors in the test program.

The setup script:
- Checks Python version (3.10+)
- Creates virtual environment
- Lets you select features and LLM models to install (core is always installed)
- Installs all selected dependencies
- Creates necessary directories
- Copies `.env.example` to `.env`
- Checks Ollama status

Note for initial installation:

When running `uv sync` for the first time, installing all dependencies—especially packages like `chromadb` and its ONNX runtime and scientific libraries—may take **5–10 minutes** (or longer, depending on connection and hardware). Please wait until the process completes; afterward, the virtual environment is ready for use.

Note on disk space: After installation (including `venv`), the project typically requires about **1.2–1.5 GB** of free disk space (v1.4: ~1.23 GB). This value may vary significantly depending on the operating system, Python packages (e.g., larger PyTorch/CUDA wheels), and additional models. Plan for ample additional space if storage is limited.

Model download sizes (approximate):

- `qwen3:4b` — ~**2–4 GB** (depending on format/quantization)
- `qwen3:8b` — ~**8–12 GB**
- `deepseek-r1:8b` — ~**6–10 GB**
- `llama3:7b` — ~**6–9 GB**
- `mistral:7b` — ~**4–8 GB**
- `phi3:14b` — ~**12–20+ GB**

Note: Model sizes vary significantly depending on the provider, format (FP16, INT8 quantization, etc.), and additional assets. Quantized models (e.g., INT8) can significantly reduce size, while FP32/FP16 or models with additional tokenizer/vocab files require more space. Plan for sufficient additional storage if using larger models or multiple models simultaneously.

### Option 2: Manual Installation

**Prerequisites:**
- Python 3.10+ ([python.org](https://www.python.org/downloads/))
- Git ([git-scm.com](https://git-scm.com/downloads))
- Ollama ([ollama.ai/download](https://ollama.ai/download))

**Windows - Step by Step:**

```cmd
# 1. Clone repository
git clone https://github.com/arn-c0de/Crawllama.git
cd Crawllama

# 2. Install dependencies with uv (creates the virtual environment, takes 5-10 min)
uv sync

# 3. Create directories
mkdir data\cache data\embeddings data\history logs plugins

# 4. Configuration
copy .env.example .env
notepad .env # Optional: Add API keys

# 5. Start Ollama (separate terminal)
ollama serve

# 6. Load model (separate terminal)
ollama pull qwen3:8b

# 7. Start Crawllama (interactive mode starts when no question is given)
uv run python main.py
```

**Linux/macOS - Step by Step:**

```bash
# 1. Clone repository
git clone https://github.com/arn-c0de/Crawllama.git
cd Crawllama

# 2. Install dependencies with uv (creates the virtual environment, takes 5-10 min)
uv sync

# 3. Create directories
mkdir -p data/cache data/embeddings data/history logs plugins

# 4. Configuration
cp .env.example .env
nano .env # Optional: Add API keys

# 5. Install and start Ollama
curl -fsSL https://ollama.ai/install.sh | sh
ollama serve &

# 6. Load model
ollama pull qwen3:8b

# 7. Start Crawllama (interactive mode starts when no question is given)
uv run python main.py
```

**Troubleshooting Installation:**

| Problem | Solution |
|---------|--------|
| `python not found` | Install Python 3.10+: [python.org](https://www.python.org/downloads/) |
| `pip install` fails | Run `python -m pip install --upgrade pip` |
| `ollama: command not found` | Install Ollama: [ollama.ai/download](https://ollama.ai/download) |
| `Connection refused` (Ollama) | Start Ollama: `ollama serve` |
| `ModuleNotFoundError` | Activate virtual environment: `venv\Scripts\activate` (Win) or `source venv/bin/activate` (Linux) |
| Disk space full | Ensure at least 5 GB free for venv + model |

---

### Option 3: Git Clone (Quick Installation)

```bash
# 1. Clone
git clone https://github.com/arn-c0de/Crawllama.git
cd Crawllama

# 2. Dependencies (uv creates the virtual environment)
uv sync

# 3. Directories
mkdir -p data/cache data/embeddings data/history logs plugins

# 4. Config
cp .env.example .env
```

### Ollama Setup

```bash
# Install Ollama
curl -fsSL https://ollama.ai/install.sh | sh # Linux/macOS
# or from https://ollama.ai/download # Windows

# Start Ollama
ollama serve

# Load model
ollama pull qwen3:8b
# Alternative: qwen3:4b, deepseek-r1:8b, llama3:7b, mistral
```

## Usage

> **Note:** 
> The first start may take significantly longer than subsequent starts! 
> Initialization, dependency installation, and model downloads may take several minutes, depending on hardware and internet connection. 
> After the first successful start, all subsequent starts are significantly faster.

### 1. CLI - Interactive Mode

```bash
# Interactive mode starts automatically when no question is given
python main.py

# Or with setup script
run.bat # Windows
./run.sh # Linux/macOS
```

```
╭──────────────────────────────────────────────────────────────╮
│ CrawlLama - Local Search and Response Agent │
│ Commands (slash-prefixed): │
│ /clear - Reset session (history + cache) │
│ /clear-cache - Clear cache only │
│ /save - Manually save session │
│ /load - Reload session │
│ /stats - Display statistics │
│ /status - Show context usage │
│ /settings - Show/edit settings │
│ /restart - Restart agent (reload config) │
│ /exit, /quit - Exit │
╰──────────────────────────────────────────────────────────────╯

 What is Machine Learning?
```

**New Commands:**

- `/status` - Shows token usage and available context capacity
 ```
 /status

 Context Usage Tracker
 ┏━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━┳━━━━━━━━━━━┓
 ┃ Source ┃ Tokens ┃ Share ┃
 ┡━━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━╇━━━━━━━━━━━┩
 │ Conversation │ 850 │ 8.5% │
 │ Search Results │ 320 │ 3.2% │
 │ Total Used │ 1,170 │ 11.7% │
 │ Available │ 8,830 │ 88.3% │
 │ Maximum │ 10,000 │ 100% │
 └───────────────────┴───────────┴───────────┘
 ```

- `/settings` - Interactive configuration editor
 ```
 /settings

 Displays all settings and allows:
 • Category selection (llm, search, rag, cache, osint, all)
 • Change LLM model (qwen3:8b, deepseek-r1:8b, etc.)
 • Adjust temperature (0.0-1.0)
 • Configure max tokens (now 16,000 for RTX 3080+)
 • Change search region (de-de, us-en, wt-wt)
 • Configure OSINT max results & rate limits
 • Enable/disable RAG
 • Enable/disable cache
 • Save changes directly to config.json
 • Auto-restart after saving (optional)
 ```

- `/restart` - Restart agent
 ```
 /restart

 • Reloads config.json
 • Fully reinitializes agent
 • Optional session preservation
 • No session interruption
 ```

### 2. Health Monitoring Dashboard

```bash
# Windows
health-dashboard.bat

# Linux/macOS
python health-dashboard.py
```

The dashboard displays:
- System health (CPU, RAM, disk, network)
- Component status (LLM, cache, RAG, tools)
- Performance metrics (response times)
- Alerts (top 5 active alerts)
- Auto-refresh (every 2 seconds)

Interactive commands:
- `r` - Refresh (manual)
- `c` - Clear error log
- `t` - Run component tests
- `q` - Quit

### 3. How does intelligent search work?

The agent automatically decides **when and how** to search:

#### Automatic decision
```
 Who is the current German Chancellor?

1. LLM analyzes: "Requires current info" 
2. Agent performs web search
3. LLM processes search results
4. Agent delivers up-to-date response
```

#### Search operators for targeted searches

**OSINT Search Operators:**
```bash
# Domain-specific search
 site:github.com machine learning

# Email Intelligence
 email:john.doe@company.com

# Phone Intelligence
 phone:"+49 151 12345678"

# IP Intelligence (NEW!)
 ip:8.8.8.8
 192.168.1.1 # Auto-detects as IP

# Social Media Intelligence (12 Platforms)
 username:elonmusk
 @microsoft
 github # Auto-detects as username

# File format search
 site:example.com filetype:pdf

# URL filter
 inurl:documentation python

# Text in content
 intext:"contact email" site:example.com
```

**Combined Searches:**
```bash
# Multiple operators
 site:linkedin.com inurl:profile "software engineer"

# Exclusion with minus
 python programming -java

# OR conjunction
 site:github.com OR site:gitlab.com "machine learning"
```

See **[OSINT Usage Guide](docs/osint/OSINT_USAGE.md)** for all features.

### 4. CLI - Direct Queries

```bash
# Standard query (agent decides automatically if web search is needed)
python main.py "What is Python?"

# Multi-Hop Reasoning (auto-selected by the adaptive system for complex queries)
python main.py "Compare Python and JavaScript for web development"

# Offline mode (no web search, only LLM knowledge)
python main.py --no-web "Explain photosynthesis"

# OSINT search with search operators
python main.py "site:github.com python projects"
python main.py "email:contact@example.com"

# With specific model
python main.py --model llama3:7b "Who discovered Einstein?"
```

### 5. FastAPI Server

```bash
# Start server
python app.py

# Or with starter scripts
run_api.bat # Windows
./run_api.sh # Linux/macOS

# Or manually
uvicorn app:app --host 0.0.0.0 --port 8000
```

**API Documentation:** http://localhost:8000/docs

**Available Endpoints:**

**Query & Reasoning:**
- `POST /query` - Execute standard or multi-hop queries
- `POST /query-adaptive` - Adaptive query (automatic agent/complexity selection)
- `POST /osint/query` - OSINT queries with operators (email:, phone:, ip:, etc.)
- `POST /osint/company` - Company-focused OSINT query (name, country, region, language)

**Memory Store (CRUD):**
- `GET /memory` - Retrieve all stored entries
- `POST /memory/remember` - Store value (email, phone, ip, username, domain; notes are CLI-only)
- `GET /memory/recall/{category}` - Retrieve category (emails, phones, ips, etc.)
- `DELETE /memory/forget` - Delete individual values, categories, or everything
- `GET /memory/stats` - Memory store statistics

**Session Management:**
- `POST /session/clear` - Reset session
- `POST /session/save` - Save session
- `POST /session/load` - Load session

**Cache:**
- `POST /cache/clear` - Clear cache
- `GET /cache/stats` - Cache statistics

**Configuration:**
- `GET /config` - Retrieve current configuration
- `PATCH /config` - Modify configuration (llm, search, rag, cache, osint)
- `GET /context/status` - Token usage & context status

**Plugins & Tools:**
- `GET /plugins` - List available plugins
- `POST /plugins/{name}/load` - Load plugin
- `POST /plugins/{name}/unload` - Unload plugin
- `GET /tools` - List available tools

**System:**
- `GET /api` - API information (name, version, links)
- `GET /health` - Health check (agent, monitoring, components)
- `GET /stats` - System statistics (agent stats, resources, performance)
- `GET /secu