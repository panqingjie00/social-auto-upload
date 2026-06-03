## Project Overview

This project, `social-auto-upload`, is a powerful automation tool designed to help content creators and operators efficiently publish video content to multiple domestic and international mainstream social media platforms in one click. The project implements video upload, scheduled release and other functions for platforms such as `Douyin`, `Bilibili`, `Xiaohongshu`, `Kuaishou`, `WeChat Channel`, `Baijiahao` and `TikTok`.

The project consists of a Python backend and a Vue.js frontend.

**Backend:**

*   Framework: Flask
*   Core Functionality:
    *   Handles file uploads and management.
    *   Interacts with a SQLite database to store information about files and user accounts.
    *   Uses `patchright` for browser automation to interact with social media platforms.
    *   Provides a RESTful API for the frontend to consume.
    *   Uses Server-Sent Events (SSE) for real-time communication with the frontend during the login process.

**Frontend:**

*   Framework: Vue.js
*   Build Tool: Vite
*   UI Library: Element Plus
*   State Management: Pinia
*   Routing: Vue Router
*   Core Functionality:
    *   Provides a web interface for managing social media accounts, video files, and publishing videos.
    *   Communicates with the backend via a RESTful API.

**Command-line Interface:**

The project provides a unified CLI entry point `sau`, currently supporting four main platforms:

*   `douyin`   — login / check / upload-video / upload-note
*   `kuaishou` — login / check / upload-video / upload-note
*   `xiaohongshu` — login / check / upload-video / upload-note
*   `bilibili` — login / check / upload-video

The CLI is implemented in `sau_cli.py`. Prefer `sau <platform> ...` over legacy `examples/` scripts or the old web path.

## Building and Running

### Prerequisites

*   Python >=3.10, <3.13
*   `uv` (recommended) — `pip install uv`

### Install

```bash
uv venv
source .venv/bin/activate   # or .venv\Scripts\activate on Windows
uv pip install -e .
```

### Install Chromium

```bash
# China mirror (faster):
$env:PLAYWRIGHT_DOWNLOAD_HOST="https://npmmirror.com/mirrors/playwright"; patchright install chromium  # Windows PowerShell
PLAYWRIGHT_DOWNLOAD_HOST="https://npmmirror.com/mirrors/playwright" patchright install chromium       # Linux/macOS

# Default source:
patchright install chromium
```

### Configuration

```bash
cp conf.example.py conf.py   # then edit conf.py as needed
```

Key config items: `LOCAL_CHROME_PATH`, `LOCAL_CHROME_HEADLESS`, `DEBUG_MODE`.

### Verify

```bash
sau --help
sau douyin --help
sau kuaishou --help
sau xiaohongshu --help
sau bilibili --help
```

Further details: `docs/install.md`, `docs/CLI.md`.

## CLI Usage

### Common flags

```
--debug              Enable debug mode
--headed / --headless Control browser visibility (default: headless)
```

### Douyin

```bash
sau douyin login --account <name>
sau douyin check --account <name>

# Video upload
sau douyin upload-video --account <name> --file <video> --title "标题" \
    [--desc "描述"] [--tags tag1,tag2] [--schedule "YYYY-MM-DD HH:MM"] \
    [--thumbnail <image>] [--product-link <url>] [--product-title "商品名"]

# Image note (图文) upload
sau douyin upload-note --account <name> --images 1.png 2.png --title "标题" \
    [--note "正文"] [--tags tag1,tag2] [--schedule "YYYY-MM-DD HH:MM"] \
    [--bgm "晴天"]
```

`--bgm`: optional BGM track name. When omitted, the first recommended track is picked randomly. When provided, searches the music panel and selects the first result.

### Kuaishou

```bash
sau kuaishou login --account <name>
sau kuaishou check --account <name>
sau kuaishou upload-video --account <name> --file <video> --title "标题" \
    [--desc "描述"] [--tags tag1,tag2] [--schedule "YYYY-MM-DD HH:MM"]
sau kuaishou upload-note --account <name> --images 1.png 2.png --title "标题" \
    [--note "正文"] [--tags tag1,tag2] [--schedule "YYYY-MM-DD HH:MM"]
```

### Xiaohongshu

```bash
sau xiaohongshu login --account <name>
sau xiaohongshu check --account <name>
sau xiaohongshu upload-video --account <name> --file <video> --title "标题" \
    [--desc "描述"] [--tags tag1,tag2] [--schedule "YYYY-MM-DD HH:MM"]
sau xiaohongshu upload-note --account <name> --images 1.png 2.png 3.png --title "标题" \
    [--note "正文"] [--tags tag1,tag2] [--schedule "YYYY-MM-DD HH:MM"]
```

### Bilibili

```bash
sau bilibili login --account <name>
sau bilibili check --account <name>
sau bilibili upload-video --account <name> --file <video> --title "标题" \
    --desc "描述" --tid <category_id> \
    [--tags tag1,tag2] [--schedule "YYYY-MM-DD HH:MM"]
```

Bilibili login requires a local interactive terminal. If the terminal QR code renders poorly, open `./qrcode.png` to scan. The `biliup` binary is auto-downloaded on first run.

### Login QR codes

Douyin / Kuaishou / Xiaohongshu logins may generate temporary QR code images. Display these images directly to the user for scanning — do not just output the file path.

## Project Memory (mandatory)

This project has persistent memory files at `C:\Users\86178\.claude\projects\E--social-auto-upload\memory\`. The index (`MEMORY.md`) is auto-loaded into context, but individual memory files are NOT.

**Before executing any task**, scan the MEMORY.md index for relevant entries. If ANY entry matches the task at hand, you MUST `Read` the full memory file before acting. Do NOT rely on the one-line summary alone — it is a pointer, not the full rule.

## Development Conventions

*   Backend code is in the root directory, `myUtils/`, and `uploader/`.
*   Frontend code is in `sau_frontend/`.
*   SQLite database: `db/database.db`.
*   Config: copy `conf.example.py` → `conf.py`.
*   Use `uv` for dependency management; `pyproject.toml` is the source of truth. `requirements.txt` is historical only.
*   `sau_cli.py` is the CLI entry point; `uploader/` contains per-platform implementations.
*   The old Web backend (`sau_backend.py`) and `examples/` directory are legacy and may not be current.
*   Platform skill docs are in `skills/<platform>-upload/`.
