# zip函数

`zip` 是 Python 中一个非常实用的**内置函数**，它的核心作用是将多个**可迭代对象**（如列表、元组、字符串）中的元素“打包”在一起，返回一个由元组组成的迭代器。

你可以把它想象成现实中的**拉链（Zipper）**：左右两边的齿孔一一对应，合并成一条。

## 基本语法

```python
zip(*iterables)
```

- `iterables`：一个或多个可迭代对象（如 list1, list2, ...）。

## 核心用法与示例

### **将多个列表“压缩”成配对数据**

这是最常见的用法，将两个（或多个）列表对应位置的元素组合成元组。

```python
names = ['Alice', 'Bob', 'Charlie']
ages = [25, 30, 35]

# 使用 zip 将名字和年龄配对
paired = zip(names, ages)
print(list(paired))
# 输出: [('Alice', 25), ('Bob', 30), ('Charlie', 35)]
```

### 解包（Unzipping）：逆向操作

如果你有一个被 `zip` 打包过的列表，可以使用 `zip(*zipped)` 将其解压还原。

```python
# 假设这是之前打包好的数据
paired = [('Alice', 25), ('Bob', 30), ('Charlie', 35)]

# 使用 * 操作符进行解包
names, ages = zip(*paired)
print(names) # 输出: ('Alice', 'Bob', 'Charlie')
print(ages)  # 输出: ('25', '30', '35')
```

### 遍历多个序列

在 `for` 循环中同时遍历多个列表，代码更简洁。

```python
fruits = ['apple', 'banana', 'cherry']
prices = [1.2, 0.5, 2.0]

for fruit, price in zip(fruits, prices):
    print(f"{fruit} costs ${price}")
# 输出:
# apple costs $1.2
# banana costs $0.5
# cherry costs $2.0
```

## 重要特性与注意事项

- **长度以最短的为准**：
  如果传入的可迭代对象长度不同，`zip` 会自动截断，只保留所有序列中**最短**的那个长度。

  ```python
  a = [1, 2, 3]
  b = ['x', 'y'] # 只有2个元素
  print(list(zip(a, b)))
  # 输出: [(1, 'x'), (2, 'y')]  (3 被丢弃了)
  ```

- **返回的是迭代器（Iterator）**：
  在 Python 3 中，`zip` 返回的是一个**迭代器**，只能被遍历一次。如果需要多次使用，建议将其转换为列表 `list(zip(...))`。

  ```python
  z = zip([1, 2], [3, 4])
  print(list(z)) # [(1, 3), (2, 4)]
  print(list(z)) # [] (第二次遍历为空)
  ```

- **配合字典使用**：
  `zip` 常用于快速创建字典，将一个列表作为键，另一个列表作为值。

  ```python
  keys = ['name', 'age', 'city']
  values = ['Tom', 28, 'Beijing']
  
  person = dict(zip(keys, values))
  print(person)
  # 输出: {'name': 'Tom', 'age': 28, 'city': 'Beijing'}
  ```

## 总结表格

| 场景         | 代码示例                  | 说明                       |
| :----------- | :------------------------ | :------------------------- |
| **打包**     | `zip(list1, list2)`       | 生成 `(x, y)` 元组的迭代器 |
| **解包**     | `zip(*zipped_list)`       | 将列表还原为原来的多个列表 |
| **创建字典** | `dict(zip(keys, values))` | 键值对快速映射             |
| **遍历**     | `for x, y in zip(a, b)`   | 同时循环多个序列           |

`zip` 是处理成对数据的神器，特别是在数据清洗、处理表格数据时非常高效。

---

# id函数

在 Python 中，`id()` 函数的主要作用是**获取对象的“身份证号码”**。

简单来说，它返回对象在内存中的**唯一标识符**（通常就是对象在内存中的地址）。这个标识符是一个整数，在对象的生命周期内是恒定不变的。

你可以把它理解为 Python 对象的**内存地址**。

## 核心用途

### 判断是否是“同一个对象”

这是 `id()` 最常用的场景。在 Python 中，`==` 比较的是**值**是否相等，而 `id()` 比较的是**身份**（内存地址）是否相同。

- **场景**：你想知道两个变量是指向同一块内存（引用关系），还是两块独立的内存。

```python
# 场景一：引用赋值（指向同一个对象）
a = [1, 2, 3]
b = a  # b 直接指向 a 的内存地址

print(id(a) == id(b))  # 输出: True
# 修改 b，a 也会变，因为它们是同一个对象

# 场景二：值相等，但对象不同
c = [1, 2, 3]
d = [1, 2, 3]

print(c == d)          # 输出: True (值相等)
print(id(c) == id(d))  # 输出: False (内存地址不同，是两个独立的列表)
```

### **调试与内存分析**

当你处理复杂的数据结构（如嵌套列表、字典）或进行深拷贝/浅拷贝操作时，`id()` 可以帮你确认数据是否被正确复制，还是仅仅复制了引用。

```python
import copy

original = [[1, 2], [3, 4]]
shallow = copy.copy(original)  # 浅拷贝

# 检查外层列表的 id
print(id(original) == id(shallow))  # False (外层是两个不同的列表)

# 检查内部元素的 id (浅拷贝内部元素是共用的)
print(id(original[0]) == id(shallow[0]))  # True (内部的小列表是同一个！)
```

### Python 的缓存机制

Python 为了优化性能，会对某些不可变对象（如小整数、短字符串）进行缓存（驻留）。使用 `id()` 可以直观地看到这一点。

```python
# 小整数缓存（通常是 -5 到 256）
x = 100
y = 100
print(id(x) == id(y))  # 输出: True (Python 复用了同一个对象)

# 大整数不缓存
m = 1000
n = 1000
print(id(m) == id(n))  # 输出: False (分别创建了新对象)
```

------

## 对比

| 比较方式     | 关注点           | 对应运算符           | 示例                        |
| :----------- | :--------------- | :------------------- | :-------------------------- |
| **值比较**   | 内容是否一样     | `==`                 | `[1,2] == [1,2]` 为 `True`  |
| **身份比较** | 内存地址是否一样 | `is` (底层用 `id()`) | `[1,2] is [1,2]` 为 `False` |

## 注意事项

1. **无需导入**：`id()` 是 Python 的**内置函数**，直接可以使用，不需要 `import` 任何模块。
2. **CPython 的实现**：在标准的 CPython 解释器中，`id()` 返回的就是内存地址。但在其他解释器（如 Jython, IronPython）中，它可能只是一个唯一的数字标识，不一定是物理地址。
3. **十六进制查看**：如果你想看类似 C 语言指针那样的十六进制地址，可以用 `hex(id(obj))` 来转换。

```python
x = "Hello"
print(hex(id(x))) 
# 输出示例: 0x7f8b1c2d3e40
```

---

# callable函数

`callable()` 函数的作用非常单一且明确：它用来**判断一个对象是否“可被调用”**。

简单来说，就是帮你检查某个东西能不能加上括号 `()` 来执行。它返回一个布尔值：

- **`True`**：表示这个对象可以像函数一样被调用（例如 `obj()`）。
- **`False`**：表示这个对象不能这样调用，否则会报错。

## “可调用”

在 Python 中，不仅仅是函数，很多对象都是可调用的。`callable()` 能帮你识别它们：

1. **函数和方法**
   这是最常见的情况。无论是自定义函数、内置函数（如 `print`），还是类的方法，都是可调用的。

   ```python
   def my_func(): pass
   print(callable(my_func))  # True
   print(callable(print))    # True
   ```

2. **类**
   类本身也是可调用的，因为调用类（如 `MyClass()`）会创建一个新的实例。

   ```python
   class MyClass: pass
   print(callable(MyClass))  # True
   ```

3. **实现了 `__call__` 方法的实例**
   这是 Python 的一个强大特性。如果一个类的实例定义了 `__call__` 方法，那么这个实例也可以像函数一样被调用。`callable()` 能准确识别出这种“伪装”成函数的对象。

   ```python
   class Adder:
       def __call__(self, x, y):
           return x + y
   
   adder_instance = Adder()
   print(callable(adder_instance))  # True
   print(adder_instance(2, 3))      # 输出: 5
   ```

## “不可调用”

大部分数据类型的实例都是不可调用的，例如数字、字符串、列表等。

```python
print(callable(42))         # False
print(callable("hello"))    # False
print(callable([1, 2, 3]))  # False
```

## 主要用途

`callable()` 的核心价值在于**防御性编程**和**动态编程**，它能提高代码的健壮性。

### 验证函数参数

当你编写一个函数，并且这个函数需要接收另一个函数作为参数（例如回调函数）时，使用 `callable()` 进行检查是一个好习惯。这可以在执行前就发现错误，而不是等到调用时才抛出晦涩的异常。

```python
def process_data(data, transform_func):
    # 在执行前检查 transform_func 是否真的是一个可调用的函数
    if not callable(transform_func):
        raise TypeError(f"transform_func 必须是可调用对象，但收到了 {type(transform_func)}")
    
    return transform_func(data)

# 正确用法
print(process_data([1, 2, 3], sum)) # 6

# 错误用法，会立刻抛出清晰的 TypeError
# print(process_data([1, 2, 3], "sum"))
```

###  实现动态分派

在一些高级场景中，你可能有一个字典，里面既存放了静态值，也存放了可以动态计算的函数。你可以用 `callable()` 来统一处理它们。

```python
config = {
    'version': '1.0',
    'timestamp': lambda: __import__('time').time()
}

def get_config(key):
    value = config.get(key)
    # 如果值是可调用的，就执行它；否则直接返回
    return value() if callable(value) else value

print(get_config('version'))    # 输出: 1.0
print(get_config('timestamp'))  # 输出: (当前时间戳)
```

## 注意事项

`callable()` 返回 `True` 只意味着对象**具备被调用的接口**，但并不保证调用一定成功。例如，一个函数可能需要特定的参数，如果你调用时参数传错了，仍然会引发异常。

```python
def add(a, b): return a + b

print(callable(add)) # True
# add() # 这行代码虽然 add 是 callable 的，但调用它会因为缺少参数而报错
```

## 总结

`callable()` 是一个用于**运行时检查**的工具。它帮你回答“这个东西能加括号执行吗？”这个问题，在编写需要处理未知对象的通用代码、框架或插件系统时非常有用。

# `iter`函数

`iter()` 是 Python 内置的一个非常重要的函数，它是**迭代器协议的核心入口**。它的主要作用是将一个可迭代对象转换为迭代器。

## 函数的基本用法

### 基本语法

```python
iter(object[, sentinel])  # sentinel 必须在 object 之后
# 所以不能这样调用：
# iter(sentinel=value)  # ❌ 错误！缺少必需参数 object
```

`object`：要转换为迭代器的对象或可调用对象。在双参数时，必须是可调用对象 （callable），会被重复调用以产生值的函数。

`sentinel`：哨兵值，作为停止迭代的标志。

| 场景           | 用法                        | 示例            |
| -------------- | --------------------------- | --------------- |
| 转换可迭代对象 | `iter(obj)`                 | `iter([1,2,3])` |
| 文件按条件读取 | `iter(f.readline, '')`      | 读到文件末尾    |
| 随机数生成     | `iter(random_func, target)` | 生成到特定值    |
| 用户输入       | `iter(input, 'quit')`       | 输入到特定指令  |

### 将可迭代对象转为迭代器

```python
# 对于列表、元组、字符串等可迭代对象
my_list = [1, 2, 3]
my_iterator = iter(my_list)

print(type(my_iterator))  # <class 'list_iterator'>

# 然后可以用 next() 获取元素
print(next(my_iterator))  # 1
print(next(my_iterator))  # 2
print(next(my_iterator))  # 3
# print(next(my_iterator))  # StopIteration 异常
```

### 带哨兵值的调用

这是 `iter()` 的一个特殊用法，用于从可调用对象创建迭代器。

```python
# 从文件逐行读取，直到遇到空行
def read_line():
    try:
        return input("输入一行（输入空行结束）: ")
    except EOFError:
        return ""

# 当 read_line() 返回 "" 时停止迭代
for line in iter(read_line, ""):
    print(f"你输入了: {line}")

# 另一个例子：生成随机数直到等于特定值
import random
counter = 0
def random_num():
    global counter
    counter += 1
    return random.randint(1, 10)

# 当 random_num() 返回 5 时停止
for num in iter(random_num, 5):
    print(f"随机数: {num}, 第{counter}次尝试")
```

## `for` 循环的关系

当你写 `for item in obj` 时，Python 内部实际上做了以下事情：

```python
# 当你写：
for item in my_list:
    print(item)

# Python 内部实际执行：
iterator = iter(my_list)  # 调用 iter(obj)，得到迭代器
while True:
    try:
        item = next(iterator)  # 调用 next(iterator)，获取下一个元素
        print(item)
    except StopIteration:      # 捕获 StopIteration 异常，退出循环
        break
```

## 工作原理

### 对于常规可迭代对象（如列表、字符串等）：

1. 调用对象的 `__iter__()` 方法
2. 返回该方法的结果（应该是一个迭代器）

### 对于不支持 `__iter__()` 但支持 `__getitem__()` 的对象：

1. 尝试通过索引调用 `__getitem__(0)`, `__getitem__(1)`...
2. 直到遇到 `IndexError` 为止

##  实际应用示例

### 手动遍历迭代器

```python
my_list = [1, 2, 3, 4, 5]
my_iter = iter(my_list)

# 手动控制迭代过程
try:
    while True:
        item = next(my_iter)
        if item % 2 == 0:  # 只处理偶数
            print(f"偶数: {item}")
except StopIteration:
    print("遍历完成")
```

###  检查对象是否可迭代

```python
def is_iterable(obj):
    try:
        iter(obj)
        return True
    except TypeError:
        return False

print(is_iterable([1, 2, 3]))    # True
print(is_iterable("hello"))      # True
print(is_iterable(42))           # False
```

### 从生成器函数获取迭代器

```python
def my_generator():
    yield 1
    yield 2
    yield 3

gen = my_generator()  # 生成器本身就是迭代器
print(iter(gen) is gen)  # True，生成器对象既是可迭代对象也是迭代器
```

## 总结

`iter()` 函数是 Python 迭代机制的基础：

- **核心作用**：将可迭代对象转换为迭代器
- **本质**：调用对象的 `__iter__()` 方法
- **连接点**：连接 `for` 循环和底层迭代器协议的桥梁
- **灵活性**：还支持带哨兵值的特殊用法，用于从函数创建迭代器

# `next`函数

`next()` 是 Python 的内置函数，用于从迭代器中获取下一个元素。它是迭代器协议的重要组成部分，与 `iter()` 和 `StopIteration` 异常一起构成了 Python 的迭代机制。

## 函数的基本用法

### 基本语法

```python
next(iterator[, default])
```

###  参数说明

- `iterator`: 必需参数，一个迭代器对象。
- `default`: 可选参数，当迭代器耗尽时返回的默认值。不设置，在耗尽时会报错。

###  基础示例

```python
# 创建一个迭代器
my_list = [1, 2, 3]
my_iter = iter(my_list)

# 使用 next() 获取元素
print(next(my_iter))  # 1
print(next(my_iter))  # 2
print(next(my_iter))  # 3
# print(next(my_iter))  # 如果没有默认值，这里会抛出 StopIteration 异常
```

## 异常处理

###  不提供默认值的情况

```python
my_iter = iter([1, 2, 3])
print(next(my_iter))  # 1
print(next(my_iter))  # 2
print(next(my_iter))  # 3

try:
    print(next(my_iter))  # 抛出 StopIteration 异常
except StopIteration:
    print("迭代器已耗尽")
```

### 提供默认值的情况

```python
my_iter = iter([1, 2, 3])
print(next(my_iter, "END"))  # 1
print(next(my_iter, "END"))  # 2
print(next(my_iter, "END"))  # 3
print(next(my_iter, "END"))  # END (不会抛出异常)
print(next(my_iter, "END"))  # END (持续返回默认值)
```

##  `for` 循环的关系

```python
# for 循环的内部实现
my_list = [1, 2, 3]
my_iter = iter(my_list)

while True:
    try:
        item = next(my_iter)  # for 循环内部就是这么工作的
        print(item)
    except StopIteration:
        break
```

## 实际应用场景

### 手动控制迭代过程

```python
def process_data(data):
    iterator = iter(data)
    
    # 处理第一个元素
    first = next(iterator, None)
    if first is not None:
        print(f"第一个元素: {first}")
    
    # 处理剩余元素
    for item in iterator:  # 注意：这里继续使用同一个迭代器
        print(f"后续元素: {item}")

process_data([10, 20, 30])  # 第一个元素: 10, 后续元素: 20, 后续元素: 30
```

### 从文件中读取数据

```python
# 逐行读取文件
with open('some_file.txt', 'r') as f:
    lines_iter = iter(f.readline, '')  # 读取到空行时停止
    for line in lines_iter:
        print(line.strip())
```

### 处理嵌套结构

```python
def get_first_item(nested_list):
    """获取嵌套列表中第一个存在的元素"""
    for sublist in nested_list:
        item = next(iter(sublist), None)
        if item is not None:
            return item
    return None

nested = [[], [1, 2], [3, 4]]
print(get_first_item(nested))  # 1
```

### 自定义迭代器中的使用

```python
class CountDown:
    def __init__(self, start):
        self.start = start
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.start <= 0:
            raise StopIteration
        self.start -= 1
        return self.start + 1

countdown = CountDown(3)
print(next(countdown))  # 3
print(next(countdown))  # 2
print(next(countdown))  # 1
# print(next(countdown))  # StopIteration
```

## 高级技巧

### 批量获取元素

```python
def take_n(iterator, n):
    """从迭代器中取出最多n个元素"""
    result = []
    for _ in range(n):
        item = next(iterator, StopIteration)  # 使用 StopIteration 作为哨兵
        if item is StopIteration:
            break
        result.append(item)
    return result

my_iter = iter(range(10))
print(take_n(my_iter, 3))  # [0, 1, 2]
print(take_n(my_iter, 3))  # [3, 4, 5]
```

###  安全的头部检查

```python
def safe_head(iterable):
    """安全地获取可迭代对象的第一个元素"""
    iterator = iter(iterable)
    first = next(iterator, None)
    return first

print(safe_head([1, 2, 3]))  # 1
print(safe_head([]))         # None
print(safe_head("hello"))    # 'h'
print(safe_head(""))         # None
```

## 性能考虑

`next()` 是一个轻量级操作，它直接调用迭代器的 `__next__()` 方法，因此性能很好。在需要精确控制迭代过程时，使用 `next()` 比 `for` 循环更灵活。

## 总结

- **核心功能**：从迭代器获取下一个元素。
- **异常处理**：到达结尾时抛出 `StopIteration` 或返回默认值。
- **灵活性**：支持默认值，避免异常处理。
- **效率**：直接调用迭代器的 `__next__` 方法，性能优秀。
- **控制力**：提供比 `for` 循环更精细的迭代控制。

# eval 函数

`eval()` 是 Python 的一个内置函数，它用于将传入的字符串当作有效的 Python 表达式来动态执行并返回结果。使用时直接传入一个包含表达式的字符串即可（例如 `eval("1 + 2")`），当你需要在运行时动态计算数学公式、解析用户输入的简单表达式或处理配置文件中的动态逻辑时，就可以使用它。

## 使用格式

```python
eval(
    expression,  # 必填，一个包含有效 Python 表达式的字符串
    globals=None,  # 可选，一个字典，用于指定表达式执行时的全局命名空间
    locals=None  # 可选，一个字典或映射对象，用于指定表达式执行时的局部命名空间
)
```

## 参数详解

| 参数名称     | 作用说明                                                     | 可选值或类型说明                                             |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `expression` | 指定需要被动态执行的 Python 表达式字符串。                   | 1. **字符串**：例如 `'2 * 3'`、`'len([1,2,3])'` <br />2. **代码对象**：通过 `compile()` 函数编译后的代码对象 |
| `globals`    | 指定表达式执行时的全局变量环境。<br />如果提供，必须是字典类型。 | 1. **字典**：例如 `{'x': 10}` <br />2. **None**（默认值）：使用当前调用 `eval()` 时的全局命名空间 |
| `locals`     | 指定表达式执行时的局部变量环境。                             | 1. **字典或映射对象**：例如 `{'y': 20}` <br />2. **None**（默认值）：使用当前调用 `eval()` 时的局部命名空间 |

## 代码示例

```python
# 基础用法：动态计算数学表达式
result = eval("2 + 3 * 4")
print(result)  # 输出：14

# 结合变量：通过 locals 参数传入变量
x = 10
expression = "x * 2 + 5"
result = eval(expression, None, {"x": x})
print(result)  # 输出：25

# 安全提示：永远不要对用户输入的不可信字符串直接使用 eval()
# 例如 eval(input("请输入: ")) 是极其危险的，可能导致任意代码执行
```

# exec 函数

`exec()` 是 Python 的一个内置函数，它用于将传入的字符串或代码对象当作完整的 Python 代码块（语句）来动态执行。使用时直接传入包含代码的字符串即可（例如 `exec("x = 1")`），当你需要在运行时动态生成并执行复杂的逻辑（如函数定义、循环、条件判断等）时，就可以使用它。

## 使用格式

```python
exec(
    object,  # 必填，一个包含有效 Python 代码的字符串或代码对象
    globals=None,  # 可选，一个字典，用于指定代码执行时的全局命名空间
    locals=None  # 可选，一个字典或映射对象，用于指定代码执行时的局部命名空间
)
```

## 参数详解

| 参数名称  | 作用说明                                                     | 可选值或类型说明                                             |
| --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `object`  | 指定需要被动态执行的 Python 代码。                           | 1. **字符串**：例如 `'x = 1'`、`'def f(): pass'` <br />2. **代码对象**：通过 `compile()` 函数编译后的代码对象 |
| `globals` | 指定代码执行时的全局变量环境。<br />如果提供，必须是字典类型。 | 1. **字典**：例如 `{'x': 10}` <br />2. **None**（默认值）：使用当前调用 `exec()` 时的全局命名空间 |
| `locals`  | 指定代码执行时的局部变量环境。                               | 1. **字典或映射对象**：例如 `{'y': 20}` <br />2. **None**（默认值）：使用当前调用 `exec()` 时的局部命名空间 |

## 简单代码示例

```python
# 基础用法：动态执行赋值语句
exec("x = 10")
print(x)  # 输出：10

# 动态定义并调用函数
code = """
def greet(name):
    return f"Hello, {name}!"
"""
exec(code)
print(greet("Alice"))  # 输出：Hello, Alice!

# 结合命名空间：通过 locals 参数传入变量并获取结果
namespace = {}
exec("result = a + b", None, {"a": 5, "b": 3, **namespace})
# 注意：exec 没有返回值，结果存储在 namespace 中
print(namespace)  # 输出：{'result': 8}
```

# compile 函数

`compile()` 是 Python 的一个内置函数，它用于将源代码字符串或抽象语法树（AST）对象编译为底层的代码对象（字节码）。使用时直接传入代码字符串、文件名标识和编译模式即可（例如 `compile("1+1", "<string>", "eval")`），当你需要预编译代码以提升重复执行的效率、实现动态代码执行或进行元编程（如模板引擎）时，就可以使用它。

## 使用格式

```python
compile(
    source,  # 必填，要编译的源代码（字符串、字节串或 AST 对象）
    filename,  # 必填，源代码所在的文件名（若非文件读取，通常写 '<string>'，用于错误提示）
    mode,  # 必填，指定编译代码的模式（'eval', 'exec' 或 'single'）
    flags=0,  # 可选，控制编译器选项和 future 特性的标志位（默认为 0）
    dont_inherit=False,  # 可选，是否忽略外围代码中的 future 特性和编译器选项（默认为 False）
    optimize=-1  # 可选，编译器的优化级别（默认为 -1，即使用解释器的优化级别）
)
```

## 参数详解

| 参数名称       | 作用说明                                               | 可选值或类型说明                                             |
| -------------- | ------------------------------------------------------ | ------------------------------------------------------------ |
| `source`       | 指定需要被编译的源代码。                               | 1. **字符串**：例如 `'x = 1'` <br />2. **字节串**：例如 `b'x = 1'` <br />3. **AST 对象**：通过 `ast.parse()` 生成的抽象语法树对象 |
| `filename`     | 指定代码的来源文件名，用于在报错时显示位置信息。       | 1. **字符串**：实际文件名（如 `'test.py'`） <br />2. **标识字符串**：非文件来源时通常使用 `'<string>'` |
| `mode`         | 指定编译代码的类型，决定了生成的代码对象如何被执行。   | 1. **'exec'**：用于编译语句序列（如函数定义、循环、多行代码） <br />2. **'eval'**：用于编译单个表达式（如 `1 + 2`） <br />3. **'single'**：用于编译单个交互式语句（执行后若非 None 会自动打印） |
| `flags`        | 控制编译时的额外选项，可通过按位或（``）组合多个标志。 |                                                              |
| `dont_inherit` | 控制是否继承外围代码的编译标志。                       | 1. **False**（默认值）：继承外围代码的 future 特性和编译器选项 <br />2. **True**：仅使用 `flags` 参数指定的选项，忽略外围代码的选项 |
| `optimize`     | 指定编译器的优化级别。                                 | 1. **-1**（默认值）：使用解释器当前的优化级别 <br />2. **0**：无优化（`__debug__` 为 True） <br />3. **1**：删除 assert 断言语句（`__debug__` 为 False） <br />4. **2**：在 1 的基础上，额外删除文档字符串（docstrings） |

## 简单代码示例

```python
# 基础用法：编译并执行单个表达式
code_obj = compile("2 + 3 * 4", "<string>", "eval")
result = eval(code_obj)
print(result)  # 输出：14

# 编译并执行多行语句块
multi_line_code = """
x = 10
y = 20
z = x + y
"""
code_obj = compile(multi_line_code, "<string>", "exec")
exec(code_obj)
print(z)  # 输出：30

# 结合优化：编译时删除 assert 和文档字符串
optimized_code = """
def add(a, b):
    '''这是一个加法函数'''
    assert isinstance(a, int)
    return a + b
"""
code_obj = compile(optimized_code, "<string>", "exec", optimize=2)
exec(code_obj)
print(add.__doc__)  # 输出：None（文档字符串已被优化删除）
```
