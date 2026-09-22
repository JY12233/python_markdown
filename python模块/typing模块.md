# 介绍

它的核心作用是为 Python 提供**类型提示**的支持。Python 是一门动态类型语言，变量的类型在运行时才确定。`typing` 模块通过引入一套类型注解系统，让开发者可以在编写代码时，为变量、函数参数和返回值等明确指定预期的类型。

这能带来两大好处：

- 增强代码可读性

  让代码的意图更清晰，方便他人（或未来的自己）理解和维护。

- 提前发现错误

  配合 VS Code、PyCharm 等编辑器的静态类型检查工具（如 Pylance、mypy），可以在代码运行前就发现潜在的类型错误。

# 内容

`typing` 模块提供了丰富的类型工具，可以大致分为以下几类：

## 通用容器类型

用于替代或补充内置的 `list`、`dict` 等，可以指定容器内元素的具体类型。

- `List[T]` / `list[T]`

  表示一个列表，其中所有元素都是 `T` 类型。

- `Dict[K, V]` / `dict[K, V]`

  表示一个字典，键是 `K` 类型，值是 `V` 类型。

- `Tuple[T1, T2]` / `tuple[T1, T2]`

  表示一个元组，可以指定每个位置元素的类型。

- `Set[T]` / `set[T]`

  表示一个集合，其中所有元素都是 `T` 类型。

## 联合与可选类型

用于处理一个变量可能是多种类型之一的情况。

- `Union[T1, T2]`

  表示一个值可以是 `T1` 类型，也可以是 `T2` 类型。在 Python 3.10+ 中，可以用 `T1 | T2` 的简洁写法。

- `Optional[T]`

  这是 `Union[T, None]` 的简写，表示一个值可以是 `T` 类型，也可以是 `None`。

## Callable 与泛型

用于定义函数类型和更复杂的泛型逻辑。

- `Callable[[Arg1Type, Arg2Type], ReturnType]`

  用于注解一个函数或方法。方括号内是参数类型列表，后面是返回值类型。

  > Python 3.9 以上推荐使用 `collections.abc.Callable`。

- `TypeVar('T')`

  用于创建泛型类型变量，让你可以编写能处理多种类型的通用函数或类。

- `Any`

  表示可以是任意类型。使用它会关闭对该变量的类型检查。

- `NoReturn`

  用于注解一个永远不会正常返回的函数（例如，总是抛出异常或进入死循环）。

# 示例

下面通过几个例子来展示 `typing` 模块中这些工具的实际用法。

## 使用 `List` 和 `Dict`

```python
from typing import List, Dict

# 定义一个函数，它接收一个字符串列表，返回一个字典
def count_words(words: List[str]) -> Dict[str, int]:
    word_count = {}
    for word in words:
        word_count[word] = word_count.get(word, 0) + 1
    return word_count

# 调用函数
my_words = ["apple", "banana", "apple"]
result = count_words(my_words)
# 编辑器会知道 result 是一个 Dict[str, int] 类型
```

## 使用 `Union` 和 `Optional`

```python
from typing import Union, Optional

# 定义一个函数，参数可以是字符串或整数
def process_id(user_id: Union[str, int]) -> str:
    return f"Processing ID: {user_id}"

# Optional[str] 等价于 Union[str, None]
def greet(name: Optional[str] = None) -> str:
    if name is None:
        return "Hello, Guest!"
    return f"Hello, {name}!"
```

## 使用 `Callable` 和 `TypeVar`

这个例子展示了如何编写一个更通用、更强大的函数，就像你之前优化的二分查找一样。

```python
from typing import Callable, List, TypeVar

# 1. 创建一个类型变量 T，它可以代表任何类型
T = TypeVar('T')

# 2. 定义一个高阶函数，它接收一个列表和一个函数作为参数
#    func 参数是一个 Callable，它接收一个 T 类型的值，并返回一个 U 类型的值
U = TypeVar('U')
def apply_func_to_list(items: List[T], func: Callable[[T], U]) -> List[U]:
    return [func(item) for item in items]

# 使用示例
numbers = [1, 2, 3]
# 将每个数字转换为字符串
str_numbers = apply_func_to_list(numbers, str) 
# 编辑器知道 str_numbers 的类型是 List[str]
```

# `Generic`

> 自定义泛型类。

`TypeVar` 是用来定义泛型函数或方法的，而 `Generic` 基类则是用来创建**泛型类**的。当你希望一个类的某些属性或方法能灵活地处理多种类型时，就需要它。

> 现在一般直接在类后面写 `[T]` 就行了，比如 `class ABC[T]`。

- 作用

  让你的类支持类型参数，就像 `List[T]` 或 `Dict[K, V]` 一样。

- 常见场景

  构建可复用的数据结构，如栈、队列、链表等。

```python
from typing import Generic, TypeVar

T = TypeVar('T')

class Stack(Generic[T]):
    def __init__(self) -> None:
        self._items: list[T] = []

    def push(self, item: T) -> None:
        self._items.append(item)

    def pop(self) -> T:
        return self._items.pop()

# 使用时，可以指定类型
int_stack = Stack[int]()
int_stack.push(1)
# int_stack.push("a")  # 类型检查器会报错

str_stack = Stack[str]()
str_stack.push("hello")
```

# `Literal`

> 精确的字面量类型。

`Literal` 允许你指定一个变量或参数必须是某个或某些**具体的值**，而不是一个宽泛的类型。

- 作用

  将类型约束到精确的字面量，如特定的字符串或数字。

- 常见场景

  定义配置项、API 参数、状态码等，确保传入的值是预定义的选项之一。

```python
from typing import Literal

# 定义一个函数，mode 参数只能是 "r", "w", 或 "a"
def open_file(filename: str, mode: Literal["r", "w", "a"]) -> None:
    print(f"Opening {filename} in {mode} mode")

open_file("data.txt", "r")   # ✅ 正确
# open_file("data.txt", "x") # ❌ 类型检查器会报错，"x" 不是允许的字面量
```

# `Final`

> 定义常量。

`Final` 用来声明一个变量或属性不应该被重新赋值。它相当于告诉类型检查器：“这是一个常量”。

- 作用

  标记不应被修改的变量。

- 常见场景

  定义全局常量、类级别的常量。

```python
from typing import Final

# 声明一个常量
MAX_CONNECTIONS: Final = 100

# MAX_CONNECTIONS = 200  # ❌ 类型检查器会发出警告，不应重新赋值

class HTTPClient:
    # 声明一个类常量
    DEFAULT_TIMEOUT: Final[int] = 5
```

# `TypedDict`

> 为字典定义精确结构。

当你有一个字典，并且明确知道它应该包含哪些键，以及每个键对应什么类型的值时，`TypedDict` 就非常有用。它比 `Dict[str, Any]` 要精确得多。

- 作用

  为字典的“结构”进行类型注解，指定键名和对应值的类型。

- 常见场景

  处理 JSON 数据、函数返回一个结构固定的字典、Django 视图中的 `request.GET` 等。

```python
from typing import TypedDict

class Movie(TypedDict):
    name: str
    year: int
    rating: float

# 现在，movie 变量必须符合 Movie 定义的结构
movie: Movie = {
    "name": "Inception",
    "year": 2010,
    "rating": 8.8
}

# movie["director"] = "Nolan" # ❌ 类型检查器会报错，'director' 不是 Movie 中定义的键
```

如果某些键可以不需要，则可以使用 `NotRequired` 类。

# `Protocol`

> 实现“鸭子类型”的结构化子类型。

`Protocol` 是实现**结构化子类型**（也叫静态鸭子类型）的强大工具。它允许你定义一个“接口”，任何拥有该接口所规定的方法和属性的类，都被视为实现了该协议，而无需显式继承。

- 作用

  定义行为规范，而不是继承关系。只要一个类“看起来像鸭子，走起来像鸭子”，它就是鸭子。

- 常见场景

- 编写高度解耦和可复用的函数，尤其是在你无法控制第三方类的继承体系时。

```python
from typing import Protocol

class Drawable(Protocol):
    def draw(self) -> None:
        ...

class Circle:
    def draw(self) -> None:
        print("Drawing a circle")

class Square:
    def draw(self) -> None:
        print("Drawing a square")

# 这个函数不关心传入的对象是 Circle 还是 Square
# 只要它有 draw 方法，类型检查器就认为它是合法的 Drawable
def render(shape: Drawable) -> None:
    shape.draw()

render(Circle()) # ✅ 正确
render(Square()) # ✅ 正确
```

# 其他常见类型别名

为了代码更简洁，`typing` 模块还定义了一些常用的类型别名：

- **`Iterable[T]`**

  表示任何可以被迭代的对象，比如 `list`，`tuple`，`str`，`dict` 等。比 `List[T]` 更通用。

- **`Sequence[T]`**

  表示一个只读的序列，比如 `list` 和 `tuple`。它比 `Iterable` 多了通过索引访问元素的能力。

- **`Mapping[K, V]`**

  表示一个只读的映射（类似字典），比如 `dict`。它比 `Dict` 更通用。

# 补充说明

从 Python 3.9 开始，许多内置的集合类型（如 `list`, `dict`, `tuple`）本身就支持泛型语法了。因此，推荐在新版本中直接使用它们，而不再需要从 `typing` 模块导入。

| Python < 3.9 (旧写法)                                        | Python >= 3.9 (新写法)                  |
| ------------------------------------------------------------ | --------------------------------------- |
| `from typing import List` `def func(nums: List[int]): ...`   | `def func(nums: list[int]): ...`        |
| `from typing import Dict` `def func(info: Dict[str, int]): ...` | `def func(info: dict[str, int]): ...`   |
| `from typing import Tuple` `def func(point: Tuple[int, int]): ...` | `def func(point: tuple[int, int]): ...` |

# 参数类型注解：函数

当你需要为一个接收“函数”作为参数的函数写类型注解时，你需要使用 `typing` 模块中的 **`Callable`** 类型。

`Callable` 就像是一个“函数说明书”，它告诉类型检查工具：这个参数必须是一个可调用的函数，并且规定了它接收什么类型的参数以及返回什么类型的值。

> callable--adj. 能够被调用的，可以被呼叫的
>
> - **作为内置函数 `callable()`**：
>   它在问：“嘿，这个东西，它**具备被调用的能力**吗？”（即它实现了 `__call__` 方法吗？）。如果返回 `True`，就意味着它签了这份“契约”，你可以放心地用 `()` 去操作它。
> - **作为类型注解 `typing.Callable`**：
>   它是在写说明书：“这个参数**必须是一个具备调用能力的东西**”。

## `Callable` 的基本语法

`Callable` 的语法结构非常直观：

```python
Callable[[参数类型1, 参数类型2, ...], 返回值类型]
```

- **第一部分 `[...]`**：一个列表，里面按顺序填写这个函数参数所需的类型。如果没有参数，就留空 `[]`。
- **第二部分 `返回值类型`**：这个函数执行后会返回什么类型的数据。

## 代码示例

假设你有一个函数 `process_data`，它接收一个整数列表和一个处理函数，这个处理函数负责把一个整数变成字符串。

```python
from typing import Callable, List

def process_data(
    numbers: List[int], 
    # 这里定义了一个函数类型的参数
    # 它必须接收一个 int，并返回一个 str
    processor: Callable[[int], str]
) -> List[str]:
    """使用 processor 函数处理列表中的每个数字"""
    return [processor(num) for num in numbers]

# 定义一个符合要求的函数
def int_to_str(num: int) -> str:
    return f"数字: {num}"

# 正确调用
result = process_data([1, 2, 3], int_to_str)
print(result) # 输出: ['数字: 1', '数字: 2', '数字: 3']
```

## 更多场景示例

### 接收一个无参数、返回布尔值的函数

这种场景常用于回调或条件检查。

```python
from typing import Callable

def check_status(is_ready: Callable[[], bool]) -> None:
    if is_ready():
        print("准备就绪！")
    else:
        print("尚未就绪。")

def my_check() -> bool:
    return True

check_status(my_check)
```

### 接收一个带多个参数的函数

比如一个通用的计算函数。

```python
from typing import Callable

def calculate(
    a: int, 
    b: int, 
    # 接收一个需要两个 int 参数，并返回一个 int 的函数
    operation: Callable[[int, int], int]
) -> int:
    return operation(a, b)

def add(x: int, y: int) -> int:
    return x + y

result = calculate(5, 3, add)
print(result) # 输出: 8
```

# 泛型

`TypeVar` 是 Python `typing` 模块中用于定义泛型类型变量的核心工厂函数。使用时通过 `T = TypeVar('T')` 创建类型变量，并将其作为类型注解应用于函数或类中。能够处理多种数据类型，同时保持输入与输出类型一致性的通用代码（如通用容器、算法函数）。

## 创建对象时的格式

```python
TypeVar(
    name,  # 必填，类型变量的名称（字符串），通常与赋值变量名保持一致
    *constraints,  # 可选，一个或多个类型，限制类型变量只能是这些具体类型之一
    bound=None,  # 可选，一个类型，限制类型变量必须是该类型或其子类
    covariant=False,  # 可选，布尔值，声明该类型变量是否为协变
    contravariant=False,  # 可选，布尔值，声明该类型变量是否为逆变
    default=NoDefault  # 可选，指定类型变量的默认类型（Python 3.13+ 新增）
)
```

## 参数详解

| 参数名称        | 作用说明                                           |
| --------------- | -------------------------------------------------- |
| `name`          | 指定类型变量的名称，用于错误提示和调试。           |
| `*constraints`  | 限定类型变量只能是指定的具体类型之一。             |
| `bound`         | 设定类型变量的上界，实际类型必须是该类型的子类。   |
| `covariant`     | 声明类型变量为协变，通常用于只读容器的返回值位置。 |
| `contravariant` | 声明类型变量为逆变，通常用于函数的参数输入位置。   |
| `default`       | 指定当未提供具体类型时，类型变量默认绑定的类型。   |

## 常用属性与常用方法

| 属性/方法名称       | 说明                                                         |
| ------------------- | ------------------------------------------------------------ |
| `__name__`          | 返回创建 `TypeVar` 时传入的名称字符串（例如 `'T'`）。        |
| `__constraints__`   | 返回一个元组，包含所有通过 `*constraints` 传入的约束类型。   |
| `__bound__`         | 返回通过 `bound` 参数设置的上界类型，若未设置则返回 `None`。 |
| `__covariant__`     | 返回布尔值，表示该类型变量是否被声明为协变。                 |
| `__contravariant__` | 返回布尔值，表示该类型变量是否被声明为逆变。                 |
| `__default__`       | 返回设置的默认类型；若未设置，返回 `typing.NoDefault`（Python 3.13+）。 |
| `has_default()`     | 检查该类型变量是否设置了默认类型，返回布尔值（Python 3.13+）。 |

## 代码示例

```python
from typing import TypeVar, List, Optional

# 1. 定义一个类型变量 T
# 它就像一个占位符，代表某种具体的类型
T = TypeVar('T')

# 2. 编写泛型函数
# 在参数和返回值中使用 T，表示它们必须是同一种类型
def get_first_item(items: List[T]) -> Optional[T]:
    """
    获取列表的第一个元素，如果列表为空则返回 None
    """
    if items:
        return items[0]
    return None

# --- 测试与类型推断演示 ---

# 场景 A：传入整数列表
int_list = [10, 20, 30]
first_int = get_first_item(int_list)
# 此时，类型检查器能精准推断出 first_int 的类型是 Optional[int]
print(first_int)  # 输出：10

# 场景 B：传入字符串列表
str_list = ["apple", "banana", "cherry"]
first_str = get_first_item(str_list)
# 此时，类型检查器能精准推断出 first_str 的类型是 Optional[str]
print(first_str)  # 输出：apple

# 场景 C：传入空列表
empty_list: List[float] = []
first_empty = get_first_item(empty_list)
# 即使列表为空，类型检查器依然知道 first_empty 的类型是 Optional[float]
print(first_empty)  # 输出：None
```

# 协变和逆变

## 定义

协变（Covariance）与逆变（Contravariance）是泛型类型系统中的核心概念，用于描述**子类型关系在泛型容器或复合类型中如何传递**。

- 前提设定

  在面向对象继承体系中，`Dog` 是 `Animal` 的子类（即 `Dog` 比 `Animal` 更具体）。

- 协变（Covariance）

  子类型关系**保持一致**。如果 `Dog` 是 `Animal` 的子类，那么 `Container[Dog]` 也被视为 `Container[Animal]` 的子类。

- 逆变（Contravariance）

  子类型关系**完全反转**。如果 `Dog` 是 `Animal` 的子类，那么 `Handler[Animal]` 反而被视为 `Handler[Dog]` 的子类。

- 不变（Invariant）

  子类型关系**不传递**。`Container[Dog]` 与 `Container[Animal]` 没有任何子类型关系，必须严格匹配。

## 适用场景

变体类型的存在是为了在保证类型安全的前提下，提升代码的灵活性。

1. 协变（只读/生产者）

   - 场景

     适用于**只读容器**或**返回值**位置。

   - 原理

     如果一个容器只能从中“读取”数据，那么将 `List[Dog]` 当作 `List[Animal]` 使用是绝对安全的，因为读出来的永远是 `Dog`（也是 `Animal`）。

   - Python 示例

     `typing.Sequence[T]`、`typing.Iterable[T]` 是协变的。

2. 逆变（写入/消费者）

   - 场景

     适用于**函数参数**或**回调/处理器**位置。

   - 原理

     如果一个函数能处理通用的 `Animal`，那么它肯定也能处理更具体的 `Dog`。因此，接受 `Animal` 的处理器可以安全地替代接受 `Dog` 的处理器。

   - Python 示例

     `typing.Callable[[T], None]` 在参数位置是逆变的。

3. 不变（可变容器）

   - 场景

     适用于**可读可写**的可变容器。

   - 原理

     如果允许将 `List[Dog]` 当作 `List[Animal]`，调用者就可以往里面塞入一只 `Cat`。这会导致后续从列表中取出元素时，原本期望是 `Dog` 却得到了 `Cat`，引发类型错误。因此，可变容器必须是不变的。

   - Python 示例

     `typing.List[T]`、`typing.Dict[K, V]` 是不变的。

## 对比表

| 变体类型 | 子类型推导方向                                               | 典型应用场景         | 核心特征               |
| -------- | ------------------------------------------------------------ | -------------------- | ---------------------- |
| **协变** | `Dog` → `Animal` 推导 `Container[Dog]` → `Container[Animal]` | 返回值、只读容器     | 同向，只读安全         |
| **逆变** | `Dog` → `Animal` 推导 `Handler[Animal]` → `Handler[Dog]`     | 函数参数、回调处理器 | 反向，写入安全         |
| **不变** | 无推导关系                                                   | 可变列表、可变字典   | 读写都有，必须严格匹配 |

## 代码示例

在 Python 的 `typing` 模块中，可以通过 `TypeVar` 显式声明变体行为。

### 声明方式

- 协变：`T_co = TypeVar('T_co', covariant=True)`
- 逆变：`T_contra = TypeVar('T_contra', contravariant=True)`

### 协变容器

```python
from typing import TypeVar, Generic, Iterator

# 声明一个协变类型变量
T_co = TypeVar('T_co', covariant=True)

class ReadOnlyBox(Generic[T_co]):
    def __init__(self, value: T_co) -> None:
        self._value = value

    def get_value(self) -> T_co:
        return self._value

# 使用示例
class Animal: pass
class Dog(Animal): pass

dog_box: ReadOnlyBox[Dog] = ReadOnlyBox(Dog())
# 协变允许将 ReadOnlyBox[Dog] 赋值给 ReadOnlyBox[Animal]
animal_box: ReadOnlyBox[Animal] = dog_box 
```

### 逆变处理器

```python
from typing import TypeVar, Generic

# 声明一个逆变类型变量
T_contra = TypeVar('T_contra', contravariant=True)

class Handler(Generic[T_contra]):
    def handle(self, item: T_contra) -> None:
        print(f"Handling {item}")

# 使用示例
class AnimalHandler(Handler[Animal]):
    def handle(self, item: Animal) -> None:
        print(f"Handling Animal: {item}")

animal_handler = AnimalHandler()
# 逆变允许将 Handler[Animal] 赋值给 Handler[Dog]
dog_handler: Handler[Dog] = animal_handler 
```

# Annotated

`Annotated` 是 Python `typing` 模块中用于为类型提示附加元数据（Metadata）的工厂类。使用时需通过 `Annotated[类型, 元数据1, 元数据2, ...]` 的格式将基础类型与任意数量的附加信息绑定。

当需要为变量或函数参数添加校验规则、文档说明、依赖注入等结构化上下文，且这些上下文需被第三方框架（如 FastAPI、Pydantic）或静态分析工具读取时，就可以使用该类。

## 创建对象时的格式

```python
Annotated(
    type,  # 必填，基础数据类型（如 int、str、list 等）
    *metadata  # 必填，一个或多个任意 Python 对象作为元数据（如字符串、Field 实例、Depends 对象等）
)
```

## 参数详解

| 参数名称    | 作用说明                                                     | 可选值或类型说明                                             |
| ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `type`      | 指定被注解的基础数据类型，类型检查器仅依据此参数进行静态类型校验。 | **任意有效类型**：<br />例如 `int`、`str`、`list[int]`、`Optional[str]` |
| `*metadata` | 附加的元数据信息，Python 运行时完全忽略，仅供第三方库或框架通过 `__metadata__` 属性读取。 | **任意 Python 对象**：<br />例如 `"这是价格"`、`Field(ge=0)`、`Depends(get_db)` |

## 常用属性与方法

| 属性/方法名称  | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| `__origin__`   | 返回 `Annotated` 中绑定的基础类型<br />（例如 `Annotated[int, "age"].__origin__` 返回 `<class 'int'>`） |
| `__metadata__` | 返回一个元组，包含所有附加的元数据对象<br />（例如 `Annotated[int, "age", "years"].__metadata__` 返回 `('age', 'years')`） |
| `__args__`     | 返回一个元组，包含基础类型和所有元数据（即 `__origin__` 与 `__metadata__` 的合并） |

## 代码示例

以下是一个结合 `Annotated` 与 Pydantic `Field` 的实战代码示例，展示了如何使用 `Annotated` 为字段添加校验规则与文档说明。

```python
from typing import Annotated
from pydantic import BaseModel, Field

# 1. 使用 Annotated 定义带有校验规则和文档说明的字段类型
# 将 int 类型与 Field 实例绑定，实现年龄字段的自动校验
Age = Annotated[int, Field(ge=0, le=150, description="用户年龄，必须在 0 到 150 之间")]

# 2. 在 Pydantic 模型中直接使用定义好的 Annotated 类型
class User(BaseModel):
    name: str
    age: Age  # 等价于 age: Annotated[int, Field(ge=0, le=150, description="...")]

# 3. 测试正常数据
user1 = User(name="Alice", age=25)
print(user1)  # 输出: name='Alice' age=25

# 4. 测试非法数据（触发校验错误）
try:
    user2 = User(name="Bob", age=200)
except Exception as e:
    print(e)
    # 输出: 1 validation error for User
    # age
    #   Input should be less than or equal to 150 [type=less_than_equal, ...]
```

1. 类型与元数据的绑定

   `Annotated[int, Field(...)]` 将基础类型 `int` 与 Pydantic 的校验规则 `Field` 绑定在一起。

2. 框架自动读取

   Pydantic 在解析 `User` 模型时，会自动读取 `Annotated` 中的 `Field` 元数据，并据此生成校验逻辑和 JSON Schema 文档。

3. 代码复用

   通过将 `Annotated` 类型赋值给变量（如 `Age`），可以在多个模型中复用同一套校验规则，避免重复编写 `Field`。