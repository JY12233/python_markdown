# 模块作用

`abc` 模块（Abstract Base Classes）提供了在 Python 中定义**抽象基类**的基础组件。它的核心功能是允许开发者定义接口规范，强制子类必须实现特定的抽象方法或属性。如果子类没有实现所有的抽象方法，Python 会阻止该子类的实例化，从而确保代码遵循统一的接口契约。

# 包含的类

- **`abc.ABC`**

  一个辅助类，使用 `ABCMeta` 作为元类。通过简单地从 `ABC` 派生，即可快速创建一个抽象基类，避免了直接使用元类可能带来的混淆。

- **`abc.ABCMeta`**

  用于定义抽象基类的元类。它负责追踪哪些方法是抽象的，并确保在抽象方法未被完全实现时，类无法被实例化。

# 常用属性与方法

| 名称                         | 类型   | 作用说明                                                     |
| ---------------------------- | ------ | ------------------------------------------------------------ |
| `@abstractmethod`            | 装饰器 | 用于声明抽象方法。<br />要求类的元类必须是 `ABCMeta` 或其派生类。<br />拥有此元类的类，除非重写了所有的抽象方法和属性，否则无法被实例化。 |
| `register(subclass)`         | 方法   | 将 `subclass` 注册为该抽象基类的“虚拟子类”。<br />注册后，`issubclass()` 和 `isinstance()` 会将其识别为子类，<br />但该抽象基类不会出现在其 MRO（方法解析顺序）中。 |
| `__subclasshook__(subclass)` | 类方法 | 用于自定义 `issubclass()` 的行为。<br />通过检查 `subclass` 是否实现了特定的接口（如包含某个方法），<br />来决定是否将其视为该抽象基类的子类，从而无需逐个调用 `register()`。 |
| `get_cache_token()`          | 函数   | 返回当前抽象基类的缓存令牌。<br />该令牌是一个不透明对象，每次调用 `ABCMeta.register()` 时都会发生更改，<br />可用于检测注册操作是否发生。 |

# `ABC` 与 `ABCMeta` 的区别

在 Python 的 `abc` 模块中，这两者的核心区别在于它们的**本质角色**不同。

- `ABCMeta` 是元类（Metaclass）

  它是真正在底层干活的“幕后大佬”。它负责拦截类的创建过程，追踪哪些方法被标记为了抽象方法，并强制规定：只要类中还有未实现的抽象方法，就绝对不允许实例化该类。

- `ABC` 是普通的基类（Base Class）

  它仅仅是一个“语法糖”。它的内部只有一行代码，即指定自己的元类为 `ABCMeta`，`class ABC(metaclass=ABCMeta): pass`。它存在的唯一目的，就是让开发者在定义抽象类时，不需要去理解复杂的元类概念，只需简单地继承 `ABC` 即可。

`ABCMeta` 是提供抽象机制的“引擎”，而 `ABC` 是方便开发者直接使用的“方向盘”。

# 代码示例

这个示例将 `ABCMeta`（强制继承）、`register`（虚拟子类）以及 `__subclasshook__`（鸭子类型检测）串联起来，直观展示它们的区别。

```python
from abc import ABC, ABCMeta, abstractmethod

# 1. 使用 ABC 定义一个标准的抽象基类（底层使用的是 ABCMeta）
class Animal(ABC):
    # 强制子类必须实现的方法
    # 子类的返回类型和参数设置必须与父类一样
    @abstractmethod
    def speak(self) -> str:
        return ""

# 2. 演示强制继承（必须老老实实继承并实现方法）
class Dog(Animal):
    def speak(self):
        return "汪汪汪"

dog = Dog()
print(f"Dog 实例化成功: {dog.speak()}")

# 3. 演示 register（虚拟子类，不需要实现任何方法，也能通过 isinstance 检查）
class Rock:
    pass

Animal.register(Rock)
rock = Rock()
print(f"Rock 是 Animal 的子类吗？ {isinstance(rock, Animal)}")  # 输出: True
print(f"Rock 的 mro 顺序：{Rock.mro()}")  # 没有 Animal
# Rock 的 mro 顺序：[<class '__main__.Rock'>, <class 'object'>]




# 4. 演示 __subclasshook__（鸭子类型，只要实现了 fly 方法，就被认为是 Flyable）
# 定义一个自定义元类
class Flyable(ABC):
    @classmethod
    def __subclasshook__(cls, C):
        # 如果 C 实现了 fly 方法，我们就认为它是 Flyable 的子类
        # 对 C 的所有的 mro 中的类，把属性和方法转化为字典，然后检查 fly 是不是在字典中
        has_fly = any("fly" in B.__dict__ for B in C.__mro__)
        if has_fly:
            return True
        return NotImplemented

class Bird:
    def fly(self):
        return "在天上飞"

class Stone:
    pass

print(f"Bird 是 Flyable 的子类吗？ {issubclass(Bird, Flyable)}")  # 输出: True
print(f"Stone 是 Flyable 的子类吗？ {issubclass(Stone, Flyable)}")  # 输出: False
```

# 核心机制总结

- **`ABC` + `@abstractmethod`**

  属于**强契约**。子类必须显式继承，并且必须实现所有抽象方法，否则无法实例化。

- **`register`**

  属于**弱契约（虚拟子类）**。不需要继承，也不需要实现任何方法。它仅仅是为了在类型检查（`isinstance`）时“骗过”解释器，常用于为第三方库的类打上标签。

- **`__subclasshook__`**

  属于**鸭子类型契约**。它完全打破了继承树，只要你的类“长得像”（实现了特定方法），Python 就认为你是它的子类。这在标准库（如 `collections.abc.Iterable`）中被广泛使用。