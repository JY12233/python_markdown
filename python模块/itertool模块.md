`itertools` 是 Python 标准库中一个非常强大的模块，专门用于**创建和操作高效的迭代器**。如

它的核心优势在于**“惰性求值”（Lazy Evaluation）**，即数据只有在被用到时才会被生成，这使得它在处理海量数据或无限序列时，内存占用极低（常数级 O(1)），性能极高。

# 无限迭代器

这三个函数可以生成无限的序列，通常配合 `for` 循环中的 `break` 或 `islice`（切片）一起使用。

- `count(start, step)`

  从 `start` 开始，按 `step` 步长无限递增。

  ```python
  from itertools import count
  # 生成从 10 开始，步长为 2 的无限序列
  for i in count(10, 2):
      if i > 20: break
      print(i, end=" ") # 输出: 10 12 14 16 18 20
  ```

- `cycle(iterable)`

  无限循环遍历一个可迭代对象（比如做轮询调度）。

  ```python
  from itertools import cycle
  servers = ['A', 'B', 'C']
  round_robin = cycle(servers)
  # 依次输出 A, B, C, A, B, C...
  ```

- `repeat(object, times)`

  将某个对象重复 `times` 次（如果不指定 `times` 则无限重复）。

  ```python
  from itertools import repeat
  print(list(repeat('hello', 3))) # 输出: ['hello', 'hello', 'hello']
  ```

# 高效组合与处理工具

- `chain(*iterables)`

  将多个可迭代对象“串”成一条链，比 `sum(lists, [])` 更省内存。

  ```python
  from itertools import chain
  list1, list2 = [1, 2], [3, 4]
  print(list(chain(list1, list2))) # 输出: [1, 2, 3, 4]
  ```

- `accumulate(iterable, func)`

  对迭代器进行累积计算。默认是累加，也可以通过 `func` 参数自定义（如累乘、取最大值）。

  ```python
  from itertools import accumulate
  import operator
  nums = [1, 2, 3, 4]
  print(list(accumulate(nums)))          # 默认累加: [1, 3, 6, 10]
  print(list(accumulate(nums, operator.mul))) # 累乘: [1, 2, 6, 24]
  ```

- `groupby(iterable, key)`

  将相邻的重复元素分组。

  **注意**：使用前必须先对数据进行排序，否则相同键值的元素会被分到不同的组。

  ```python
  from itertools import groupby
  data = sorted(['apple', 'bat', 'atom', 'book'])
  for key, group in groupby(data, key=lambda x: x[0]):
      print(key, list(group))
  # 输出:
  # a ['apple', 'atom']
  # b ['bat', 'book']
  ```

# 排列组合生成器

在解决算法题、生成测试用例或穷举密码时，这三个函数是“终极武器”，比手写多层嵌套循环简洁得多。

- `product(*iterables)`

  计算笛卡尔积，完美替代嵌套的 `for` 循环。

  ```python
  from itertools import product
  colors = ['红', '蓝']
  sizes = ['S', 'M']
  print(list(product(colors, sizes)))
  # 输出: [('红', 'S'), ('红', 'M'), ('蓝', 'S'), ('蓝', 'M')]
  ```

- `permutations(iterable, r)`

  生成**排列**（顺序敏感，无重复元素）。

  ```python
  from itertools import permutations
  print(list(permutations('ABC', 2)))
  # 输出: [('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'C'), ('C', 'A'), ('C', 'B')]
  ```

- `combinations(iterable, r)`

  生成**组合**（顺序无关，无重复元素）。

  ```python
  from itertools import combinations
  print(list(combinations([1, 2, 3], 2)))
  # 输出: [(1, 2), (1, 3), (2, 3)]
  ```

# 快速总结表

| 类别         | 核心函数                                  | 核心作用           | 典型应用场景                       |
| ------------ | ----------------------------------------- | ------------------ | ---------------------------------- |
| **无限迭代** | `count`，`cycle`，`repeat`                | 生成无限序列       | 序号生成、轮询调度、填充数据。     |
| **组合处理** | `chain`，`accumulate`，`groupby`          | 拼接、累积、分组   | 扁平化列表、前缀和计算、数据分组。 |
| **排列组合** | `product`，`permutations`，`combinations` | 生成排列与笛卡尔积 | 替代多层循环、生成测试用例、穷举。 |