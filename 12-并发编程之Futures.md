目录链接：https://www.jianshu.com/p/e1e201bea601


## 区分并发和并行

### 发并

在 Python 中， 并发并不是指同一时刻有多个操作（thread、 task） 同时进行。 相反， 某个特定的时刻， 它只允许有一个操作发生， 只不过线程 / 任务之间会互相切换， 直到完成。 

![图片来自极客时间 Python核心技术与实战.png](https://upload-images.jianshu.io/upload_images/16846478-c87000b5e001302a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

图中出现了 thread 和 task 两种切换顺序的不同方式， 分别对应 Python 中并发的两种形式——threading 和 asyncio。


对于 threading，操作系统知道每个线程的所有信息， 因此它会做主在适当的时候做线程切换。容易出现 race condition的情况。

对于 asyncio，主程序想要切换任务时， 必须得到此任务可以被切换的通知， 这样一来也就可以避免 race condition 的情况。

### 并行

并行指的才是同一时刻、 同时发生。 Python 中的 multi-processing 便是这个意思，对于 multi-processing， 可以简单地这么理解：比如你的电脑是 6 核处理器， 那么在运行程序时， 就可以强制 Python 开 6 个进程， 同时执行， 以加快运行速度

![图片来自极客时间 Python核心技术与实战.png](https://upload-images.jianshu.io/upload_images/16846478-2f4023adc2b05f45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


并发通常应用于 I/O 操作频繁的场景，比如要从网站上下载多个文，件I/O 操作的时间可能会比 CPU 运行处理的时间长得多。并行则更多应用于 CPU heavy 的场景。


## 并发编程之 Futures

### 单线程与多线程性能

下载一些网站的内容并打印

单线程版本（忽略了异常处理）：

```python
import requests
import time
def download_one(url):
    resp = requests.get(url)
    print('Read {} from {}'.format(len(resp.content), url))
def download_all(sites):
    for site in sites:
        download_one(site)
def main():
    sites = [
        'https://en.wikipedia.org/wiki/Portal:Arts',
        'https://en.wikipedia.org/wiki/Portal:History',
        'https://en.wikipedia.org/wiki/Portal:Society',
        'https://en.wikipedia.org/wiki/Portal:Biography',
        'https://en.wikipedia.org/wiki/Portal:Mathematics',
        'https://en.wikipedia.org/wiki/Portal:Technology',
        'https://en.wikipedia.org/wiki/Portal:Geography',
        'https://en.wikipedia.org/wiki/Portal:Science',
        'https://en.wikipedia.org/wiki/Computer_science',
        'https://en.wikipedia.org/wiki/Python_(programming_language)',
        'https://en.wikipedia.org/wiki/Java_(programming_language)',
        'https://en.wikipedia.org/wiki/PHP',
        'https://en.wikipedia.org/wiki/Node.js',
        'https://en.wikipedia.org/wiki/The_C_Programming_Language',
        'https://en.wikipedia.org/wiki/Go_(programming_language)'
    ]
    start_time = time.perf_counter()
    download_all(sites)
    end_time = time.perf_counter()
    print('Download {} sites in {} seconds'.format(len(sites), end_time - start_time))
if __name__ == '__main__':
    main()
```

![单线程版本.png](https://upload-images.jianshu.io/upload_images/16846478-4948de474cdbbe0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

先是遍历存储网站的列表，然后对当前网站执行下载操作，等到当前操作完成后， 再对下一个网站进行同样的操作，直到结束。绝大多数时间， 都浪费在了 I/O 等待上。 程序每次对一个网站执行下载操作， 都必须等到前一个站下载完成后才能开始。

多线程版本：

```python
import concurrent.futures
import requests
import time
def download_one(url):
    resp = requests.get(url)
    print('Read {} from {}'.format(len(resp.content), url))
def download_all(sites):
    with concurrent.futures.ThreadPoolExecutor(max_workers=5) as executor:
        executor.map(download_one, sites)
def main():
    sites = [
        'https://en.wikipedia.org/wiki/Portal:Arts',
        'https://en.wikipedia.org/wiki/Portal:History',
        'https://en.wikipedia.org/wiki/Portal:Society',
        'https://en.wikipedia.org/wiki/Portal:Biography',
        'https://en.wikipedia.org/wiki/Portal:Mathematics',
        'https://en.wikipedia.org/wiki/Portal:Technology',
        'https://en.wikipedia.org/wiki/Portal:Geography',
        'https://en.wikipedia.org/wiki/Portal:Science',
        'https://en.wikipedia.org/wiki/Computer_science',
        'https://en.wikipedia.org/wiki/Python_(programming_language)',
        'https://en.wikipedia.org/wiki/Java_(programming_language)',
        'https://en.wikipedia.org/wiki/PHP',
        'https://en.wikipedia.org/wiki/Node.js',
        'https://en.wikipedia.org/wiki/The_C_Programming_Language',
        'https://en.wikipedia.org/wiki/Go_(programming_language)'
    ]
    start_time = time.perf_counter()
    download_all(sites)
    end_time = time.perf_counter()
    print('Download {} sites in {} seconds'.format(len(sites), end_time - start_time))
if __name__ == '__main__':
    main()
```
![多线程版本.png](https://upload-images.jianshu.io/upload_images/16846478-794e54de3762f716.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

多线程版本和单线程版的主要区别所在：

```python
   with concurrent.futures.ThreadPoolExecutor(max_workers=5) as executor:
        executor.map(download_one, sites)
```

这里创建了一个线程池，总共有 5 个线程可以分配使用，executer.map() 表示对 sites 中的每一个元素， 并发地调用函数download_one()。由于 requests.get() 是线程安全的（thread-safe） ，在多线程的环境下， 它也可以安全使用， 并不会出现 race condition 的情况。

由于线程的创建、 维护和删除也会有一定的开销，所以线程数并不是越多越好。

也可以用并行的方式去提高程序运行效率，只需要在 download_all() 函数中将ThreadPoolExecutor(workers)修改为ProcessPoolExecutor()
```python
with concurrent.futures.ThreadPoolExecutor(workers) as executor
=>
with concurrent.futures.ProcessPoolExecutor() as executor: 
```

![并行多进程.png](https://upload-images.jianshu.io/upload_images/16846478-dce744398df34a95.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

函数 ProcessPoolExecutor() 表示创建进程池， 使用多个进程并行的执行程序。通常省略参数 workers， 因为系统会自动返回 CPU 的数量作为可以调的进程数。

并行的方式一般用在 CPU heavy 的场景中， 因为对于 I/O heavy 的操作， 多数时间都会用于等待，相⽐于多线程， 使用多进程并不会提升效率。 反而很多时候， 因为 CPU 数量的限制， 会导致其执行效率不如多线程版本。

### 什么是 Futures 

可参看：[https://docs.python.org/zh-cn/3/library/concurrent.futures.html](https://docs.python.org/zh-cn/3/library/concurrent.futures.html)

Python 中的 Futures 模块， 位于 concurrent.futures 和 asyncio 中， 它们都表示带有延迟的操作。 Futures 会将处于等待状态的操作包裹起来放到队列中， 这些操作的状态随时可以查询， 当然， 它们的结果或是异常， 也能够在操作完成后被获取。

一些函数：
```python
Executor.submit(fn, *args, **kwargs) ：
调度可调用对象 fn，以 fn(*args **kwargs) 方式执行并返回 Future 对像代表可调用对象的执行。

Futures.done():
如果调用已被取消或正常结束那么返回 True,False 表示没有完成。done() 是 non-blocking 的， 会立即返回结果。

Futures.add_done_callback(fn)：
Futures 完成后， 相对应的参数函数 fn， 会被通知并执行调用。

Futures.result(timeout=None):
当 future 完成后， 返回其对应的结果或异常。如果调用还没完成那么这个方法将等待 timeout 秒。如果在 timeout*秒内没有执行完成，concurrent.futures.TimeoutError将会被触发。

futures.as_completed(fs, timeout=None):
针对给定的 future 迭代器 fs， 在其完成后， 返回完成后的迭代器。任何由 fs 所指定的重复future将只被返回一次。
```
多线程版本还可以改写成：
```python
import concurrent.futures
import requests
import time
def download_one(url):
    resp = requests.get(url)
    print('Read {} from {}'.format(len(resp.content), url))
def download_all(sites):
    with concurrent.futures.ThreadPoolExecutor(max_workers=5) as executor:
        to_do = []
        for site in sites:
            future = executor.submit(download_one,site)
            to_do.append(future)
        for future in concurrent.futures.as_completed(to_do):
            future.result()
def main():
    sites = [
        'https://en.wikipedia.org/wiki/Portal:Arts',
        'https://en.wikipedia.org/wiki/Portal:History',
        'https://en.wikipedia.org/wiki/Portal:Society',
        'https://en.wikipedia.org/wiki/Portal:Biography',
        'https://en.wikipedia.org/wiki/Portal:Mathematics',
        'https://en.wikipedia.org/wiki/Portal:Technology',
        'https://en.wikipedia.org/wiki/Portal:Geography',
        'https://en.wikipedia.org/wiki/Portal:Science',
        'https://en.wikipedia.org/wiki/Computer_science',
        'https://en.wikipedia.org/wiki/Python_(programming_language)',
        'https://en.wikipedia.org/wiki/Java_(programming_language)',
        'https://en.wikipedia.org/wiki/PHP',
        'https://en.wikipedia.org/wiki/Node.js',
        'https://en.wikipedia.org/wiki/The_C_Programming_Language',
        'https://en.wikipedia.org/wiki/Go_(programming_language)'
    ]
    start_time = time.perf_counter()
    download_all(sites)
    end_time = time.perf_counter()
    print('Download {} sites in {} seconds'.format(len(sites), end_time - start_time))
if __name__ == '__main__':
    main()
```

![多线程版本.png](https://upload-images.jianshu.io/upload_images/16846478-89c32e7dc27d70f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

先调用 executor.submit()， 将下载每一个网站的内容都放进 future 队列 to_do， 等待执行。 然后是 as_completed() 函数， 在 future 完成后， 便输出结果。


## 参考资料：

*极客时间 Python核心技术与实战学习*

Python核心技术与实战（极客时间）链接：
http://gk.link/a/103Sv


concurrent.futures --- 启动并行任务：
[https://docs.python.org/zh-cn/3/library/concurrent.futures.html](https://docs.python.org/zh-cn/3/library/concurrent.futures.html)





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
