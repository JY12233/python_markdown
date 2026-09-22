# 创建 Logger 对象的格式

在 Python 的 `logging` 模块中，并没有一个可以直接通过 `logging.Logger()` 来实例化的传统“类”。`Logger` 对象始终通过模块级函数 `logging.getLogger(name)` 来获取。

```python
logger = logging.getLogger(
    name=None  # 记录器（Logger）的名称，建议使用模块名（如 __name__），支持以点号分隔的层级结构
)

# 设置该记录器的日志级别阈值（低于此级别的日志将被忽略）
logger.setLevel(
    logging.DEBUG  # 日志级别常量，如 logging.DEBUG, logging.INFO, logging.WARNING 等
)

# 创建处理器（Handler），决定日志输出的目标位置（如控制台或文件）
handler = logging.StreamHandler()  # 将日志输出到控制台（标准输出流）
# handler = logging.FileHandler('app.log')  # 将日志输出到指定文件

# 创建格式化器（Formatter），决定日志在最终输出时的布局样式
formatter = logging.Formatter(
    fmt='%(asctime)s - %(levelname)s - %(message)s',  # 日志消息的格式字符串
    datefmt='%Y-%m-%d %H:%M:%S'  # 时间字段 %(asctime)s 的具体显示格式
)

# 将格式化器绑定到处理器上
handler.setFormatter(formatter)

# 将处理器添加到记录器中
logger.addHandler(handler)
```

# 核心参数与组件详解

`Logger` 的创建与配置主要涉及 `getLogger` 的参数以及 `Handler` 和 `Formatter` 的核心参数。以下是它们的作用及可选值：

| 参数/组件名称              | 作用说明                 | 可选值 / 备注                                                |
| -------------------------- | ------------------------ | ------------------------------------------------------------ |
| `name` (`getLogger`)       | 指定记录器的名称         | 字符串；<br />支持层级结构（如 `my_app.web`）；<br />推荐使用 `__name__` |
| `level` (`setLevel`)       | 设置记录器的日志级别阈值 | `logging.DEBUG` (10), <br />`logging.INFO` (20), <br />`logging.WARNING` (30), <br />`logging.ERROR` (40), <br />`logging.CRITICAL` (50) |
| `fmt` (`Formatter`)        | 指定日志输出的格式模板   | 包含 `%(levelname)s`, `%(message)s`, `%(asctime)s` 等占位符的字符串 |
| `datefmt` (`Formatter`)    | 指定时间戳的显示样式     | 与 Python `datetime` 模块相同的格式化字符串，如 `%Y-%m-%d`   |
| `filename` (`FileHandler`) | 指定日志输出的文件路径   | 字符串；<br />若指定此项，日志将写入磁盘文件而非控制台       |
| `filemode` (`FileHandler`) | 指定日志文件的打开模式   | `'a'`（默认，追加写入）, <br />`'w'`（覆盖写入）             |
| `stream` (`StreamHandler`) | 指定日志输出的目标流     | `sys.stdout`（默认，标准输出）, <br />`sys.stderr`（标准错误输出） |

# Logger 对象的常用属性

`Logger` 对象提供了一些属性，用于控制日志的传播行为和获取当前的有效级别：

| 属性名称     | 作用说明                                                     |
| ------------ | ------------------------------------------------------------ |
| `.name`      | 获取该记录器的名称（即创建时传入的 `name` 参数）             |
| `.level`     | 获取该记录器显式设置的日志级别（若未设置则为 `NOTSET`）      |
| `.propagate` | 控制日志是否向上传播给祖先记录器（`True` 默认开启，`False` 则不传递） |
| `.handlers`  | 返回绑定到该记录器的所有处理器（Handler）对象的列表          |

# Logger 对象的常用方法

`Logger` 对象的方法主要用于记录不同严重程度的事件，以及动态管理处理器和级别：

| 方法名称               | 作用说明                                                     |
| ---------------------- | ------------------------------------------------------------ |
| `setLevel(lvl)`        | 动态设置该记录器的日志级别阈值（如 `logger.setLevel(logging.INFO)`） |
| `addHandler(hdlr)`     | 向该记录器添加一个处理器（Handler），决定日志的输出目标      |
| `removeHandler(hdlr)`  | 从该记录器中移除指定的处理器                                 |
| `debug(msg, *args)`    | 记录 `DEBUG` 级别的日志（通常用于详细的问题诊断）            |
| `info(msg, *args)`     | 记录 `INFO` 级别的日志（确认程序按预期正常运行）             |
| `warning(msg, *args)`  | 记录 `WARNING` 级别的日志（发生意外情况，但程序仍能正常工作） |
| `error(msg, *args)`    | 记录 `ERROR` 级别的日志（由于严重问题，导致某些功能无法执行） |
| `critical(msg, *args)` | 记录 `CRITICAL` 级别的日志（严重错误，可能导致程序无法继续运行） |
| `getEffectiveLevel()`  | 获取该记录器的有效日志级别（若自身未设置，则沿层级向上查找祖先的级别） |
| `isEnabledFor(lvl)`    | 判断指定级别的日志是否会被当前记录器处理（返回布尔值）       |

# 完整代码示例

```python
import logging
import logging.handlers
import os
import sys

def setup_logger(log_dir="logs", log_file_name="app.log"):
    """
    配置一个同时输出到控制台和文件的日志记录器
    """
    # 1. 确保日志目录存在
    if not os.path.exists(log_dir):
        os.makedirs(log_dir)
    
    # 2. 创建 Logger 对象
    logger = logging.getLogger("MyApp")
    # 设置最低级别为 DEBUG，确保所有级别的日志都能被捕获，后续由 Handler 过滤
    logger.setLevel(logging.DEBUG)

    # 3. 定义日志格式
    formatter = logging.Formatter(
        fmt='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
        datefmt='%Y-%m-%d %H:%M:%S'
    )

    # 4. 创建控制台处理器 (StreamHandler)
    # 控制台通常只打印 INFO 及以上级别的重要信息，避免刷屏
    console_handler = logging.StreamHandler(sys.stdout)
    console_handler.setLevel(logging.INFO)
    console_handler.setFormatter(formatter)
    logger.addHandler(console_handler)

    # 5. 创建文件处理器 (TimedRotatingFileHandler)
    # 将 DEBUG 及以上级别的所有日志写入文件，并按天切割，最多保留 7 天
    log_path = os.path.join(log_dir, log_file_name)
    file_handler = logging.handlers.TimedRotatingFileHandler(
        filename=log_path, 
        when='D',       # 按天切割 (D=天, H=小时, M=分钟)
        interval=1,     # 每 1 天切割一次
        backupCount=7,  # 最多保留 7 个历史日志文件
        encoding='utf-8'
    )
    file_handler.setLevel(logging.DEBUG)
    file_handler.setFormatter(formatter)
    logger.addHandler(file_handler)

    return logger

# 获取配置好的 logger 实例
logger = setup_logger()

# 6. 测试不同级别的日志输出
if __name__ == "__main__":
    logger.debug("这是一条调试信息，只会被写入文件，不会在控制台显示。")
    logger.info("程序正常运行，控制台和文件都会记录这条信息。")
    logger.warning("注意！发生了一些非预期的状况。")
    logger.error("出错了！某个功能执行失败。")
    logger.critical("严重错误！程序可能即将崩溃。")
```

## 代码解析

1. **双重输出与级别过滤** 
   `Logger` 本身的级别设为了 `DEBUG`（最低），但控制台 `StreamHandler` 设为了 `INFO`。这意味着：所有的日志都会完整地保存在文件里（方便事后排查问题），但控制台只会显示重要的 `INFO`、`WARNING` 等信息，保持界面清爽。

2. **日志文件自动切割** 
   使用了 `logging.handlers.TimedRotatingFileHandler` 而不是普通的 `FileHandler`。普通文件处理器会让日志无限增长，最终占满磁盘；而 `TimedRotatingFileHandler` 可以按天（或按小时）自动切割日志，并配合 `backupCount` 自动删除过期的旧日志，完全不需要人工维护。

3. **防止重复添加处理器** 
   在实际的大型项目中，`setup_logger()` 可能会被多次调用。为了避免每次调用都重复添加 `Handler` 导致一条日志被打印多次，你可以在创建 `Logger` 后加一行判断：

   ```python
   if not logger.handlers:
       logger.addHandler(console_handler)
       logger.addHandler(file_handler)
   ```

## 对应的 git 忽略文件

```text
# ------------------- 系统与编辑器文件 -------------------
.DS_Store          # Mac 系统隐藏文件
Thumbs.db          # Windows 系统缩略图文件
.vscode/           # VS Code 编辑器配置
.idea/             # PyCharm / IntelliJ IDEA 配置
*.swp              # Vim 编辑器临时文件

# ------------------- Python 专属文件 -------------------
__pycache__/       # Python 编译缓存文件夹
*.py[cod]          # Python 编译生成的字节码文件
*$py.class         # Jython 编译文件

# ------------------- 虚拟环境 -------------------
venv/              # 常见的 Python 虚拟环境目录
env/               # 另一种常见的虚拟环境目录
.venv/             # 现代 Python 工具（如 uv）常用的虚拟环境目录

# ------------------- 日志与数据文件 -------------------
logs/              # 存放应用日志的文件夹（你刚刚创建的）
*.log              # 所有后缀为 .log 的日志文件
data/              # 存放原始数据的文件夹（按需添加）

# ------------------- 环境变量与敏感信息 -------------------
.env               # 存放敏感配置（如数据库密码、API密钥）的环境变量文件
.env.local
```

