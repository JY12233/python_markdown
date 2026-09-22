Python 的 `collections` 模块是 Python 标准库中一个非常强大的工具箱，它提供了许多高性能、功能增强的容器数据类型，可以看作是内置数据类型（如 `dict`, `list`, `set`, `tuple`）的“超级加强版”。

对于数据处理、算法编写和日常开发来说，它能极大地简化代码并提升效率。

# `defaultdict`

> 自带“默认值”的字典。

这是 `dict` 的子类，最大的特点是：当访问一个不存在的键时，它不会报错，而是自动为该键生成一个由 `default_factory` 指定类型的默认值（如 `int`, `list`, `set`）。

- **痛点解决**：省去了判断键是否存在、初始化默认值的繁琐代码。
- **典型场景**：分组统计、构建列表字典。

```python
from collections import defaultdict

# 创建一个默认值为列表的字典
d = defaultdict(list)

# 直接往不存在的键里追加元素，不会报错
d['fruits'].append('apple')
d['fruits'].append('banana')
d['numbers'].append(1)

print(d) 
# 输出: defaultdict(<class 'list'>, {'fruits': ['apple', 'banana'], 'numbers': [1]})
```

# `Counter`

> 专业的“计数器”。

专门用于统计可迭代对象中元素出现的次数。它本质上是一个字典，键是元素，值是出现的次数。

- **痛点解决**：一行代码搞定复杂的频次统计。
- **典型场景**：词频统计、找列表中出现次数最多的元素。

```python
from collections import Counter

words = ['a', 'b', 'c', 'a', 'b', 'a']
count = Counter(words)
print(count) 
# 输出: Counter({'a': 3, 'b': 2, 'c': 1})

# 获取出现次数最多的前2个
print(count.most_common(2)) 
# 输出: [('a', 3), ('b', 2)]
```

# `deque`

> 双向队列 (Double-Ended Queue)。

一种类似列表的容器，支持在两端快速地添加或删除元素。相比于普通列表（`list`），它在头部插入和删除元素的效率极高（O(1) vs O(n)）。

- **痛点解决**：列表在开头插入数据很慢，`deque` 解决了这个问题。
- **典型场景**：实现队列（FIFO）、栈（LIFO）、滑动窗口算法。

```python
from collections import deque

# 创建双向队列
dq = deque([1, 2, 3])

# 右侧添加
dq.append(4) 

# 左侧添加
dq.appendleft(0) 

# 右侧弹出
dq.pop() 

# 左侧弹出
dq.popleft() 

print(dq) 
# 输出: deque([1, 2, 3])
```

# `namedtuple`

> 带名字的元组。

普通的元组是通过索引访问的（`t[0]`, `t[1]`），可读性差。`namedtuple` 允许你给元组的每个位置起一个名字，既保留了元组的轻量级和不可变性，又增加了可读性。

- **痛点解决**：让代码更易懂，避免“魔法数字”索引。
- **典型场景**：表示简单的数据记录（如坐标、RGB颜色）。

```python
from collections import namedtuple

# 定义一个名为 'Point' 的元组，包含 x 和 y 两个字段
Point = namedtuple('Point', ['x', 'y'])

p = Point(10, 20)
print(p.x, p.y) # 输出: 10 20
print(p)        # 输出: Point(x=10, y=20)
```

# `OrderedDict`

> 记住插入顺序的字典。

在 Python 3.7 之前，普通的字典是不保证顺序的，`OrderedDict` 就是用来解决这个问题的。虽然现在普通字典已经默认保持插入顺序，但 `OrderedDict` 依然有其独特的用途。

- **独特优势**：`OrderedDict` 有一个 `move_to_end()` 方法，并且在比较两个字典时，会严格比较键值对的顺序。
- **典型场景**：需要频繁调整键顺序，或者需要严格比较字典顺序的场景。

```python
from collections import OrderedDict

od = OrderedDict()
od['a'] = 1
od['b'] = 2
od['c'] = 3

# 移动到末尾
od.move_to_end('b') 
print(od) 
# 输出: OrderedDict([('a', 1), ('c', 3), ('b', 2)])
```

# 快速对比总结表

| 类型              | 继承自   | 核心特性                       | 最佳应用场景               |
| ----------------- | -------- | ------------------------------ | -------------------------- |
| **`defaultdict`** | `dict`   | 访问不存在的键时自动创建默认值 | 分组、构建嵌套字典/列表    |
| **`Counter`**     | `dict`   | 专门用于计数统计               | 词频分析、找众数           |
| **`deque`**       | `object` | 两端操作效率极高               | 队列、栈、滑动窗口         |
| **`namedtuple`**  | `tuple`  | 通过名字访问字段，不可变       | 表示简单的不可变数据结构   |
| **`OrderedDict`** | `dict`   | 严格保持插入顺序               | 需要重排顺序或严格顺序比较 |

# `ChainMap`

不常用。它可以把多个字典“链”在一起，当作一个字典来使用。当你查找键时，它会从头到尾依次查找。这在处理多层级配置（如默认配置、用户配置、临时配置）时非常有用。

```python
from collections import ChainMap

default_config = {'host': 'localhost', 'port': 8080}
user_config = {'port': 9000}

# 合并配置，user_config 优先级高于 default_config
config = ChainMap(user_config, default_config)
print(config['port']) # 输出: 9000 (取的是第一个找到的)
print(config['host']) # 输出: localhost
```