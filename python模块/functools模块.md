# 介绍

`functools` 是 Python 标准库中用于**高阶函数**（即操作或返回其他函数的函数）的工具模块。它主要支持函数式编程风格，提供了许多实用的工具来简化函数的包装、缓存、参数绑定等操作，从而提升代码的可复用性和执行效率。

# 缓存与性能优化工具

- `@lru_cache(maxsize=128, typed=False)`

  为函数添加“最近最少使用（LRU）”缓存机制。当函数被重复调用且参数相同时，直接返回缓存结果，避免重复计算，极大提高性能（常用于递归或耗时计算）。

- `@cache`

  Python 3.9+ 新增的简单无界缓存装饰器，相当于 `@lru_cache(maxsize=None)`。

- `@cached_property`

  将类的方法转换为惰性求值属性。首次访问时计算并缓存结果，后续访问直接返回缓存值（常用于计算开销大但结果不变的属性）。

# 偏函数应用（参数绑定）

- `partial(func, \*args, \**kwargs)`

  固定原函数的部分参数，生成一个新的简化函数。例如，若总是用特定编码打开文件，可以固定 `encoding` 参数生成一个新函数。

- `partialmethod(func, \*args, \**kwargs)`

  与 `partial` 类似，但专门用于在类中绑定方法。

# 装饰器与元信息保留

- **`@wraps(wrapped)`**

  在自定义装饰器时，将其加在内部包装函数（wrapper）上。它会自动将原函数的元数据（如 `__name__`、`__doc__`）复制到包装函数上，防止原函数的信息丢失。

- `update_wrapper(wrapper, wrapped)`

  `@wraps` 的底层实现，用于手动更新包装函数的元信息。

# 排序与比较工具

- `cmp_to_key(func)`

  将旧式的两参数比较函数（返回负数、0或正数）转换为现代排序工具（如 `sorted()`、`min()`）可接受的 `key` 函数。

- `@total_ordering`

  类装饰器。只要类中定义了 `__eq__()` 和至少一个其他比较方法（如 `__lt__`），它就能自动补全剩余的所有比较方法（如 `__gt__`、`__le__` 等）。

# 泛函数与累积操作

- `@singledispatch`

  将普通函数转换为泛函数，根据传入的第一个参数的类型，自动分派到不同的实现逻辑（类似于函数重载）。

- `@singledispatchmethod`

  与 `@singledispatch` 类似，但用于类的方法。

- `reduce(func, iterable[, initializer])`

  对可迭代对象进行累积计算。将函数从左到右依次作用于序列元素，最终合并为一个单一值（如计算列表元素的累乘）。

## `reduce` 函数介绍

`reduce` 是 Python 中用于对序列（如列表、元组等）进行**累积计算**的高阶函数。它将序列中的元素按照指定的规则“折叠”成一个单一的结果。

### 使用格式

```python
result = reduce(
    function,      # 一个接收两个参数的函数，用于定义累积计算的规则
    iterable,      # 可迭代对象（如列表、元组等），提供参与计算的数据序列
    initializer=None  # 累积计算的初始值。若提供，则作为第一次计算的第一个参数；若不提供，则使用序列的第一个元素作为初始值
)
```

### 参数详解

| 参数名称      | 作用说明                                                     | 常见可选值或示例                                             |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `function`    | 累积计算函数，必须接收两个参数（通常命名为 `x` 和 `y`）。<br />`x` 代表当前的累积结果，<br />`y` 代表序列中的下一个元素 | 自定义函数、<br />`lambda` 表达式                            |
| `iterable`    | 提供参与计算的数据序列                                       | 列表（`list`）、<br />元组（`tuple`）、<br />字符串（`str`）<br />等可迭代对象 |
| `initializer` | 累积计算的初始值。<br />提供该参数时，计算从 `initializer` 与序列第一个元素开始；<br />不提供时，计算从序列的前两个元素开始。<br />若序列为空且未提供该参数，会抛出 `TypeError` | 任意 Python 对象<br />（如 `0`、`""`、`[]` 等，取决于计算逻辑） |

### 代码示例

以下示例演示了如何使用 `reduce` 进行列表求和以及字符串拼接：

```python
from functools import reduce

# 1. 列表求和（不带初始值）
numbers = [1, 2, 3, 4, 5]
sum_result = reduce(lambda x, y: x + y, numbers)
print(sum_result)  # 输出: 15 (计算过程: ((((1+2)+3)+4)+5))

# 2. 列表求和（带初始值）
sum_with_init = reduce(lambda x, y: x + y, numbers, 10)
print(sum_with_init)  # 输出: 25 (计算过程: (((((10+1)+2)+3)+4)+5))

# 3. 字符串拼接
words = ["Hello", " ", "World", "!"]
sentence = reduce(lambda x, y: x + y, words)
print(sentence)  # 输出: Hello World!

# 要把序列 [1, 3, 5, 7, 9] 变换成整数 13579
reduce(lambda x, y: x * 10 + y, [1, 3, 5, 7, 9]) # 结果：13579
```

# 简单使用示例

```python
import functools

# 1. 使用 partial 创建固定参数的新函数
def power(base, exponent):
    return base ** exponent
# 固定 exponent 为 2，创建一个求平方的新函数
square = functools.partial(power, exponent=2)
print(square(5))  # 输出: 25

# 2. 使用 lru_cache 缓存计算结果
@functools.lru_cache(maxsize=32)
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)
print(fibonacci(30))  # 快速返回结果，避免了大量重复递归

# 3. 使用 reduce 计算累乘
nums = [1, 2, 3, 4]
product = functools.reduce(lambda x, y: x * y, nums)
print(product)  # 输出: 24
```