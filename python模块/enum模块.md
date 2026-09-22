# 概述

`enum` 是 Python 标准库中用于定义和管理枚举类型的模块。使用时需从该模块导入 `Enum` 基类并继承它来创建枚举类，当需要定义一组固定的、具有语义化名称的常量（如状态码、星期、颜色等）以替代魔法数字或字符串时，就可以使用该模块。

## 核心作用

1. 定义常量集合

   将一组相关的常量组织为一个枚举类，避免使用分散的字符串或整数，提高代码的可读性和可维护性。

2. 类型安全

   枚举成员具有唯一性，防止意外赋值或比较错误，支持严格的类型检查。

3. 支持迭代与比较

   枚举类是可迭代的，可以遍历所有成员；枚举成员支持身份比较（`is`）和相等比较（`==`）。

4. 防止拼写错误

   使用枚举成员名称替代硬编码字符串，IDE 可以提供自动补全，避免逻辑错误。

5. 不可变性

   枚举成员一旦定义，其值不可修改，也不能动态添加新成员。

## 核心类与工具

1. `Enum`：创建枚举的基类。
2. `IntEnum`：枚举成员同时也是 `int` 的子类，可以直接与整数进行比较和运算。
3. `Flag`：支持位运算（如 `&`、`|`、`^`、`~`）的枚举，适用于权限控制等场景。
4. `IntFlag`：同时支持位运算和整数运算的枚举。
5. `StrEnum`（Python 3.11+）：枚举成员同时也是 `str` 的子类，支持字符串操作。
6. `auto`：用于自动分配枚举值（默认从 1 开始递增）。
7. `unique`：装饰器，用于确保枚举成员的值唯一，若有重复值会抛出 `ValueError`。

## 常用属性与方法

| 属性/方法名称 | 说明                                           |
| ------------- | ---------------------------------------------- |
| `.name`       | 返回枚举成员的名称（字符串）                   |
| `.value`      | 返回枚举成员绑定的值                           |
| `__members__` | 返回一个包含所有枚举成员的有序字典（包括别名） |

## 常用操作与内置方法

| 操作/方法                  | 说明                                     |
| -------------------------- | ---------------------------------------- |
| `EnumClass.MEMBER`         | 通过点号访问枚举成员                     |
| `EnumClass['MEMBER']`      | 通过字符串名称访问枚举成员               |
| `EnumClass(value)`         | 通过值访问枚举成员                       |
| `for member in EnumClass:` | 遍历枚举类中的所有成员（不包含别名）     |
| `member1 == member2`       | 比较两个枚举成员是否相等                 |
| `member1 is member2`       | 比较两个枚举成员是否为同一个对象（推荐） |

## 代码示例

```py
from enum import Enum, auto, unique, IntEnum, Flag

# 1. 基础枚举：定义一组固定的常量
class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3

# 2. 自动赋值：使用 auto() 让 Python 自动分配值
class Status(Enum):
    PENDING = auto()  # 自动赋值为 1
    RUNNING = auto()  # 自动赋值为 2
    FINISHED = auto() # 自动赋值为 3

# 3. 唯一性校验：使用 @unique 装饰器确保值不重复
@unique
class Direction(Enum):
    NORTH = 1
    SOUTH = 2
    # EAST = 1  # 如果取消注释，会抛出 ValueError，因为值 1 已存在

# 4. 整数枚举：成员可以直接与整数比较和运算
class Priority(IntEnum):
    LOW = 1
    MEDIUM = 2
    HIGH = 3

# 5. 位标志枚举：支持按位或（|）、按位与（&）等运算，常用于权限控制
class Permission(Flag):
    READ = auto()    # 1
    WRITE = auto()   # 2
    EXECUTE = auto() # 4

# --- 常用方法与属性演示 ---

# 访问枚举成员
print(Color.RED)             # 输出: Color.RED
print(Color['GREEN'])        # 通过字符串名称访问，输出: Color.GREEN
print(Color(3))              # 通过值访问，输出: Color.BLUE

# 获取名称和值
member = Color.RED
print(member.name)           # 输出: RED
print(member.value)          # 输出: 1

# 遍历枚举
print(list(Status))          # 输出: [<Status.PENDING: 1>, <Status.RUNNING: 2>, <Status.FINISHED: 3>]

# 整数枚举的比较
print(Priority.HIGH == 3)    # 输出: True
print(Priority.LOW < PriorAity.HIGH) # 输出: True

# 位标志枚举的运算
user_perm = Permission.READ | Permission.WRITE  # 组合权限
print(Permission.READ in user_perm)             # 输出: True
print(Permission.EXECUTE in user_perm)          # 输出: False
```

