# 介绍

`pathlib` 是 Python 3.4 引入的标准库模块，它提供了一种面向对象的方式来处理文件系统路径。使用时只需通过 `from pathlib import Path` 导入核心的 `Path` 类即可。

当需要进行跨平台的路径拼接、文件与目录的遍历、创建、删除或读写等复杂操作时，强烈建议使用它来替代传统的 `os.path` 模块。

`pathlib` 模块的核心作用是将文件路径封装为对象，从而让路径操作变得更加直观和安全。它自动处理了不同操作系统（如 Windows 的反斜杠 `\` 与 Unix 的正斜杠 `/`）之间的路径分隔符差异，支持使用 `/` 运算符进行优雅的路径拼接，并提供了丰富的属性和方法来直接对文件或目录进行查询、修改及 I/O 操作。

# 核心类

`pathlib` 模块主要包含以下核心类：

- **`PurePath`**

  纯路径基类，仅提供路径计算操作，不访问文件系统。

- **`PurePosixPath`**

  `PurePath` 的子类，专门用于处理 POSIX（Unix、macOS）风格的路径。

- **`PureWindowsPath`**

  `PurePath` 的子类，专门用于处理 Windows 风格的路径。

- **`Path`**

  具体路径基类，继承自 `PurePath`，在纯路径操作的基础上增加了文件系统 I/O 操作（如读取文件、创建目录）。它是日常开发中最常用的类。

- **`PosixPath`**

  `Path` 的子类，用于非 Windows 系统。

- **`WindowsPath`**

  `Path` 的子类，用于 Windows 系统。

# `Path` 创建对象

```python
Path(*pathsegments)  # 接收一个或多个路径片段（字符串或路径对象），并将它们拼接成一个完整的路径
```

| 参数名称        | 作用说明                                                     | 可选值或类型说明                                             |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `*pathsegments` | 用于拼接成最终路径的一个或多个片段。<br />`Path` 会将传入的所有片段按顺序用操作系统的路径分隔符拼接起来。 | 1. **字符串**：例如 `'home'`，`'user'`，`'file.txt'` <br />2. **Path 对象**：可以传入已有的 `Path` 对象进行进一步拼接 <br />3. **空参数**：如果不传任何参数，`Path()` 会生成一个代表当前目录的路径对象（相当于 `Path('.')`） |

# 常用属性

| 属性名称        | 说明                                                       |
| --------------- | ---------------------------------------------------------- |
| `Path.name`     | 返回路径最后的组件（即文件名或目录名，包含扩展名）         |
| `Path.stem`     | 返回路径最后组件的名称（不包含扩展名）                     |
| `Path.suffix`   | 返回路径最后组件的扩展名（包含点号 `.`）                   |
| `Path.suffixes` | 返回路径最后组件的所有扩展名列表（例如 `['.tar', '.gz']`） |
| `Path.parent`   | 返回当前路径的父目录路径对象                               |
| `Path.parents`  | 返回一个包含所有祖先目录的不可变序列                       |
| `Path.parts`    | 返回一个包含路径各个组成部分的元组                         |
| `Path.anchor`   | 返回路径的锚点（即根目录或驱动器盘符）                     |

# 常用方法

| 方法                                                         | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `Path.cwd()`                                                 | 类方法，返回当前工作目录的路径对象                           |
| `Path.home()`                                                | 类方法，返回当前用户主目录的路径对象                         |
| `Path.exists()`                                              | 判断该路径指向的文件或目录是否存在                           |
| `Path.is_file()`                                             | 判断该路径是否为一个普通文件                                 |
| `Path.is_dir()`                                              | 判断该路径是否为一个目录                                     |
| `Path.resolve(strict=False)`                                 | 返回该路径的绝对路径，并解析所有的符号链接。<br />若 `strict` 为 `True` 且路径不存在则抛出异常 |
| `Path.mkdir(mode=0o777, parents=False, exist_ok=False)`      | 创建目录。<br />若 `parents` 为 `True` 则递归创建；<br />若 `exist_ok` 为 `True` 则目录已存在时不报错 |
| `Path.rmdir()`                                               | 删除该路径指向的空目录                                       |
| `Path.unlink(missing_ok=False)`                              | 删除该路径指向的文件。<br />若 `missing_ok` 为 `True` 则文件不存在时不报错 |
| `Path.iterdir()`                                             | 返回一个生成器，用于遍历该目录下的所有文件和子目录           |
| `Path.glob(pattern)`                                         | 根据通配符 `pattern` 匹配并返回该目录下的路径生成器（不递归） |
| `Path.rglob(pattern)`                                        | 根据通配符 `pattern` 匹配并返回该目录及其子目录下的路径生成器（递归） |
| `Path.read_text(encoding=None)`                              | 以文本模式读取文件内容并返回字符串                           |
| `Path.write_text(data, encoding=None)`                       | 以文本模式将 `data` 写入文件（会覆盖原内容）                 |
| `Path.rename(target)`                                        | 将文件或目录重命名或移动至 `target` 路径                     |
| `Path.joinpath(*pathsegments)`                               | 将当前路径与传入的一个或多个路径片段拼接，返回新路径（等价于使用 `/` 运算符） |
| `Path.absolute()`                                            | 返回该路径的绝对路径，但不解析符号链接                       |
| `Path.expanduser()`                                          | 解析路径中的 `~` 和 `~user` 为用户主目录，返回新路径         |
| `Path.touch(mode=0o777, exist_ok=True)`                      | 在该路径创建一个空文件。<br />若文件已存在且 `exist_ok` 为 `True`，则更新修改时间 |
| `Path.replace(target)`                                       | 将文件或目录移动至 `target` 路径，若目标已存在则无条件覆盖   |
| `Path.stat(*, follow_symlinks=True)`                         | 返回 `os.stat_result` 对象，包含文件大小、修改时间等详细信息 |
| `Path.lstat()`                                               | 类似 `stat()`，但如果路径是符号链接，返回链接本身的信息而非目标文件的信息 |
| `Path.chmod(mode, *, follow_symlinks=True)`                  | 修改文件或目录的权限模式                                     |
| `Path.lchmod(mode)`                                          | 修改符号链接自身的权限（不跟随链接）                         |
| `Path.symlink_to(target, target_is_directory=False)`         | 在该路径创建一个指向 `target` 的符号链接                     |
| `Path.hardlink_to(target)`                                   | 在该路径创建一个指向 `target` 的硬链接（Python 3.10+）       |
| `Path.readlink()`                                            | 返回符号链接所指向的目标路径（Python 3.9+）                  |
| `Path.read_bytes()`                                          | 以二进制模式读取文件内容并返回字节串                         |
| `Path.write_bytes(data)`                                     | 以二进制模式将字节串 `data` 写入文件（会覆盖原内容）         |
| `Path.open(mode='r', buffering=-1, encoding=None, errors=None, newline=None)` | 打开文件并返回文件对象，行为等同于内置 `open()`              |
| `Path.walk(top_down=True, on_error=None, follow_symlinks=False)` | 递归遍历目录树，生成 `(dirpath, dirnames, filenames)` 三元组（Python 3.12+） |
| `Path.is_symlink()`                                          | 判断该路径是否为符号链接                                     |
| `Path.is_absolute()`                                         | 判断该路径是否为绝对路径                                     |
| `Path.is_mount()`                                            | 判断该路径是否为挂载点                                       |
| `Path.is_block_device()`                                     | 判断该路径是否为块设备                                       |
| `Path.is_char_device()`                                      | 判断该路径是否为字符设备                                     |
| `Path.is_fifo()`                                             | 判断该路径是否为命名管道（FIFO）                             |
| `Path.is_socket()`                                           | 判断该路径是否为套接字文件                                   |
| `Path.is_relative_to(other)`                                 | 判断该路径是否为 `other` 的子路径（Python 3.9+）             |
| `Path.samefile(other_path)`                                  | 判断该路径与 `other_path` 是否指向同一个文件（通过 inode 判断） |
| `Path.match(pattern, *, case_sensitive=None)`                | 判断该路径是否匹配给定的通配符模式                           |
| `Path.relative_to(other, walk_up=False)`                     | 计算该路径相对于 `other` 的相对路径。若 `walk_up` 为 `True` 则允许使用 `..`（Python 3.12+） |
| `Path.with_name(name)`                                       | 返回一个新路径，将原路径的最后一级名称替换为 `name`          |
| `Path.with_stem(stem)`                                       | 返回一个新路径，将原路径的文件名（不含扩展名）替换为 `stem`（Python 3.9+） |
| `Path.with_suffix(suffix)`                                   | 返回一个新路径，将原路径的扩展名替换为 `suffix`              |
| `Path.as_posix()`                                            | 将路径转换为 POSIX 格式字符串（使用 `/` 分隔）               |
| `Path.as_uri()`                                              | 将绝对路径转换为 `file://` 格式的 URI 字符串                 |
| `Path.from_uri(uri)`                                         | 类方法，解析 `file://` 格式的 URI 并返回路径对象（Python 3.13+） |
| `Path.owner()`                                               | 返回该文件的所有者用户名                                     |
| `Path.group()`                                               | 返回该文件所属的用户组名                                     |

# 代码示例

## 批量重命名

```text
['06_递归.py', '07_递归N皇后问题.py', '08_字符串的全排列.py', '09_走楼梯.py', '10_波兰表达式.py', '11_得到24.py', '12_7的倍数.py']

把序号变为从 01 开始
```

```python
from pathlib import Path

# 1. 指定目标文件夹路径（这里假设是当前目录下的 'my_folder'）
folder_path = Path('my_folder')

# 2. 获取文件夹下所有的 .py 文件，并排序（确保顺序正确）
files = sorted(folder_path.glob('*.py'))

# 3. 遍历文件并进行重命名
for index, file_path in enumerate(files, start=1):
    # 提取原文件名中下划线后面的部分（例如 '06_递归.py' -> '递归.py'）
    if '_' in file_path.name:
        new_name_part = file_path.name.split('_', 1)[1]
    else:
        new_name_part = file_path.name

    # 生成新的序号（两位数，不足补零）和新文件名
    new_index = str(index).zfill(2)
    new_filename = f"{new_index}_{new_name_part}"

    # 使用 pathlib 的 rename 方法，直接传入新的文件名或新路径对象即可
    new_file_path = folder_path / new_filename
    file_path.rename(new_file_path)
    print(f"已重命名: {file_path.name} -> {new_filename}")
```

