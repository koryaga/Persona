# Persona — universal AI agent

## Purpose

Persona is a lightweight AI agent running from cli that:
- performs general user tasks, like document processing, internet search and data manipulation
- supports [Anthropic skills](https://agentskills.io/home) from the `skills/` directory to satisfy user requests

## Key features

- exchanges files with the user via the folder (see `--mnt-dir`)
- Runs shell commands and executes generated Python code inside a disposable Docker Ubuntu sandbox container
- Loads and exposes skill files to the agent

## Quick start

Prerequisites: Docker and Python 3.13+.

1. Set up environment:

Below are default for local _ollama_ usage with _Cogito v1_ LLM model:
```bash
OPENAI_MODEL=cogito:14b
OPENAI_API_KEY=your-api-key
OPENAI_API_BASE=http://localhost:11434/v1
```

2. Create and activate the virtualenv:

```bash
uv venv
source .venv/bin/activate
uv sync
```

2. Build the sandbox image (the project uses `ubuntu.sandbox` by default):

```bash
docker build -t ubuntu.sandbox .
```

4. Run the agent:

Usage: `python3 main.py [--mnt-dir PATH] [--skills-dir PATH]`

- `--mnt-dir`: Host directory to mount at `/mnt` inside the sandbox (default:  `mnt` folder in current directory). The directory is only mounted if it exists on the host.
- `--skills-dir`: Host directory to mount at `/skills` inside the sandbox (default:  `skills` folder in current directory). The directory is only mounted if it exists on the host.

Examples:
```bash
# start with default  mounts (only mounted if dirs exist)
python3 main.py

# specify absolute host paths for mounts
python3 main.py --mnt-dir /home/user/project/ --skills-dir /home/user/persona/skills

# or specify relative paths (they will be expanded to absolute paths)
python3 main.py --mnt-dir ./mnt --skills-dir ./skills
```