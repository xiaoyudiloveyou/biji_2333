### 进程和线程

     Mac OS X，UNIX，Linux，Windows 等，都是支持“多任务”的操作系统。
    
    真正的并行执行多任务只能在多核 CPU 上实现，但是，由于任务数量
    远远多于 CPU 的核心数量，所以，操作系统也会自动把很多任务轮流
    调度到每个核心上执行。
    
    对于操作系统来说，一个任务就是一个进程（Process）
    
    在一个进程内部，要同时干多件事，就需要同时
    运行多个“子任务”，我们把进程内的这些“子任务”称为线程（Thread）。
    
    由于每个进程至少要干一件事，所以，一个进程至少有一个线程。
    
    多任务的实现有 3 种方式：

     多进程模式；
     多线程模式；
     多进程+多线程模式。

#### 小结
     线程是最小的执行单元，而进程由至少一个线程组成。如何调度进程和
     线程，完全由操作系统决定，程序自己不能决定什么时候执行，执行多
     长时间。

    多进程和多线程的程序涉及到同步、数据共享的问题，编写起来更复杂。

### 多进程
让 Python 程序实现多进程（multiprocessing）

    Unix/Linux 操作系统提供了一个 fork()系统调用，它非常特殊。普通的
    函数调用，调用一次，返回一次，但是 fork()调用一次，返回两次，因
    为操作系统自动把当前进程（称为父进程）复制了一份（称为子进程），
    然后，分别在父进程和子进程内返回。
    子进程永远返回 0，而父进程返回子进程的 ID。这样做的理由是，一个
    父进程可以 fork 出很多子进程，所以，父进程要记下每个子进程的 ID，
    而子进程只需要调用 getppid()就可以拿到父进程的 ID。
    Python 的 os 模块封装了常见的系统调用，其中就包括 fork，可以在
    Python 程序中轻松创建子进程：

```python
import os

print('Process (%s) start...' % os.getpgid())
# Only works on Unix/Linux/Mac:
pid = os.fork()
if pid == 0:
    print('I am child process (%s) and my parent is %s.' % (os.getpid(), os.getpgid()))

else:
    print('I (%s) just created a child process (%s).' % (os.getpid(), pid))

# 运行结果如下：
#
# process (876) start...
# I (876) just created a child process (877).
# I am child process (877) and my parent is 876.

```

    Mac 系统是基于 BSD（Unix 的一种）内核，所以，在 Mac 下运行是
    没有问题的，推荐大家用 Mac 学 Python！
    
    有了 fork 调用，一个进程在接到新任务时就可以复制出一个子进程来处
    理新任务，常见的 Apache 服务器就是由父进程监听端口，每当有新的
    http 请求时，就 fork 出子进程来处理新的 http 请求。

#### multiprocessing
multiprocessing 模块就是跨平台版本的多进程模块

multiprocessing 模块提供了一个 Process 类来代表一个进程对象，下面
的例子演示了启动一个子进程并等待其结束：

```python
from multiprocessing import Process
import os

#子进程要执行的代码
def run_proc(name):
    print('Run child process %s (%s)...' % (name, os.getpid()))

if __name__ == '__main__':
    print('Parent process %s.' % os.getpid())
    p = Process(target=run_proc, args=('test',))
    print('Child process will start.')
    p.start()
    p.join()
    print('Child process end.')

# Parent process 6760.
# Child process will start.
# Run child process test (16352)...
# Child process end.

```

    创建子进程时，只需要传入一个执行函数和函数的参数，创建一个
    Process 实例，用 start()方法启动，这样创建进程比 fork()还要简单。
    join()方法可以等待子进程结束后再继续往下运行，通常用于进程间的
    同步。
[Python多进程和多线程中的join()](https://www.cnblogs.com/cnkai/p/7504980.html)
#### Pool

```python
from multiprocessing import Pool
import os, time, random

def long_time_task(name):
    print('Run task %s (%s)...' % (name, os.getpid()))
    start = time.time()
    time.sleep(random.random() * 3)
    end = time.time()
    print('Task %s runs %0.2f seconds.' % (name, (end - start)))

if __name__ == '__main__':
    print('Parent process %s.' % os.getpid())
    p = Pool(4)
    for i in range(5):
        p.apply_async(long_time_task, args=(i,))
    print('Waiting for all subprocess done...')
    p.close()
    p.join()
    print('All subprocesses done.')

    # 于 Pool 的默认大小是 CPU 的核数，如果你不幸拥有 8 核 CPU，你要
    # 提交至少 9 个子进程才能看到上面的等待效果。
```

#### 子进程

    很多时候，子进程并不是自身，而是一个外部进程。我们创建了子进程
    后，还需要控制子进程的输入和输出。
    
    subprocess 模块可以让我们非常方便地启动一个子进程，然后控制其输
    入和输出。

```python
import subprocess

print('$ nslookup www.python.org')
print('===')
r = subprocess.call(['nslookup', 'www.python.org'])
print('===')
print('Exit code:', r)
```

如果子进程还需要输入，则可以通过 communicate()方法输

```python
import subprocess

print('$ nslookup')
p = subprocess.Popen(['nslookup'], stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE)

output, err = p.communicate(b'set q=mx\npython.org\nexit\n')
print(output.decode('gb18030'))
print('Exit code:', p.returncode)

```

#### 进程间通信

    Process 之间肯定是需要通信的，操作系统提供了很多机制来实现进程
    间的通信。Python 的 multiprocessing 模块包装了底层的机制，提供了
    Queue、Pipes 等多种方式来交换数据。

我们以 Queue 为例，在父进程中创建两个子进程，一个往 Queue 里写数
据，一个从 Queue 里读数据：

```python
from multiprocessing import Process, Queue
import os, time, random

def write(q):
    print('Process to write: %s' % os.getpid() )
    for value in ['A', 'B', 'C']:
        print('Put %s to queue...' % value)
        q.put(value)
        time.sleep(random.random())

def read(q):
    print('Process to read: %s' % os.getpid())
    while True:
        value = q.get(True)
        print('Get %s form queue.' % value)

if __name__ == '__main__':
    #父进程创建Queue，并传给各个子进程：
    q = Queue()
    pw = Process(target=write, args=(q,))
    pr = Process(target=read, args=(q,))
    # 启动子进程pw, 写入：
    pw.start()
    # 启动子进程 pr，读取:
    pr.start()
    #等待pw结束：
    pw.join()
    # pr进程里是死循环，无法等待其结束，只能强行终止：
    pr.terminate()

# ====
# Process to write: 17840
# Put A to queue...
# Process to read: 15968
# Get A form queue.
# Put B to queue...
# Get B form queue.
# Put C to queue...
# Get C form queue.
# 2333
```

    在 Unix/Linux 下，multiprocessing 模块封装了 fork()调用，使我们不需
    要关注 fork()的细节。由于 Windows 没有 fork 调用，因此，
    multiprocessing 需要“模拟”出 fork 的效果，父进程所有 Python 对象都
    必须通过 pickle 序列化再传到子进程去，所有，如果 multiprocessing 在
    Windows 下调用失败了，要先考虑是不是 pickle 失败了。

#### 小结
     在 Unix/Linux 下，可以使用 fork()调用实现多进程。
     要实现跨平台的多进程，可以使用 multiprocessing 模块。
     进程间通信是通过 Queue、Pipes 等实现的。

### 多线程
多任务可以由多进程完成，也可以由一个进程内的多线程完成。

由于线程是操作系统直接支持的执行单元，因此，高级语言通常都内置
多线程的支持，Python 也不例外，并且，Python 的线程是真正的 Posix
Thread，而不是模拟出来的线程。

Python 的标准库提供了两个模块：_thread 和 threading，_thread 是低级
模块，threading 是高级模块，对_thread 进行了封装。绝大多数情况下，
我们只需要使用 threading 这个高级模块。

```python
import time, threading

def loop():
    print('thread %s is running...' % threading.current_thread().name)
    n = 0
    while n < 5:
        n = n + 1
        print('thread %s >>> %s' % (threading.current_thread().name, n))
        time.sleep(1)
    print('thread %s running...' % threading.current_thread().name)

print('thread %s is running...' % threading.current_thread().name)
t = threading.Thread(target=loop, name='LoopThread')
t.start()
t.join()
print('thread %s ended.' % threading.current_thread().name)

# thread MainThread is running...
# thread LoopThread is running...
# thread LoopThread >>> 1
# thread LoopThread >>> 2
# thread LoopThread >>> 3
# thread LoopThread >>> 4
# thread LoopThread >>> 5
# thread LoopThread running...
# thread MainThread ended.
```
#### Lock

    多线程和多进程最大的不同在于，多进程中，同一个变量，各自有一份
    拷贝存在于每个进程中，互不影响，而多线程中，所有变量都由所有线
    程共享，所以，任何一个变量都可以被任何一个线程修改，因此，线程
    之间共享数据最大的危险在于多个线程同时改一个变量，把内容给改乱
    了。

来看看多个线程同时操作一个变量怎么把内容给改乱了：

```python
import time, threading

# 假定这是你的银行存款：
balance = 0

def change_it(n):
    # 先存后取，结果应该为0：
    global balance
    balance = balance + n
    balance = balance - n

def run_thread(n):
    for i in range(100000):
        change_it(n)

t1 = threading.Thread(target=run_thread, args=(5, ))
t2 = threading.Thread(target=run_thread, args=(8, ))

t1.start()
t2.start()
t1.join()
t2.join()
print(balance) # 5

```
threading.Lock()来实现

```python
import time, threading

# 假定这是你的银行存款：
balance = 0
lock = threading.Lock()

def change_it(n):
    # 先存后取，结果应该为0：
    global balance
    balance = balance + n
    balance = balance - n

def run_thread(n):
    for i in range(100000):
        # 先要获取锁
        lock.acquire()
        try:
            change_it(n)
        finally:
            lock.release()

t1 = threading.Thread(target=run_thread, args=(5, ))
t2 = threading.Thread(target=run_thread, args=(8, ))

t1.start()
t2.start()
t1.join()
t2.join()
print(balance) # 5

```

    锁的好处就是确保了某段关键代码只能由一个线程从头到尾完整地执
    行，坏处当然也很多，首先是阻止了多线程并发执行，包含锁的某段代
    码实际上只能以单线程模式执行，效率就大大地下降了。其次，由于可
    以存在多个锁，不同的线程持有不同的锁，并试图获取对方持有的锁时，
    可能会造成死锁，导致多个线程全部挂起，既不能执行，也无法结束，
    只能靠操作系统强制终止。

#### 多核 CPU

    如果你不幸拥有一个多核 CPU，你肯定在想，多核应该可以同时执行多
    个线程。
    如果写一个死循环的话，会出现什么情况呢？
    打开 Mac OS X 的 Activity Monitor，或者 Windows 的 Task Manager，都
    可以监控某个进程的 CPU 使用率。
    我们可以监控到一个死循环线程会 100%占用一个 CPU。
    如果有两个死循环线程，在多核 CPU 中，可以监控到会占用 200%的
    CPU，也就是占用两个 CPU 核心。
    要想把 N 核 CPU 的核心全部跑满，就必须启动 N 个死循环线程。
    试试用 Python 写个死循环：
    import threading, multiprocessing
    Python3 基础教程【完整版】 http://www.yeayee.com/
    281/531
    def loop():
     x = 0
     while True:
     x = x ^ 1
    for i in range(multiprocessing.cpu_count()):
     t = threading.Thread(target=loop)
     t.start()
    启动与 CPU 核心数量相同的 N 个线程，在 4 核 CPU 上可以监控到 CPU
    占用率仅有 102%，也就是仅使用了一核。
    但是用 C、C++或 Java 来改写相同的死循环，直接可以把全部核心跑满，
    4 核就跑到 400%，8 核就跑到 800%，为什么 Python 不行呢？
    因为 Python 的线程虽然是真正的线程，但解释器执行代码时，有一个
    GIL 锁：Global Interpreter Lock，任何 Python 线程执行前，必须先获得
    GIL 锁，然后，每执行 100 条字节码，解释器就自动释放 GIL 锁，让别
    的线程有机会执行。这个 GIL 全局锁实际上把所有线程的执行代码都给
    上了锁，所以，多线程在 Python 中只能交替执行，即使 100 个线程跑
    在 100 核 CPU 上，也只能用到 1 个核。
    GIL 是 Python 解释器设计的历史遗留问题，通常我们用的解释器是官方
    实现的 CPython，要真正利用多核，除非重写一个不带 GIL 的解释器。
    所以，在 Python 中，可以使用多线程，但不要指望能有效利用多核。
    如果一定要通过多线程利用多核，那只能通过 C 扩展来实现，不过这样
    就失去了 Python 简单易用的特点。
    Python3 基础教程【完整版】 http://www.yeayee.com/
    282/531
    不过，也不用过于担心，Python 虽然不能利用多线程实现多核任务，但
    可以通过多进程实现多核任务。多个 Python 进程有各自独立的 GIL 锁，
    互不影响。

#### 小结
    多线程编程，模型复杂，容易发生冲突，必须用锁加以隔离，同时，又
    要小心死锁的发生。
    
    Python 解释器由于设计时有 GIL 全局锁，导致了多线程无法利用多核。
    多线程的并发在 Python 中就是一个美丽的梦

### ThreadLocal

    用一个全局 dict 存放所有的 Student 对象，然后以 thread 自身作为
    key 获得线程对应的 Student 对象

```python
global_dict = {}
def std_thread(name):
 std = Student(name)
 # 把 std 放到全局变量 global_dict 中：
 global_dict[threading.current_thread()] = std
 do_task_1()
 do_task_2()
def do_task_1():
 # 不传入 std，而是根据当前线程查找：
 std = global_dict[threading.current_thread()]
 ...
def do_task_2():
 # 任何函数都可以查找出当前线程的 std 变量：
 std = global_dict[threading.current_thread()]
 ...

```

    这种方式理论上是可行的，它最大的优点是消除了 std 对象在每层函数
    中的传递问题，但是，每个函数获取 std 的代码有点丑。

ThreadLocal 应运而生，不用查找 dict，ThreadLocal 帮你自动做这件事：

```python
import threading

local_school = threading.local()

def process_student():
    # 获取当前线程的关联的student:
    std = local_school.student
    print('Hello %s (in %s)' % (std, threading.current_thread().name))

def process_thread(name):
    # 绑定ThreadLocal的student
    local_school.student = name
    process_student()

t1 = threading.Thread(target=process_thread, args=('Alice', ), name='Thread-A')
t2 = threading.Thread(target=process_thread, args=('Bob', ), name='Thread-B')
t1.start()
t2.start()
t1.join()
t2.join()

# Hello Alice (in Thread-A)
# Hello Bob (in Thread-B)
```

    ThreadLocal 最常用的地方就是为每个线程绑定一个数据库连接，HTTP
    请求，用户身份信息等，这样一个线程的所有调用到的处理函数都可以
    非常方便地访问这些资源。

#### 小结
     一个 ThreadLocal 变量虽然是全局变量，但每个线程都只能读写自己线
     程的独立副本，互不干扰。ThreadLocal 解决了参数在一个线程中各个函
     数之间互相传递的问题。

### 进程 vs. 线程

实现多任务，通常我们会设计 Master-Worker 模式

    Master 负责分配任务，Worker 负责执行任务，因此，多任务环境下，通常是一
    个 Master，多个 Worker。
    
    如果用多进程实现 Master-Worker，主进程就是 Master，其他进程就是
    Worker。
    如果用多线程实现 Master-Worker，主线程就是 Master，其他线程就是
    Worker。
    
    多进程模式最大的优点就是稳定性高，多进程模式的缺点是创建进程的代价大
    
    多线程模式通常比多进程快一点，但是也快不到哪去，而且，多线程模
    式致命的缺点就是任何一个线程挂掉都可能直接造成整个进程崩溃，因
    为所有线程共享进程的内存

    在 Windows 下，多线程的效率比多进程要高，所以微软的 IIS 服务器默
    认采用多线程模式。由于多线程存在稳定性的问题，IIS 的稳定性就不
    如 Apache。为了缓解这个问题，IIS 和 Apache 现在又有多进程+多线程
    的混合模式，真是把问题越搞越复杂。

#### 线程切换

多任务一旦多到一个限度，就会消耗掉系统所有的资源，结果效
率急剧下降，所有任务都做不好。

#### 计算密集型 vs. IO 密集型

是否采用多任务的第二个考虑是任务的类型。我们可以把任务分为计算
密集型和 IO 密集型。

计算密集型任务由于主要消耗 CPU 资源，因此，代码运行效率至关重
要。Python 这样的脚本语言运行效率很低，完全不适合计算密集型任务。
对于计算密集型任务，最好用 C 语言编写。

第二种任务的类型是 IO 密集型，涉及到网络、磁盘 IO 的任务都是 IO
密集型任务，这类任务的特点是 CPU 消耗很少，任务的大部分时间都
在等待 IO 操作完成（因为 IO 的速度远远低于 CPU 和内存的速度）。
对于 IO 密集型任务，任务越多，CPU 效率越高，但也有一个限度。常
见的大部分任务都是 IO 密集型任务，比如 Web 应用。

对于 IO 密集型任务，最合适
的语言就是开发效率最高（代码量最少）的语言，脚本语言是首选，C
语言最差。

#### 异步 IO

考虑到 CPU 和 IO 之间巨大的速度差异，一个任务在执行的过程中大部
分时间都在等待 IO 操作，单进程单线程模型会导致别的任务无法并行
执行，因此，我们才需要多进程模型或者多线程模型来支持多任务并发
执行。
现代操作系统对 IO 操作已经做了巨大的改进，最大的特点就是支持异
步 IO。如果充分利用操作系统提供的异步 IO 支持，就可以用单进程单
线程模型来执行多任务，这种全新的模型称为事件驱动模型，Nginx 就
是支持异步 IO 的 Web 服务器，它在单核 CPU 上采用单进程模型就可
以高效地支持多任务。在多核 CPU 上，可以运行多个进程（数量与 CPU
核心数相同），充分利用多核 CPU。由于系统总的进程数量十分有限，
因此操作系统调度非常高效。用异步 IO 编程模型来实现多任务是一个
主要的趋势。

#### 分布式进程

在 Thread 和 Process 中，应当优选 Process，因为 Process 更稳定，而且，
Process 可以分布到多台机器上，而 Thread 最多只能分布到同一台机器
的多个 CPU 上。

    服务进程，服务进程负责启动 Queue，把 Queue 注册到网络上，
    然后往 Queue 里面写入任务：

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import random, time, queue
from multiprocessing.managers import BaseManager

# 发送任务的队列:
task_queue = queue.Queue()
# 接收结果的队列:
result_queue = queue.Queue()

# 自定义函数re_task_queue
def re_task_queue():
    global task_queue
    return task_queue

# 自定义函数re_result_queue
def re_result_queue():
    global result_queue
    return result_queue

# 从BaseManager继承的QueueManager:
class QueueManager(BaseManager):
    pass

if __name__ == '__main__':

    # 把两个Queue都注册到网络上, callable参数关联了Queue对象:
    QueueManager.register('get_task_queue', callable=re_task_queue)
    QueueManager.register('get_result_queue', callable=re_result_queue)
    # 绑定端口5000, 设置验证码'abc':
    manager = QueueManager(address=('127.0.0.1', 5000), authkey=b'abc')
    # 启动Queue:
    manager.start()
    # 获得通过网络访问的Queue对象:
    task = manager.get_task_queue()
    result = manager.get_result_queue()
    # 放几个任务进去:
    for i in range(10):
        n = random.randint(0, 10000)
        print('Put task %d...' % n)
        task.put(n)
    # 从result队列读取结果:
    print('Try get results...')
    for i in range(10):
        r = result.get(True)
        print('Result: %s' % r)
    # 关闭:
    manager.shutdown()
    print('master exit.')
```

```python
# task_worker.py

import time, sys, queue
from multiprocessing.managers import BaseManager

# 创建类似的QueueManager：
class QueueManager(BaseManager):
    pass

# 由于这个QueueManager只从网络上获取Queue,所以注册时提供名字：
QueueManager.register('get_task_queue')
QueueManager.register('get_result_queue')

# 连接到服务器，也就是运行task_master.py的机器：
server_addr = '127.0.0.1'
print('Connect to server %s...' % server_addr)
# 端口和验证码注意保持与task_master.py设置的完全一致：
m = QueueManager(address=(server_addr, 5000), authkey=b'abc')
# 从网络链接：
m.connect()
# 获取Queue的对象：
task = m.get_task_queue()
result = m.get_result_queue()
# 从task队列取任务，并把结果写入result队列：
for i in range(10):
    try:
        n = task.get(timeout=1)
        print('run task %d * %d...' % (n, n))
        r = '%d * %d = %d' % (n, n, n*n)
        time.sleep(1)
        result.put(r)
    except ValueError:
        print('task queue is empty')
# 处理结束
print('worker exit')

```

#### 小结
     Python 的分布式进程接口简单，封装良好，适合需要把繁重任务分布到
     多台机器的环境下。
     注意 Queue 的作用是用来传递任务和接收结果，每个任务的描述数据量
     要尽量小。比如发送一个处理日志文件的任务，就不要发送几百兆的日
     志文件本身，而是发送日志文件存放的完整路径，由 Worker 进程再去
     共享的磁盘上读取文件。

```python
# 建议使用 Python 的 r 前缀，就不用考虑转义的问题了：
s = 'ABC\\-001'
print(s)
s = r'ABC\-001'

```
```python
import re

m = re.match(r'^(\d{3})-(\d{3,8})$', '010-12345')
print(m.group(0))
print(m.group(1))
print(m.group(2))
for i in m.groups():
    print(i)

# 010-12345
# 010
# 12345
```

提取子串非常有用。来看一个更凶残的例子    
```python
import re

t = '19:05:30'
m = re.match(r'^(0[0-9]|1[0-9]|2[0-3]|[0-9])\:(0[0-9]|1[0-9]|2[0-9]|3[0-9]|4[0-9]|5[0-9]|[0-9])\:(0[0-9]|1[0-9]|2[0-9]|3[0-9]|4[0-9]|5[0-9]|[0-9])$', t)
print(m.groups())
```

    这个正则表达式可以直接识别合法的时间。但是有些时候，用正则表达
    式也无法做到完全验证，比如识别日期：
    '^(0[1-9]|1[0-2]|[0-9])-(0[1-9]|1[0-9]|2[0-9]|3[0-1]|[0-9])$'
    对于'2-30'，'4-31'这样的非法日期，用正则还是识别不了，或者说写
    出来非常困难，这时就需要程序配合识别了。

#### 贪婪匹配

```
最后需要特别指出的是，正则匹配默认是贪婪匹配，也就是匹配尽可能
多的字符。举例如下，匹配出数字后面的 0：
>>> re.match(r'^(\d+)(0*)$', '102300').groups()
('102300', '')
由于\d+采用贪婪匹配，直接把后面的 0 全部匹配了，结果 0*只能匹配
空字符串了。
必须让\d+采用非贪婪匹配（也就是尽可能少匹配），才能把后面的 0
匹配出来，加个?就可以让\d+采用非贪婪匹配：
>>> re.match(r'^(\d+?)(0*)$', '102300').groups()
('1023', '00')

```

#### 编译

当我们在 Python 中使用正则表达式时，re 模块内部会干两件事情：
1. 编译正则表达式，如果正则表达式的字符串本身不合法，会报错；
2. 用编译后的正则表达式去匹配字符串。
如果一个正则表达式要重复使用几千次，出于效率的考虑，我们可以预
编译该正则表达式，接下来重复使用时就不需要编译这个步骤了，直接
匹配：

```
>>> import re
# 编译:
>>> re_telephone = re.compile(r'^(\d{3})-(\d{3,8})$')
# 使用：
>>> re_telephone.match('010-12345').groups()
('010', '12345')
>>> re_telephone.match('010-8086').groups()
('010', '8086')
编译后生成 Regular Expression 对象，由于该对象自己包含了正则表达
式，所以调用对应的方法时不用给出正则字符串。
```

