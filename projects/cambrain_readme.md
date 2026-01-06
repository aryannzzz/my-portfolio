<div align="center">

# 🧠 CamBrain

### AI-Powered Desktop Automation Agent

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Tests](https://img.shields.io/badge/tests-162%20passed-brightgreen.svg)]()
[![Code Style: Black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

**CamBrain is a vision-language desktop agent that understands your screen and executes tasks autonomously.**

[Features](#-features) • [Installation](#-installation) • [Quick Start](#-quick-start) • [Architecture](#-architecture) • [API Reference](#-api-reference) • [Contributing](#-contributing)

</div>

---

## 🎯 Overview

CamBrain combines computer vision, large language models (LLMs), and desktop automation to create an intelligent agent that can:

- **See** your screen through real-time capture and OCR
- **Understand** UI elements using vision encoders (SigLIP/CLIP)
- **Plan** multi-step tasks using LLM reasoning (ReAct pattern)
- **Execute** actions like clicking, typing, and scrolling
- **Learn** from successes and failures through reflexion
- **Stay safe** with built-in guardrails and rate limiting

## ✨ Features

### Vision Pipeline (Week 1)
- 🖥️ **Screen Capture**: Real-time screen capture at configurable FPS
- 📝 **OCR Extraction**: Text detection using PaddleOCR with bounding boxes
- 👁️ **Vision Encoding**: Scene understanding with SigLIP/CLIP embeddings
- 🔍 **Screen Parsing**: Structured JSON representation of UI elements

### LLM Integration (Week 2)
- 🤖 **Multi-Provider LLM**: Support for Gemini, OpenAI, Ollama, and more via LiteLLM
- 🔄 **ReAct Agent**: Thought → Action → Observation reasoning loop
- 📋 **Task Planning**: Decompose complex tasks into atomic actions
- 📝 **Prompt Templates**: Extensible prompt engineering system

### Action Execution (Week 3)
- 🖱️ **Action Mapping**: Natural language → screen coordinates
- ✅ **Action Verification**: Before/after comparison to verify success
- 🎯 **Fuzzy Matching**: Find UI elements even with typos
- 🔄 **Retry Logic**: Automatic retry on failure

### Memory & Learning (Week 4)
- 🔁 **Reflexion Loop**: Self-correcting agent with failure analysis
- 💾 **Memory Buffer**: Short-term context (last N screen states)
- 🗄️ **Vector Memory**: Long-term storage with ChromaDB (optional)
- 📊 **Preference Tracking**: Learn user patterns over time

### API & UI (Week 5)
- 🌐 **FastAPI Server**: REST API for agent control
- 📡 **WebSocket Streaming**: Real-time updates and logs
- 🖥️ **CLI Interface**: Command-line access to all features
- 🔧 **Configuration**: YAML-based settings

### Safety & Polish (Week 6)
- 🛡️ **Safety Guardrails**: Block dangerous actions (rm -rf, format, etc.)
- ⏱️ **Rate Limiting**: Prevent runaway automation
- ↩️ **Rollback**: Undo capabilities for reversible actions
- 🧹 **Input Sanitization**: Clean and validate action inputs

---

## 📦 Installation

### Prerequisites

- Python 3.10 or higher
- CUDA-capable GPU (recommended for vision models)
- Linux/macOS/Windows

### Quick Install

```bash
# Clone the repository
git clone https://github.com/yourusername/CamBrain.git
cd CamBrain

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Install CamBrain in development mode
pip install -e .
```

### GPU Support (Recommended)

```bash
# For NVIDIA GPUs, install CUDA-enabled PyTorch
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118

# For GPU-accelerated PaddleOCR
pip install paddlepaddle-gpu
```

### Optional Dependencies

```bash
# Vector memory (ChromaDB)
pip install chromadb sentence-transformers

# Local LLM support (Ollama)
pip install ollama
```

---

## 🚀 Quick Start

### Basic Usage (Python)

```python
from cambrain.agent import create_agent

# Create agent in supervised mode (requires confirmation for actions)
agent = create_agent(mode="supervised")

# Analyze current screen
analysis = agent.capture_and_analyze()
print(f"Found {len(analysis.elements)} UI elements")

# Run a task
result = agent.run_task("Open Chrome and search for Python tutorials")
print(f"Task {'succeeded' if result.success else 'failed'}")
```

### CLI Usage

```bash
# Analyze current screen
python main.py agent analyze

# Run a task
python main.py agent run "open calculator and compute 15 * 23"

# Start the API server
python main.py agent server --port 8000

# Interactive mode
python main.py agent interactive

# Check system status
python main.py agent status
```

### API Usage

```bash
# Start the server
python main.py agent server

# In another terminal, use the API
curl http://localhost:8000/api/status

curl -X POST http://localhost:8000/api/task/start \
  -H "Content-Type: application/json" \
  -d '{"task": "open notepad and type hello world"}'
```

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         CamBrain Agent                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐      │
│  │   Vision     │    │     LLM      │    │   Actions    │      │
│  │  Pipeline    │───▶│   Reasoning  │───▶│  Execution   │      │
│  └──────────────┘    └──────────────┘    └──────────────┘      │
│         │                   │                   │               │
│         ▼                   ▼                   ▼               │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐      │
│  │   Screen     │    │    ReAct     │    │   Action     │      │
│  │   Capture    │    │    Agent     │    │   Mapper     │      │
│  ├──────────────┤    ├──────────────┤    ├──────────────┤      │
│  │     OCR      │    │    Task      │    │   Action     │      │
│  │  Extractor   │    │   Planner    │    │  Verifier    │      │
│  ├──────────────┤    └──────────────┘    └──────────────┘      │
│  │   Vision     │                                               │
│  │   Encoder    │           ┌──────────────────────┐           │
│  ├──────────────┤           │       Memory         │           │
│  │   Screen     │           ├──────────────────────┤           │
│  │   Parser     │◀─────────▶│  Reflexion Loop     │           │
│  └──────────────┘           │  Memory Buffer      │           │
│                             │  Vector Memory      │           │
│                             │  Preference Tracker │           │
│                             └──────────────────────┘           │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                    Safety Layer                          │  │
│  │  [Rate Limiter] [Guardrails] [Sanitizer] [Rollback]     │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │                    API & Interface                        │  │
│  │        [FastAPI Server] [WebSocket] [CLI]                │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Project Structure

```
cambrain/
├── __init__.py           # Package initialization
├── core.py               # Main CamBrain class
├── cli.py                # Command-line interface
│
├── capture/              # Week 1: Screen capture
│   ├── __init__.py
│   └── screen.py         # ScreenCapture, Screenshot
│
├── ocr/                  # Week 1: Text extraction
│   ├── __init__.py
│   └── extractor.py      # OCRExtractor, OCRResult
│
├── vision/               # Week 1: Vision encoding
│   ├── __init__.py
│   └── encoder.py        # VisionEncoder
│
├── parser/               # Week 1: Screen parsing
│   ├── __init__.py
│   └── screen_parser.py  # ScreenParser, ScreenAnalysis
│
├── llm/                  # Week 2: LLM integration
│   ├── __init__.py
│   ├── client.py         # GeminiClient, LiteLLMClient
│   ├── react_agent.py    # ReActAgent
│   ├── task_planner.py   # TaskPlanner
│   ├── prompts.py        # PromptTemplates
│   └── primitives.py     # ActionResult, ActionExecutor
│
├── actions/              # Week 3: Action execution
│   ├── __init__.py
│   ├── mapper.py         # ActionMapper, ActionType
│   └── verifier.py       # ActionVerifier
│
├── memory/               # Week 4: Memory & learning
│   ├── __init__.py
│   ├── reflexion_loop.py # ReflexionLoop
│   ├── memory_buffer.py  # MemoryBuffer
│   ├── vector_memory.py  # VectorMemory
│   └── preference_tracker.py  # PreferenceTracker
│
├── api/                  # Week 5: REST API
│   ├── __init__.py
│   └── server.py         # FastAPI app
│
├── safety/               # Week 6: Safety guardrails
│   ├── __init__.py
│   └── layer.py          # SafetyGuard, RollbackManager
│
├── agent/                # Unified agent
│   ├── __init__.py
│   └── orchestrator.py   # CamBrainAgent
│
└── utils/                # Utilities
    ├── __init__.py
    ├── config.py         # Configuration
    └── logger.py         # Logging
```

---

## 📖 API Reference

### CamBrainAgent

The main agent class that orchestrates all components.

```python
from cambrain.agent import CamBrainAgent, AgentConfig, AgentMode

# Configuration options
config = AgentConfig(
    mode=AgentMode.SUPERVISED,      # AUTONOMOUS, SUPERVISED, OBSERVE_ONLY
    llm_model="gemini/gemini-2.0-flash",
    max_retries=3,
    enable_safety=True,
    memory_buffer_size=10,
)

# Create agent
agent = CamBrainAgent(config)

# Methods
agent.capture_and_analyze()     # Capture and analyze screen
agent.plan_task("task")         # Plan steps for a task
agent.execute_action("action")  # Execute single action
agent.run_task("task")          # Run complete task
agent.pause()                   # Pause agent
agent.resume()                  # Resume agent
agent.get_status()              # Get current status
agent.get_statistics()          # Get execution statistics
```

### Safety Guard

```python
from cambrain.safety import SafetyGuard, RiskLevel

guard = SafetyGuard()

# Check action safety
check = guard.check_action("delete important file")
print(f"Risk: {check.risk_level}, Blocked: {check.blocked}")

# Validate with confirmation
allowed, check = guard.validate_and_confirm("rm -rf /")
# blocked=True for dangerous actions
```

### Screen Analysis

```python
from cambrain.parser import ScreenParser

parser = ScreenParser()
analysis = parser.parse(screenshot, ocr_result)

# Access elements
for element in analysis.elements:
    print(f"{element.element_type}: {element.content} at {element.bbox}")
```

---

## 🧪 Testing

```bash
# Run all tests
pytest tests/ -v

# Run specific week tests
pytest tests/test_week1_comprehensive.py -v
pytest tests/test_week3_comprehensive.py -v
pytest tests/test_week5_6_comprehensive.py -v

# Run with coverage
pytest tests/ --cov=cambrain --cov-report=html
```

### Test Results

| Module | Tests | Status |
|--------|-------|--------|
| Week 1: Vision Pipeline | 18 | ✅ Pass |
| Week 2: LLM Integration | 15 | ✅ Pass |
| Week 3: Action Execution | 19 | ✅ Pass |
| Week 4: Memory | 25 | ✅ Pass |
| Week 5-6: API & Safety | 63 | ✅ Pass |
| Integration | 22 | ✅ Pass |
| **Total** | **162** | **✅ Pass** |

---

## ⚙️ Configuration

### config.yaml

```yaml
# Screen capture settings
capture:
  monitor: 0
  fps: 2.0
  scale: 1.0

# OCR settings
ocr:
  language: en
  use_gpu: true
  min_confidence: 0.5

# Vision encoder
vision:
  model: ViT-B-32
  pretrained: laion2b_s34b_b79k

# LLM settings
llm:
  model: gemini/gemini-2.0-flash
  temperature: 0.7
  max_tokens: 4096

# Safety settings
safety:
  enabled: true
  rate_limit_per_minute: 60
  blocked_patterns:
    - "rm -rf"
    - "format"
    - "shutdown"

# Memory settings
memory:
  buffer_size: 10
  enable_vector_memory: false
```

### Environment Variables

```bash
# LLM API Keys
export GOOGLE_API_KEY="your-gemini-key"
export OPENAI_API_KEY="your-openai-key"
export ANTHROPIC_API_KEY="your-anthropic-key"

# For OpenRouter
export OPENROUTER_API_KEY="your-openrouter-key"
```

---

## 🛡️ Safety Features

CamBrain includes multiple safety layers:

### Blocked Actions
- Disk formatting commands
- System shutdown/reboot
- Recursive deletion (`rm -rf /`)
- Registry editing (Windows)
- Sudo destructive commands

### High-Risk Actions (Require Confirmation)
- File deletion
- Sending emails/messages
- Financial transactions
- Password entry
- Software installation

### Rate Limiting
- Maximum actions per minute: 60
- Maximum actions per second: 5
- Cooldown after errors: 2 seconds

---

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### Development Setup

```bash
# Clone and install in development mode
git clone https://github.com/yourusername/CamBrain.git
cd CamBrain
pip install -e ".[dev]"

# Run tests
pytest tests/ -v

# Format code
black cambrain/
isort cambrain/

# Type checking
mypy cambrain/
```

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR) for OCR capabilities
- [OpenCLIP](https://github.com/mlfoundations/open_clip) for vision encoding
- [LiteLLM](https://github.com/BerriAI/litellm) for unified LLM access
- [FastAPI](https://fastapi.tiangolo.com/) for the API framework
- [ReAct Paper](https://arxiv.org/abs/2210.03629) for the reasoning pattern
- [Reflexion Paper](https://arxiv.org/abs/2303.11366) for self-correction

---

<div align="center">

**Built with ❤️ for autonomous desktop automation**

[Report Bug](https://github.com/yourusername/CamBrain/issues) • [Request Feature](https://github.com/yourusername/CamBrain/issues)

</div>