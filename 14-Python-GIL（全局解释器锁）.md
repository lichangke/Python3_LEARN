目录链接：https://www.jianshu.com/p/e1e201bea601

GIL（Global Interpreter Lock， 即全局解释器锁）

## 一个不解之谜

假设有下面这段很简单的 cpu-bound 代码：

```python
def CountDown(n):
    while n > 0:
        n -= 1
```
如果有一个很大的数 n = 100000000，单线程与多线程的情况下执行CountDown(n)

### 单线程

![单线程.png](https://upload-images.jianshu.io/upload_images/16846478-f53ad8d7ebb4f36f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 多线程

![多线程.png](https://upload-images.jianshu.io/upload_images/16846478-087d662a194f30cd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

多线程试了2个线程，5个线程其耗时和单线程基本相同

## 为什么有 GIL？

是什么导致上面的情况出现呢？“罪魁祸首”就是 GIL， 导致了 Python 线程的性能并不像我们期望的那样。

GIL， 是Python 解释器 CPython 中的一个技术术语。 它的意思是全局解释器锁， 本质上是类似操作系统的 Mutex。 每一个 Python 线程， 在 CPython 解释器中执行时， 都会先锁住自己的线程， 阻止别的线程执行。当然， CPython 会 轮流执行 Python 线程，用户看到的就是“伪并行”——Python 线程在交错执行， 来模拟真正并行的线程。

为什么 CPython 需要 GIL 呢？这和 CPython 的实现有关。CPython 使用引用计数来管理内存， 所有 Python 脚本中创建的实例， 都会有一个引用计数， 来记录有多少个指针指向它。 当引用计数只有 0 时， 则会自动释放内存。

![引用计数.png](https://upload-images.jianshu.io/upload_images/16846478-0509d27855aaa6a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

a 的引用计数是 3， 因为有 a、 b 和作为参数传递的 getrefcount 这三个地方， 都引用个一空列表。

这样一来， 如果有两个 Python 线程同时引用了 a， 就会造成引用计数的 race condition， 引用计数可能最终只增加 1， 这样就会造成内存被污染。 第一个线程结束时， 会把引用计数减少 1，这时可能达到条件释放内存， 当第二个线程再试图访问 a 时， 就找不到有效的内存了。

CPython 引进 GIL 其实主要就是这么两个原因：

- 设计者为了规避类似于内存管理这样的复杂的竞争风险问题（race condition） ；
- 因为 CPython 大量使用 C 语言库， 但大部分 C 语言库都不是原生线程安全的（线程安全会降低性能和增加复杂度） 。

## GIL 是如何工作的？

如下图， 就是一个 GIL 在 Python 程序的工作示例。 

![图片来自极客时间 Python核心技术与实战.png](https://upload-images.jianshu.io/upload_images/16846478-6576cbd2c0bfe6e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Thread 1、 2、 3 轮流执行， 每一个线程在开始执行时， 都会锁住 GIL， 以阻止别的线程执行；同样的， 每一个线程执行完一段后，会释放 GIL， 以允许别的线程开始利用资源

Python 线程会去主动释放 GIL，不然别的线程就都没有了运行的机会。CPython 中还有另⼀个机制， 叫做 check_interval， 意思是 CPython 解释器会去轮询检查线程 GIL 的锁住情况。 每隔一段时间， Python 解释器就会强制当前线程去释放 GIL， 这样让别的线程有执行机会。

## Python 的线程安全

有了 GIL， 并不意味着我们 Python 编程者就不用去考虑线程安全了。虽然 GIL仅允许一个 Python 线程执行，Python 还有 check interval 这样的抢占机制。

```python
import threading
n = 0
def foo():
    global n
    n += 1
threads = []
for i in range(100):
    t = threading.Thread(target=foo)
    threads.append(t)
for t in threads:
    t.start()
for t in threads:
    t.join()
print(n)
```
会发现出现打印 99 或者 98的情况。因为， n+=1 这一句代码让线程并不安全。作为 Python 的使用者， 我们还是需要 lock 等工具， 来确保线程安全。

```python
n = 0
lock = threading.Lock()
def foo():
    global n
    with lock:
        n += 1
```

## 如何绕过 GIL？

绕过 GIL 的大致思路有这么两种：
1. 绕过 CPython， 使用 JPython（Java 实现的 Python 解释器） 等别的实现；
2. 把关键性能代码， 放到别的语言（一般是 C++） 中实现。


## 参考资料：

*极客时间 Python核心技术与实战学习*

Python核心技术与实战（极客时间）链接：
http://gk.link/a/103Sv

----
>*GitHub链接：*
>*[https://github.com/lichangke/LeetCode](https://github.com/lichangke/LeetCode)*

>*知乎个人首页：*
>*[https://www.zhihu.com/people/lichangke/](https://www.zhihu.com/people/lichangke/)*

>*简书个人首页：*
>*[https://www.jianshu.com/u/3e95c7555dc7](https://www.jianshu.com/u/3e95c7555dc7)*

>*个人Blog:*
>*[https://lichangke.github.io/](https://lichangke.github.io/)*

>*欢迎大家来一起交流学习*
