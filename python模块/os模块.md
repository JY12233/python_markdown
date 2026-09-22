# 介绍

`os` 模块是 Python 标准库中用于与操作系统进行交互的核心接口，它通过 `import os` 导入后即可使用，主要用于编写跨平台的文件系统操作、进程管理及环境变量处理脚本，当需要执行创建或删除文件、遍历目录、获取系统信息或执行系统命令等底层操作时，就该使用它。

`os` 模块的主要作用是屏蔽不同操作系统（如 Windows、Linux、macOS）之间的差异，提供统一的接口来访问文件系统（如文件和目录的创建、删除、重命名、遍历）、处理文件路径（如路径拼接、分割、判断）、管理进程（如获取进程 ID、执行系统命令）以及访问和修改系统环境变量。

`os` 模块内部包含一个重要的子模块 `os.path`，专门用于处理路径相关的操作；同时，它还依赖或关联了多个底层类和异常，例如用于表示目录条目的 `DirEntry` 类（由 `os.scandir()` 返回）、用于表示文件状态的 `stat_result` 类、用于表示终端大小的 `terminal_size` 类，以及统一的异常基类 `OSError`（其别名为 `os.error`）。

# 常用属性

| 属性名称     | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| `os.name`    | 返回当前操作系统的名称，例如 `'nt'`（Windows）或 `'posix'`（Linux、macOS） |
| `os.sep`     | 返回当前操作系统的路径分隔符，例如 `'\\'`（Windows）或 `'/'`（Unix） |
| `os.linesep` | 返回当前操作系统的行终止符，例如 `'\r\n'`（Windows）或 `'\n'`（Unix） |
| `os.environ` | 一个映射对象（类似字典），包含当前系统的所有环境变量         |
| `os.curdir`  | 返回代表当前目录的字符串，始终为 `'.'`                       |
| `os.pardir`  | 返回代表上级目录的字符串，始终为 `'..'`                      |
| `os.pathsep` | 返回用于分隔环境变量（如 `PATH`）中不同路径的分隔符，例如 `';'`（Windows）或 `':'`（Unix） |
| `os.devnull` | 返回空设备的路径，例如 `'nul'`（Windows）或 `'/dev/null'`（Unix） |

# 常用方法

| 方法名称                            | 说明                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| `os.getcwd()`                       | 获取当前工作目录的绝对路径                                   |
| `os.chdir(path)`                    | 将当前工作目录更改为指定的 `path` 路径                       |
| `os.listdir(path='.')`              | 返回指定 `path` 目录下的所有文件和子目录名称列表             |
| `os.mkdir(path)`                    | 创建单层目录，若目录已存在或父目录不存在则会报错             |
| `os.makedirs(path, exist_ok=False)` | 递归创建多层目录，若 `exist_ok` 为 `True`，则目录已存在时不会报错 |
| `os.remove(path)`                   | 删除指定的文件，若路径为目录或文件不存在则会报错             |
| `os.rmdir(path)`                    | 删除指定的空目录，若目录不为空则会报错                       |
| `os.rename(src, dst)`               | 将文件或目录从 `src` 重命名或移动至 `dst`                    |
| `os.walk(top, topdown=True)`        | 递归遍历目录树，生成包含 `(dirpath, dirnames, filenames)` 的三元组 |
| `os.stat(path)`                     | 获取指定 `path` 的文件或目录的详细状态信息（如大小、修改时间） |
| `os.system(command)`                | 在子 Shell 中执行系统 `command` 命令，并返回退出状态码       |
| `os.getenv(key, default=None)`      | 获取指定 `key` 的环境变量值，若不存在则返回 `default`        |
| `os.cpu_count()`                    | 返回当前系统的逻辑 CPU 核心数量                              |
| `os.getpid()`                       | 获取当前进程的 ID                                            |

# 子模块 `Path` 介绍

## 介绍

`os.path` 是 Python 标准库中 `os` 模块的一个子模块，它提供了一系列用于处理文件路径字符串的函数。使用时通过 `import os` 导入后，直接以 `os.path.函数名()` 的形式调用。当需要进行跨平台的路径拼接、分割、判断文件类型或获取文件属性等底层字符串操作时，就可以使用它。

`os.path` 模块的核心作用是屏蔽不同操作系统（如 Windows、Linux、macOS）之间路径分隔符的差异，提供统一的接口来对路径字符串进行解析和处理。它的主要功能包括：路径的拼接与分割、判断路径是否为文件或目录、获取文件的修改时间与大小、判断路径是否存在以及获取绝对路径等。

`os.path` 模块本身是一个模块对象，它内部没有定义额外的子模块，但依赖于 `os` 模块中的 `stat` 模块来获取文件状态信息。在实际开发中，它通常与 `os` 模块配合使用，共同完成文件系统的底层操作。

## 常用属性

| 属性名称          | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| `os.path.sep`     | 返回当前操作系统的路径分隔符，例如 `'\\'`（Windows）或 `'/'`（Unix） |
| `os.path.altsep`  | 返回当前操作系统的备用路径分隔符，例如 Windows 下为 `'/'`，Unix 下为 `None` |
| `os.path.extsep`  | 返回文件名与扩展名之间的分隔符，始终为 `'.'`                 |
| `os.path.pardir`  | 返回代表上级目录的字符串，始终为 `'..'`                      |
| `os.path.curdir`  | 返回代表当前目录的字符串，始终为 `'.'`                       |
| `os.path.devnull` | 返回空设备的路径，例如 `'nul'`（Windows）或 `'/dev/null'`（Unix） |

## 常用方法

| 方法名称                     | 说明                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| `os.path.join(path, *paths)` | 将一个或多个路径片段拼接成一个完整的路径字符串               |
| `os.path.split(path)`        | 将路径分割为目录和文件名两部分，返回一个元组 `(head, tail)`  |
| `os.path.splitext(path)`     | 将路径分割为文件名和扩展名两部分，返回一个元组 `(name, ext)` |
| `os.path.basename(path)`     | 返回路径中的文件名部分（即最后一个 `/` 或 `\` 之后的内容）   |
| `os.path.dirname(path)`      | 返回路径中的目录部分（即最后一个 `/` 或 `\` 之前的内容）     |
| `os.path.exists(path)`       | 判断指定的路径（文件或目录）是否存在                         |
| `os.path.isfile(path)`       | 判断指定的路径是否为一个普通文件                             |
| `os.path.isdir(path)`        | 判断指定的路径是否为一个目录                                 |
| `os.path.getsize(path)`      | 返回指定文件的大小（以字节为单位）                           |
| `os.path.getmtime(path)`     | 返回指定文件的最后修改时间（时间戳格式）                     |
| `os.path.abspath(path)`      | 返回指定路径的绝对路径                                       |
| `os.path.normpath(path)`     | 规范化路径字符串，处理多余的分隔符和 `..` 等                 |

# 代码示例

## 批量重命名

```text
['06_递归.py', '07_递归N皇后问题.py', '08_字符串的全排列.py', '09_走楼梯.py', '10_波兰表达式.py', '11_得到24.py', '12_7的倍数.py']

把序号变为从 01 开始
```

```python
import os

# 1. 指定目标文件夹路径（这里假设是当前目录下的 'my_folder'）
folder_path = 'my_folder'

# 2. 获取文件夹下所有的 .py 文件，并排序（确保顺序正确）
files = sorted([f for f in os.listdir(folder_path) if f.endswith('.py')])

# 3. 遍历文件并进行重命名
for index, filename in enumerate(files, start=1):
    # 提取原文件名中下划线后面的部分（例如 '06_递归.py' -> '递归.py'）
    if '_' in filename:
        new_name_part = filename.split('_', 1)[1]
    else:
        new_name_part = filename

    # 生成新的序号（两位数，不足补零）和新文件名
    new_index = str(index).zfill(2)  # zfill(2) 会将 1 变成 '01'
    new_filename = f"{new_index}_{new_name_part}"

    # 拼接完整的文件路径
    old_file_path = os.path.join(folder_path, filename)
    new_file_path = os.path.join(folder_path, new_filename)
    
    # 执行重命名
    os.rename(old_file_path, new_file_path)
    print(f"已重命名: {filename} -> {new_filename}")
```

>在进行批量重命名这种**不可逆**的操作前，强烈建议先不要直接调用 `os.rename()`，而是先 `print()` 打印出新旧文件名的对应关系，确认无误后再执行重命名逻辑。