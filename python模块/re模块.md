# 正则表达式（re模块）

## 介绍

是一种由特定语法规则组成的文本模式，用来描述、匹配字符串中符合特定规则的字符序列。

是一种模式匹配工具，允许用户通过简洁的语法进行复杂的文本搜索、匹配、提取和替换工作。

## 语法

| 字符           | 描述                                                         |
| -------------- | :----------------------------------------------------------- |
| 普通字符       | 字母、数字、汉字及大多数字符，直接匹配自身                   |
| `.`            | 匹配除`\n`意外的任意一个字符                                 |
| `\d`           | 匹配数字0-9                                                  |
| `\s`           | 匹配空格。                                                   |
| `\D`           | 匹配非数字                                                   |
| `\w`           | 匹配单词字符，即a-z、A-Z、0-9、_、其他语言字符（中文、日文、韩文等） |
| `\W`           | 匹配非单词字符（加号、等号等）                               |
| `[aeiou]`      | 匹配列表中的任何单个字符                                     |
| `[^aeiou]`     | 求反，匹配不在列表中的任何单个字符                           |
| `[0-5]`        | 表示范围                                                     |
| `*`            | 出现任意次（0或无数）                                        |
| `+`            | 至少出现一次                                                 |
| `?`            | 至多出现一次                                                 |
| `{m}`          | 出现m次                                                      |
| `{m,}`         | 至少出现m次（`,`后不能有空格）                               |
| `{m,n}`        | 出现m到n次（`,`后不能有空格）                                |
| `a|b`          | 或，匹配左右任意一个表达式，多个一起使用可用`()`             |
| `()`           | 分组，将括号里的多个字符视为一个单元                         |
| `(?P<分组名>)` | 设置分组，只是设置名字，后面还要写正则表达式                 |
| `(?P=分组名)`  | 引用分组                                                     |
| `^`            | 匹配字符串开头                                               |
| `$`            | 匹配字符串结尾                                               |
| `\num`         | 引用分组num**匹配到的字符串**，num是整数                     |

`1[3-9]\d{9}`：表示第一位必须是数字1，第二位必须在3-9之间，`\d`表示0-9的数字，`{9}`表示`\d`出现9次。这个正则表达式用来匹配手机号。

如果使用`\w`不想匹配其他语言字符，可以使用`re.ASCII`，比如`re.match(pattern, str, re.ASCII)`。

使用`()`，如果是`match`或者`search`，可以在`group`中添加数字选择获取的组，如果是`findall`，返回的是由只包含了每个组元组组成的列表。

## re模块常见函数

```python
import re

# 根据正则匹配内容，从开头开始匹配，返回Match对象，没有则返回None
result = re.match(pattern: str, str)
print(result.group()) # 通过group拿到对象内容

# 从任一位置开始匹配，返回第一个匹配项，Match对象
result = re.search(pattern, str)
print(result.group())

# 根据正则匹配内容，返回所有匹配项，返回的是一个列表
result = re.findall(pattern, str)
```

**代码举例**

```python
import re

s = "我的手机号是17788012158，另一个手机号是15896356874，你记住了吗？QQ号是1779989922。"

result = re.match(r'1[3-9]\d{9}', s)
if result: # None没有group方法，判断防止报错
    print(result.group()) # 这里没有输出，因为match从第一个开始匹配，匹配不上

result = re.search(r'1[3-9]\d{9}', s)
if result:
    print(result.group()) # 输出17788012158

result = re.findall(r'1[3-9]\d{9}', s)
print(result) # 输出['17788012158', '15896356874']
```

**Match对象的其他方法：**

```python
import re

result = re.Match

result.span() # 获取匹配项的开始与结束索引，返回元组
result.start() # 获取匹配项的开始索引，返回int
result.end() # 获取匹配项的结束索引，返回int
```

**正则替换**

```python
import re

a = 'cjnacnaso'

# 使用compile函数，直接写sub也是可以的
pattern = re.compile('[nbq].*')
print(pattern) # re.compile('[nbq].*')
print(type(pattern)) # <class 're.Pattern'>

new_a = pattern.sub('你好呀', a) # 第一个参数是替换后的文字，第二个参数是要被替换的字符串，默认替换所有
print(new_a) # cj你好呀
```