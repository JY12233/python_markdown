# 概述

`multiprocessing` 是 Python 标准库中用于实现多进程并发编程的核心模块。使用时需通过实例化 `Process` 类创建子进程，或使用 `Pool` 类创建进程池来批量管理任务，当需要执行 CPU 密集型任务（如大规模计算、图像处理）以绕过全局解释器锁（GIL）并充分利用多核 CPU 时，就可以使用该模块。

## 核心作用

1. 绕过 GIL 限制

   通过创建独立的子进程，每个进程拥有独立的 Python 解释器和内存空间，从而实现真正的多核并行计算。

2. 进程管理与复用

   提供进程池（`Pool`）机制，自动管理和复用多个工作进程，避免频繁创建和销毁进程带来的巨大开销。

3. 进程间通信（IPC）

   提供多种安全的通信机制（如 `Queue`、`Pipe`），允许在不同进程之间传递数据和交换消息。

4. 数据共享与同步

   支持共享内存（`Value`、`Array`）和同步原语（`Lock`、`Semaphore`），用于在进程间安全地共享状态和防止数据竞争。

## 核心类与组件

1. `Process`

   进程类，用于创建和控制单个子进程，其 API 与 `threading.Thread` 类似。

2. `Pool`

   进程池类，用于管理固定数量的工作进程，并提供 `map`、`apply_async` 等方法来并行执行任务。

3. `Queue`

   进程安全的队列，用于在多个进程之间传递数据，实现生产者-消费者模式。

4. `Pipe`

   管道类，返回一对连接对象，用于两个进程之间的双向通信。

5. `Value` / `Array`

   共享内存类，用于在进程间共享单个变量或数组，需配合锁使用以保证安全。

6. `Manager`

   管理器类，提供一个独立的服务进程，用于创建和管理可在多个进程间共享的复杂数据结构（如 `list`、`dict`）。

7. `Lock` / `Semaphore`

   同步原语，用于进程间的同步控制，防止多个进程同时访问共享资源。

## 常用函数或属性

| 函数或属性名称                       | 作用说明                                                     |
| ------------------------------------ | ------------------------------------------------------------ |
| `current_process()`                  | 返回当前正在运行的 `Process` 对象，<br />可用于获取当前进程的名称或 PID 等信息。 |
| `active_children()`                  | 返回当前进程的所有活跃子进程列表，便于监控和管理子进程状态。 |
| `cpu_count()`                        | 返回当前计算机的 CPU 核心数量，<br />常用于设置进程池（`Pool`）的默认工作进程数。 |
| `set_start_method(method)`           | 设置创建子进程的启动方式（如 `spawn`、`fork`、`forkserver`），<br />必须在主模块的 `if __name__ == "__main__":` 块中调用且仅能调用一次。 |
| `get_start_method(allow_none=False)` | 获取当前设置的进程启动方式。<br />若未设置且 `allow_none` 为 `False`，则返回当前平台的默认启动方式。 |
| `get_context(method=None)`           | 返回一个上下文对象，该对象具有与 `multiprocessing` 模块相同的 API，<br />但使用指定的启动方法，适用于需要在同一程序中混用不同启动方式的场景。 |
| `freeze_support()`                   | 提供对 Windows 上“冻结”脚本（如使用 PyInstaller 打包的程序）的支持，<br />必须放在程序入口处以确保多进程功能正常工作。 |
| `parent_process()`                   | 返回当前进程的父进程对象（Python 3.8+ 引入），若当前进程为主进程则返回 `None`。 |

## 注意事项

### Windows 平台保护

在 Windows 上，必须将创建进程的代码放在 `if __name__ == "__main__":` 保护块内，以防止子进程无限递归创建。

**底层原理：Windows 缺乏 `fork` 机制，只能使用 `spawn`（孵化）方式。**

1. Linux 的 `fork` 机制

   在 Linux 系统中，创建子进程使用的是 `fork()` 系统调用。`fork` 会直接复制父进程的整个内存空间（包括代码段、数据段、堆栈等）。因此，子进程天生就“拥有”父进程的所有代码和状态，不需要重新执行 Python 脚本。

2. Windows 的 `spawn` 机制

   Windows 操作系统没有 `fork` 系统调用。Python 在 Windows 上创建子进程时，只能启动一个全新的 Python 解释器进程。为了让这个新进程知道“要执行什么任务”，Python 必须**重新导入（import）主脚本文件**。

3. 无限递归的陷阱

   如果不加 `if __name__ == "__main__":` 保护，当子进程重新导入主脚本时，脚本顶部的 `Process(...).start()` 代码会被再次执行。这会导致子进程在启动时又去创建新的子进程，新子进程又再次导入脚本……最终导致无限递归，直到系统资源耗尽或抛出异常。

4. 保护块的作用

   `if __name__ == "__main__":` 确保了只有当该脚本被**直接运行**时，创建进程的代码才会执行。当子进程通过 `spawn` 方式重新导入该脚本时，`__name__` 的值是模块名（如 `"__mp_main__"`），而不是 `"__main__"`，从而跳过了创建进程的代码，避免了无限递归。

```py
import multiprocessing


def task():
    print(name)


if __name__ == "__main__":
    name = []

    p = multiprocessing.Process(target=task)
    p.start()
    """
    spawn 在创建内存时不会复制或继承父进程的内存空间，而是创建一个全新的
    Python 解释器来重新运行脚本，也就是说它有自己的内存空间。
    这个解释器在运行脚本的时候不是直接运行，而是采用导入的方式，所以 if main
    里面的代码不会被执行，子进程无法获取 name，就会报错。
    这也就是为什么 spawn 模式下进程的创建必须要在 if main 里面，
    不然每次导入的时候都要去创建一个进程，导致无限递归。
    """

```

### 数据序列化

进程间传递的对象必须可被 `pickle` 序列化，因此 lambda 函数、嵌套函数等通常无法作为任务参数传递。

**底层原理：进程间内存隔离，数据传递必须经过“序列化-反序列化”过程。**

1. 内存隔离

   与线程共享同一块内存空间不同，每个进程都拥有自己独立的虚拟内存空间。子进程无法直接访问父进程内存中的对象引用（指针）。

2. 序列化（Pickling）

   为了将数据从进程 A 传递到进程 B，必须将内存中的 Python 对象转换成一种可以存储或传输的字节流格式。Python 标准库提供了 `pickle` 模块来完成这一工作。当你在 `multiprocessing` 中传递参数或获取返回值时，底层会自动调用 `pickle.dumps()` 将对象序列化为字节流。

3. 反序列化（Unpickling）

   接收方进程收到字节流后，会调用 `pickle.loads()` 将其还原为一个新的 Python 对象。注意，这个新对象是原对象的一个**深拷贝**，两者在内存中完全独立。

4. 为什么 `lambda` 和嵌套函数不行

   `pickle` 的序列化机制依赖于对象的“可引用性”。`lambda` 函数和嵌套函数（闭包）在 Python 内部是动态生成的代码对象，它们没有全局唯一的名称和模块路径，`pickle` 无法通过名称重新构建它们，因此会抛出序列化错误。只有定义在模块顶层的、有明确名称的函数才能被 `pickle` 正确序列化和反序列化。

```py
"""
spawn 模式下，一些特殊的对象，比如文件或者线程锁无法传递。
"""

import multiprocessing
import threading


def task():
    print(f)
    print(lock)


if __name__ == "__main__":
    try:
        f = open("data/ex.txt", "w", encoding="utf-8")
        lock = threading.RLock()

        p = multiprocessing.Process(target=task, args=(f, lock))
        p.start()

    finally:
        f.close()


```

### 启动方式差异

不同操作系统的默认进程启动方式不同（Windows 默认为 `spawn`，Linux 默认为 `fork`），这会影响子进程对父进程资源的继承行为。

> 可以使用 `multiprocessing.set_satrt_method()` 来改变启动模式，但是在 Windows 中没用，因为 Windows 只支持 spawn。

**底层原理：操作系统级别的进程创建机制完全不同。**

| 对比维度         | `fork`（Linux/macOS 默认）                                   | `spawn`（Windows 默认，Linux 可选）                      |
| :--------------- | :----------------------------------------------------------- | :------------------------------------------------------- |
| **底层系统调用** | 调用操作系统的 `fork()` 系统调用。                           | 启动一个全新的 Python 解释器进程。                       |
| **内存继承**     | 子进程继承父进程的**整个内存空间**（采用写时复制技术 Copy-on-Write）。 | 子进程拥有**全新的、空的内存空间**。                     |
| **代码执行**     | 子进程直接从父进程复制代码和执行状态，无需重新导入模块。     | 子进程必须重新导入主脚本和相关模块，并重新执行目标函数。 |
| **全局变量状态** | 子进程继承父进程中全局变量的**当前值**。                     | 子进程中的全局变量会被重新初始化为其**原始定义值**。     |
| **资源继承**     | 继承父进程打开的文件描述符、锁等所有资源。                   | 不继承父进程的文件描述符和锁，一切从零开始。             |
| **性能开销**     | 较低（得益于写时复制，无需重新加载解释器和模块）。           | 较高（需要启动新解释器、重新导入模块、重新初始化环境）。 |

这种差异意味着，同一段多进程代码在 Linux 和 Windows 上可能表现出完全不同的行为。例如，如果在模块顶层修改了一个全局变量，`fork` 创建的子进程会看到这个修改，而 `spawn` 创建的子进程看到的则是未修改的原始值。为了保证跨平台行为的一致性，Python 官方推荐在开发时显式使用 `spawn` 方式进行测试。

```py
"""
在 fork 模式下，锁会被复制一份到子进程中，
但是，复制并不是复制全部，可以理解为它创建了一把自己的锁并给自己锁上了。
拷贝的也是一份被申请了的锁，被子进程中的主线程申请了的锁。
"""

import multiprocessing
import time
from multiprocessing import Process
from threading import RLock, Thread


def func():
    print("线程来了")
    with lock:
        print(666)
        time.sleep(1)


def task():
    for i in range(10):
        t = Thread(target=func)
        t.start()

    print("子进程中的主线程准备释放锁")
    time.sleep(2)
    lock.release()


if __name__ == "__main__":
    multiprocessing.set_start_method("fork")
    lock = RLock()
    lock.acquire()

    p = Process(target=task)
    p.start()
    """
    因为是 windows，无法演示。我就用文字说明一下。
    首先，fork 会拷贝资源。前面的 lock 是已经被锁上了，所以在拷贝的
    时候，这个锁也是锁上的，不过锁的不是父进程的主线程，而是子进程的主线程。
    也就是说，这个子进程中的锁是一个新创建的，跟父进程中的不一样，并且把自己的主线程
    锁上了。

    如果子进程中的主线程不释放锁，那子线程运行的时候都会停到申请锁的地方，
    因为此时这把锁在主线程上。
    """
```

# Process

`multiprocessing.Process` 是 Python 标准库 `multiprocessing` 模块中用于创建和控制独立子进程的核心类。使用时需通过实例化 `Process` 类并传入目标函数及参数，随后调用 `start()` 方法启动进程，当需要执行 CPU 密集型任务以绕过全局解释器锁（GIL）并充分利用多核 CPU，或需要运行与主程序完全隔离的独立任务时，就可以使用该类。

## 创建对象时的格式

```python
multiprocessing.Process(
    group=None,  # 保留参数，目前未使用，默认为 None
    target=None,  # 必填，子进程要执行的可调用对象（函数或方法）
    name=None,  # 可选，进程的名称，默认为 "Process-N"（N 为自增数字）
    args=(),  # 可选，传递给 target 的位置参数元组
    kwargs={},  # 可选，传递给 target 的关键字参数字典
    daemon=None  # 可选，设置是否为守护进程，默认为 None（继承父进程的 daemon 属性）
)
```

## 参数详解

| 参数名称 | 作用说明                              | 可选值或类型说明                                             |
| -------- | ------------------------------------- | ------------------------------------------------------------ |
| `group`  | 保留参数，目前未使用，始终为 `None`。 | **`None`**：无需修改。                                       |
| `target` | 指定子进程启动后要执行的目标函数。    | **可调用对象或 `None`**：<br />默认为 `None`（若为 `None`，则需重写 `run()` 方法）。 |
| `name`   | 设置进程的名称，便于调试和日志追踪。  | **字符串或 `None`**：<br />默认为 `None`（自动生成 "Process-N"）。 |
| `args`   | 传递给目标函数的位置参数。            | **元组**：默认为空元组 `()`。                                |
| `kwargs` | 传递给目标函数的关键字参数。          | **字典**：默认为空字典 `{}`。                                |
| `daemon` | 设置进程是否为守护进程。              | **布尔值或 `None`**：<br />默认为 `None`（继承父进程属性）。<br />若设为 `True`，主进程结束时该进程会被强制终止。 |

## 常用属性

| 属性名称   | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| `name`     | 进程的名称（字符串）。                                       |
| `pid`      | 进程的整数 ID（进程启动后才有值）。                          |
| `daemon`   | 进程是否为守护进程的布尔值。                                 |
| `exitcode` | 进程的退出码。`None` 表示尚未结束，负数 `-N` 表示被信号 `N` 终止，正数表示正常退出码。 |
| `authkey`  | 进程的身份验证密钥（字节串），用于进程间通信的安全验证。     |

## 常用方法

| 方法名称             | 说明                                                         |
| -------------------- | ------------------------------------------------------------ |
| `start()`            | 启动进程，安排 `run()` 方法在子进程中执行。每个进程对象只能调用一次。 |
| `join(timeout=None)` | 阻塞当前进程，直到目标子进程终止或超时。`timeout` 为 `None` 时表示无限等待。 |
| `terminate()`        | 强制终止进程，不会执行清理代码（如 `finally` 块），在 Unix 上发送 `SIGTERM` 信号。 |
| `kill()`             | 强制终止进程，在 Unix 上发送 `SIGKILL` 信号，无法被捕获或忽略（Python 3.7+）。 |
| `is_alive()`         | 返回进程是否仍处于运行状态（布尔值）。                       |
| `run()`              | 子进程实际执行的方法。<br />默认调用 `target(*args, **kwargs)`，可通过继承 `Process` 类并重写此方法来自定义行为。 |
| `close()`            | 释放与进程对象关联的所有资源。在进程终止并调用 `join()` 后应调用此方法（Python 3.7+）。 |

# 资源共享

## 基于值或者数组

```py
"""
使用 Value 和 Array 来实现。
这个是基于底层的 C 语言实现的，因此一些写法与 C 类似。
"""

from multiprocessing import Array, Process, Value
from multiprocessing.sharedctypes import (
    Synchronized,
    SynchronizedArray,
)


def task01(
    num: Synchronized, m: Synchronized, n: Synchronized
):
    num.value = 666
    m.value = b"C"
    n.value = "好"


def task02(data: SynchronizedArray):
    data[0] = -111


if __name__ == "__main__":
    # 具体的类型可以去官方文档看
    num = Value("i", 123)  # int
    m = Value("c", b"A")  # char
    n = Value("u", "你")  # wchar，表示可以存中文
    data = Array("i", [1, 2, 3, 4])
    # 表示一个只能存整数的数组，数组长度固定

    p1 = Process(target=task01, args=(num, m, n))
    p2 = Process(target=task02, args=(data,))

    p1.start()
    p2.start()

    p1.join()
    p2.join()

    print(num.value, m.value, n.value, end="\t")
    print(data[:])
    range(10)
```

## 基于 Manager

```py
from multiprocessing import Manager, Process
from multiprocessing.managers import DictProxy, ListProxy


def task(d: DictProxy, l: ListProxy):
    d["姓名"] = "张三"
    d["年龄"] = 18
    l.extend([2, 5, 7, 9])


if __name__ == "__main__":
    with Manager() as manager:
        d = manager.dict()
        l = manager.list()

        p = Process(target=task, args=(d, l))
        p.start()

        p.join()
        print(d)
        print(l)

```

## 基于队列

```py
from multiprocessing import Process, Queue


def task(q: Queue):
    for i in range(10):
        q.put(i)


if __name__ == "__main__":
    queue = Queue()

    p = Process(target=task, args=(queue,))
    p.start()
    p.join()

    for i in range(5):
        print(queue.get())
```

## 基于管道

```py
"""
这个可以想成是两头都通的管道，跟 socket 很像，
两头都可以放东西和接收东西，并且接收的时候是阻塞的。
"""

import time
from multiprocessing import Pipe, Process
from multiprocessing.connection import PipeConnection


def task(con: PipeConnection):
    print("准备发送给父")
    time.sleep(2)
    con.send([123, 456, 789])

    print("准备接收父的数据")
    info = con.recv()
    print("接收到父的数据：",info)


if __name__ == "__main__":
    # 这里的父子只是区分，实际上并没有 “上下” 这种关系，是平级的
    parent_con, child_con = Pipe()

    p = Process(target=task, args=(child_con,))
    p.start()

    # 接收另一边传来的数据，会阻塞程序运行
    print("准备接收子的数据")
    info = parent_con.recv()
    print("接收到子的数据：",info)

    print("准备发送给子")
    time.sleep(2)
    parent_con.send([789, 456, 123])
```

