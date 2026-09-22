# 块

## 介绍

模块是python程序的基本组织单位，一个.py文件就是一个模块。在模块中可以定义变量、函数、类以及可执行的代码。

模块分为官方模块和自定义模块。

使用模块需要导入，使用`import`。

## 导入模块

```python
import 模块名 # 同时导入多个使用逗号分隔
import 模块名 as 别名
from 模块名 import 功能名
from 模块名 import 功能名 as 别名
from 模块名 import * # 导入模块所有功能
```

## 自定义模块

自己写的.py文件可以使用import导入，必须在同一个文件夹。

导入模块时不仅会导入里面的方法，还会执行里面的代码。

## 包

1. 介绍

   本质就是一个文件夹，可包含若干python模块，还会包含一个`__init__.py`文件，用于描述包的信息。

2. 导入

   与导入模块时的语法同，一样地，需要在同级文件夹。

   ```python
   import 包名.模块名
   from 包名 import 模块名
   from 包名 import * # 导入所有模块
   from 包名.模块名 import 功能名
   from 包名.模块名 import * # 导入所有功能，需要在__init__.py文件中添加__all__
   ```

   使用绝对路径就可以不在同级文件夹。

   ```python
   文件夹.包
   ```

# 特殊符号

>`'\r'`：让光标回到行首，这样打印出来的内容能覆盖之前的内容

```python
import os
import time
import random

# 1. 清屏
os.system('cls' if os.name == 'nt' else 'clear')

print("=== 系统正在优化中... ===")

# 2. 模拟进度条
for i in range(101):
    # 随机停顿 0.01 到 0.05 秒，模拟不稳定的速度
    time.sleep(random.uniform(0.01, 0.05))
    
    # \r 的意思是光标回到行首，这样数字会在原地跳动，而不是换行
    # // 2 的目的是减小打印的长度，因为系统端的大小是固定的，超过一定长度直接跳到下一行
    print(f"进度: {i}% [{'=' * (i//2)}{' ' * (50 - i//2)}]", end='\r')

print("\n优化完成！✅")
```

# 运算符号

>`// `：整除
>
>`% `：取余
>
>`+=` ：加上一个数（直接修改原数）
>
>`-=` ：减去一个数（直接修改原数）

# 三元运算符

```python
结果1 if condition else 结果二
```

`condition`为真，则返回结果一，否则返回结果二。

# 海象运算符`:=`

1. 正式名称

   “赋值表达式”（Assignment Expression）。

2. 特点

   海象运算符的主要作用是**在表达式内部进行赋值**。这与传统的赋值运算符 `=` 不同，`=` 是一个独立的语句，不能出现在表达式中。

3. 应用

   - 避免重复运算

     ```python
     # 旧方法：可能需要重复计算
     data = [2, 8, 4, 10, 6]
     # 想要找出平方大于50的数字
     large_squares = []
     for num in data:
         square = num * num  # 重复计算
         if square > 50:
             large_squares.append(square)
     
     # 新方法：使用海象运算符
     large_squares = []
     for num in data:
         if (square := num * num) > 50:
             large_squares.append(square)
     ```

   - 正则表达式

     ```python
     import re
     
     text = "年龄是25岁，身高175厘米"
     pattern = r'(\d+)'
     
     # 旧方法
     match = re.search(pattern, text)
     if match:
         age = match.group(1)
         print(f"找到数字: {age}")
     
     # 新方法
     if (match := re.search(pattern, text)):
         age = match.group(1)
         print(f"找到数字: {age}")
     ```

   - while循环

     ```python
     # 旧方法：需要重复调用
     while True:
         line = input("输入一行（输入'quit'退出）: ")
         if line == 'quit':
             break
         print(f"你输入了: {line}")
     
     # 新方法：更简洁
     while (line := input("输入一行（输入'quit'退出）: ")) != 'quit':
         print(f"你输入了: {line}")
     ```

   - 列表推导式

     ```python
     # 旧方法：可能需要两次计算
     data = [1, 2, 3, 4, 5]
     result = [x for x in data if x * x > 10]
     
     # 如果需要保存中间结果，新方法更有用
     data = [1, 2, 3, 4, 5]
     # 想要保存每个元素的平方，但只保留平方大于10的结果
     squares = [square for x in data if (square := x * x) > 10]
     print(squares)  # [16, 25]
     ```

# 路径写法

`.`当前目录

`..`上一级目录

使用绝对路径，可以使用`\\`或者`/`。

# 函数

## :one:类型注解

允许在代码中显式声明函数参数和返回值的类型。

普通变量注解

```python
var1: type = ...
var2: list[str] = [string, string, string, ...]
var3: dict[str, int] = [string: int, ...]
var4: tuple[str, int] = ...
```

```python
def function(para1: type1, para2:type2) -> type3:
    ...
    return ...
```

type1、type2：参数类型

type3：函数返回值的类型，多个返回值使用`tuple[type4, type5]`

函数注解（numpy风格）

```python
def function(para1: type, para2: type) -> returns: # 一个返回值有多个类型，使用type1 | type2
    r'''             如果没有大量 \ ，可以不加r
    文本
    
    Parameters       没有冒号
    ----------
    para1: type      有其它的解释，可以使用-，表现为分段，显示一个空心圆圈
    	文本
    para2: type
    	文本
    	
    Returns
    -------
    type              没有冒号
        文本          如果type是列表字典等，可以用Parameters的描述方式
    '''
    ...
    
    
def funtion2():
    '''
    Returns
    ------
    tuple           函数有多个返回对象时，一般以元组形式返回，描述方式同上
    	文本
    '''
```

## :two:变量作用域

- 全局变量：在函数之外定义的变量，在整个文件种都能访问，通常定义在文件的底部。
- 局部变量：在函数内部定义的变量，只能在函数内部访问。如果局部变量的名字与全局变量一样，在函数结束后全局变量不会变。如果要改变，需使用`global`。

## :three:传参方式

在调用函数时，传递实参的方式

1. 位置参数：调用函数时根据函数定义时的位置来传递参数。要求调用函数与定义函数时的参数一一对应。

2. 关键字传参：调用函数时以函数定义时形参名称作为关键字，形式：键 = 值，不要求顺序。

   ```python
   def function(para1, para2):
       ...
   
   funciton(para1 = ..., para2 = ...)
   ```

   这两种方式可以混用，但是**位置参数**必须在**前面**。

>在 Python 函数或类的参数列表中，`/` 和 `*` 是**参数传递方式的分隔符**，它们用于强制规范调用者如何传参，提升代码的健壮性和可读性。
>
>- `/`：表示其左侧的参数**只能以位置参数**（positional argument）形式传入，**不能使用关键字参数**（keyword argument）形式。
>- `*`：表示其右侧的参数**必须以关键字参数**形式传入，**不能以位置参数**形式传入。

## :four:默认参数

也叫缺省参数，在函数定义时提供默认值。调用函数时，可以不传递有默认值的参数。

```python
def function(para = ...):
    ...
```

默认参数可以有很多个，但必须在没有默认值的参数之后。

与类型注解结合，默认值写在type后面。

```python
def function(para: type = ...)
```

> 使用 `__defaults__` 查看参数的默认情况。

## :five:不定长参数

也叫可变参数，用于函数定义或调用时函数参数不确定的场景。

1. 位置传递

   ```python
   def function(*args) # args = (...)
   ```

   把传入的参数全部装进一个==元组==中。

2. 关键字传递

   ```python
   def function(**kwargs) # kwargs = {...: ...}
   
   function(name="Alice", age="20")
   ```

   键 = 值，会封装为一个==字典==类型，键的类型会变成字符串。
   
   两个方法可以混用。

## :six:匿名函数：lambda表达式

```python
lambda arg: ...
```

单行，一般比较简单，参数可以省略，也可以有很多个，以逗号分隔，函数体可以是操作，也可以返回值（不需`return`）。

不能访问自己参数列表之外或全局命名空间里的参数，只能完成非常简单的功能。

调用需要赋值给一个变量。

```python
var1 = lambda x: x + 1
var1(arg)

var2 = lambda : print('---')
var2()
```

## :seven:递归调用

函数自己调用自己。

## :eight:函数的类

函数属于`function`类，也是一种对象，具有对象的属性。

```python
def secret_agent():
    """我是特工"""
    pass

print(secret_agent.__name__) # 输出: secret_agent
print(secret_agent.__doc__)  # 输出: 我是特工
print(type(secret_agent))    # 输出: <class 'function'>
```

## :nine: 函数的分类

- 无参无返回值的函数

  ```python
  定义：def 函数名():...
  调用：函数名()
  ```

- 有参无返回值的函数
  ```python
  定义：def 函数名(形参):...
  调用：函数名(实参)
  ```

- 无参有返回值的函数
  ```python
  定义：def 函数名():... return 返回值
  调用：用变量接收返回值 = 函数名()
  ```

- 有参有返回值的函数
  ```python
  定义：def 函数名(形参):... return 返回值
  调用：用变量接收返回值 = 函数名(实参)
  ```

---

# 面向对象方法

## :one:介绍

类：描述一组具有属性和方法的模板。

对象：类的实例，基于类创建。

## :two:类与对象

1. 简单定义

   ```python
   # 定义类
   class 类名：
   	pass
   
   对象名 = 类名() # 创建对象
   对象.属性 = 属性值 # 添加属性
   ```

   类名的命名规范：大驼峰命名法，每个单词的首字母大写且没有分隔符。

   ```python
   class Car():
       pass
   
   # 动态添加属性
   cl = Car()
   cl.name = '丰田'
   cl.price = 10000
   cl.brand = 'zh'
   print(cl.__dict__) # 输出{'name': '丰田', 'price': 10000, 'brand': 'zh'}
   print(cl.name) # 输出丰田
   ```

   `__dict__`是一个特殊属性，用于以字典的形式存储对象的属性。

   直接`print(cl)`会输出类型 + 对象 + 内存地址。

2. 进阶定义

   ```python
   class 类名:
       def __init__(self, para): # 定义在类之中，称为方法
           self.属性名 = 参数值
           
   对象名 = 类名(arg)
   ```

   `__init__`：初始化方法，对象创建后自动调用，用于设置对象的初始状态。

   `self`：方法的第一个参数，表示当前创建的实例对象。在传递参数时不用管`self`

   ```python
   class Car:
       def __init__(self, c_name, c_price, c_brand):
           self.name = c_name # 添加属性
           self.price  = c_price
           self.brand = c_brand
   
   c1 = Car('丰田', 10000, 'zh')
   print(c1.__dict__) # 输出{'name': '丰田', 'price': 10000, 'brand': 'zh'}
   ```

   这个方法只能是这种格式，因为是python自带的。

## :three:实例方法

与函数的定义方法一致。

```python
class 类名:
    def __init__(self, para):
        self.属性名 = para
        
    def 方法名(self, para):
        ...
        
对象名 = 类名(arg1)
对象名.方法名(arg2)
```

```python
class Car:
    def __init__(self, c_name, c_price, c_brand, c_velocity, c_mileage):
        self.name = c_name
        self.price  = c_price
        self.brand = c_brand
        self.velocity = c_velocity
        self.mileage = c_mileage

    def total_time(self): # 使用self获取当前实例
        return round(self.mileage / self.velocity, 1)
    
c = Car('丰田', 1000, 'zh', 20, 5000)
print(c.total_time()) # 输出250.0
```

## :four:魔法方法

指以双下滑先开头和结尾的特殊方法，用于定义类的特殊行为。

不需手动调用，python会自动调用。

1. `__init__`

   初始化方法。

2. `__str__`

   字符串的表示方法。

   在`print()`时调用，输出必须是字符串。

3. `__eq__`

   比较两个对象是否相等（equal）。

   在使用`=`时调用。

   ```python
   class Car:
       def __init__(self, c_name, c_price, c_brand, c_velocity, c_mileage):
           self.name = c_name
           self.price  = c_price
           self.brand = c_brand
           self.velocity = c_velocity
           self.mileage = c_mileage
   
       def total_time(self):
           return round(self.mileage / self.velocity, 1)
       
       def __eq__(self, other): # other是自定义参数，可以更换
           return self.price == other.price
   
   c1 = Car('丰田', 1000, 'zh', 20, 5000)
   c2 = Car('奔驰', 2000, 'en', '80', 12000)
   c3 = Car('丰田', 1000, 'zh', 20, 5000)
   
   print(c1 == c2) # False
   print(c1.__dict__ == c3.__dict__) # True
   print(c1.__eq__(c3)) # True
   print(c1 == c3) # True
   ```

   在把`__eq__`方法注释掉之后第21行会输出NotImplemented，因为没有定义方法；第22行会输出False，因为现在是在比较两个对象的内存地址。

4. > `__lt__`
   >
   > 小于（==l==ess ==t==han）。
   >
   > 在使用`<`时调用。
   >
   > `__le__`
   >
   > 小于等于（==l==ess than or ==e==qual）。
   >
   > 在使用`<=`时调用。
   >
   > `__gt__`
   >
   > 大于（==g==reater ==t==han）。
   >
   > 在使用`>`时调用。
   >
   > `__ge__`
   >
   > 大于等于（==g==reater than or ==e==qual）。
   >
   > 在使用`>=`时调用。

   比较两个对象的大小。

## :five:属性

1. 实例属性：属于每个具体对象的属性，每个对象是独立的。

   ```python
   class Car:
       def __init__(self, c_name, c_price, c_brand, c_velocity, c_mileage):
           self.name = c_name
           self.price  = c_price
           self.brand = c_brand
           self.velocity = c_velocity
           self.mileage = c_mileage
           # 这里的self.attribute就是实例属性，通过obj.attribute操作
   ```

   通过实例对象查找属性时，会先查找实例属性，如果实例属性不存在，则会查找类属性。

2. 类属性：属于类本身的属性，所有实例共享。

   ```python
   class Car:
       wheel = 4 # 类属性 通过类名.属性名操作
       
       def __init__(self, c_name, c_price, c_brand, c_velocity, c_mileage):
           self.name = c_name
           self.price  = c_price
           self.brand = c_brand
           self.velocity = c_velocity
           self.mileage = c_mileage
   ```

3. 获取属性

   * `dir()`

     获取所有属性名称（包含方法和内置属性）。

     ```python
     class Person:
         def __init__(self, name):
             self.name = name
             self.age = 18
     
     p = Person("Alice")
     
     
     all_attrs = dir(p)
     # print(all_attrs) 
     # 输出示例: ['__class__', '__delattr__', ..., 'age', 'name']
     
     for attr in all_attrs:
         if '_' not in attr:
             print(attr) # age name
     ```

   * `__dict__`

     获取实例属性。

     ```python
     class Person:
         def __init__(self, name):
             self.name = name
             self.age = 18
     
     p = Person("Bob")
     
     # 直接获取属性字典
     print(p.__dict__) 
     # 输出: {'name': 'Bob', 'age': 18}
     
     # 获取属性名称列表
     print(p.__dict__.keys()) 
     # 输出: dict_keys(['name', 'age'])
     ```

   * `hasattr`

     检查属性是否存在。

     ```python
     class Person:
         def __init__(self, name):
             self.name = name
             self.age = 18
     
     p = Person("Charlie")
     
     # 检查是否有 'name' 属性
     if hasattr(p, 'name'):
         print("有这个属性") # 会打印
     
     # 检查是否有 'gender' 属性
     if hasattr(p, 'gender'):
         print("有这个属性")
     else:
         print("没有这个属性") # 会打印这个
     ```

   * `getattr`

     动态获取属性的值。

     ```python
     class Person:
         def __init__(self, name):
             self.name = name
             self.age = 18
             
     p = Person("David")
     attr_name = "name"  # 假设这是用户输入的
     
     # 动态获取值，相当于 p.name
     value = getattr(p, attr_name)
     print(value) # 输出: David
     
     # 如果属性不存在，可以设置默认值，避免报错
     missing_value = getattr(p, "non_existent_attr", "默认值")
     print(missing_value) # 输出: 默认值
     ```

# python 包标识文件

`__init__.py` 是 Python 包的核心标识文件，它的主要作用是将一个普通目录标记为可导入的 Python 包，并在首次导入该包时自动执行其中的代码。

## 核心作用

- **包标识**

  这是最基础的功能。没有这个文件，Python 解释器不会将该目录视为一个包，也就无法通过 `import package_name` 的方式导入其中的模块。

- **初始化执行**

  当用户第一次导入该包或其任何子模块时，`__init__.py` 中的代码会自动运行。这非常适合用来设置包级别的全局变量、配置日志、或执行一些必要的初始化逻辑。

- **API 暴露与命名空间管理**

  通过在 `__init__.py` 中使用 `from .module import Class, function` 语句，可以将包内部深层模块中的类、函数或变量“提升”到包的顶层命名空间。这样用户就可以直接使用 `package.Class`，而不需要写冗长的 `package.module.submodule.Class`。

- **控制导入行为**

  可以定义 `__all__` 列表来明确指定当用户使用 `from package import *` 时，哪些名称会被导入，避免导入不必要的内部实现细节。

## 内容

根据你的项目需求，`__init__.py` 的内容可以从简单到复杂：

- **最简单情况（空文件）**

  如果你只需要让目录成为一个包，且不需要任何初始化逻辑或 API 聚合，一个空文件就足够了。

- **中等复杂度（暴露公共 API）**

  这是最常见的做法。将包内各个模块中希望对外暴露的核心类、函数或常量导入到 `__init__.py` 中，为用户提供一个简洁、统一的接口。

- **高级用法（包含初始化逻辑）**

  如果包在首次使用时需要进行一些设置（如连接数据库、加载配置文件、初始化日志系统等），可以将这些逻辑写在 `__init__.py` 中。但需注意，应避免在此文件中放置耗时过长的操作，以免影响导入速度。

## 模板

以下是一个适用于你“电力负荷预测”项目的 `src/power_load_forecast/__init__.py` 模板，它结合了 API 暴露和版本信息：

```python
"""
电力负荷预测包 - 基于XGBoost的多变量单步预测模型

该包提供了数据预处理、模型训练和预测的核心功能。
"""

# 包的版本信息，便于管理和追踪
__version__ = "0.1.0"

# 从子模块中导入核心类和函数，构建简洁的公共API
# 假设你的 src/power_load_forecast/ 目录下有 data_processing.py, model_training.py, prediction.py 等模块
from .data_processing import load_data, preprocess_features
from .model_training import train_xgboost_model, evaluate_model
from .prediction import predict_load

# 定义 __all__ 以明确控制 from power_load_forecast import * 的行为
__all__ = [
    "__version__",
    "load_data",
    "preprocess_features",
    "train_xgboost_model",
    "evaluate_model",
    "predict_load",
]
```

这个模板清晰地定义了包的用途、版本，并通过 `from .module import ...` 的方式将分散在不同文件中的核心功能聚合起来，使得外部使用者可以非常方便地调用。同时，`__all__` 列表确保了只有明确列出的名称才会被通配符导入，保持了接口的整洁性。

> **通用模板**
>
> ```python
> """
> [在这里写一段简短的包介绍，说明这个包的主要功能和用途]
> """
> 
> # 1. 包的版本信息
> __version__ = "0.1.0"
> 
> # 2. 从内部子模块中导入核心类、函数或变量，构建简洁的公共 API
> # 注意：使用相对导入（以 . 开头），避免硬编码包名
> from .module_name import ClassName, function_name
> from .sub_package import AnotherClass
> 
> # 3. 定义 __all__ 列表
> # 明确控制当用户使用 `from your_package import *` 时，哪些名称会被公开导出
> __all__ = [
>     "__version__",
>     "ClassName",
>     "function_name",
>     "AnotherClass",
> ]
> ```


---

# 格式、格式化与格式指令

这三个概念其实是一条完整的流水线：**“格式”是模具，“格式化”是加工的过程，而“格式指令”就是操作说明书。**

## :one: 什么是“格式”？

**格式就是“模具”或“规矩”。**

想象你在一家巧克力工厂工作。

- **数据**就是融化的巧克力。
- **格式**就是你手里的模具（比如：必须是方形、必须重 100 克、包装纸必须是红色的）。

在计算机里，**格式**就是一套**规则**，规定了数据应该长什么样。

- 比如电话号码的格式：`138-xxxx-xxxx`。
- 比如日期的格式：`2026年04月21日`。

> **总结**：格式 = 数据的外观标准。

------

## :two:什么是“格式化”？

**格式化就是“加工”或“整形”的动作。**

还是那个工厂，现在你有一坨融化的巧克力（原始数据 `x = 7`），客户要求必须是一盒 5 块装的礼盒（目标格式 `00007`）。

- 你把 `7` 扔进机器。
- 机器自动在前面塞了 4 个 `0`。
- 最后吐出来一个漂亮的字符串 `"00007"`。

这个过程，把**“原始数据”**变成**“符合特定格式的数据”**的过程，就叫**格式化**。

> **总结**：格式化 = 把数据整理成特定样子的过程。

------

## :three: 什么是“格式指令”？

**格式指令就是给机器的“操作说明书”。**

在代码 `f"{x:05d}"` 中，冒号后面的 `05d` 就是**格式指令**。它告诉 Python 解释器（机器）具体该怎么干活：

- **`d`**：说明书第 1 条 —— “这是一个整数，不要搞成小数。”
- **`5`**：说明书第 2 条 —— “不管数字多小，必须占满 5 个格子。”
- **`0`**：说明书第 3 条 —— “如果格子填不满，空的地方用 0 补齐。”

如果没有这个指令，Python 就会按默认习惯来（比如直接显示 `7`），这就不符合你的要求了。

> **总结**：格式指令 = 告诉程序“怎么排、怎么补、怎么显示”的具体代码参数。

------

## :four: Python 格式指令速查表

### **核心公式**

`{变量:填充字符 对齐方式 宽度 .精度 类型}`

**注意**：除了“宽度”是必须的（如果你想限制宽度的话），其他都是可选的。且**顺序不能乱**。

### **详细参数表**

| 组成部分        | 符号 / 代码        | 含义           | 备注                           |
| :-------------- | :----------------- | :------------- | :----------------------------- |
| **1. 分隔符**   | **`:`**            | **开始写指令** | 冒号后面才是格式指令           |
| **2. 填充字符** | `0` / `*` / `-` 等 | **空位补什么** | 默认是空格。若要补0，直接写0   |
| **3. 对齐方式** | **`<`**            | 左对齐         | 内容在左，补位在右             |
|                 | **`>`**            | 右对齐         | 内容在右，补位在左（数字默认） |
|                 | **`^`**            | 居中对齐       | 内容在中，两边补位             |
| **4. 宽度**     | `5` / `10` / `20`  | **总长度**     | 至少占多少个字符位置           |
| **5. 精度**     | `.`                | 保留多少位小数 |                                |
| **6. 类型**     | **`d`**            | 整数           | Decimal                        |
|                 | **`f`**            | 小数           | Float (默认6位小数)            |
|                 | **`%`**            | 百分数         | 自动乘以100并加%               |
|                 | **`x`**            | 十六进制       | Hexadecimal                    |
|                 | **`s`**            | 字符串         | String (默认)                  |

# 按位运算

Python 中的按位运算符（Bitwise Operators）是直接对整数在内存中的**二进制位**进行操作的符号。

为了让你直观理解，我们先设定两个数字作为例子：

- `a = 5` （二进制：`0101`）
- `b = 3` （二进制：`0011`）

以下是 Python 中 6 种核心按位运算符的详细解析：

## 按位与（`&`）

- 规则

  只有当两个对应的二进制位**都为 1** 时，结果才为 1，否则为 0。

- 示例：`5 & 3`

  ```text
    0101  (5)
  & 0011  (3)
  ------
    0001  (结果：1)
  ```

- 用途

  常用于判断奇偶（`n & 1 == 1` 为奇数），或者提取某些特定的位。

## 按位或（`|`）

- 规则

  只要两个对应的二进制位中**有一个为 1**，结果就为 1。

- **示例**：`5 | 3`

  ```text
    0101  (5)
  | 0011  (3)
  ------
    0111  (结果：7)
  ```

- 用途

  常用于将某些特定的位置为 1。

## 按位异或（`^`）

- 规则

  两个对应的二进制位**不相同**时，结果为 1；相同时为 0。

- 示例：`5 ^ 3`

  ```text
    0101  (5)
  ^ 0011  (3)
  ------
    0110  (结果：6)
  ```

- 用途

  非常神奇的一个运算。任何数与自身异或结果为 0（`a ^ a = 0`），任何数与 0 异或结果为自身（`a ^ 0 = a`）。常用于**不借助临时变量交换两个数**，或者简单的加密/解密。

## 按位取反（`~`）

- 规则

  将二进制中的 0 变成 1，1 变成 0。

- 示例

  `~5`

  ```text
    0101  (5)
  ------
    1010  (取反后)
  ```

- 注意

  在 Python 中，整数是有符号的（使用补码表示）。`~x` 的结果在数学上等于 `-(x + 1)`。所以 `~5` 的结果是 `-6`。

## 左移（`<<`）

- 规则

  将二进制位整体向左移动，右边空出的位补 0。每左移 1 位，相当于**乘以 2**。

- 示例

  `5 << 1`

  ```text
    0101  (5)
  << 1
  ------
    1010  (结果：10)
  ```

- 用途

  快速计算 2 的幂次，或者进行数据压缩。

## 右移（`>>`）

- 规则

  将二进制位整体向右移动。对于正数，左边空出的位补 0。每右移 1 位，相当于**整除 2**（向下取整）。

- 示例：`5 >> 1`

  ```text
    0101  (5)
  >> 1
  ------
    0010  (结果：2)
  ```

- 用途

  快速进行除法运算，或者提取二进制中的某一位。

## 与逻辑运算的区别

初学者最容易混淆的是 `&` / `|` 和 `and` / `or`：

| 运算符        | 类型         | 操作对象                   | 结果                   |
| ------------- | ------------ | -------------------------- | ---------------------- |
| `&`，`|`，`~` | **按位运算** | 将数字转为二进制，逐位计算 | 返回一个新的整数。     |
| `and`，`or`   | **逻辑运算** | 将整体视为 True/False      | 返回布尔值或原操作数。 |

**示例对比**

- `5 & 3` = `1` （二进制按位计算）
- `5 and 3` = `3` （5 为真，所以返回第二个操作数 3）

# 逻辑运算

Python 中的逻辑运算符（Logical Operators）主要用于组合布尔表达式，控制程序的执行流程。

Python 的逻辑运算与 C/Java 等语言有一个最大的不同：**它的结果不一定是布尔值（True/False），而是返回决定运算结果的那个“原始操作数”。**

为了让你直观理解，我们设定：

- `a = 5` （非零，视为 True）
- `b = 0` （零，视为 False）
- `c = "hello"` （非空字符串，视为 True）

## `and`（逻辑与）

- 规则

  只有当所有操作数都为 True 时，结果才为 True。

- 返回值

  如果所有条件都为真，返回**最后一个**操作数；如果中间遇到假值，立即返回**第一个假值**（短路效应）。

- 示例

  - `5 and 3` → 结果是 `3` （5 为真，继续看 3，3 也是真，返回最后一个 3）
  - `0 and 3` → 结果是 `0` （0 为假，直接短路返回 0，不再看后面的 3）
  - `5 and 0` → 结果是 `0` （5 为真，继续看 0，0 为假，返回 0）

## `or`（逻辑或）

- 规则

  只要有一个操作数为 True，结果就为 True。

- 返回值

  如果第一个操作数为真，立即返回**第一个**操作数（短路效应）；如果第一个为假，则返回**第二个**操作数。

- 示例

  - `5 or 3` → 结果是 `5` （5 为真，直接短路返回 5，不再看后面的 3）
  - `0 or 3` → 结果是 `3` （0 为假，继续看 3，3 为真，返回 3）
  - `0 or ""` → 结果是 `""` （0 为假，继续看 ""，"" 也为假，返回最后一个假值 ""）

## `not`（逻辑非）

- 规则

  对操作数的布尔值取反。

- 返回值

  **永远返回布尔值**（True 或 False）。

- 示例

  - `not 5` → 结果是 `False` （5 为真，取反为 False）
  - `not 0` → 结果是 `True` （0 为假，取反为 True）
  - `not ""` → 结果是 `True` （空字符串为假，取反为 True）

## 短路效应

Short-circuit Evaluation。

这是 Python 逻辑运算中最重要、也最实用的特性。Python 在计算逻辑表达式时，**从左到右**进行，一旦结果已经确定，就会立即停止计算后面的表达式。

- `and` 的短路：只要遇到一个假值，就立刻返回该假值，后面的都不看了。
- `or` 的短路：只要遇到一个真值，就立刻返回该真值，后面的都不看了。

**实战应用示例**

```python
# 场景：安全地访问字典中的值，防止 KeyError
user = {"name": "Alice"}

# 如果直接写 user["age"]，程序会报错崩溃。
# 利用 and 的短路效应：
age = user.get("age") and user["age"] 
# 因为 user.get("age") 返回 None (假)，and 直接短路返回 None，根本不会执行后面的 user["age"]
```

## 代码示例

```python
def greet(name=None, excitement=0):
    # 利用 or 的短路效应设置默认值
    # 如果 name 传入了有效值（真），就用传入的值；如果没传或传了 None/空字符串（假），就用 "World"
    final_name = name or "World"

    # 如果 excitement 传入了非零值（真），就用传入的值；如果没传或传了 0（假），就用 1
    final_level = excitement or 1

    print(f"Hello, {final_name}! Excitement level: {final_level}")

# 测试一下
greet()                  # 都没传，使用全部默认值
greet("Alice")           # 传了名字，excitement 使用默认值
greet("Bob", 5)          # 都传了，使用传入的值
greet("", 0)             # 传了空字符串和 0，依然使用默认值
```

一般不会这么写，因为 0 和空字符串也是假值，而我们希望是在 `None` 时才使用后面的。

一般用在 `get` 方法上。

# 感叹号（!）

在 Python 中，感叹号（`!`）并不是一个独立的逻辑运算符（这与 C 语言或 Java 不同）。

## “不等于”比较运算符

在标准 Python 语法中，感叹号必须与等号（`=`）结合使用，构成 `!=` 运算符。

- 作用

  用于判断两个值是否不相等。如果两个值不相等，表达式返回 `True`，否则返回 `False`。

- 用法

  ```python
  a = 5
  b = 3
  if a != b:
      print("a 不等于 b")
  ```

## 执行 Shell 命令

在 Jupyter Notebook、IPython 等交互式环境中，感叹号被用作“魔法前缀”，用于直接在代码单元格中执行操作系统的 Shell 命令。

- 作用

  允许在不离开 Python 环境的情况下，调用系统命令（如列出文件、安装库等）。

- 用法

  ```python
  # 列出当前目录下的文件
  !ls 
  
  # 安装 Python 包
  !pip install numpy
  ```

此用法仅在 IPython/Jupyter 环境中有效。在标准的 Python 脚本（`.py` 文件）或标准解释器中，直接使用 `!` 会引发语法错误。

## 字符串格式化中的转换标志

在使用 `str.format()` 方法或 f-string 进行字符串格式化时，感叹号用于指定转换标志。

- 作用

  在格式化之前，强制对值进行特定的转换。

- 可选值

  - `!s`：调用 `str()` 函数，将值转换为字符串。
  - `!r`：调用 `repr()` 函数，将值转换为解释器可读取的字符串表示。
  - `!a`：调用 `ascii()` 函数，将值转换为 ASCII 编码的字符串表示。

- 用法

  ```python
  name = "Alice"
  # 使用 !r 获取带引号的字符串表示
  print(f"Hello, {name!r}") 
  # 输出: Hello, 'Alice'
  ```

## 常见误区澄清

1. Python 中没有独立的 `!` 逻辑非运算符

   在 C 语言或 Java 中，`!` 表示逻辑非（NOT）。但在 Python 中，逻辑非操作必须使用关键字 `not`（例如 `if not x:`）。

2. 标准脚本中不可用

   除了 `!=` 和字符串格式化标志外，在标准的 `.py` 脚本中直接使用 `!` 会导致 `SyntaxError`。若需在标准脚本中执行系统命令，应使用 `subprocess` 或 `os` 模块。
