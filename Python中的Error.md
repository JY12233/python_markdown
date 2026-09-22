常见的 Error 类型及其出现情况：

### :one:SyntaxError：

出现情况：当Python解释器遇到无法解析的代码时，会引发 SyntaxError。这通常是由于代码中存在语法错误，例如忘记添加冒号、括号不匹配、错误的关键词使用等。
　　示例：
if True
print("Hello, World!")
　　这段代码会引发 SyntaxError，因为在 if 语句后缺少冒号。

### :two:NameError：

出现情况：当尝试访问一个未定义的变量时，会引发 NameError。
　　示例：
print UndefinedVariable
　　这段代码会引发 NameError，因为 UndefinedVariable 没有被定义。

### :three:TypeError：

出现情况：当一个操作或函数应用于不适当类型的对象时，会引发 TypeError。
　　示例：
"Hello" + 5
　　这段代码会引发 TypeError，因为字符串和整数类型之间不能直接进行加法操作。

### :four:IndexError：

出现情况：当尝试使用一个不存在的序列索引时，会引发 IndexError。
　　示例：
list = [1, 2, 3]
print(list[5])
　　这段代码会引发 IndexError，因为索引 5 超出了列表的范围。

### :five:KeyError：

出现情况：当尝试访问一个字典中不存在的键时，会引发KeyError。
　　示例：
dict = {'a': 1, 'b': 2}
print(dict['c'])
　　这段代码会引发 KeyError，因为键 'c' 不存在于字典中。

### :six:ValueError：

　　出现情况：当一个操作或函数收到一个具有正确类型但值不适当的参数时，会引发 ValueError。
　　示例：
int('abc')
　　这段代码会引发 ValueError，因为字符串 "abc" 不能被转换为一个整数。

### :seven:AttributeError：

出现情况：当你尝试访问一个对象不存在的属性或方法时，会引发 AttributeError。
　　示例：
list = [1, 2, 3]
list.append(4)
list.add(5)  # 'list' object has no attribute 'add'
　　这段代码的最后一行会引发 AttributeError，因为列表对象没有add方法。

### :eight:ImportError：

出现情况：当导入模块失败时，会引发 ImportError。
　　示例：
import non_existent_module
　　这段代码会引发 ImportError，因为 non_existent_module 模块不存在。

ZeroDivisionError：
出现情况：当你尝试除以零时，会引发 ZeroDivisionError。
　　示例：
121 / 0
　　这段代码会引发 ZeroDivisionError，因为除数为零。

### :nine:FileNotFoundError：

　　出现情况：当尝试访问一个不存在的文件时，会引发 FileNotFoundError。
　　示例：
with open('non_existent_file.txt', 'r') as file:
content = file.read()
　　这段代码会引发 FileNotFoundError，因为指定的文件不存在。

### :one::zero:RecurisonError:

递归错误，递归超过了最大深度。
