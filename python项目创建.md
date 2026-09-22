# Python 项目初始化命令行工具指南

本文档整理了当前主流的 Python 项目脚手架与包管理工具的 CLI 用法，适用于在 VS Code 等编辑器外快速创建标准化项目结构。

## uv（推荐）

高性能 Rust 实现的 Python 包管理器与项目管理器，兼容 pip/poetry 生态。

### 安装

```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# 或通过 pip/pipx
pip install uv
```

### 创建项目

```bash
# 创建应用项目 (含 pyproject.toml, README.md, .gitignore)
uv init my-app

# 创建库项目 (src layout)
uv init --lib my-lib

# 指定 Python 版本
uv init --python 3.12 my-app

# 使用特定模板
uv init --template fastapi my-api
```

### 生成目录结构

```text
my-app/
├── pyproject.toml
├── README.md
├── .gitignore
└── src/
    └── my_app/
        └── __init__.py
```

------

## Poetry

成熟的依赖管理与打包工具，社区生态完善。

### 安装

```bash
curl -sSL https://install.python-poetry.org | python3 -
```

### 创建项目

```bash
# 标准 src layout
poetry new my-package

# 扁平 layout (无 src 目录)
poetry new --flat my-package

# 在已有目录中初始化
cd existing-project && poetry init
```

### 生成目录结构

```text
my-package/
├── pyproject.toml
├── README.md
├── src/
│   └── my_package/
│       └── __init__.py
└── tests/
    └── __init__.py
```

------

## Cookiecutter

基于 Jinja2 模板引擎的项目生成器，支持自定义或社区模板。

### 安装

```bash
pip install cookiecutter
# 或
pipx install cookiecutter
```

### 使用模板

```bash
# 交互式选择模板参数
cookiecutter https://github.com/cookiecutter/cookiecutter-django

# 本地模板
cookiecutter /path/to/local-template

# 指定分支
cookiecutter https://github.com/user/template --checkout develop

# 非交互模式 (传入参数文件)
cookiecutter template-url --no-input --config-file params.yaml
```

### 常用社区模板

| 模板                      | 用途            | 仓库地址                                        |
| ------------------------- | --------------- | ----------------------------------------------- |
| cookiecutter-django       | Django 全栈项目 | github.com/cookiecutter/cookiecutter-django     |
| cookiecutter-fastapi      | FastAPI 微服务  | github.com/tiangolo/full-stack-fastapi-template |
| cookiecutter-data-science | 数据科学项目    | github.com/drivendata/cookiecutter-data-science |
| cookiecutter-pypackage    | Python 库发布   | github.com/audreyfeldroy/cookiecutter-pypackage |

------

## 框架专用脚手架

部分框架提供官方 CLI 工具：

```bash
# FastAPI (需额外安装)
pip install fastapi-cli
fastapi new my-api

# Django
pip install django
django-admin startproject mysite

# Flask
pip install flask
flask --app myapp init  # Flask 2.3+

# Streamlit
streamlit hello  # 生成示例应用
```

------

## 工具选型对比

| 特性         | uv          | Poetry          | Cookiecutter | 框架 CLI |
| ------------ | ----------- | --------------- | ------------ | -------- |
| 执行速度     | 极快 (Rust) | 较慢            | 中等         | 快       |
| 依赖解析     | 兼容 pip    | 自有解析器      | 不涉及       | 不涉及   |
| 虚拟环境管理 | 内置        | 内置            | 不涉及       | 不涉及   |
| 模板灵活性   | 内置少量    | 无              | 极高         | 固定     |
| 适用场景     | 通用项目/库 | 库发布/团队规范 | 复杂定制结构 | 特定框架 |
| 学习成本     | 低          | 中              | 高           | 低       |

## VS Code 集成注意事项

- 使用上述任一工具创建项目后，通过 `File > Open Folder` 打开。
- Pylance 会自动检测 `pyproject.toml` / `setup.cfg` 并配置解释器路径。
- 若未自动识别，按 `Ctrl+Shift+P` 执行 `Python: Select Interpreter` 手动指定虚拟环境中的 Python。
- 建议在 `.vscode/settings.json` 中显式声明 Python 路径以避免歧义：

```json
{
  "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python"
}
```

# uv 使用指南

uv 是由 Astral 团队（Ruff 开发者）构建的极速 Python 包管理器与项目管理器，采用 Rust 编写。它整合了 pip、pip-tools、virtualenv、poetry 和 pyenv 的功能，旨在提供统一的 Python 开发工作流。

## 核心特性

- **极致性能**：依赖解析与安装速度比 pip 快 10-100 倍，比 Poetry 快 50-100 倍。
- **一体化设计**：单二进制文件涵盖包管理、虚拟环境、Python 版本管理、项目脚手架、脚本运行等全部功能。
- **标准兼容**：原生支持 PEP 621 (`pyproject.toml`)，兼容 pip 接口与 requirements.txt 格式。
- **跨平台**：提供 Windows、macOS、Linux 预编译二进制，无需 Python 运行时即可安装。
- **磁盘高效**：全局缓存 + 硬链接机制，多项目共享依赖时几乎不占用额外空间。

## 安装

```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# 通过 pip/pipx（需要已有 Python 环境）
pip install uv
```

安装后执行 `uv self update` 可随时升级到最新版本。

## 项目管理

### 初始化项目

```bash
# 创建应用项目（扁平布局）
uv init my-app # 文件路径
uv init my-app --name 英文别名 # uv 不能识别中文名称，因此需要英文别名

# 创建库项目（src 布局）
uv init --lib my-lib

# 指定 Python 版本
uv init --python 3.12 my-app

# 在中文目录下创建（显式指定合法包名）
uv init . --name machine_learning

# 使用内置模板
uv init --template fastapi my-api
```

### 依赖管理

```bash
# 添加依赖（自动写入 pyproject.toml 并锁定），自动下载对应的包
uv add requests pandas>=2.0 # 同时依赖 requests 和 pandas，注意符号与单词之间没有空格，可以使用""

# 不自动下载，仅写入
uv add requests --frozen

# 添加开发依赖
uv add --dev pytest ruff mypy

# 添加可选依赖组
uv add --optional gpu torch torchvision

# 移除依赖
uv remove requests

# 从 requirements.txt 批量导入
uv pip install -r requirements.txt
```

运行 `uv add/remove` 时，如果当前目录下没有虚拟环境，uv 会自动在当前项目根目录创建一个名为 `.venv` 的虚拟环境，并将依赖安装进去。如果不要创建，一定要加 `--frozen` 参数。

### 同步与锁定

```bash
# 根据 lockfile 精确安装所有依赖
uv sync

# 仅更新 lockfile 而不安装
uv lock

# 升级特定依赖并更新 lockfile
uv lock --upgrade-package requests
```

`uv.lock` 是跨平台确定性锁文件，应提交至版本控制以确保团队协作与环境部署的一致性。

> **TOML 文件结构**
>
> ```toml
> [project]
> name = "machine-learning"        # 包名（发布到 PyPI 时的标识）
> version = "0.1.0"                # 语义化版本号
> description = ""                 # 项目描述
> requires-python = ">=3.12"       # Python 版本约束
> dependencies = []                # 运行时依赖列表
> 
> [build-system]
> requires = ["hatchling"]         # 构建后端（等价于 Maven 的打包插件）
> build-backend = "hatchling.build"
> ```

## 虚拟环境与 Python 版本管理

### 虚拟环境

```bash
# 创建虚拟环境（默认 .venv）
uv venv

# 指定 Python 版本创建
uv venv --python 3.11

# 激活（与标准 venv 相同）
source .venv/bin/activate      		# macOS/Linux
source .venv\Scripts\activate       # Windows

# 退出
deactivate # 就这个就够了
```

uv 创建的虚拟环境与标准 `venv` 完全兼容，VS Code Pylance 可自动识别。

### Python 版本管理

```bash
# 列出可安装的 Python 版本
uv python list

# 安装指定版本（自动下载官方构建）
uv python install 3.12 3.11 pypy3.10

# 为当前项目固定 Python 版本（写入 .python-version）
uv python pin 3.12

# 查看当前项目使用的 Python
uv python find
```

uv 管理的 Python 版本独立于系统 Python，不会影响操作系统环境。

## 工具运行与脚本执行

### 命令行工具隔离运行

```bash
# 在临时环境中运行工具（不污染项目依赖）
uvx ruff check .
uvx black --line-length 100 src/
uvx pytest --cov

# 等价于 uv tool run
uv tool run ruff check .
```

`uvx` 会自动缓存工具环境，后续调用零延迟。

### 内联脚本元数据

支持在单文件中声明依赖，实现自包含可执行脚本：

```python
# /// script
# requires-python = ">=3.12"
# dependencies = ["requests", "rich"]
# ///

import requests
from rich import print
print(requests.get("https://httpbin.org/ip").json())
# 自动创建临时环境、安装依赖并运行
uv run script.py
```

## 与传统工具命令对照

| 传统命令                 | uv 等价命令                           | 说明              |
| ------------------------ | ------------------------------------- | ----------------- |
| `pip install pkg`        | `uv add pkg`                          | 项目内添加依赖    |
| `pip install -r req.txt` | `uv pip install -r req.txt`           | 兼容模式批量安装  |
| `pip freeze > req.txt`   | `uv export --format requirements-txt` | 导出锁定依赖      |
| `python -m venv .venv`   | `uv venv`                             | 创建虚拟环境      |
| `poetry install`         | `uv sync`                             | 按 lockfile 安装  |
| `poetry add --dev pkg`   | `uv add --dev pkg`                    | 添加开发依赖      |
| `pyenv install 3.12`     | `uv python install 3.12`              | 安装 Python 版本  |
| `npx ruff`               | `uvx ruff`                            | 隔离运行 CLI 工具 |

## VS Code 集成

uv 生成的项目开箱即用，无需额外插件配置：

1. 使用 `File > Open Folder` 打开 uv 项目。
2. Pylance 自动检测 `.venv` 与 `pyproject.toml`，完成解释器绑定与路径解析。
3. 若未自动识别，执行 `Python: Select Interpreter` 手动选择 `.venv` 中的 Python。

建议在 `.vscode/settings.json` 中显式声明以避免多环境歧义：

```json
{
  "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python"
}
```

## 注意事项

- `uv add/remove/sync/lock` 仅在包含 `pyproject.toml` 的项目目录中可用；非项目环境请使用 `uv pip` 子命令。
- `uv.lock` 是二进制格式，不可手动编辑，所有变更必须通过 uv 命令完成。
- uv 默认使用全局缓存目录（`~/.cache/uv`），可通过 `UV_CACHE_DIR` 环境变量自定义。
- 迁移现有 Poetry 项目时，直接删除 `poetry.lock` 后执行 `uv lock && uv sync` 即可，`pyproject.toml` 无需修改。

# uv 命令行参考手册

uv 采用统一的子命令架构，所有功能均通过 `uv <subcommand>` 调用。以下按使用场景分类整理全部核心命令及其关键参数。

## 项目管理

项目生命周期管理命令，仅在包含 `pyproject.toml` 的项目目录中可用。

| 命令         | 作用                             | 关键参数                                                     |
| ------------ | -------------------------------- | ------------------------------------------------------------ |
| `uv init`    | 初始化新项目                     | `--name` 指定包名；<br />`--lib` 创建 src 布局库项目；<br />`--python` 指定 Python 版本；<br />`--template` 使用内置模板 |
| `uv add`     | 添加依赖并更新 lockfile          | `--dev` 开发依赖；<br />`--optional <group>` 可选依赖组；<br />`--editable` 可编辑安装；<br />`--constraint` 约束文件<br />`--frozen` 静态添加 |
| `uv remove`  | 移除依赖并更新 lockfile          | 支持同时移除多个包；自动清理 lockfile 中的传递依赖           |
| `uv sync`    | 按 lockfile 精确安装依赖到 .venv | `--frozen` 禁止更新 lockfile；<br />`--no-dev` 排除开发依赖；<br />`--extra <group>` 包含可选依赖组 |
| `uv lock`    | 仅解析并生成/更新 lockfile       | `--upgrade-package <pkg>` 升级指定包；<br />`--check` CI 中验证 lockfile 是否过期 |
| `uv export`  | 导出依赖为其他格式               | `--format requirements-txt` 导出 pip 格式；<br />`--no-hashes` 省略哈希值 |
| `uv build`   | 构建 sdist/wheel 分发包          | `--out-dir` 指定输出目录；默认输出到 `dist/`                 |
| `uv publish` | 发布包到 PyPI                    | `--token` API Token；<br />`--index-url` 自定义仓库地址      |

## 虚拟环境与 Python 版本

独立于项目的运行时管理能力。

| 命令                  | 作用                         | 关键参数                                                     |
| --------------------- | ---------------------------- | ------------------------------------------------------------ |
| `uv venv`             | 创建虚拟环境                 | `--python` 指定版本；<br />`--seed` 预装 pip/setuptools；<br />`--allow-existing` 覆盖已有环境 |
| `uv python list`      | 列出可安装的 Python 版本     | `--all-versions` 显示全部小版本；<br />`--only-installed` 仅显示已安装版本 |
| `uv python install`   | 下载并安装 Python 版本       | 支持 CPython / PyPy；多版本并行安装；自动校验签名            |
| `uv python pin`       | 固定项目 Python 版本         | 写入 `.python-version` 文件；<br />`--global` 设置全局默认版本 |
| `uv python find`      | 显示当前项目使用的解释器路径 | 遵循 `.python-version` <br />→ `pyproject.toml` <br />→ PATH 的查找优先级 |
| `uv python uninstall` | 卸载 uv 管理的 Python 版本   | 不影响系统 Python                                            |

## 工具运行

在隔离临时环境中执行 CLI 工具，不污染项目依赖。

| 命令                  | 作用                 | 关键参数                                       |
| --------------------- | -------------------- | ---------------------------------------------- |
| `uvx` / `uv tool run` | 一次性运行工具       | 自动缓存环境；支持内联依赖声明 `--with <pkg>`  |
| `uv tool install`     | 全局安装 CLI 工具    | 安装到 `~/.local/bin/`；`--force` 覆盖已有安装 |
| `uv tool list`        | 列出已全局安装的工具 | `--show-paths` 显示安装路径                    |
| `uv tool upgrade`     | 升级已安装的工具     | `--all` 升级全部工具                           |
| `uv tool uninstall`   | 卸载全局工具         | 清理关联的缓存环境                             |

## 脚本执行

支持带内联元数据的自包含 Python 脚本。

| 命令            | 作用             | 关键参数                                                     |
| --------------- | ---------------- | ------------------------------------------------------------ |
| `uv run`        | 运行脚本或模块   | 自动解析 PEP 723 内联依赖；`--script` 强制以脚本模式运行；`--module` 以 `-m` 方式运行 |
| `uv run --with` | 临时附加依赖运行 | 不修改项目 lockfile；适合一次性调试                          |

> **PEP 723 内联元数据示例：**
>
> ```python
> # /// script
> # requires-python = ">=3.12"
> # dependencies = ["requests", "rich"]
> # ///
> ```
>
> `uv run` 会自动识别此块并创建临时环境执行。

## pip 兼容接口

为非项目场景提供与 pip 完全兼容的命令，适用于 Dockerfile、CI 等传统工作流。

| 命令               | 等价 pip 命令   | 说明                                           |
| ------------------ | --------------- | ---------------------------------------------- |
| `uv pip install`   | `pip install`   | 支持 `-r`、`-e`、`--index-url` 等全部 pip 参数 |
| `uv pip uninstall` | `pip uninstall` | 批量卸载                                       |
| `uv pip freeze`    | `pip freeze`    | 输出已安装包列表                               |
| `uv pip list`      | `pip list`      | 格式化列表                                     |
| `uv pip show`      | `pip show`      | 查看包详情                                     |
| `uv pip compile`   | `pip-compile`   | 从 requirements.in 生成锁定文件                |
| `uv pip sync`      | `pip-sync`      | 使环境精确匹配 requirements.txt                |

`uv pip` 系列命令**不读取** `pyproject.toml` 和 `uv.lock`，仅操作目标虚拟环境。在项目目录中应优先使用 `uv add/sync`。

## 全局选项

所有子命令均支持的通用参数：

| 选项                          | 作用                      |
| ----------------------------- | ------------------------- |
| `--verbose` / `-v`            | 详细输出（可叠加 `-vvv`） |
| `--quiet` / `-q`              | 静默模式                  |
| `--color <auto|always|never>` | 控制彩色输出              |
| `--native-tls`                | 使用系统 TLS 而非 rustls  |
| `--cache-dir <path>`          | 自定义缓存目录            |
| `--no-cache`                  | 禁用缓存                  |
| `--config-file <path>`        | 指定配置文件路径          |
| `--help` / `-h`               | 显示帮助信息              |

## 配置与环境变量

命令行参数可通过配置文件或环境变量设置默认值，优先级：**CLI 参数 > 环境变量 > 配置文件**。

### 配置文件

`uv.toml` 或 `pyproject.toml` 中的 `[tool.uv]` 段：

```toml
# uv.toml
[pip]
index-url = "https://mirrors.aliyun.com/pypi/simple/"

[python]
preferred-version = "3.12"
```

### 常用环境变量

| 变量                       | 等价配置        | 典型用途                             |
| -------------------------- | --------------- | ------------------------------------ |
| `UV_CACHE_DIR`             | `--cache-dir`   | CI 中指定缓存挂载点                  |
| `UV_INDEX_URL`             | `pip.index-url` | 镜像源                               |
| `UV_PYTHON_INSTALL_MIRROR` | -               | 国内 Python 下载镜像                 |
| `UV_LINK_MODE`             | `link-mode`     | 设为 `copy` 避免跨文件系统硬链接问题 |
| `UV_NO_PROGRESS`           | -               | CI 中禁用进度条                      |
| `VIRTUAL_ENV`              | -               | 指定目标虚拟环境路径                 |

## 自我维护

| 命令                       | 作用                   |
| -------------------------- | ---------------------- |
| `uv self update`           | 升级到最新版本         |
| `uv self update --preview` | 升级到预览版           |
| `uv cache clean`           | 清理全部缓存           |
| `uv cache prune`           | 仅清理未引用的缓存条目 |
| `uv cache dir`             | 显示缓存目录路径       |