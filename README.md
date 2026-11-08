
# To-Do List App

<div align="right">

[![Python](https://img.shields.io/badge/python-3.10%2B-blue)](https://www.python.org/)
[![CI](https://github.com/SagarBiswas-MultiHAT/SafeTodoManager/actions/workflows/python-ci.yml/badge.svg)](https://github.com/SagarBiswas-MultiHAT/SafeTodoManager/actions)
[![PyPI](https://img.shields.io/pypi/v/todo-app.svg)](https://pypi.org/project/todo-app)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
<!-- [![Coverage](https://codecov.io/gh/SagarBiswas-MultiHAT/SafeTodoManager/branch/main/graph/badge.svg)](https://codecov.io/gh/SagarBiswas-MultiHAT/SafeTodoManager) -->

</div>

A production-ready, offline-first To-Do List application built with Python and Tkinter. It ships with a clean UI, structured storage backends, optional encryption, robust logging, and full developer tooling.

## Features

- Tkinter UI with search/filter, keyboard shortcuts, import/export, and undo.
- Storage backends: filesystem (text), SQLite, and optional encrypted file storage.
- Atomic saves with backup rotation and rotating logs.
- Headless CLI for exports and automated backups.
- Tested, linted, typed, and ready for packaging.

## Requirements

- Python 3.11+ (3.10 supported where feasible)
- Tkinter (bundled with standard Python installers)

## Installation

Clone the repository and install dev tools when needed:

```bash
git clone https://github.com/SagarBiswas-MultiHAT/SafeTodoManager.git
cd SafeTodoManager
```

## Quick Start

Run the UI directly:

```bash
python -m todo_app
```

Or use the CLI entry point:

```bash
todo-app ui
```

## CLI Installation

Installing the package into your active environment registers the `todo-app` console script:

```bash
pip install -e .
```

If you prefer not to install the project you can continue to run the commands via `python -m todo_app` as shown above.

### Keyboard Shortcuts

- `Ctrl+N`: add task
- `Del`: remove selected task
- `Ctrl+F`: focus search field
- `Ctrl+I`: import JSON
- `Ctrl+E`: export JSON
- `Ctrl+Shift+I`: import CSV
- `Ctrl+Shift+E`: export CSV

## CLI Usage

```bash
todo-app --storage fs ui
todo-app --storage sqlite ui
todo-app --storage encrypted --password "your password" ui
```

Headless export:

```bash
todo-app export --format json --output backups/tasks.json
todo-app export --format csv --output backups/tasks.csv
```

Automated backup (JSON):

```bash
todo-app backup --output backups/tasks-$(date +%Y%m%d).json
```

## Storage Options

The data directory defaults to an OS-appropriate location.

- Windows: `%APPDATA%\todo-app`
- macOS/Linux: `$XDG_DATA_HOME/todo-app` or `~/.local/share/todo-app`

Override with:

```bash
todo-app --data-dir /path/to/data ui
```

## Encrypted Storage

Encrypted storage uses a password-derived key (PBKDF2-HMAC-SHA256 with a per-file salt).

Install encryption extras:

```bash
pip install .[encrypt]
```

Run with:

```bash
todo-app --storage encrypted --password "your password" ui
```

If you omit `--password` when launching the UI, a GUI prompt will ask for it.

### Private Tasks

Use the **Private Tasks** menu in the UI to open a separate encrypted task list.

- **Open Private Tasks**: prompts for the password and opens the private list.
- **New Private Task**: opens the private list and focuses the task entry.

If `cryptography` is missing, the app will prompt you to install it. The password is never stored.

## Import/Export

Use the File menu in the UI to import or export tasks to JSON or CSV. The CLI provides headless exports for automation.

## Logging

Rotating logs are stored in the user config directory:

- Windows: `%APPDATA%\todo-app\todo-app.log`
- macOS/Linux: `$XDG_CONFIG_HOME/todo-app/todo-app.log` or `~/.config/todo-app/todo-app.log`

## Development

Install dev dependencies:

```bash
pip install -r requirements-dev.txt
```

Run linting, type checks, and tests:

```bash
ruff check .
black --check .
mypy todo_app
pytest --maxfail=1 --disable-warnings -q --cov=todo_app
```

> When working from Windows, run the repo-installed linter so the options and version match CI.
>
> ```powershell
> .\.venv\Scripts\python.exe -m ruff check .
> ```
>
> Or activate the virtual environment first and run `ruff check .` directly.

Run CI locally with tox:

```bash
tox
```

## Packaging

Install the build tooling before running any packaging commands:

```bash
pip install build pyinstaller
```

Build a wheel:

```bash
python -m build
```

### PyInstaller (rebuild the executable)

```bash
pyinstaller --onefile --noconsole --name todo-app todo_app/__main__.py
```

### One-click Launch

- `run_todo_app.bat` (on Windows) calls `python -m todo_app` and lives at the repo root for double-click launches.
- Use `todo-app.spec` with PyInstaller to emit `dist/todo-app.exe` for a fully packaged experience:
	```bash
	pyinstaller todo-app.spec
	```
	The bundled executable appears in the `dist/` directory and can be copied alongside the shared config/storage directory. (.\dist\todo-app.exe)

## Troubleshooting

- **UI does not open**: ensure Tkinter is installed with your Python distribution.
- **Encrypted storage error**: install `cryptography` and confirm the password.
- **Permission errors**: choose a custom `--data-dir` with write access.

## Security

See [SECURITY.md](SECURITY.md) for the threat model and secure defaults.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).


## 本地运行说明（课程作业补充）

- 测试环境：Windows 11，Python 3.13.17，Tkinter 8.6（Python 官方安装包自带，安装时需勾选 "tcl/tk and IDLE"）。
- 运行依赖：见 `requirements.txt`。本项目 `pyproject.toml` 中 `dependencies = []`，运行时不依赖任何第三方库。
- 启动命令（在仓库根目录执行）：

```bash
python -m todo_app
```

- Windows 上若同时装有多个 Python，请用 `py -3 -m todo_app` 指定带 tkinter 的解释器；也可双击 `run_todo_app.bat`。
- 可选：无界面导出 `python -m todo_app export --format json --output backups/tasks.json`。


## 项目来源与本次改造

- 原仓库：<https://github.com/SagarBiswas-MultiHAT/SafeTodoManager>，MIT 许可，本仓库为其 Fork 后的课程作业改造版。
- 改动 1：应用名／主窗口标题由 `To-Do List App` 改为 `我的待办清单`（`todo_app/view_tk.py`）。
- 改动 2：主界面菜单、按钮、状态栏与提示文案中文化（同一个文件）。
- 改动均在 `feature/my-mod` 分支完成，通过 PR 合并回 `main`。

## 构建与打包

按仓库原有的 PyInstaller 方式，产物为同格式的 Windows 可执行文件：

```bash
python -m pip install pyinstaller
pyinstaller todo-app.spec
```

产物位于 `dist/todo-app.exe`，可直接双击运行。等价命令（原 README 已给出）：

```bash
pyinstaller --onefile --noconsole --name todo-app todo_app/__main__.py
```

`dist/`、`build/`、`.venv/`、`*.exe` 已写入 `.gitignore`，构建产物只上传 Releases，不提交进仓库。
