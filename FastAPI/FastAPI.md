# 介绍

基于 python 的高性能 Web框架，专门用于快速构建 API 接口服务。

- 原生异步支持，释放真正性。
- 在根目录后加 `/docs` 进入交互界面。 

> **运行项目**
>
> - 在命令行中 `univcorn 文件路径:FastAPI实例名 --reload`
> - `univcorn.run(FastAPI实例, host)`

# 框架入口

`FastAPI` 类是整个 FastAPI 框架的核心入口，用于创建 Web 应用程序实例。通过实例化这个类，可以定义路由、配置中间件、设置 API 文档元数据等，它是所有接口开发的基石。

## 创建对象时的格式

```python
app = FastAPI(
    *,  # 强制后续参数必须使用关键字传参
    debug=False,  # 是否开启调试模式，错误时返回详细跟踪信息
    routes=None,  # 自定义路由列表（通常不建议直接使用）
    title="FastAPI",  # API 的标题，显示在自动生成的文档中
    summary=None,  # API 的简短摘要，显示在文档中
    description="",  # API 的详细描述，支持 Markdown 格式
    version="0.1.0",  # API 的版本号（非 FastAPI 框架版本）
    openapi_url="/openapi.json",  # OpenAPI 架构的 URL，设为 None 可禁用文档
    openapi_tags=None,  # 用于文档的 OpenAPI 标签列表，便于对接口分组
    servers=None,  # 服务器列表，描述 API 部署的地址
    default_response_class=Default(JSONResponse),  # 默认的响应类
    redirect_slashes=True,  # 是否自动重定向带或不带尾部斜杠的路径
    docs_url="/docs",  # Swagger UI 文档的路径，设为 None 可禁用
    redoc_url="/redoc",  # ReDoc 文档的路径，设为 None 可禁用
    swagger_ui_oauth2_redirect_url="/docs/oauth2-redirect",  # Swagger UI 的 OAuth2 重定向 URL
    swagger_ui_init_oauth=None,  # Swagger UI 初始化 OAuth2 的配置字典
    middleware=None,  # 中间件列表
    exception_handlers=None,  # 自定义异常处理器字典
    on_startup=None,  # 应用启动时执行的回调函数列表
    on_shutdown=None,  # 应用关闭时执行的回调函数列表
    lifespan=None,  # 上下文管理器，用于处理应用启动和关闭的生命周期事件
    terms_of_service=None,  # 服务条款的 URL，显示在文档中
    contact=None,  # 联系人信息字典（包含 name、url、email）
    license_info=None,  # 许可证信息字典（包含 name、url）
    root_path_in_servers=True,  # 是否在生成的服务器 URL 中包含 root_path
    responses=None,  # 全局额外的响应定义，合并到 OpenAPI 文档中
    callbacks=None,  # 全局回调定义
    webhooks=None,  # 全局 Webhooks 定义
    deprecated=None,  # 是否将整个 API 标记为已弃用
    include_in_schema=True,  # 是否将路由包含在 OpenAPI 架构中
    swagger_ui_parameters=None,  # 传递给 Swagger UI 的额外配置参数
    generate_unique_id_function=Default(generate_unique_id),  # 生成操作唯一 ID 的函数
    separate_input_output_schemas=True,  # 是否为输入和输出生成独立的 OpenAPI 架构
    openapi_external_docs=None,  # 外部文档链接字典（包含 url、description）
    strict_content_type=True,  # 是否严格检查请求的 Content-Type 头
    **extra  # 允许传入额外的键值对，会作为自定义元数据添加到 OpenAPI 架构中
)
```

## 参数详细说明

| 参数名                           | 作用说明                                 | 常见可选值或类型                                     |
| -------------------------------- | ---------------------------------------- | ---------------------------------------------------- |
| `debug`                          | 控制是否返回调试跟踪信息                 | `True`, `False`（默认）                              |
| `routes`                         | 传入预定义的路由列表                     | `list[BaseRoute]`, `None`（默认）                    |
| `title`                          | 设置 API 文档的标题                      | 字符串，如 `"我的API"`（默认 `"FastAPI"`）           |
| `summary`                        | 设置 API 文档的简短摘要                  | 字符串, `None`（默认）                               |
| `description`                    | 设置 API 文档的详细描述，支持 Markdown   | 字符串, `""`（默认）                                 |
| `version`                        | 设置当前 API 的业务版本号                | 字符串，如 `"1.0.0"`（默认 `"0.1.0"`）               |
| `openapi_url`                    | 指定 OpenAPI JSON 架构的访问路径         | 字符串，如 `"/openapi.json"`（默认）, `None`（禁用） |
| `openapi_tags`                   | 为接口分组并添加描述                     | 字典列表, `None`（默认）                             |
| `servers`                        | 声明 API 的服务器地址列表                | 字典列表, `None`（默认）                             |
| `default_response_class`         | 设置全局默认的响应类                     | `JSONResponse`（默认）, `HTMLResponse` 等            |
| `redirect_slashes`               | 自动处理路径末尾斜杠的重定向             | `True`（默认）, `False`                              |
| `docs_url`                       | Swagger UI 交互式文档的访问路径          | 字符串，如 `"/docs"`（默认）, `None`（禁用）         |
| `redoc_url`                      | ReDoc 文档的访问路径                     | 字符串，如 `"/redoc"`（默认）, `None`（禁用）        |
| `swagger_ui_oauth2_redirect_url` | Swagger UI 的 OAuth2 重定向端点          | 字符串, `None`                                       |
| `swagger_ui_init_oauth`          | 初始化 Swagger UI 的 OAuth2 配置         | 字典, `None`（默认）                                 |
| `middleware`                     | 注册全局中间件                           | 中间件类列表, `None`（默认）                         |
| `exception_handlers`             | 注册全局异常处理函数                     | 字典, `None`（默认）                                 |
| `on_startup`                     | 应用启动时执行的函数列表                 | 函数列表, `None`（默认）                             |
| `on_shutdown`                    | 应用关闭时执行的函数列表                 | 函数列表, `None`（默认）                             |
| `lifespan`                       | 管理应用启动与关闭的上下文管理器         | 异步上下文管理器, `None`（默认）                     |
| `terms_of_service`               | 服务条款的 URL                           | 字符串, `None`（默认）                               |
| `contact`                        | API 联系人信息                           | 字典, `None`（默认）                                 |
| `license_info`                   | API 许可证信息                           | 字典, `None`（默认）                                 |
| `root_path_in_servers`           | 是否在文档的服务器列表中拼接 `root_path` | `True`（默认）, `False`                              |
| `responses`                      | 全局附加的响应状态码及模型               | 字典, `None`（默认）                                 |
| `callbacks`                      | 全局回调定义                             | 字典, `None`（默认）                                 |
| `webhooks`                       | 全局 Webhooks 定义                       | 字典, `None`（默认）                                 |
| `deprecated`                     | 标记整个 API 为已弃用状态                | `True`, `False`（默认）                              |
| `include_in_schema`              | 控制路由是否展示在 OpenAPI 文档中        | `True`（默认）, `False`                              |
| `swagger_ui_parameters`          | 自定义 Swagger UI 的渲染参数             | 字典, `None`（默认）                                 |
| `generate_unique_id_function`    | 自定义生成路由唯一 ID 的逻辑             | 可调用对象, 默认生成函数                             |
| `separate_input_output_schemas`  | 分离请求体和响应体的 OpenAPI 架构        | `True`（默认）, `False`                              |
| `openapi_external_docs`          | 指向外部文档的链接                       | 字典, `None`（默认）                                 |
| `strict_content_type`            | 严格校验请求头中的 Content-Type          | `True`（默认）, `False`                              |
| `**extra`                        | 传入任意额外的自定义元数据               | 任意键值对                                           |

## 常用属性

| 属性名                     | 说明                                |
| -------------------------- | ----------------------------------- |
| `app.title`                | 获取或修改当前 API 的标题           |
| `app.version`              | 获取或修改当前 API 的版本号         |
| `app.description`          | 获取或修改当前 API 的描述信息       |
| `app.routes`               | 获取当前应用注册的所有路由列表      |
| `app.middleware_stack`     | 获取应用构建的中间件处理栈          |
| `app.openapi_schema`       | 获取生成的 OpenAPI 架构字典（缓存） |
| `app.dependency_overrides` | 用于测试时覆盖依赖项的字典          |

## 常用方法

| 方法名                    | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| `app.get()`               | 注册处理 HTTP GET 请求的路由                                 |
| `app.post()`              | 注册处理 HTTP POST 请求的路由                                |
| `app.put()`               | 注册处理 HTTP PUT 请求的路由                                 |
| `app.delete()`            | 注册处理 HTTP DELETE 请求的路由                              |
| `app.patch()`             | 注册处理 HTTP PATCH 请求的路由                               |
| `app.include_router()`    | 将 `APIRouter` 实例挂载到当前应用                            |
| `app.add_middleware()`    | 动态向应用添加中间件                                         |
| `app.exception_handler()` | 装饰器，用于注册自定义异常处理函数                           |
| `app.middleware()`        | 装饰器，用于注册 HTTP 中间件                                 |
| `app.on_event()`          | 注册启动（`"startup"`）或关闭（`"shutdown"`）事件（已逐渐被 `lifespan` 取代） |
| `app.openapi()`           | 生成并返回 OpenAPI 架构字典                                  |

# 路由

1. 介绍

   **URL 地址**和**处理函数**之间的映射关系，决定了当用户访问某个特定网址时，服务器应该执行哪段代码返回结果。

   - 当用户访问一个特定的网址（URL）时，FastAPI 需要知道应该把请求交给哪个函数去处理。
   - 这个“地址 → 函数”的对应规则，就是路由。

2. 组成

   1. 路径（Path）

      即 URL 路径，如 `/users/123`。

   2. HTTP 方法（Method）

      即请求的动作，如 `GET`（获取）、`POST`（创建）等。

3. 定义

   借助于 python 中的装饰器。

   ```python
   @app.get("/")
   async def root():
       return {"message": "Hello World"}
   ```

   - `app`：FastAPI 实例。
   - `get`：请求方法。
   - `"/"`：请求路径，这里代表根路径。
   - `return`：这里叫响应结果。

   > 注意路由**别重名**。

所有注册的路由都会统一保存到 `app.routes` 中。

# 参数

## 介绍

是客户端发送请求时附带的额外信息和指令。

## 作用

让同一个接口能根据不同的输入，返回不同的输出，实现动态交互。

## 分类

| 类别     | 位置                                                        | 作用                                   | 方法                 |
| -------- | ----------------------------------------------------------- | -------------------------------------- | -------------------- |
| 路径参数 | URL 路径的一部分 `/book/{id}`                               | 指向唯一的、特定的资源。               | `GET`                |
| 查询参数 | URL `?` 之后 `k1=v1&k2=v2`                                  | 对资源集合进行过滤、排序、分页等操作。 | `GET`                |
| 请求体   | HTTP 请求的消息体（body）中 携带大量数据，<br />如： `JSON` | 创建、更新资源。                       | `POST`<br />`PUT` 等 |

## `Path` 函数

`fastapi.Path`

### 作用

用于声明和验证**路径参数（Path Parameters）**，对 URL 路径中的动态部分（如 `/items/{item_id}` 中的 `item_id`）添加额外的元数据、校验规则和文档描述。

1. 参数校验
   对路径参数施加约束，例如最小值、最大值、正则表达式匹配等，不符合规则时自动返回 422 错误。

2. API 文档生成
   为 OpenAPI（Swagger UI / ReDoc）提供参数的标题、描述、示例等信息，使自动生成的接口文档更清晰。

3. 显式声明

   当路径参数需要设置默认值以外的元数据时，必须使用 `Path()` 进行显式声明（因为路径参数本质上是必填的，不能像查询参数那样直接赋默认值）。

4. 类型提示增强
   配合 Python 类型提示，让 FastAPI 自动完成类型转换与验证。

### 参数介绍

| 参数名              | 类型      | 说明                               | 适用类型 |
| ------------------- | --------- | ---------------------------------- | -------- |
| `default=...`       | Any       | 必填标记，**路径参数固定**传 `...` | 所有     |
| `title`             | str       | API 文档中的参数标题               | 所有     |
| `description`       | str       | 参数详细描述，支持 Markdown        | 所有     |
| `ge`                | float/int | 大于等于                           | 数值     |
| `gt`                | float/int | 大于                               | 数值     |
| `le`                | float/int | 小于等于                           | 数值     |
| `lt`                | float/int | 小于                               | 数值     |
| `multiple_of`       | float/int | 必须是该值的倍数                   | 数值     |
| `min_length`        | int       | 最小长度                           | 字符串   |
| `max_length`        | int       | 最大长度                           | 字符串   |
| `pattern`           | str       | 正则表达式匹配                     | 字符串   |
| `examples`          | list      | 文档中展示的示例值列表             | 所有     |
| `deprecated`        | bool      | 标记参数已弃用                     | 所有     |
| `include_in_schema` | bool      | 是否在 OpenAPI 文档中显示          | 所有     |

### 代码示例

```python
# 路径参数：限制数值范围
@app.get("/get_book_id/{id}")
async def get_book_id(
    id: int = Path(..., gt=0, lt=100, description="书籍的id，范围 1 ~ 99")
):
    return {"id": id, "title": f"这是第{id}本书。"}
```

## `Query` 函数

`fastapi.Query`

### 介绍

用于显式声明和校验**查询参数（Query Parameters）**，调用时FastAPI 会知道该参数来自 URL 的 `?key=value` 部分，并自动完成解析、类型转换、数据校验以及 OpenAPI 文档生成。

- 来源标识
  明确告诉 FastAPI 这个参数是查询参数（区别于路径参数 `Path`、请求体 `Body` 等）。
- 数据校验
  支持长度、大小、正则、枚举等约束。
- 文档增强
  为 Swagger UI / ReDoc 提供参数描述、示例值、是否必填等元数据。
- 默认值处理
  支持设置默认值，或标记为必填（使用 `...`）。

### 参数介绍

| 参数                | 类型    | 说明                                                         |
| ------------------- | ------- | ------------------------------------------------------------ |
| `default`           | `Any`   | 默认值。<br />设为 `...` (Ellipsis) 表示必填；设为 `None` 或其他值则为可选。 |
| `alias`             | `str`   | 参数别名。<br />当 URL 中的 key 与 Python 变量名不一致时使用（如 `item-id` → `item_id`）。 |
| `title`             | `str`   | 在 OpenAPI 文档中显示的标题。                                |
| `description`       | `str`   | 参数的详细描述，支持 Markdown。                              |
| `ge` / `gt`         | `float` | 数值校验：大于等于 / 大于。                                  |
| `le` / `lt`         | `float` | 数值校验：小于等于 / 小于。                                  |
| `min_length`        | `int`   | 字符串最小长度。                                             |
| `max_length`        | `int`   | 字符串最大长度。                                             |
| `pattern`           | `str`   | 正则表达式校验（仅适用于字符串）。                           |
| `examples`          | `list`  | 文档中展示的示例值列表（OpenAPI 3.1+ 推荐）。                |
| `deprecated`        | `bool`  | 标记该参数已弃用，文档中会显示删除线。                       |
| `include_in_schema` | `bool`  | 是否在文档中显示，默认 `True`。                              |

### 代码示例

```python
# 查询新闻：分页
@app.get("/news/news_list")
async def get_news_list(
    skip: int = Query(0, description="跳过的记录数", lt=100),  # 默认值
    limit: int = Query(10, description="返回的记录数"),
):
    """
    查询新闻：分页

    Parameters
    ----------
    skip : int
        跳过的记录数

    limit : int
        返回的记录数
    """
    return {"skip": skip, "limit": limit}
```

## 请求体参数

- 定义类型

  ```python
  from pydactic import BaseModel
  class User(BaseModel):
      username: str
      password: str
  ```

- 类型注解

  ```python
  @app.get("/register")
  async def register(user: User):
      return user
  ```

## `Field` 函数

`pydantic.Field`

### 介绍

 Pydantic 框架中用于**自定义模型字段（Model Field）元数据与校验规则**的核心函数。

它的作用类似于 FastAPI 中的 `Query`、`Path`、`Body`，但专门用于 **Pydantic 模型内部**。当需要在 `BaseModel` 中对某个字段添加默认值、校验约束、文档描述或序列化配置时，就需要使用 `Field()`。

- 字段校验

  支持数值范围、字符串长度、正则匹配等约束。

- 默认值管理：设置静态默认值或动态工厂函数（如 `default_factory`）。

- 文档生成

  为 OpenAPI / JSON Schema 提供 `title`、`description`、`examples` 等元数据。

- 序列化控制

  通过 `alias`、`exclude`、`repr` 等参数控制字段的输入/输出行为。

- 类型增强

  配合 `Annotated` 实现更清晰的类型声明（Pydantic V2 推荐写法）。

### 参数介绍

| 参数                        | 类型       | 说明                                                         |
| --------------------------- | ---------- | ------------------------------------------------------------ |
| `default`                   | `Any`      | 字段默认值。设为 `...` (Ellipsis) 表示必填。                 |
| `default_factory`           | `Callable` | 动态生成默认值的工厂函数（如 `list`，`dict`，`uuid4`），与 `default` 互斥。 |
| `alias`                     | `str`      | 字段别名，用于输入/输出时的键名映射（如驼峰 ↔ 蛇形）。       |
| `title`                     | `str`      | JSON Schema 中的字段标题。                                   |
| `description`               | `str`      | 字段详细描述，支持 Markdown。                                |
| `ge` / `gt`                 | `float`    | 数值校验：≥ / >。                                            |
| `le` / `lt`                 | `float`    | 数值校验：≤ / <。                                            |
| `min_length` / `max_length` | `int`      | 字符串/序列长度约束。                                        |
| `pattern`                   | `str`      | 正则表达式校验（仅字符串）。                                 |
| `examples`                  | `list`     | 文档示例值列表（OpenAPI 3.1+ / JSON Schema draft 2020-12）。 |
| `exclude`                   | `bool`     | 是否在序列化时排除该字段。                                   |
| `repr`                      | `bool`     | 是否包含在模型的 `__repr__` 中，默认 `True`。                |
| `json_schema_extra`         | `dict`     | 向 JSON Schema 注入额外自定义属性。                          |

### 代码示例

```python
class User(BaseModel):
    username: str = Field(
        "张三", min_length=1, max_length=5, description="用户名，长度 1 ~ 5"
    )
    password: str = Field(min_length=1, max_length=10)
```



# 响应类型

## 介绍

默认情况下，FastAPI 会自动将路径操作函数返回的 Python 对象（字典、列表、Pydantic 模型等），经由 `jsonable_encoder` 转换为 JSON 兼容格式，并包装为 `JSONResponse` 返回。这省去了手动序列化的步骤，让开发者能更专注于业务逻辑。

## 分类

`fastapi.responses`

| 响应类型            | 用途                           | 示例                                |
| ------------------- | ------------------------------ | ----------------------------------- |
| `JSONResponse`      | 默认响应，返回 **JSON 数据**。 | `return {"key": "value"}`           |
| `HTMLResponse`      | 返回 **HTML 内容**。           | `return HTMLResponse(html_content)` |
| `PlainTextResponse` | 返回纯文本。                   | `return PlainTextResponse("text")`  |
| `FileResponse`      | 返回**文件下载**。             | `return FileResponse(path)`         |
| `StreamingResponse` | 流式响应。                     | 生成器函数返回数据                  |
| `RedirectResponse`  | 重定向。                       | `return RedirectResponse(url)`      |

可在 `Response` 或者 `content-type` 处查看类型。

## 响应类型的设置方式

- 装饰器中指定响应类（`response_class`）

  固定返回类型：HTML、纯文本等。

- 返回响应对象（`return 类(内容)`）

  文件下载、图片、流式响应。

## 自定义响应数据格式

`response_model` 

1. 是路径操作装饰器（如 `@app.get` 或 `@app.post`）的关键参数。
2. 它通过一个 Pydantic 模型来严格定义和约束 API 端点的输出格式。
3. 在提供自动数据验证和序列化的同时保障数据安全性。

>> 返回的数据必须能够被自定义模型成功验证和序列化。
>
>- 返回的字典中，**必须包含模型定义的所有必填字段**（否则会抛出验证错误）。
>- 字段的**类型必须匹配**（或能被 Pydantic 自动转换），例如 `id` 必须是 `int` 或可转为 `int` 的值。
>
>| 场景                       | 是否允许 | 说明                                                         |
>| -------------------------- | -------- | ------------------------------------------------------------ |
>| 返回了模型未定义的额外字段 | 默认允许 | Pydantic v2 默认 `extra='ignore'`，多余字段会被**静默忽略**，<br />不会报错，也不会出现在响应中 |
>| 缺少某个必填字段           | 不允许   | 触发 422 验证错误                                            |
>| 字段类型不匹配且无法转换   | 不允许   | 触发 422 验证错误                                            |
>| 直接返回 模型实例而非字典  | 允许     | `return News(id=1, title="...", content="...")` 同样有效     |
>
>`response_model=News` 做了两件事：
>
>1. **验证**：确保返回值符合 模型 的结构。
>2. **过滤/序列化**：即使你返回了一个包含额外字段的字典，响应中也**只会包含 模型 定义的字段**。
>
>```python
>class News(BaseModel): #! 注意，一定要继承
>     id: int
>     title: str
>     content: str
>
># 这样写完全合法，不会报错
>@app.get("/news/{id}", response_model=News)
>async def get_news(id: int):
>     return {
>        "id": id,
>        "title": f"这是第{id}本书。",
>        "content": "这是一本好书。",
>        "extra_field": "这个字段会被自动丢弃"  # ← 不会出现在响应中
>     }
>```
>
>> 返回的数据必须包含模型的所有必填字段且类型兼容；多出的字段会被自动忽略，不会报错但也不会出现在响应中。

# 异常处理

1. 介绍

   对于客户端引发的错误（如 4xx，例如资源未找到、认证失败），应使用 `raise fastapi.HTTPException` 来中断正常处理流程，并返回标准错误响应。

2. 主要作用

   - **中断请求处理**：一旦抛出，FastAPI 会立即停止后续代码执行，进入异常处理流程。
   - **自定义错误响应**：允许开发者精确控制返回的 HTTP 状态码、错误描述及额外元数据。
   - **自动序列化**：默认以 `application/json` 格式返回结构化错误信息，无需手动构造 Response 对象。
   - **与 OpenAPI 集成**：在自动生成的 API 文档中，对应的接口会自动标注可能返回的错误状态码及示例。

3. 参数介绍

   | 参数名        | 类型               | 说明                                      |
   | ------------- | ------------------ | ----------------------------------------- |
   | `status_code` | `int`              | HTTP 状态码，决定响应的状态类别，必须写。 |
   | `detail`      | `str | list | Any` | 错误详情，支持字符串或结构化数据。        |
   | `headers`     | `dict[str, str]`   | 附加到错误响应的自定义 HTTP 头。          |

# 中间件

1. 介绍

   是一个每次请求进入 FastAPI 应用时都会被执行的**函数**。再请求到达实际路径操纵（路由处理函数）之前运行，并在响应返回给客户端之前再运行一次。

   1. 请求进入时，按注册顺序**由外到内**依次执行各中间件的前置逻辑；
   2. 到达路由处理函数并得到响应后，再按**由内到外**的顺序执行各中间件的后置逻辑；
   3. 任一中间件均可选择提前终止请求（如鉴权失败直接返回 401），不再向内传递。

   ![中间件的执行](./FastAPI.assets/06_中间件的执行.png)

2. 作用

   - **全局请求/响应处理**：对**所有**或指定范围的请求**统一**添加日志记录、耗时统计、请求ID注入等操作。
   - **安全与鉴权前置**：在请求进入业务层前完成 CORS 校验、Token 验证、IP 白名单过滤等安全检查。
   -  **响应头与状态码统一修改**：批量添加安全响应头（如 `X-Content-Type-Options`）、压缩响应体、统一错误格式包装等。
   -  **异常兜底与监控**：捕获未被路由处理的异常，上报性能指标到 Prometheus / Sentry 等监控系统。
   -  **请求体预处理**：例如自动解压 gzip 请求体、解析非标准编码、限流计数等。

3. 写法

   使用装饰器 `@app.middleware("http")`，**函数签名固定格式**。

   `@app.middleware("http")` 本质上是一个语法糖，它会将定义的普通异步函数包装成一个标准的 ASGI 中间件类。框架在调用该函数时，会**严格按照位置传入**两个参数：第一个是**请求对象**，第二个是**调用链的下一个环节**。如果参数数量或顺序**不匹配**，运行时会直接**报错**。

   因此，以下签名是强制性的：

   ```python
   async def middleware_name(request: Request, call_next):
       ...
       response = await call_next(request)
       ...
       return response
   ```

   - `request`

       - **类型**：`starlette.requests.Request`
       - **作用**：封装了当前 HTTP 请求的全部信息。
       - **常用属性/方法**：

       | 属性/方法              | 说明                               |
       | ---------------------- | ---------------------------------- |
       | `request.method`       | 请求方法（GET、POST 等）           |
       | `request.url`          | 完整请求 URL                       |
       | `request.headers`      | 请求头字典                         |
       | `request.client`       | 客户端 IP 和端口                   |
       | `request.state`        | 可在中间件与路由间传递数据的容器   |
       | `await request.body()` | 读取原始请求体（注意：只能读一次） |

   -  `call_next`

       - **类型**：可等待的异步可调用对象。

       - **作用**：代表“洋葱模型”中的**下一层处理逻辑**。调用它会将请求传递给后续中间件或最终的路由处理函数，并返回其生成的响应对象。

       - 关键行为

         1. **必须用 `await` 调用**：`response = await call_next(request)`
         2. **返回值是 Response 对象**：包含状态码、响应头、响应体。
         3. **不调用 = 拦截请求**：若省略此调用，请求不会继续向下传递，此时需自行构造并返回一个 Response。
         4. **只能调用一次**：重复调用会导致未定义行为或异常。

   > **注意**
   >
   > 1. **不要修改 `request` 对象本身**：`Request` 是不可变的，若需在中间件间传递数据，应使用 `request.state.xxx = value`。
   > 2. **避免在 `call_next` 之前读取 `request.body()`**：这会消耗请求流，导致后续路由无法再次读取请求体。如确需读取，应使用缓存机制或改用依赖注入。
   > 3. **`call_next` 之后的代码属于“响应阶段”**：此时请求已处理完毕，仅可对 `response` 做后处理（如添加头、记录耗时等）。

# 依赖注入

1. 介绍

   可重用的组件（函数/类），负责提供某种功能或数据，是一种**声明式获取外部资源或前置逻辑**的机制。

   - 代码复用
   - 解耦：业务逻辑与基础设施代码分离。
   - 易于测试：轻松地用模拟依赖替换真实依赖进行测试，可通过 `app.dependency_overrides` 轻松替换依赖项，实现单元测试隔离。

2. 步骤

   1. 导入 `fastapi.Depends`
   2. 创建依赖项
   3. 声明依赖项

# ORM工具

## 介绍

Object-RelationMapping，对象关系映射，一种编程技术，用于再面向对象编程语言和关系型数据库之间建立映射。允许开发者通过操作对象的方式与数据库进行交互而无需复杂的 SQL 语句。

- 常用工具：SQLAlchemy ORM

- 装包：`pip install sqlalchemy[asyncio] aiomysql`



## 建库建表

1. 步骤

   1. 创建数据库引擎

      ```python
      from sqlalchemy.ext.asyncio import create_async_engine
      
      # 创建地址
      ASYNC_DATABASE_URL = "mysql+aiomysql://root:zjy1234@127.0.0.1:3306/fastapi_test?charset=utf8"
      
      # 创建异步引擎
      async_engine = create_async_pool_from_url(
          ASYNC_DATABASE_URL,
          echo=True, # 可选，输出 SQL 日志
          pool_size=10, # 设置连接池中保持的持久连接数
          max_overflow=20 # 设置连接池允许创建的额外连接数
      )
      ```

   2. 定义模型类

   3. 启动应用时建表
