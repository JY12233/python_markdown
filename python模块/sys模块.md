# 介绍

`sys` 是 Python 标准库中用于与 Python 解释器进行交互的核心模块。使用时只需通过 `import sys` 导入即可，当你需要获取命令行参数、访问或修改解释器内部状态、处理标准输入输出流或控制程序退出时，就该使用它。

`sys` 模块的主要作用是提供了一系列与 Python 解释器紧密相关的变量和函数。它的核心功能包括：获取命令行参数列表、访问标准输入输出及错误流、获取当前解释器的版本与平台信息、修改模块搜索路径、动态修改引用计数以及强制退出程序等。

# 核心类

`sys` 模块本身不包含子模块，但它定义或关联了多个重要的类和异常：

- `sys.flags`

  一个命名元组，包含传递给解释器的命令行标志（如 `-O` 优化模式等）。

- `sys.version_info`

  一个命名元组，包含当前 Python 解释器的版本信息（主版本号、次版本号等）。

- `sys.implementation`

  一个命名元组，包含当前 Python 实现（如 CPython、PyPy）的详细信息。

- `sys.float_info`

  一个命名元组，包含当前平台浮点数的底层实现细节（如最大最小值、精度等）。

- `sys.meta_path`

  一个列表，包含当前所有已安装的元路径查找器（Meta Path Finders），用于自定义模块导入机制。

- `sys.path_hooks`

  一个列表，包含用于创建路径导入器（Path Importers）的钩子函数。

- 异常类

  `sys` 模块关联了多个内置异常，如 `SystemExit`（由 `sys.exit()` 触发）、`KeyboardInterrupt`（用户中断）等。

# 常用属性

| 属性名称                  | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| `sys.argv`                | 一个列表，包含传递给 Python 脚本的命令行参数（第一个元素是脚本名称） |
| `sys.path`                | 一个列表，包含模块搜索路径，程序启动时由环境变量 `PYTHONPATH` 等初始化 |
| `sys.modules`             | 一个字典，映射已加载的模块名称到模块对象，用于缓存已导入的模块 |
| `sys.version`             | 一个字符串，包含当前 Python 解释器的版本号及编译信息         |
| `sys.version_info`        | 一个命名元组，包含当前 Python 解释器的版本信息（如 `(3, 11, 5, 'final', 0)`） |
| `sys.platform`            | 一个字符串，标识当前运行平台（如 `'win32'`、`'linux'`、`'darwin'`） |
| `sys.executable`          | 一个字符串，包含当前 Python 解释器的绝对路径                 |
| `sys.stdin`               | 标准输入流对象，通常用于接收用户键盘输入                     |
| `sys.stdout`              | 标准输出流对象，通常用于 `print()` 函数的输出目标            |
| `sys.stderr`              | 标准错误流对象，通常用于输出错误信息和警告                   |
| `sys.maxsize`             | 一个整数，表示 Python 原生整数类型支持的最大值（与平台字长相关） |
| `sys.byteorder`           | 一个字符串，表示当前平台的字节序（`'little'` 或 `'big'`）    |
| `sys.flags`               | 一个命名元组，包含传递给解释器的命令行标志状态               |
| `sys.float_info`          | 一个命名元组，包含当前平台浮点数的底层实现细节               |
| `sys.int_info`            | 一个命名元组，包含当前平台整数的底层实现细节（如内部位数）   |
| `sys.implementation`      | 一个命名元组，包含当前 Python 实现的详细信息                 |
| `sys.meta_path`           | 一个列表，包含当前所有已安装的元路径查找器                   |
| `sys.path_hooks`          | 一个列表，包含用于创建路径导入器的钩子函数                   |
| `sys.dont_write_bytecode` | 一个布尔值，若为 `True` 则禁止生成 `.pyc` 字节码文件         |
| `sys.warnoptions`         | 一个列表，包含当前生效的警告过滤器选项                       |

# 常用方法

| 方法名称                                 | 说明                                                         |
| ---------------------------------------- | ------------------------------------------------------------ |
| `sys.exit([arg])`                        | 退出当前程序。可选参数 `arg` 可以是退出状态码（整数）或错误信息（字符串），默认返回 `0` |
| `sys.getrefcount(object)`                | 返回指定对象的引用计数（注意：调用此函数本身会增加一次临时引用） |
| `sys.getsizeof(object[, default])`       | 返回指定对象在内存中占用的字节数。若对象类型不支持，则返回 `default` |
| `sys.setrecursionlimit(limit)`           | 设置 Python 解释器的最大递归深度限制，防止栈溢出             |
| `sys.getrecursionlimit()`                | 获取当前设置的最大递归深度限制                               |
| `sys.settrace(tracefunc)`                | 设置全局追踪函数，用于调试器或代码覆盖率工具                 |
| `sys.setprofile(profilefunc)`            | 设置全局性能分析函数，用于性能分析工具                       |
| `sys.intern(string)`                     | 将字符串放入“内部字符串池”并返回，用于加速字符串比较和节省内存 |
| `sys.exc_info()`                         | 返回当前正在处理的异常信息，格式为 `(type, value, traceback)` |
| `sys.displayhook(value)`                 | 控制交互式解释器中表达式的输出行为（如打印 `None` 或格式化结果） |
| `sys.excepthook(type, value, traceback)` | 控制未捕获异常的全局处理行为，可被重写以自定义错误报告       |
| `sys.addaudithook(hook)`                 | 添加一个审计钩子函数，用于监控解释器内部事件（如文件打开、网络请求） |