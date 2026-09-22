# random模块

```python
random.random() # 随机返回0-1之间的浮点数

random.choice(seq) # 从一个具有长度的序列中返回一个值，可以是元组、列表、字符串、字典，为字典时返回值

random.choices(seq, weight = [], cum_weight = [], k = int)
'''
接受一个序列，从中有放回地随机返回由k个值组成的列表。weight表示权重，如weight = [1, 1, 2]，第三个是前面两个的两倍；cum_wehght表示累加权重，如cum_weight[1, 1, 2] 与 weight[1, 2, 4] 相同，不能与weight同时出现。
'''

random.sample(seq, k = int, count = [])
'''
接受一个序列，从中无放回地随机返回由k个值组成的列表。count决定序列中每个元素在选取时的个数，如sample(['red', 'blue'], counts=[4, 2], k=5) 与 sample(['red', 'red', 'red', 'red', 'blue', 'blue'], k=5) 相同。
'''

random.uniform(a, b) # 返回a-b之间的一个浮点数（左闭）

random.randint(a, b) # 返回a-b之间的一个整数（闭区间）
```

---

# copy模块

```python
copy.deepcopy() # 深度复制
```

---

# os模块

```python
os.getcwd() # 返回当前工作目录

os.name() # 返回操作系统的名字，通常Windows为'nt'，Mac/Linux 为 'posix'

os.environ.get() # 返回环境变量，比如用户名

os.path.join(folder, file) # 返回拼接的路径

os.path.exists() # 判断某个路径下的文件是否存在

os.mkdir(folder) # 创建文件夹（如果不存在的话）

os.makerdirs(folder, exist_ok = Flase) # 创建多级文件夹，默认已存在会报错

os.listdir() # 返回当前文件夹下的所有内容，可用'.'代表当前目录

os.system('cls') # 清屏，Mac/Linux 为 'clear'
```

---

# time模块

```python
time.sleep(num) # 使程序停顿num秒

time.time() # 返回距离1970年1月1日的时间戳（应该不是北京时间）

time.localtime() # 将时间戳转化为年月日时分秒类型（但是可读性很低）

time.strftime("%Y年%m月%d日%H点%M分", arg) # 将第五行的代码结果转为为字符串
```

---

# json模块

:one:`json.dumps()`：自动将字典转化为json格式的字符串

```python
import json

data = {'name': '张三', 'age': '20', 'job': None}

json_str = json.dumps(data, ensure_ascii = False, indent = 4) 

print(json_str) 
'''
输出
{
    "name": "张三",
    "age": "20",
    "job": null
}
'''

print(type(json_str)) # <class 'str'>
```

`ensure_ascii`：保留中文等非ASCII字符，不转为Unicode编码（默认为True，会将中文转为`\u5f20\u4e09`等形式）。

`indent`：设置缩进空格数，美化输出格式（默认无缩进）。

:two:`json.loads()`：将json字符串转化为python对象

```python
import json

json_str = '{"name": "张三", "age": 22, "is_student": false}'

python_dict = json.loads(json_str)
print(python_dict)
'''
输出：{'name': '张三', 'age': '20', 'job': None}
'''
```

:three:`json.dump()`:将python对象写入json文件---序列化

只是以json格式写入。

```python
import json

data = {"name": "李四", "age": 23, "major": "人工智能"}

with open("student.json", "w", encoding="utf-8") as f:
    json.dump(data, f, ensure_ascii=False, indent=4)
```

**关键参数**：

- **`obj`**：要序列化的Python对象
- **`fp`**：文件对象（必须以可写模式打开）
- **`skipkeys`**：默认False，若为True则跳过非基本类型的键
- **`ensure_ascii`**：同`dumps()`，控制非ASCII字符输出
- **`allow_nan`**：默认True，若为False则将NaN、Infinity转为null
- **`indent`**：设置缩进，美化输出
- **`separators`**：自定义分隔符，如( ',', ':' )减小文件体积
- **`default`**：自定义序列化函数

:four:`json.load()`：从json文件读取数据---反序列化

```python
import json

with open("student.json", "r", encoding="utf-8") as f:
    python_dict = json.load(f)
    
print("读取文件数据：", python_dict)
print("专业：", python_dict["major"])  # 人工智能
```

> 注意指定文件编码，否则可能乱码。

# csv模块

全称为 Comma-Separated Values，逗号分隔值，是一种简单、通用的==文本文件==格式，用于储存表格数据，可以使用excel打开。

csv以`, `分隔列数。

csv文本操作方式可以与操作txt一样。

```python
# 写入数据
with open('./python练习数据保存/csv_data.csv', 'w', encoding='utf-8') as f:
    f.write('姓名,性别,年龄,爱好\n') # 表头
    f.write('张三,男,18,python\n') # 数据
    f.write('李四,20,女,java\n')
    f.write('王五,22,男,c')

# 读取数据
with open('./python练习数据保存/csv_data.csv', 'r', encoding='utf-8') as f:
    csv_list = f.readlines()
print(csv_list)
```

也可以借助cvs模块。

```python
import csv

# open写入默认有一个换行符，传入newline以替代换行符
with open(file, 'w', encoding='utf-8', newline='') as f:
    # 写入内容，创建一个对象
    # 第一个参数f是写入的文件，第二个参数代表表头，使用列表写入
    writer = csv.DictWrite(f, fieldnames)
    
    # 写入表头，因为在创建的时候已经写了，所以这里没有传递参数
    writer.writerheader()
    
    # 写入单行内容
    # 字典的键对应每一个表头
    writer.writerow(dict)
    
# 读取数据
with open(file, 'r', encoding='utf-8') as f:
    # 创建对象
    reader = csv.DictReader(f)
    for row in reader:
        print(row) # row就是储存数据的字典
```

```python
import csv

with open('./python练习数据保存/csv_data2.csv', 'w', encoding='utf-8', newline='') as f:
    writer = csv.DictWriter(f, ['姓名','性别' ,'年龄' ,'爱好'])
    
    writer.writeheader()
    writer.writerow({'姓名':'张三','性别':'男','年龄':'18','爱好':'python'})
    writer.writerow({'姓名':'李四','性别':'女','年龄':'20','爱好':'java'})
    writer.writerow({'姓名':'王五','性别':'男','年龄':'22','爱好':'c'})

with open('./python练习数据保存/csv_data2.csv', 'r', encoding='utf-8') as f:
    reader = csv.DictReader(f)
    print(reader)
    for row in reader:
        print(row)
```

---

# tkinter模块

图形用户界面库。

一个 tkinter 程序通常包含以下几个核心部分：

1. **主窗口 (Root Window)**
   这是程序的“画布”，所有按钮、标签等元素都放在这个窗口里。通过 `tk.Tk()` 来创建。
2. **控件 (Widgets)**
   构成界面的基本元素，比如：
   - `Label`: 显示文本或图片。
   - `Button`: 可点击的按钮。
   - `Entry`: 单行文本输入框。
   - `Text`: 多行文本框。
3. **布局管理 (Geometry Management)**
   决定控件在窗口中的位置。tkinter 提供了三种布局方式：
   - **`pack()`**: 最简单，按顺序自动排列（上、下、左、右）。
   - **`grid()`**: 最常用，将窗口看作一个表格，用行（row）和列（column）来精确定位。
   - **`place()`**: 使用绝对坐标（x, y）进行定位，灵活性高但较少使用。
4. **事件循环 (Main Loop)**
   程序的“心脏”。通过调用 `mainloop()` 启动，它会让窗口持续显示，并监听用户的点击、输入等操作。

```python
import tkinter as tk

# 创建主窗口
root = tk.Tk()

root.title('标题') # 设置标题
root.geometry('num x num') # 设置窗口大小

# 3. 添加一个标签控件
label = tk.Label(root, text="Hello, World!", font=("Arial", 16))
label.pack()  # 使用 pack() 方法将标签放入窗口

# 4. 启动事件循环
root.mainloop()
```

| 控件类            | 功能描述         |
| ----------------- | ---------------- |
| `tk.Label`        | 显示文本或图片   |
| `tk.Button`       | 创建可点击的按钮 |
| `tk.Entry`        | 单行文本输入框   |
| `tk.Text`         | 多行文本编辑区   |
| `tk.Checkbutton`  | 复选框           |
| `ttk.Combobox`    | 下拉选择框       |
| `ttk.Progressbar` | 进度条           |

# Pandas

是一个功能强大的结构化数据分析的工具集，底层基于Numpy构建，适用于数据分析和大数据开发。

官网：https://pandas.pydata.org

## 核心：DataFrame（类似表格）、Series（类似表格中的一列）。

以下以表头为`str`为例。

构建DataFrame的方式有很多种。

```python
import pandas as pd

# 方法 1：列表套字典 (List of Dictionaries)
# 每个字典代表一行数据
df1 = pd.DataFrame([
    {'姓名': '张三', '语文': 85, '数学': 92, '英语': 78},
    {'姓名': '李四', '语文': 78, '数学': 88, '英语': 95},
    {'姓名': '王五', '语文': 92, '数学': 96, '英语': 89}
])

# 方法 2：字典套列表 (Dictionary of Lists)
# 键是列名，值是包含该列所有数据的列表
df2 = pd.DataFrame({
    '姓名': ['张三', '李四', '王五'],
    '语文': [85, 78, 92],
    '数学': [92, 88, 96],
    '英语': [78, 95, 89]
})

# 方法 3：列表套元组 (List of Tuples) + 指定列名
# 适合结构化数据，需要显式指定 columns 参数
df3 = pd.DataFrame([
    ('张三', 85, 92, 78),
    ('李四', 78, 88, 95),
    ('王五', 92, 96, 89)
], columns=['姓名', '语文', '数学', '英语'])

# 方法 4：列表套列表 (List of Lists) + 指定列名和索引
# 最通用的方式，同时指定 columns (列名) 和 index (行索引)
df4 = pd.DataFrame([
    ['张三', 85, 92, 78],
    ['李四', 78, 88, 95],
    ['王五', 92, 96, 89]
], columns=['姓名', '语文', '数学', '英语'], index=['a', 'b', 'c'])
```

DataFrame相关属性：

```python
from pandas import DataFrame as df

# 获取属性（不用加()）
df.index

# 获取列名
df.columns

# 获取数据
df.values

# 获取每一列的类型
df.dtypes

# 获取单元格的数量
df.size

# 获取数据的维度
# 返回包含行数与列数的元组
df.shape
```

Series也可以单独创建，因为只有一列，因此创建的Series没有列名。

```python
import pandas as pd

# 使用列表创建
df1 = pd.Series([10, 20, 30, 40])

# 使用元组创建
df2 = pd.Series((10, 20, 30, 40), index=['a', 'b', 'c', 'd']) # 指定索引

# 使用字典创建
df3 = pd.Series(
	{'a': 10, 'b': '20', 'c': '30', 'd': 40} # 字典的键就代表索引	
)
```

也可以通过DataFrame的列名获取Series。

Series的属性与DataFrame相同，只是没有columns属性。

## 数据的读取和写入

基于pandas的API，可方便的读取各类文件并写入。

```python
import pandas as pd

# 读取
# 传递usecols参数list[str]选择读取哪几列
pd.read_csv(file, usecols=None)
pd.read_xlsx()
...

# 写入
pd.to_csv()
pd.to_xlsx()
...
```

## 数据查看

```python
from pandas import DataFrame as df

# 查看前n行数据
df.head(n)

# 查看结尾n行数据
df.tail(n)

# 数值列的统计描述
df.describe()

# 查看数据信息（列名、非空计数、数据类型）
df.info()

# 数据维度
df.shape

# 列名
df.columns
```

## 数据选择

```python
from pandas import DataFrame as df

# 操作单列
df['列名']
df.列名 # 不推荐

# 操作多列
df[['列名1', '列名2']]

# 基于行号完成切片（不包括stop），从0开始
# iloc --> integer location
df.iloc[start:stop:step]

# 基于索引标签完成切片（含stop）
df.loc[start:stop:step]
```

## 数据过滤

```python
from pandas import DataFrame as df

# 单条件，最外面[]里是大的条件，返回布尔索引序列，通过df返回布尔索引序列是True的行
df[条件表达式]

df[df['列名'].isin(['名称1', '名称2'])] # 筛选出这个列里面包含某个名称的行

df[df['列名'].between(a, b)] # 筛选出在ab之间的行，可以传递inclusive参数选择包不包含ab，默认包含

# 多条件，每个单条件需要使用()组成一个整体
& # 并且
| # 或者
```

## 数据清洗

指发现并纠正数据中可识别的错误的过程，包括处理缺失值、重复值、异常值，统一数据格式，保证数据的一致性。

缺失值：

```python
from pandas import DataFrame as df

# 查看是否缺失
df.isnull()

# 删除缺失值，返回值，并不改变原df
df.dropna()

# 填充缺失值
df.fillna(value)

# 使用上一行的数据填充
df.ffill()

# 使用下一行的数据填充
df.bfill()
```

重复值：

```python
from pandas import DataFrame as df

# 查看重复值，默认所有行的数据全都一样才算重复，可以传递subset参数
df.dupliacted()

# 删除重复值，keep代表保留哪一行的
df.drop_dupliactes(subset, keep)
```

异常值：

```python
from pandas import DataFrame as df
from typing import Literal

# 删除
df.drop(columns|index, errors=Literal['raise', 'ignore'], inplace=bool)
# 多个列或行使用[]
# errors表示没存在的话是否报错，inplace表示是否原地删除

# 修复根据需要选择方法
# 替换，根据单元格进行替换，返回值，不对df修改
df['列名'].replace(to_replace, value)
df[条件表达式] = ...
# 也可以使用访问器
df['列名'].str # 可以调用字符串的方法
```

## 数据排序

```python
from pandas import DataFrame as df

# 按照值进行排序，ascending代表上升，默认为True
#value为str或者list[str]，ascending为bool或者list[bool]
df.sort_values(value, ascending)
```

## 数据分组

```python
from pandas import DataFrame as df

# 分组
df.groupby(by).agg([function1, function2]) # 聚合方法，可以同时执行多个函数
df.groupby(by).agg({'列名': function1, '列名': function2}) # 针对某一列进行操作
```

## 盖帽法

```python
from pandas import DataFrame as df
from pandas import Series

# 假设我们要处理 '票房' 这一列的异常值
Q1: float = df['票房'].quantile(0.25)
Q3: float = df['票房'].quantile(0.75)
IQR = Q3 - Q1
# quantile表示计算分位数，把一串数据排序，取第几个，0.25就是取排在25%的位置的值，没找到则返回两边的平均值

# 计算边界
lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

# 核心代码：小于下限的变下限，大于上限的变上限
df['票房'] = df['票房'].clip(lower_bound, upper_bound)


# 也可以定义一个类似的函数
def cap_outliers(series: Series, limits=(0.01, 0.99)):
    """
    使用分位数盖帽法
    limits: 默认取 1% 和 99% 分位数作为边界
    """
    lower_limit, upper_limit = series.quantile(limits)
    # 这里的 copy() 是为了避免 SettingWithCopyWarning 警告
    series_copy = series.copy()
    series_copy[series_copy < lower_limit] = lower_limit
    series_copy[series_copy > upper_limit] = upper_limit
    return series_copy

# 使用
df['票房'] = cap_outliers(df['票房'])
```



# Matplotlib

## 介绍

是一个全面的库，用于python创建静态、动画和交互可视化的图标。

## 相关术语

- 画布（figure）：需要设置在plot之前
- 标题（title）
- X轴（Xaxis）
- Y轴（Yaxis）
- X轴标签（Xlabel）
- Y轴标签（Ylabel）
- X轴刻度（Xtick）
- X轴刻度标签（Xtick_label）
- Y轴刻度（Ytick）
- Y轴刻度标签（Ytick_label）
- 网格线（grid）
- 图例（legend）

## 折线图

大部分参数没有提示，需要查阅官方文档或者使用help函数。

```python
import matplotlib.pyplot as plt
import random

# 展示中文（黑体）
plt.rcParams['font.sans-serif'] = ['SimHei']

x = [tem for tem in range(24)]
y_1 = [random.randint(0, 24) for i in x]
y_2 = [random.randint(10, 24) for i in x]

plt.figure(figsize=(10, 5))
plt.plot(x, y_1, label='北京')
plt.plot(x, y_2, label='西安')
plt.title('一天中的气温随时间变化图')
plt.xlabel('时间') # 可以传递rotation参数
plt.ylabel('温度')
plt.xticks(range(0, 24, 2))
plt.tick_params(axis, rotation) # 调整刻度的样式，axis选择哪个轴，rotation是旋转角度
plt.yticks(range(0, 24, 2))
plt.grid(linestyle, alpha) # 位置参数，设置网格线格式和透明度
plt.legend()
plt.show()
```

创建多个画布：

```python
import matplotlib.pyplot as plt

figure, axes = plt.subplots(nrows, ncols, figsize=(width, height), dpi, squeeze=False)
'''
nrows, ncols代表图表的行与列，axes代表由图表索引组成的元组，dpi代表像素
squeeze=False保证子图是二维的
'''
```

## 柱状图

```python
import matplotlib.pyplot as plt
from matplotlib.axes import Axes

figure, axes = plt.subplots(1, 3, figsize=(20, 5), dpi=100)

# 创建柱状图
axes1: Axes = axes[0] # 指定变量类型，方便有提示
# 修饰子图需要使用set_xxx
countries = ['中国', '美国', '日本', '德国', '法国', '印度', '沙特阿拉伯']
oil = [100, 80, 60, 50, 40, 30, 20]

axes1.set_title('石油储备', fontsize=15)
axes1.bar(countries, oil, 0.3, color='gold') # 后面两个设置宽度和颜色
axes1.grid(linestyle='--', alpha=0.5)
axes1.set_xlabel('国家')
axes1.set_ylabel('石油量')
```

## 饼状图

```python
import matplotlib.pyplot as plt
from matplotlib.axes import Axes

figure, axes = plt.subplots(1, 3, figsize=(20, 5), dpi=100)

countries2 = ['中国', '美国', '日本', '德国', '法国', '印度', '沙特阿拉伯', '其他']
population = [14.00, 3.30, 1.25, 0.83, 0.67, 13.80, 0.34, 67.88]

axes2: Axes = axes[1]

# 执行绘图命令，数据写入 axes2
wedges, texts, autotexts = axes2.pie(
    population, 
    labels=countries2,
    autopct='%1.1f%%',      # 显示百分比，%在autopct有特殊含义，因此需要转义，第一个是最小字段占位符，第二个是小数位数
    startangle=90,          # 起始角度，默认为0且顺时针绘制
    pctdistance=0.85,        # 百分比距离圆心的距离
    radius=1			# 半径，默认为1，不推荐改
)

axes2.legend(loc, ncol, bbox_to_anchor=(horizontal, vertical)) # 第一个是图例基准位置，第二个是图例分几栏，第三个是图例具体位置

# 优化字体大小，防止重叠
for text in texts:
    text.set_fontsize(8)
for autotext in autotexts:
    autotext.set_fontsize(8)

axes2.set_title('各国人口占比', fontsize=15)
```

## 保存至图片

```python
import matplotlib.pyplot as plt

plt.savfig(path) # 之前不能有plt.show()，不然保存的是一张空白图片
```

# Streamlit学习

## 介绍

Streamlit是一个开源的python库，专为数据工程师和机器学习工程师设计，用来快速基于python代码构建交互式的web网站。

## 官网

https://streamlit.io

## 基本​语法

```python
import streamlit as st

# 标题
st.title()

# 一级标题
st.header()

# 二级标题
st.subheader()

# 段落
st.write()

# 图片
st.image()
'''
width = num 图片大小
'''

# 音频
st.audio() # 可用 .\ 代表当前目录

# 视频
st.video()

# logo
sr.logo() # 显示在网页左上角

# 分割线
st.divider()

# 表格
data = {key1: [], key: []} # 每个key代表一行
st.table(data)

# 输入框
st.text_input(label: str)
# label：告诉用户这个输入框需要填什么
# 一旦发送，文件会重新运行

# 单选按钮
st.radio(label: str, options, index = 0)
# label：告诉用户这个选择是什么
# options：选项
# index：默认选择的选项，默认值为0，也就是第一个选项。设置为None可保持不勾选

# 缓存
st.session_state
# 类似与字典的缓存内容，在多次重新运行时数据不会被覆盖
# 调用时可以st.session_state['key']也可以st.session_state.key

# 重新运行
st.rerun()

# 运行
streamlit run xxx.py
```

示例：

```python
import streamlit as st

st.title('网站')
st.divider()
st.header('一级')
st.subheader('二级')

st.write('你好')
st.text('我是谁？')

data = {'张三': 100, '王五': 100}
st.table(data)

st.image(r'D:\Screen\cat.jpg')

st.audio(r'D:\Screen\River Flows in You.mp3')

st.video(r'D:\Screen\Gravity (2).mp4')

st.logo(r'D:\Screen\cat.jpg')

name= st.text_input('请输入姓名')
if name:
    st.write(f'您的姓名是{name}')

id = st.radio('你的身份是：', ['狼人', '平民', '预言家'], index = None)

def check():
    st.session_state['msg'] = id

st.button('查看身份', on_click = check)

if 'msg' in st.session_state:
    st.write(st.session_state['msg'])

```

![streamlit结果展示](./python%E4%B8%AD%E7%9A%84%E6%A8%A1%E5%9D%97.assets/image-20260406193311057.png)

## 页面设置

```python
import streamlit as st

#设置页面配置项
st.set_page_config(
    
    # 网页的名字
	page_title = str,
    
    # 网页的图标
    page_icon = page_icon,
    
    # 网页布局
    layout = Literal["centered", "wide"],
    
    # 侧边栏布局
    initial_sidebar_state = Literal["auto", "expanded", "collapsed"] | int，
    
    # 帮助菜单
    menu_items = {
        Mapping[MenuKey, str | None]
    }
    # MenuKey = Literal['get help', 'report a bug', 'about']，第一个字母可以大写，这里就不写了
    # 第一个是键，第二个是值。前两个的值必须为url。
)
```

示例：

```python
st.set_page_config(
    page_title = '我的网页',
    page_icon = '🌤',
    layout = 'wide',
    initial_sidebar_state = 'expanded',
    menu_items = {
        "Get help": 'https://www.bilibili.com',
        "Report a bug": 'https://www.baidu.com',
        "about": '不知道'
    }
)
```

![页面布局](./python%E4%B8%AD%E7%9A%84%E6%A8%A1%E5%9D%97.assets/image-20260406195856122.png)

## 适用范围

用于快速基于python代码构建web网页的python库，主要在数据科学以及机器学习领域。

# FastAPI

## web基础

1. 定义

   全球广域网，也称万维网（www World Wide Web），能够通过浏览器访问到的网站。

2. 核心

   - 前端程序

     > 负责界面展示。网页由HTML（结构）、CSS（样式）、JavaScript（动作）组成。

   - 服务端程序

     > 负责业务逻辑处理。可以基于python中的Django、Flask或者FastAPI来进行开发。

   - 数据库

     > 负责数据存储和管理。基础知识中数据存储在文件中。

## 简介

[FastAPi](https://fastapi.org.cn)是一个现代、快速、高性能的Web框架，用于基于标准的python类型提示构建API接口服务。

快速使用步骤：

1. 导入FastAPI

2. 创建FastAPI实例对象

3. 创建路径操作函数，定义访问路径

4. 运行FastAPI服务

   > `fastapi dev .py文件`
   >
   > `uvicorn 文件:fastapi实例 --reload`文件不需要.py后缀
   >
   > ```python
   > import uvicorn
   > uvicorn.run(app, host, port)
   > ```

**代码演示**

```python
# 导入包
from fastapi import FastAPI
import uvicorn

# 创建fastapi实例
app = FastAPI()

# 接口访问路径为 / ，请求方式为GET
@app.get('/')
def root() -> dict:
    """
    定义路由（接口）：根据访问路径调用函数

    Returns
    -------
    dict
        API接口返回的数据
    """
    return {"message": "Hello World"}


@app.get('/users')
def users() -> list:
    """
    获取用户数据

    Returns
    -------
    list
        用户数据
    """
    return [
        {'id': '0001', 'name': '张三'},
        {'id': '0002', 'name': '李四'},
        {'id': '0003', 'name': '王五'}
    ]

# 使用uvicorn启动服务
"""
uvicorn是python中的轻量web服务器
"""
if __name__ == "__main__":
    uvicorn.run(app, host='0.0.0.0')
    """
    host默认是127.0.0.1，只能在本机上访问。设置为0.0.0.0则是所有的都能访问。
    port（端口号）默认是8000。
    """
```

## Restful规范

指遵循REST架构风格的API接口服务，而REST（REpresentational State Transfer，表述形状态转换）是一种软件架构风格。

| 传统风格url                                  | 请求方式 | 含义              |
| -------------------------------------------- | -------- | ----------------- |
| `http://localhost:8000/user/getById?id=1`    | GET      | 查询id为1的用户。 |
| `http://localhost:8000/user/saveUser`        | POST     | 新增用户。        |
| `http://localhost:8000/user/updateUser`      | POST     | 修改用户。        |
| `http://localhost:8000/user/deleteUser?id=1` | GET      | 删除id为1的用户。 |

> 传统风格很难维护，因为每个程序员习惯不同，定义的动词也不同。

| REST风格url                     | 请求方式 | 含义              |
| ------------------------------- | -------- | ----------------- |
| `http://localhost:8000/users/1` | GET      | 查询id为1的用户。 |
| `http://localhost:8000/users/1` | DELETE   | 删除id为1的用户。 |
| `http://localhost:8000/users`   | POST     | 新增用户。        |
| `http://localhost:8000/users`   | PUT      | 修改用户。        |

URL定位资源 HTTP动词描述操作 简洁、规范、优雅。
