# 介绍

`lxml` 是 Python 生态中性能最强悍、功能最完备的 HTML/XML 解析库。它的底层基于 C 语言编写的 `libxml2` 和 `libxslt` 库，因此解析速度远超纯 Python 实现的库（如 BeautifulSoup），在大规模数据抓取和复杂文档处理时优势极其明显。

# lxml 核心功能与常用方法总结表

| 核心模块/方法        | 数据类型/参数      | 核心作用与爬虫实战场景                                       |
| -------------------- | ------------------ | ------------------------------------------------------------ |
| `etree.parse()`      | 文件路径 (str)     | 直接从本地文件解析 XML 或 HTML。<br />适合处理本地保存的网页源码或配置文件。 |
| `etree.fromstring()` | 字符串 (str)       | 从字符串中解析 XML 或 HTML。<br />爬虫中常与 `requests` 配合，直接解析 `response.text`。 |
| `html.fromstring()`  | 字符串 (str)       | 专为 HTML 设计的解析器。<br />具备极强的容错能力，能自动修复缺失闭合标签、编码混乱等不规范 HTML。 |
| `.xpath()`           | 表达式 (str)       | **数据提取的核心利器**。<br />支持完整的 XPath 1.0 语法，能通过标签、属性、层级精准定位并提取节点文本或属性值。 |
| `.cssselect()`       | 选择器 (str)       | 类似 jQuery 的 CSS 选择器。<br />对前端开发者更友好，直观匹配 HTML 元素（需额外安装 `cssselect` 库）。 |
| `etree.tostring()`   | 元素对象 (Element) | 将解析后的元素树序列化回字符串（可带缩进）。<br />常用于调试查看解析结果，或将修改后的文档保存。 |
| `etree.Element()`    | 标签名 (str)       | 创建新的 XML/HTML 根元素。<br />配合 `SubElement()` 可以动态构建、修改文档结构。 |

# etree 库

## 介绍

`lxml.etree` 的底层基于 C 语言编写的 `libxml2` 和 `libxslt` 库，因此在性能、稳定性、兼容性及功能完整性方面，远超 Python 内置的 `xml.etree.ElementTree` 以及 BeautifulSoup 等纯 Python 解析器。它不仅完整支持 XML 1.0 与 1.1 规范，还全面兼容 HTML5 解析，能够自动修复不规范标签、闭合缺失标签、纠正编码错误，并将 HTML 文档转换为结构清晰、可 XPath 查询的标准树形对象。

## 核心功能与实战技巧总结表

| 核心功能/方法        | 核心作用与爬虫实战场景                                       |
| -------------------- | ------------------------------------------------------------ |
| **解析与生成**       | 支持 `etree.HTML()` 解析 HTML，<br />`etree.XML()` 解析 XML；<br />支持动态创建节点（`etree.Element()`）、追加（`append()`）、插入（`insert()`）、删除（`remove()`）及替换节点。 |
| **XPath 属性定位**   | 支持复合条件匹配，如 `[@class="item"]`、`[@id and @data-id]`、`[@class="active" or @class="highlight"]`。 |
| **XPath 文本定位**   | 使用 `//div/text()` 获取直接子文本，<br />`//p//text()` 递归提取所有文本节点；<br />结合 `normalize-space()` 去除首尾空白与多余换行。 |
| **XPath 层级与索引** | 灵活运用 `/`（子元素）、`//`（任意后代）、`../`（父节点）、`following-sibling::`（后续同级）等轴表达式；支持 `//li[1]`（首个）、`//tr[position()>1]`（排除表头）等数组语法。 |
| **XPath 模糊匹配**   | 依赖字符串函数组合，如 `contains(@href,"/product/")`、`starts-with(@src,"https://cdn.")`；配合 `re:match()` 可实现正则校验类名模式。 |
| **序列化输出**       | 通过 `etree.tostring(elem, encoding="utf-8", pretty_print=True, method="html")` 实时将变更后的节点序列化为格式化 HTML/XML 字符串。 |
| **大文件流式解析**   | 提供 `iterparse()` 实现流式大文件解析，内存占用恒定 O(1)，有效避免因处理 GB 级 XML 导致的内存溢出（OOM）。 |

## 综合实战代码演示

以下代码块展示了 `lxml.etree` 在爬虫全栈流程中的核心应用，涵盖了从解析、多策略 XPath 定位到数据提取与修改的完整操作：

```python
from lxml import etree
import copy

# 模拟一段包含不规范标签和复杂嵌套的 HTML 源码
html_string = """
<div id="container">
    <div class="item active" data-id="101">
        <a href="/product/iphone-15">iPhone 15 官方介绍</a>
        <span>  价格：7999元  </span>
    </div>
    <div class="item disabled" data-id="102">
        <a href="/product/iphone-14">iPhone 14 官方介绍</a>
        <span>  价格：6999元  </span>
    </div>
    <img src="https://cdn.example.com/banner.jpg" alt="Banner"/>
</div>
"""

# ====================== 1. 解析 HTML 并自动修复 ======================
# etree.HTML() 会自动补全缺失的 html/body 标签并纠正编码
root = etree.HTML(html_string)

# ====================== 2. XPath 多策略精准定位与提取 ======================
# 【属性与模糊匹配】抓取含特定URL路径且链接文字含关键词的商品入口
products = root.xpath('//a[contains(@href,"/product/") and contains(text(),"iPhone")]')
for p in products:
    print(f"商品链接: {p.get('href')}, 商品名称: {p.text}")

# 【层级与文本清洗】提取价格并去除首尾空白
prices = root.xpath('//div[@class="item"]/span/text()')
for price in prices:
    print(f"原始价格: '{price}', 清洗后: '{price.strip()}'")

# 【索引与复合谓词】获取第一个非 disabled 状态的 item 的 data-id
first_active_id = root.xpath('//div[@class="item" and not(contains(@class,"disabled"))][1]/@data-id')
print(f"第一个有效商品ID: {first_active_id[0]}")

# ====================== 3. 动态修改节点与序列化输出 ======================
# 找到第一个 item，动态修改其属性
first_item = root.xpath('//div[@class="item"]')[0]
first_item.set('data-status', 'crawled')  # 添加新属性

# 深度克隆一个节点
cloned_item = copy.deepcopy(first_item)

# 将修改后的文档树实时序列化为格式化的 HTML 字符串
result_html = etree.tostring(root, encoding="utf-8", pretty_print=True, method="html")
print("\n修改并序列化后的 HTML 结构：")
print(result_html.decode("utf-8"))
```

# html 库

## 介绍

如果说 `lxml.etree` 是处理 XML/HTML 的通用基石，那么 `lxml.html` 就是专门为**网络爬虫和网页解析**量身定制的“特种兵”。它底层依然基于 `lxml.etree`，但针对 HTML 的混乱特性进行了大量的增强和封装，是你在爬虫实战中处理网页源码时最顺手的工具。

## lxml.html 核心特性与常用方法总结表

| 核心功能/方法        | 核心作用与爬虫实战场景                                       |
| -------------------- | ------------------------------------------------------------ |
| **超强容错解析**     | 自动修复破损标签、缺失闭合标签、自闭合标签误用，并能智能识别并纠正编码混乱（如 UTF-8/GBK 自动探测）。 |
| **CSS 选择器支持**   | 原生集成 `.cssselect()` 方法，支持类似 jQuery 的 CSS 选择器语法，极大降低了前端背景开发者的学习门槛。 |
| **安全过滤 (clean)** | 内置 `clean` 模块，能够一键过滤 XSS 危险标签与属性，清洗网页中嵌入的恶意脚本或样式干扰内容。 |
| **链接提取**         | 提供便捷的链接提取接口，能快速遍历并获取页面中所有的超链接（`<a>` 标签的 `href` 属性）。 |
| **表单处理**         | 内置表单解析工具，可以自动识别网页中的表单字段、输入框及提交按钮，极大简化模拟登录和表单提交的爬虫逻辑。 |
| **树形导航与遍历**   | 提供高度一致的 Element 类接口，支持 `parent`（父节点）、`children`（子节点）、`siblings`（兄弟节点）等树形导航。 |

## 综合实战代码演示

以下代码块展示了 `lxml.html` 在爬虫实战中的独特优势，涵盖了容错解析、CSS 选择器提取、安全清洗及表单处理：

```python
from lxml import html, etree

# 模拟一段极其不规范、包含缺失标签和潜在 XSS 风险的网页源码
dirty_html = """
<html>
<head><title>测试页面</title></head>
<body>
    <!-- 缺失闭合标签的 div -->
    <div class="content">
        <p>这是一段正常的文本。
        <p>这是另一段文本。
        <!-- 潜在的 XSS 攻击脚本 -->
        <script>alert('XSS Attack!');</script>
    </div>
    
    <!-- 一个简单的登录表单 -->
    <form action="/login" method="post">
        <input type="text" name="username" value="test_user" />
        <input type="password" name="password" />
        <button type="submit">登录</button>
    </form>
</body>
</html>
"""

# ====================== 1. 容错解析与自动修复 ======================
# lxml.html 能够完美处理缺失闭合标签的 HTML，并自动补全树结构
doc = html.fromstring(dirty_html)
print("自动修复后的段落文本：")
for p in doc.cssselect('p'):
    print(p.text)

# ====================== 2. 安全清洗 (过滤 XSS) ======================
# 使用 clean 模块移除危险的 script 标签
from lxml.html.clean import Cleaner
cleaner = Cleaner(scripts=True, javascript=True)
clean_doc = cleaner.clean_html(doc)
# 检查 script 标签是否被成功移除
if not clean_doc.xpath('//script'):
    print("\n安全清洗成功：危险的 script 标签已被移除！")

# ====================== 3. 表单处理 ======================
# 自动识别页面中的表单
forms = doc.forms
for form in forms:
    print(f"\n表单提交地址: {form.action}")
    # 提取表单内的所有输入字段
    for input_elem in form.inputs:
        if input_elem.name:
            print(f"字段名: {input_elem.name}, 默认值: {input_elem.value}")

# ====================== 4. CSS 选择器与链接提取 ======================
# 使用 CSS 选择器快速匹配元素
content_div: list[HtmlElement] = doc.cssselect('div.content')[0]
print(f"\n内容区块的文本: {content_div.text_content()}") # 和 xpath 一样没有提示（不加注解）

# 提取页面所有链接（如果页面有 a 标签）
links = doc.links  # 返回一个包含所有 href 的集合
print(f"页面包含的链接数: {len(links)}")
```

# 综合实战代码演示

以下代码块模拟了爬虫中“解析网页”与“提取数据”的最常用场景，你可以直接复制到编辑器中运行测试：

```python
from lxml import etree, html

# 模拟 requests 获取到的网页 HTML 源码
html_string = """
<html>
    <head>
        <title>Python爬虫练习页面</title>
    </head>
    <body>
        <h1>欢迎来到 lxml 的世界</h1>
        <div class="article-list">
            <div class="item" id="item-1">
                <a href="https://example.com/article1">第一篇技术文章</a>
                <span class="date">2026-06-20</span>
            </div>
            <div class="item" id="item-2">
                <a href="https://example.com/article2">第二篇实战教程</a>
                <span class="date">2026-06-21</span>
            </div>
        </div>
    </body>
</html>
"""

# ====================== 1. 解析 HTML 字符串 ======================
# 使用 lxml.html 的容错解析器，即使网页标签不规范也能完美解析
doc = html.fromstring(html_string)

# ====================== 2. 使用 XPath 精准提取数据 ======================
# 提取页面标题
page_title = doc.xpath('//title/text()')[0]
print(f"页面标题: {page_title}")

# 提取所有文章的链接和标题
# '//div[@class="item"]/a' 表示查找所有 class 为 item 的 div 下的 a 标签
articles = doc.xpath('//div[@class="item"]/a')
for article in articles:
    title = article.text
    link = article.get('href')  # 获取标签的属性值
    print(f"文章标题: {title}, 链接: {link}")

# ====================== 3. 使用 CSS 选择器提取数据 ======================
# 提取所有 class 为 date 的 span 标签文本（需安装 cssselect: pip install cssselect）
dates = doc.cssselect('span.date')
for date in dates:
    print(f"发布日期: {date.text}")

# ====================== 4. 修改与序列化文档 ======================
# 假设我们要修改第一个 h1 标签的内容
h1_tag = doc.xpath('//h1')[0]
h1_tag.text = "lxml 数据提取实战"

# 将修改后的文档树转换回字符串并打印（pretty_print=True 会自动美化排版）
print("\n修改后的 HTML 结构：")
print(etree.tostring(doc, pretty_print=True, encoding='unicode'))
```