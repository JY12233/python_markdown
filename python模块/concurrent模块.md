# 概述

`concurrent` 是 Python 标准库中用于并发编程的顶层模块。使用时需导入其子模块（主要是 `concurrent.futures`），通过实例化执行器（如 `ThreadPoolExecutor` 或 `ProcessPoolExecutor`）来管理线程池或进程池，并提交任务，当需要高效地执行大量独立的 I/O 密集型或 CPU 密集型任务，且希望避免手动管理底层线程或进程的复杂生命周期时，就可以使用该模块。

## 核心作用

1. 统一并发接口

   提供了一套高级且统一的 API，简化了多线程与多进程编程的复杂性，使代码更易于编写和维护。

2. 资源池化管理

   通过线程池和进程池复用线程与进程资源，避免了频繁创建和销毁线程/进程带来的巨大开销。

3. 异步结果获取

   引入 `Future` 对象，允许以非阻塞的方式提交任务，并在未来某个时间点获取任务的执行结果或异常信息。

4. 任务调度与控制

   支持批量提交任务、设置超时时间、取消任务以及按完成顺序处理结果等高级调度功能。

## 核心子模块与类

1. `concurrent.futures`

   该模块最核心的子模块，提供了 `Executor` 抽象基类及其两个主要实现。

   - `ThreadPoolExecutor`：线程池执行器，适用于 I/O 密集型任务（如网络请求、文件读写）。
   - `ProcessPoolExecutor`：进程池执行器，适用于 CPU 密集型任务（如复杂计算、数据处理），可绕过 GIL 限制。

2. `Future`

   表示异步执行任务的未来结果对象，提供了 `result()`、`exception()`、`cancel()` 等方法来查询任务状态或获取结果。

3. 辅助函数

   - `as_completed(fs, timeout=None)`

     返回一个迭代器，按任务完成的先后顺序 yield 对应的 `Future` 对象。

   - `wait(fs, timeout=None, return_when=ALL_COMPLETED)`

     阻塞等待一组 `Future` 对象完成，并返回已完成和未完成的集合。

# futures 模块

`concurrent.futures` 是 Python 标准库中用于高级并发编程的核心模块。使用时需导入该模块，通过实例化 `ThreadPoolExecutor` 或 `ProcessPoolExecutor` 来创建线程池或进程池，并利用 `submit()` 方法提交任务或使用 `map()` 方法批量执行，当需要高效地并发执行大量独立任务，且希望避免手动管理底层线程或进程的复杂生命周期时，就可以使用该模块。

## 核心作用

1. 统一并发接口

   提供了一套高级且统一的 API，简化了多线程与多进程编程的复杂性，使代码更易于编写和维护。

2. 资源池化管理

   通过线程池和进程池复用线程与进程资源，避免了频繁创建和销毁线程或进程带来的巨大开销。

3. 异步结果获取

   引入 `Future` 对象，允许以非阻塞的方式提交任务，并在未来某个时间点获取任务的执行结果或异常信息。

4. 任务调度与控制

   支持批量提交任务、设置超时时间、取消任务以及按完成顺序处理结果等高级调度功能。

## 核心类与函数

1. `Executor`

   抽象基类，定义了执行器的通用接口，不应直接实例化。

2. `ThreadPoolExecutor`

   线程池执行器，适用于 I/O 密集型任务（如网络请求、文件读写）。

3. `ProcessPoolExecutor`

   进程池执行器，适用于 CPU 密集型任务（如复杂计算、数据处理），可绕过 GIL 限制。

4. `Future`

   表示异步执行任务的未来结果对象，提供了 `result()`、`exception()`、`cancel()` 等方法来查询任务状态或获取结果。

5. `as_completed(fs, timeout=None)`

   辅助函数，返回一个迭代器，按任务完成的先后顺序 yield 对应的 `Future` 对象。

6. `wait(fs, timeout=None, return_when=ALL_COMPLETED)`

   辅助函数，阻塞等待一组 `Future` 对象完成，并返回已完成和未完成的集合。

## 常用 `Executor` 方法

| 方法名称                                           | 说明                                                         |
| -------------------------------------------------- | ------------------------------------------------------------ |
| `submit(fn, *args, **kwargs)`                      | 提交一个可调用对象 `fn` 到执行器，并立即返回一个 `Future` 对象 |
| `map(func, *iterables, timeout=None, chunksize=1)` | 以并发方式将 `func` 应用于 `iterables` 中的每个元素，返回一个结果迭代器 |
| `shutdown(wait=True, cancel_futures=False)`        | 关闭执行器，释放资源。`wait=True` 时会等待所有已提交任务完成 |

# 线程池和进程池

线程池和进程池是 Python 并发编程中用于管理线程和进程资源的高级抽象类。使用时需从 `concurrent.futures` 模块导入 `ThreadPoolExecutor`（线程池）或 `ProcessPoolExecutor`（进程池），通过实例化并指定工作线程或进程的数量来创建池对象，随后使用 `submit()` 方法提交任务或使用 `map()` 方法批量执行，当需要高效地并发执行大量独立任务，且希望避免手动管理底层线程或进程的复杂生命周期时，就可以使用这些类。

> 在进程池中，锁必须用 `Mannager` 对象里的锁，而不是 `multiprocessing` 模块里的锁。

## 创建对象时的格式

```python
# 线程池创建格式
concurrent.futures.ThreadPoolExecutor(
    max_workers=None,  # 可选，池中最大工作线程数，默认为 None（自动设置为 CPU 核心数 * 5）
    thread_name_prefix=""  # 可选，线程名称前缀，用于调试和日志识别
)

# 进程池创建格式
concurrent.futures.ProcessPoolExecutor(
    max_workers=None,  # 可选，池中最大工作进程数，默认为 None（自动设置为 CPU 核心数）
    mp_context=None,  # 可选，多进程上下文对象，用于指定进程启动方式（如 'spawn' 或 'fork'）
    initializer=None,  # 可选，每个工作进程启动时调用的可调用对象
    initargs=()  # 可选，传递给 initializer 的参数元组
)
```

## 参数详解

| 参数名称             | 作用说明                                               | 可选值或类型说明                                             | 示例                                   |
| -------------------- | ------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------------- |
| `max_workers`        | 指定池中最大工作线程或进程的数量。                     | 整数或 `None`<br />默认为 `None`<br />线程池默认为 `CPU核心数 * 5`，<br />进程池默认为 `CPU核心数` | `4`、`8`                               |
| `thread_name_prefix` | 设置线程池中线程的名称前缀，<br />便于调试和日志追踪。 | 字符串：默认为空字符串                                       | `"Worker-Thread"`                      |
| `mp_context`         | 指定多进程的启动上下文，控制进程的创建方式。           | `multiprocessing.context.BaseContext` 对象或<br /> `None`，默认为 `None` | `multiprocessing.get_context("spawn")` |
| `initializer`        | 指定每个工作进程启动时自动执行的初始化函数。           | 可调用对象或 `None`，默认为 `None`                           | `init_worker`                          |
| `initargs`           | 传递给 `initializer` 函数的参数。                      | 元组：默认为空元组 `()`                                      | `(arg1, arg2)`                         |

## 常用属性与常用方法

| 属性/方法名称                                      | 说明                                                         |
| -------------------------------------------------- | ------------------------------------------------------------ |
| `submit(fn, *args, **kwargs)`                      | 提交一个可调用对象 `fn` 到池中执行，<br />并立即返回一个 `Future` 对象 |
| `map(func, *iterables, timeout=None, chunksize=1)` | 以并发方式将 `func` 应用于 `iterables` 中的每个元素，<br />返回一个结果迭代器 |
| `shutdown(wait=True, cancel_futures=False)`        | 关闭池，释放资源。`wait=True` 时会等待所有已提交任务完成     |
| `__enter__()`                                      | 支持上下文管理器协议（`with` 语句），自动管理池的生命周期    |
| `__exit__(exc_type, exc_val, exc_tb)`              | 退出上下文管理器时自动调用 `shutdown(wait=True)`             |

1. `submit` 

   在池中提交一个任务，池中如果有空闲线程，则分配一个线程（进程）去执行，执行完毕后再将线程（线程）交还给池；如果没有空闲线程，则等待。
   
   > 等待在池的内部等待，在外面的代码中依然继续执行。

## 代码示例

### 线程池

```python
import time
from concurrent.futures import ThreadPoolExecutor

# 定义一个模拟 I/O 操作的耗时任务
def io_task(n):
    print(f"任务 {n} 开始执行...")
    time.sleep(1)  # 模拟网络请求或文件读写的等待时间
    return f"任务 {n} 完成"

# 使用上下文管理器创建包含 3 个工作线程的线程池
with ThreadPoolExecutor(max_workers=3) as executor:
    # 批量提交 5 个任务，并获取包含 Future 对象的列表
    futures = [executor.submit(io_task, i) for i in range(5)]
    
    # 遍历 Future 对象，阻塞等待并获取每个任务的返回结果
    for future in futures:
        print(future.result())
```

### 进程池

```python
import time
from concurrent.futures import ProcessPoolExecutor

# 定义一个模拟 CPU 密集计算的耗时任务
def cpu_task(n):
    print(f"任务 {n} 开始计算...")
    total = sum(i * i for i in range(10**6))  # 模拟大量的数学运算
    return f"任务 {n} 计算结果: {total}"

# 使用上下文管理器创建包含 4 个工作进程的进程池
with ProcessPoolExecutor(max_workers=4) as executor:
    # 使用 map 方法批量提交任务，自动将参数分配给不同的进程
    results = executor.map(cpu_task, range(5))
    
    # 遍历结果迭代器，按任务提交顺序获取返回值
    for result in results:
        print(result)
```

## 核心区别

| 对比维度     | 线程池（`ThreadPoolExecutor`）    | 进程池（`ProcessPoolExecutor`）      |
| ------------ | --------------------------------- | ------------------------------------ |
| **适用场景** | I/O 密集型（网络请求、文件读写）  | CPU 密集型（复杂计算、数据处理）     |
| **底层原理** | 利用 I/O 等待时间切换执行其他任务 | 绕过 GIL，利用多核 CPU 真正并行计算  |
| **资源开销** | 较低（线程间共享内存）            | 较高（进程间内存独立，需序列化通信） |

# Future 对象

`Future` 是 Python `concurrent.futures` 模块中的核心类，代表一个异步执行任务的未来结果。使用时需通过执行器（如 `ThreadPoolExecutor` 或 `ProcessPoolExecutor`）的 `submit()` 方法自动创建，随后调用 `result()` 获取返回值或 `done()` 检查状态，当需要以非阻塞方式提交任务，并在未来某个时间点获取任务的执行结果、异常信息或进行任务取消等操作时，就可以使用该类。

## 创建对象时的格式

```python
# Future 对象通常不直接实例化，而是通过执行器的 submit 方法自动创建并返回
future = executor.submit(
    fn,  # 必填，要异步执行的可调用对象（函数或方法）
    *args,  # 可选，传递给 fn 的位置参数
    **kwargs  # 可选，传递给 fn 的关键字参数
)
```

## 参数详解

| 参数名称   | 作用说明                     | 示例                  |
| ---------- | ---------------------------- | --------------------- |
| `fn`       | 指定要异步执行的目标函数。   | `download_file`       |
| `*args`    | 传递给目标函数的位置参数。   | `1, "https://..."`    |
| `**kwargs` | 传递给目标函数的关键字参数。 | `timeout=30, retry=3` |

## 常用属性与常用方法

| 属性/方法名称             | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| `result(timeout=None)`    | 获取任务的返回值。<br />若任务未完成，会阻塞当前线程直到完成或超时。若任务抛出异常，此方法会重新抛出该异常。 |
| `exception(timeout=None)` | 获取任务执行过程中抛出的异常对象。若任务正常完成，返回 `None`。 |
| `done()`                  | 返回任务是否已完成。完成状态包括正常完成、异常完成或被取消。 |
| `running()`               | 返回任务是否正在执行中。若任务正在运行且无法被取消，返回 `True`。 |
| `cancelled()`             | 返回任务是否已被成功取消。                                   |
| `cancel()`                | 尝试取消任务。<br />若任务正在执行或已完成，则无法取消，返回 `False`；若成功取消，返回 `True`。 |
| `add_done_callback(fn)`   | 注册一个回调函数 `fn`。当任务完成时（无论成功、失败或取消），<br />该函数会被自动调用，且 `Future` 对象会作为**唯一参数传入**。 |

1. `add_done_callback` 

   在池中，一个任务完成，如果是线程池，则回调函数由子线程调用；进程池中，则回调函数由主进程执行。