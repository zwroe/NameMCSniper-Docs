# Installation

This guide installs the current NameMCSniper v3.0.0 CLI.

## Requirements

- Python **3.10 or newer**
- Git
- A terminal: PowerShell on Windows, or a shell on Linux/macOS
- A correct system clock for live timing

!!! info "No account required for testing"
    The default mode is safe simulation. You can install, configure, and test the program without a Minecraft account, bearer token, Discord webhook, proxy, VPS, NTP access, or public network calls.

## Clone the Repository

```bash
git clone https://github.com/zwroee/NameMCSniper.git
cd NameMCSniper
```

## Create a Virtual Environment

Windows:

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install --upgrade pip
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
```

Linux/macOS:

```bash
python3 -m venv .venv
.venv/bin/python -m pip install --upgrade pip
.venv/bin/python -m pip install -r requirements.txt
```

## Create the Default Config

```bash
python Main.py config-create
```

This creates `config.yaml`. The app also supports sibling files named `tokens.txt` and `proxies.txt` for secrets and proxy lists.

!!! warning "Do Not Commit Secrets"
    `config.yaml`, `tokens.txt`, `proxies.txt`, and generated proxy files are ignored by the app repository because they can contain credentials.

## Verify the Install

```bash
python Main.py --version
python Main.py --help
python Main.py config-validate
python Main.py simulate
```

Expected version:

```text
NameMCSniper, version 3.0.0
```

## Run Easy Mode

```bash
python Main.py easy
```

Easy mode asks for the target username, whether to run a safe simulation or a real claim, and when the run should happen.

## Development Checks

For contributors:

```bash
python -m pip install -r requirements-dev.txt
python -m pytest
python -m ruff check .
```

## Installation Troubleshooting

**`python` is not recognized**

Install Python 3.10+ from python.org or your package manager, then reopen the terminal. On Windows, enable "Add Python to PATH" during installation or use the `py` launcher if available.

**Virtual environment uses the wrong Python**

Delete and recreate `.venv` with a known Python 3.10+ executable.

**Dependencies fail to install**

Upgrade pip first:

```bash
python -m pip install --upgrade pip
```

**Config validation asks for live-only values**

Safe dry runs do not require a token. Live claims require valid tokens and, for scheduled runs, an explicit timezone.
