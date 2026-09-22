# 介绍

`requests` 库是 Python 生态中最流行、最基础的第三方 HTTP 客户端库，它的核心理念是“HTTP for Humans”（让 HTTP 服务于人类）。相比 Python 内置的 `urllib`，它的 API 设计极其简洁直观，能够自动处理 URL 编码、连接池管理、Cookie 处理等底层细节，让你能专注于爬虫业务逻辑的实现。

# 核心请求函数速查表

| 核心函数            | 核心用途                      | 常用关键参数                          |
| ------------------- | ----------------------------- | ------------------------------------- |
| `requests.get()`    | 获取网页源码、JSON 数据       | `url`，`params`，`headers`，`timeout` |
| `requests.post()`   | 模拟表单提交、登录、发送 JSON | `url`，`data`，`json`，`headers`      |
| `requests.head()`   | 仅获取响应头（不下载内容）    | `url`，`headers`                      |
| `requests.put()`    | 完整更新服务器上的资源        | `url`，`data`                         |
| `requests.delete()` | 向服务器发送删除请求          | `url`                                 |

> **参数**
>
> | 参数名称          | 数据类型                       | 核心作用与爬虫实战场景                                       |
> | ----------------- | ------------------------------ | ------------------------------------------------------------ |
> | `url`             | 字符串 (str)                   | 请求的目标地址。<br />爬虫的起点，可以是网页链接或 API 接口。 |
> | `headers`         | 字典 (dict)                    | 伪装浏览器的核心。<br />用于设置 `User-Agent`（伪装浏览器）、<br />`Cookie`（保持登录）、<br />`Referer`（防盗链）等。 |
> | `params`          | 字典 (dict)                    | 自动处理 URL 查询参数（即 URL 中 `?` 后面的键值对）。<br />库会自动进行 URL 编码并拼接，避免手动拼接出错。 |
> | `data`            | 字典/字节                      | 作为请求体发送，常用于 模拟表单提交（如登录、搜索）。<br />提交时默认 Content-Type 为 `application/x-www-form-urlencoded`。 |
> | `json`            | 字典/列表                      | 作为请求体发送，常用于 提交 JSON 数据（现代网站 API 常用）。<br />库会自动将其序列化为 JSON 字符串并设置对应的 Content-Type。 |
> | `timeout`         | 整型/元组                      | 设定请求的超时时间（秒）。<br />爬虫必备，防止因目标服务器无响应导致程序长时间卡死。 |
> | `proxies`         | 字典 (dict)                    | 设置代理服务器。<br />当 IP 被目标网站封禁或需要突破网络限制时，通过代理更换 IP 发起请求。 |
> | `cookies`         | 字典/CookieJar                 | 手动携带 Cookie 信息。<br />常用于在多个请求之间保持用户的登录状态或特定会话。 |
> | `allow_redirects` | 布尔值 (bool)                  | 默认为 `True`。是否允许自动跟随重定向。<br />若设为 `False`，可获取重定向前的响应头（如获取真实的跳转链接）。 |
> | `verify`          | 布尔值（bool），字符串（str）  | 控制 SSL 证书验证。<br />可选值为 `true`（默认验证）、`false`（跳过验证）或 CA 证书路径 |
> | `stream`          | 布尔值（bool）                 | 控制响应内容的下载方式。<br />可选值为 `false`（默认立即下载）、<br />`true`（延迟下载，用于流式读取） |
> | `auth`            | 元组（tuple），`AuthBase` 子类 | HTTP 认证信息，如 `('username', 'password')` 或<br /> `HTTPBasicAuth` 实例 |

## 核心模块与类

`requests` 库的内部架构高度模块化，主要包含以下核心模块与类：

1. `api` 模块

   暴露了 `get()`、`post()` 等顶层快捷函数，内部本质是调用 `Session.request()`。

2. `sessions` 模块

   包含核心的 `Session` 类，负责维护连接池、全局 Headers、Cookies 及请求生命周期。

3. `models` 模块

   定义了 `Request`、`PreparedRequest` 及 `Response` 对象，封装了请求与响应的完整数据结构。

4. `adapters` 模块

   包含 `HTTPAdapter` 类，作为 `requests` 与底层 `urllib3` 库的桥梁，负责连接池管理与重试机制。

5. `exceptions` 模块

   定义了完整的异常体系，如 `RequestException`、`ConnectionError`、`Timeout` 等。

6. `cookies` 模块

   包含 `RequestsCookieJar` 类，用于跨域、跨路径的 Cookie 管理。

7. `auth` 模块

   提供 `HTTPBasicAuth`、`HTTPDigestAuth` 等认证实现类。

# Response 响应对象的常用属性与方法

| 名称                       | 核心作用与说明                                               |
| -------------------------- | ------------------------------------------------------------ |
| `status_code`              | 获取 HTTP 响应状态码（如 `200`、`404`、`500`）               |
| `text`                     | 获取 Unicode 格式的字符串响应内容，自动根据 `encoding` 解码  |
| `content`                  | 获取原始的二进制字节流（`bytes`），适用于图片、视频等非文本文件 |
| `headers`                  | 获取服务器返回的响应头（`CaseInsensitiveDict` 字典，键名不区分大小写） |
| `encoding`                 | 获取或手动设置响应内容的编码格式（如 `utf-8`、`gbk`）        |
| `cookies`                  | 获取服务器返回的 Cookie 对象（`RequestsCookieJar`）          |
| `url`                      | 获取最终实际访问的 URL（若发生重定向，返回重定向后的地址）   |
| `history`                  | 获取重定向历史列表，按时间顺序记录所有中间响应对象           |
| `json()`                   | 自动将 JSON 格式的响应体解析为 Python 字典或列表             |
| `raise_for_status()`       | 若状态码表示请求失败（`4xx` 或 `5xx`），自动抛出 `HTTPError` 异常 |
| `iter_content(chunk_size)` | 以迭代器形式逐块读取响应内容，需配合 `stream=true` 使用      |
| `close()`                  | 关闭与服务器的连接，释放底层资源                             |

# 综合实战代码演示

以下代码块综合展示了 `requests` 库在爬虫中最常用的操作场景，包含了 GET/POST 请求、参数传递、异常处理及数据提取：

```python
import requests

# ====================== 1. 基础 GET 请求与伪装浏览器 ======================
url = "https://httpbin.org/get"
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36"
}
params = {"keyword": "python爬虫", "page": 1}  # URL查询参数

try:
    response = requests.get(url, headers=headers, params=params, timeout=10)
    response.raise_for_status()  # 检测请求是否成功，失败则抛出异常
    print("状态码:", response.status_code)
    print("实际访问的URL:", response.url)
    print("响应内容:", response.text)
except requests.exceptions.RequestException as e:
    print(f"请求出错: {e}")

# ====================== 2. POST 请求模拟表单提交 ======================
post_url = "https://httpbin.org/post"
form_data = {"username": "test_user", "password": "123456"}

post_resp = requests.post(post_url, data=form_data, headers=headers)
# 如果服务器返回的是 JSON 数据，可以直接解析为字典
print("解析后的JSON数据:", post_resp.json())

# ====================== 3. 解决中文乱码与下载二进制文件 ======================
# 假设爬取某网页出现乱码，可以修正编码
# response.encoding = response.apparent_encoding 

# 下载图片（使用 content 获取二进制流）
img_url = "https://httpbin.org/image/png"
img_resp = requests.get(img_url, headers=headers)
if img_resp.status_code == 200:
    with open("downloaded_image.png", "wb") as f:
        f.write(img_resp.content)
    print("图片下载成功！")
```