目录链接：https://www.jianshu.com/p/e1e201bea601

在处理 I/O 操作时， 使用多线程与普通的单线程相比， 效率得到了极大的提高， 为什么还需要 Asyncio？

多线程有诸多优点且应用广泛，但也存在一定的局限性：

- 多线程运行过程容易被打断， 因此有可能出现 race condition 的情况；
- 线程切换本身存在一定的损耗， 线程数不能无限增加， 因此， 如果你的 I/O 操作非常heavy， 多线程很有可能满足不了高效率、高质量的需求。

## 什么是 Asyncio

### Sync（同步）  VS Async（异步） 

**Sync（同步）** 指操作一个接一个地执行， 下一个操作必须等上一个操作完成后才能执行。
**Async（异步）** 指不同操作间可以相互交替执行，如果其中的某个操作被 block 了， 程序并不会等待， 而是会找出可执行的操作继续执行。

### Asyncio 工作原理

Asyncio 和其他 Python 程序一样， 是单线程的， 它只有一个主线程， 但是可以进行多个不同的任务（task） ， 这里的任务， 就是特殊的 future 对象。 这些不同的任务， 被一个叫做event loop 的对象所控制。 

可以假设任务只有两个状态：一是预备状态；一是等待状态。 所谓的预备状态， 是指任务当前空闲， 但随时待命准备运行。所谓的等待状态， 是指任务已经运行， 但正在等待外部的操作完成， 如 I/O 操作。在这种情况下， event loop 会维护两个任务列表， 分别对应这两种状态。

event loop 选取预备状态的一个任务， 使其运行，一直到这个任务把控制权交还给 event loop 为止。当任务把控制权交还给 event loop 时， event loop 会根据其是否完成， 把任务放到预备或等待状态的列表， 然后遍历等待状态列表的任务， 查看他们是否完成。如果完成， 则将其放到预备状态的列表；如果未完成， 则继续放在等待状态的列表。原先在预备状态列表的任务位置仍旧不变， 因为它们还未运⾏。

对于 Asyncio 来说， 它的任务在运行时不会被外部的⼀些因素打断， 因此 Asyncio内的操作不会出现 race condition 的情况， 这样你就不需要担心线程安全的问题了。

### Asyncio 用法

```python
import asyncio
import aiohttp
import time
async def download_one(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as resp:
            print('Read {} from {}'.format(resp.content_length, url))
async def download_all(sites):
    # asyncio.create_task(coro)Python 3.7+ 新增的， 如果是之前的版本 可以asyncio.ensure_future(coro) 等效替代
    tasks = [asyncio.create_task(download_one(site)) for site in sites]
    await asyncio.gather(*tasks)
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
    asyncio.run(download_all(sites))
    end_time = time.perf_counter()
    print('Download {} sites in {} seconds'.format(len(sites), end_time - start_time))
if __name__ == '__main__':
    main()
```
![Asyncio 用法01.png](https://upload-images.jianshu.io/upload_images/16846478-f568c15471428784.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

主函数里的 asyncio.run(coro) 是 Asyncio 的 root call， 表示拿到 event loop， 运行输入的coro， 直到它结束， 最后关闭这个 event loop。 asyncio.run() 是 Python3.7+ 才引入，起源码如下

```python
def run(main, *, debug=False):
    """Run a coroutine.

    This function runs the passed coroutine, taking care of
    managing the asyncio event loop and finalizing asynchronous
    generators.

    This function cannot be called when another asyncio event loop is
    running in the same thread.

    If debug is True, the event loop will be run in debug mode.

    This function always creates a new event loop and closes it at the end.
    It should be used as a main entry point for asyncio programs, and should
    ideally only be called once.

    Example:

        async def main():
            await asyncio.sleep(1)
            print('hello')

        asyncio.run(main())
    """
    if events._get_running_loop() is not None:
        raise RuntimeError(
            "asyncio.run() cannot be called from a running event loop")

    if not coroutines.iscoroutine(main):
        raise ValueError("a coroutine was expected, got {!r}".format(main))

    loop = events.new_event_loop()
    try:
        events.set_event_loop(loop)
        loop.set_debug(debug)
        return loop.run_until_complete(main)
    finally:
        try:
            _cancel_all_tasks(loop)
            loop.run_until_complete(loop.shutdown_asyncgens())
        finally:
            events.set_event_loop(None)
            loop.close()
```

老版本可以使用以下语句

```python
loop = asyncio.get_event_loop()
try:
    loop.run_until_complete(coro)
finally:
    loop.close()

```
相比于https://www.jianshu.com/p/8b943eca9294多线程版本效率更高了

### 多线程还是 Asyncio

多线程和Asyncio 到底如何选择呢？

- 如果是 I/O bound， 并且 I/O 操作很慢， 需要很多任务 / 线程协同实现， 那么使用 Asyncio更合适
- 如果是 I/O bound， 但是 I/O 操作很快， 只需要有限数量的任务 / 线程， 那么使用多线程就可以了。
- 如果是 CPU bound， 则需要使用多进程来提高程序运行效率。

```python
if io_bound:
    if io_slow:
        
        print('Use Asyncio')
    else:
        print('Use multi-threading')
else if cpu_bound:
    print('Use multi-processing')
```


## 参考资料：

*极客时间 Python核心技术与实战学习*

Python核心技术与实战（极客时间）链接：
http://gk.link/a/103Sv

Event Loop:
[https://docs.python.org/3/library/asyncio-eventloop.html](https://docs.python.org/3/library/asyncio-eventloop.html)

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
