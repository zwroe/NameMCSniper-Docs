# NameMCSniper Documentation

This site documents the Rust edition of NameMCSniper. The app uses Cargo. The documentation site uses Python for MkDocs and Material for MkDocs. [Application repository](https://github.com/zwroe/NameMCSniper)

## Preview the docs

On Windows, from this folder, run the following in PowerShell.

```powershell
py -3 -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m mkdocs serve
```

On Linux or macOS, run these commands.

```sh
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python -m mkdocs serve
```

Open [the local preview](http://127.0.0.1:8000/).

## Update and build

Edit the Markdown under `docs/` and navigation in `mkdocs.yml`. Check commands and menu options against `src/main.rs` and `src/menu.rs` in the Rust application checkout.

With the MkDocs environment active, build the site.

```sh
python -m mkdocs build --strict
```

MkDocs writes generated output to `site/`. This snapshot also includes a rendered copy at the project root. If you refresh that copy, build into a separate directory and copy the generated files into the root. Keep Markdown and configuration as the source of truth.

The GitHub Pages workflow builds and deploys `site/` from the documentation repository. [Documentation repository](https://github.com/zwroe/NameMCSniper-Docs)
