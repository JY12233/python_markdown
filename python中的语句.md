# del语句

在 Python 中，`del` 是一个用于**删除对象引用**的关键字（或语句）。

它的核心作用是从内存中移除对某个对象的引用，而不是直接销毁对象本身。当对象的引用计数降为 0 时，Python 的垃圾回收机制会自动回收该对象所占用的内存。

## `del` 的主要用法

`del` 的功能非常强大，可以应用于多种场景：

### 删除变量

这是 `del` 最常见的用法。它会从当前命名空间中移除变量名。

```python
x = 10
del x
# print(x)  # 再次访问会报错：NameError: name 'x' is not defined
```

### 删除数据结构中的元素

`del` 可以精确地删除列表、字典等数据结构中的特定项。

- 删除列表元素

  可以通过索引或切片来删除。

  ```python
  my_list = [1, 2, 3, 4, 5]
  del my_list[2]      # 删除索引为2的元素 (值为3)
  # my_list 变为 [1, 2, 4, 5]
  
  del my_list[0:2]    # 删除切片，即索引0和1的元素
  # my_list 变为 [4, 5]
  ```

- 删除字典键值对

  通过指定键来删除。

  ```python
  my_dict = {'a': 1, 'b': 2, 'c': 3}
  del my_dict['b']
  # my_dict 变为 {'a': 1, 'c': 3}
  ```

- 删除对象属性

  可以移除对象的某个属性。

  ```python
  class Person:
      def __init__(self, name):
          self.name = name
  
  p = Person("Alice")
  del p.name
  # print(p.name) # 再次访问会报错：AttributeError
  ```

## 重要注意事项

1. `del` 是语句，不是函数

   这是初学者常犯的错误。`del` 后面不应该加括号。

   - **正确写法**: `del my_list[0]`
   - **错误写法**: `del(my_list[0])`（这会引发 `TypeError`）

2. `del` vs `clear()`
   对于列表或字典，`del` 和 `clear()` 的作用不同。
   - `my_list.clear()`：清空列表的所有内容，但 `my_list` 这个变量依然存在，它指向一个空列表 `[]`。
   - `del my_list`：直接删除 `my_list` 这个变量，之后再访问它会报错。

## 与其他删除方式的对比

在删除列表元素时，`del`、`pop()` 和 `remove()` 各有侧重：

| 方法       | 删除依据                  | 是否返回被删元素 |
| :--------- | :------------------------ | :--------------- |
| `del`      | **索引** (或切片)         | 否               |
| `pop()`    | **索引** (默认为最后一个) | **是**           |
| `remove()` | **值** (删除第一个匹配项) | 否               |

## `del` 语句 vs `__del__` 方法

请不要将 `del` 语句与类的 `__del__` 方法混淆。

- `del` 语句

  如上所述，用于删除引用。

- `__del__` 方法

  这是一个特殊的类方法，被称为**析构器**。当对象即将被垃圾回收时，Python 会自动调用这个方法，通常用于执行一些清理工作，比如关闭文件、断开网络连接等。但它的调用时机是不确定的，不应依赖它来管理关键资源。

---

# match语句

Python 的 `match` 语句（Python 3.10 引入，也叫**结构模式匹配**）类似于其他语言中的 `switch-case`，但功能更强大——它可以匹配常量、类型、结构（如列表、字典、类实例）以及使用通配符和守卫条件。

## :one: 基本语法

```python
match value:
    case pattern1:
        # 当 value 匹配 pattern1 时执行
    case pattern2:
        # 当 value 匹配 pattern2 时执行
    case _:
        # 通配符，匹配任何情况（类似 default）
```

**注意**：`case` 块不需要 `break`，匹配成功后只会执行匹配到的第一个块，然后自动退出。

## :two: 匹配常量

```python
status_code = 404

match status_code:
    case 200:
        print("OK")
    case 404:
        print("Not Found")
    case 500:
        print("Server Error")
    case _:
        print("Other code")
# 输出：Not Found
```

## :three:通配符 `_` 和 `|`

- `_` 匹配任何值，作为最后的兜底。
- `|` 可以组合多个模式（“或”）。

```python
command = "quit"

match command:
    case "quit" | "exit" | "bye":
        print("Exiting program")
    case "help":
        print("Showing help")
    case _:
        print("Unknown command")
```

## :four: 解构序列

`match` 可以匹配序列的结构并提取元素。

```python
point = (0, 5)

match point:
    case (0, 0):
        print("Origin")
    case (0, y):
        print(f"On Y-axis, y={y}")
    case (x, 0):
        print(f"On X-axis, x={x}")
    case (x, y):
        print(f"Point at ({x}, {y})")
# 输出：On Y-axis, y=5
```

也可以匹配列表：

```python
items = ["hello", 123]

match items:
    case [str(s), int(n)]:
        print(f"String '{s}' and integer {n}")
    case _:
        print("Not matched")
```

## :five: 匹配字典

通过键匹配，只检查部分键即可。

```python
person = {"name": "Alice", "age": 30}

match person:
    case {"name": "Alice", "age": age}:
        print(f"Alice is {age} years old")
    case {"name": name, "age": age}:
        print(f"{name} is {age} years old")
    case _:
        print("Unknown person")
```

## :six: 匹配类实例

需要配合类的 `__match_args__` 或使用关键字模式。

```python
class Point:
    __match_args__ = ("x", "y")   # 声明解构顺序
    def __init__(self, x, y):
        self.x = x
        self.y = y

p = Point(2, 3)

match p:
    case Point(0, 0):
        print("Origin")
    case Point(x, y):
        print(f"Point({x}, {y})")
```

或者使用关键字模式（不依赖 `__match_args__`）：

```python
match p:
    case Point(x=0, y=0):
        print("Origin")
    case Point(x=x, y=y):
        print(f"Point({x}, {y})")
```

## :seven: 守卫条件

在模式后添加 `if` 进一步限制匹配。

```python
value = (10, 20)

match value:
    case (x, y) if x > y:
        print(f"x > y: {x} > {y}")
    case (x, y) if x < y:
        print(f"x < y: {x} < {y}")
    case (x, y):
        print(f"x == y: {x} == {y}")
# 输出：x < y: 10 < 20
```

## :eight:捕获子模式

可以用 `as` 给某个子模式绑定变量。

```python
items = [1, 2, 3]

match items:
    case [first, second, third] as whole:
        print(f"First: {first}, Whole list: {whole}")
```

## :nine: 匹配任意类型但需要变量时

如果你想匹配任意值并**同时绑定变量**，直接使用单个变量名即可（不要用 `_`，因为 `_` 是通配符，不绑定）。

```python
value = "hello"

match value:
    case str(s):      # 匹配字符串并绑定到 s
        print(f"It's a string: {s}")
    case int(n):
        print(f"It's an integer: {n}")
    case other:       # 匹配任何其他类型，并绑定到 other
        print(f"Something else: {other}")
```

## :one::zero:与 `if-elif` 的比较

- `match` 更专注于**结构解构**和**模式匹配**，代码更声明式。
- 对于简单的常量比较，`if-elif` 足够，但 `match` 在处理复杂数据结构（嵌套、类型、提取）时非常优雅。
- `match` 在 Python 中执行速度通常比一连串 `if-elif` 快，但主要优势是可读性。

## :one::one:小结

| 需求             | 写法                    |
| :--------------- | :---------------------- |
| 常量匹配         | `case 200:`             |
| 多模式“或”       | `case "quit" | "exit":` |
| 序列解构         | `case (x, y):`          |
| 字典部分匹配     | `case {"name": name}:`  |
| 类实例匹配       | `case Point(x, y):`     |
| 添加额外条件     | `case (x, y) if x > y:` |
| 绑定整个匹配对象 | `case [a, b] as whole:` |
| 默认（通配符）   | `case _:`               |

---

# `with`语句

`with` 关键字是 Python 中一个非常重要且强大的特性，它用于**上下文管理**（Context Management）。它的核心目标是**确保资源能够被正确地获取和释放**，无论在使用过程中是否发生异常。

简单来说，`with` 的最佳使用时机是：

> 当你需要“先做一件事（如打开文件、获取锁），然后在代码块结束后，无论成功还是失败，都要做另一件事（如关闭文件、释放锁）”的时候。

## 文件操作

这是 `with` 最广为人知的应用。

```python
# ❌ 不推荐：手动管理资源，容易出错
file = open('example.txt', 'r')
try:
    content = file.read()
    print(content)
finally:
    file.close()  # 必须手动关闭，否则文件句柄会泄露

# ✅ 推荐：使用 with，自动管理资源
with open('example.txt', 'r') as file:
    content = file.read()
    print(content)
# 即使 read() 或 print() 出现异常，file.close() 也会被自动调用
```

**为什么 `with` 更好？**

因为它把“打开文件”和“关闭文件”这两个必须成对出现的操作绑定在一起。你只需要关心中间的读写逻辑。

------

##  锁的管理

```python
import threading

lock = threading.Lock()

# ❌ 不推荐：手动 acquire/release，容易忘记 release 导致死锁
lock.acquire()
try:
    # 临界区代码
    pass
finally:
    lock.release()

# ✅ 推荐：使用 with，自动 acquire/release
with lock:
    # 临界区代码
    pass
# 退出 with 块时，lock.release() 会自动被调用
```

------

## 数据库连接和游标

```python
import sqlite3

# ❌ 手动管理
conn = sqlite3.connect('example.db')
cursor = conn.cursor()
try:
    cursor.execute("SELECT * FROM table_name")
    results = cursor.fetchall()
finally:
    cursor.close()
    conn.close()

# ✅ 使用 with
with sqlite3.connect('example.db') as conn:
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM table_name")
    results = cursor.fetchall()
# conn.commit() 并且 conn.close() 会自动执行
```

##  临时修改状态

*如改变目录、捕获输出*

```python
import os
from contextlib import redirect_stdout

# 场景：临时切换工作目录
original_dir = os.getcwd()
try:
    os.chdir('/tmp')
    # 在 /tmp 目录下执行一些操作
    do_something_in_tmp()
finally:
    os.chdir(original_dir)  # 确保切回来

# 使用 with 和 os.chdir 的上下文管理器（需要自己实现或使用第三方库）
# from contextlib_chdir import chdir  # 假设有一个这样的库
# with chdir('/tmp'):
#     do_something_in_tmp() # 自动切回原目录

# 场景：重定向 print 输出到文件
with open('output.txt', 'w') as f:
    with redirect_stdout(f):
        print("这段文字会被写入文件，而不是打印到屏幕")
```

## 核心原理

`with` 能起作用，是因为对象实现了**上下文管理协议**，即定义了 `__enter__` 和 `__exit__` 两个特殊方法。

```python
class MyContextManager:
    def __enter__(self):
        print("进入 with 块前执行")
        # 返回的对象赋值给 as 后面的变量
        return "返回值"

    def __exit__(self, exc_type, exc_value, traceback):
        print("退出 with 块后执行")
        # exc_type, exc_value, traceback 分别是异常类型、值、追踪信息
        # 如果 with 块内发生异常，这三个参数会被传入
        # 如果 __exit__ 返回 True，异常会被吞掉；返回 False 或 None，异常会向上抛出
        if exc_type:
            print(f"处理了一个 {exc_type.__name__} 异常")
        return False # 不吞掉异常

# 使用
with MyContextManager() as value:
    print(f"as 变量的值是: {value}")
    raise ValueError("模拟一个异常")
```

##  总结

**每当遇到以下情况时，优先考虑 `with`：**

- **打开/关闭资源**：文件、网络连接、数据库连接、锁等。
- **获取/释放资源**：获取锁、分配内存池等。
- **临时修改环境**：修改全局变量、切换目录、捕获标准输出等。
- **任何需要“配对”操作的场景**：进入某个状态，离开时必须恢复。

**一句话概括：**

`with` 是 Python 提供的一种优雅的语法糖，用于确保**成对出现的“开始-结束”操作**能被正确执行，从而让你的代码更简洁、更安全、更不容易出错。



---