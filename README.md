# NameMCSniper Documentation

<img width="1536" height="324" alt="NameMCSniper documentation banner" src="https://github.com/user-attachments/assets/36f6664e-9671-4a6c-a5b0-8b4e03973d2d" />

Documentation source for [NameMCSniper](https://github.com/zwroee/NameMCSniper), built with MkDocs and Material for MkDocs.

The docs currently target **NameMCSniper v3.0.0**.

## Local Preview

```bash
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m mkdocs serve
```

Linux/macOS:

```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python -m mkdocs serve
```

Open:

```text
http://127.0.0.1:8000
```

## Build

```bash
python -m mkdocs build
```

Strict build:

```bash
python -m mkdocs build --strict
```

## Source Layout

```text
docs/
  index.md
  getting-started/
    installation.md
    configuration.md
    authentication.md
  usage/
    basic-usage.md
    commands.md
    discord-setup.md
    proxy-setup.md
  advanced/
    how-it-works.md
    performance.md
    troubleshooting.md
  legal/
    legal-notice.md
    support.md
mkdocs.yml
requirements.txt
```

## Updating Content

1. Update Markdown files under `docs/`.
2. Keep command references aligned with `NameMCSniper/Main.py`.
3. Keep config examples aligned with `NameMCSniper/config.yaml.example` and `src/config/config.py`.
4. Run `mkdocs build --strict`.
5. Commit the source changes.

## Deployment

The GitHub Actions workflow builds the site with MkDocs and deploys the generated `site/` artifact to GitHub Pages.

## Notes

Generated static output may exist in this repository from older builds. The canonical docs source is the Markdown under `docs/` plus `mkdocs.yml`.
