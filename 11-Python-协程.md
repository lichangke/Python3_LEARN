目录链接：https://www.jianshu.com/p/e1e201bea601

## 什么是协程？

### 从一个爬虫说起
一个简单的爬虫例子：
```python
import time
def crawl_page(url):
    print('crawling {}'.format(url))
    sleep_time = int(url.split('_')[-1])
    time.sleep(sleep_time)
    print('OK {}'.format(url))
def main(urls):
    for url in urls:
        crawl_page(url)
star = time.perf_counter()
main(['url_1', 'url_2', 'url_3', 'url_4'])
print('Wait all time: {:.2f}s'.format(time.perf_counter() - star))
```
![从一个爬虫说起01.png](https://upload-images.jianshu.io/upload_images/16846478-a9447fe4aed7e6f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


注意：主要目的是协程的基础概念， 因此简化爬虫的 scrawl_page 函数为休眠数秒， 休眠时间取决于 url 最后的那个数字。 

这个例子串行执行，五个页面分别用了 1 秒到 4 秒的时间， 加起来一共用了 10 秒。

使用协程：

```python
import time
import asyncio
async def crawl_page(url):
    print('crawling {}'.format(url))
    sleep_time = int(url.split('_')[-1])
    await asyncio.sleep(sleep_time)
    print('OK {}'.format(url))
async def main(urls):
    for url in urls:
        await crawl_page(url)
star = time.perf_counter()
asyncio.run(main(['url_1', 'url_2', 'url_3', 'url_4']))
print('Wait all time: {:.2f}s'.format(time.perf_counter() - star))
```
![从一个爬虫说起02.png](https://upload-images.jianshu.io/upload_images/16846478-e543de15fbbbdb65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 Python 3.7 以上版本

 import asyncio， 这个库包含了大部分实现协程所需的工具。async 修饰词声明异步函数， 于是， 这里的 crawl_page 和 main 都变成了异步函数。而调用异步函数， 我们便可得到一个协程对象（coroutine object） 。

执行协程有多种方法，这里介绍一下常用的三种

1、首先， 可以通过 await 来调用。await 执行的效果，程序会阻塞在这里， 进入被调用的协程函数， 执行完毕返回后再继续， 这也是 await 的字面意思。 await asyncio.sleep(sleep_time) 会在这休息若干秒， await crawl_page(url) 则会执行crawl_page() 函数。

2、其次， 可以通过 asyncio.create_task() 来创建任务

3、最后， 需要 asyncio.run 来触发运行。 asyncio.run 这个函数是 Python 3.7 之后才有的特性。一个非常好的编程规范是， asyncio.run(main()) 作为主程序的入口函数，在程序运行周期内， 只调用一次asyncio.run。

await 是同步调用， 程序会阻塞在这里，因此， crawl_page(url) 在当前的调用结束之前， 是不会触发下⼀次调用，还是10 秒。从一个爬虫说起02这个代码效果就和从一个爬虫说起01完全一样了，用异步接口写了个同步代码。

引入协程中的一个重要概念任务（Task） 

```python
import time
import asyncio
async def crawl_page(url):
    print('crawling {}'.format(url))
    sleep_time = int(url.split('_')[-1])
    await asyncio.sleep(sleep_time)
    print('OK {}'.format(url))
async def main(urls):
    tasks = [ asyncio.create_task(crawl_page(url)) for url in urls]
    for task in tasks:
        await task
star = time.perf_counter()
asyncio.run(main(['url_1', 'url_2', 'url_3', 'url_4']))
print('Wait all time: {:.2f}s'.format(time.perf_counter() - star))
```
![从一个爬虫说起03.png](https://upload-images.jianshu.io/upload_images/16846478-f202d0470a124d14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有了协程对象后， 便可以通过 asyncio.create_task 来创建任务，任务创建后很快就会被调度执行， 这样， 代码也不会阻塞在任务这里。 为了等所有任务都结束， 可以用 for task in tasks: await task ，也可以用await asyncio.gather(*tasks) 见下一段代码
可以看到最后的运行时间等于运行时间最长的爬虫。

执行 tasks， 还有另一种做法：

```python
import time
import asyncio
async def crawl_page(url):
    print('crawling {}'.format(url))
    sleep_time = int(url.split('_')[-1])
    await asyncio.sleep(sleep_time)
    print('OK {}'.format(url))
async def main(urls):
    tasks = [ asyncio.create_task(crawl_page(url)) for url in urls]
    # for task in tasks:
    #     await task
    await asyncio.gather(*tasks)
star = time.perf_counter()
asyncio.run(main(['url_1', 'url_2', 'url_3', 'url_4']))
print('Wait all time: {:.2f}s'.format(time.perf_counter() - star))
```
![从一个爬虫说起04.png](https://upload-images.jianshu.io/upload_images/16846478-166380250fb1ba36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注意这里 *tasks 解包列表， 将列表变成了函数的参数；与之对应的是， ** dict 将字典变成了函数的参数。

### 协程运行时

看如下两段代码：

```python
import time
import asyncio
async def worker_1():
    print('worker_1 start')
    await asyncio.sleep(1)
    print('worker_1 done')
async def worker_2():
    print('worker_2 start')
    await asyncio.sleep(2)
    print('worker_2 done')
async def main():
    print('before await')
    await worker_1()
    print('awaited worker_1')
    await worker_2()
    print('awaited worker_2')
star = time.perf_counter()
asyncio.run(main())
print('Wait all time: {:.2f}s'.format(time.perf_counter() - star))
```

![协程运行时01.png](https://upload-images.jianshu.io/upload_images/16846478-225ae725ee2dac79.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```python
import time
import asyncio
async def worker_1():
    print('worker_1 start')
    await asyncio.sleep(1)
    print('worker_1 done')
async def worker_2():
    print('worker_2 start')
    await asyncio.sleep(2)
    print('worker_2 done')
async def main():
    task1 = asyncio.create_task(worker_1())
    task2 = asyncio.create_task(worker_2())
    print('before await')
    await task1
    print('awaited worker_1')
    await task2
    print('awaited worker_2')
star = time.perf_counter()
asyncio.run(main())
print('Wait all time: {:.2f}s'.format(time.perf_counter() - star))
```

![协程运行时02.png](https://upload-images.jianshu.io/upload_images/16846478-39472fb61bf60ab3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

协程运行时02这段代码发生了什么呢？下面拆解了整个过程

1、asyncio.run(main()) ，程序进入 main() 函数， 事件循环开启；

2、task1 和 task2 任务被创建，并进入事件循环等待运行；print打印输出'before await'；

3、await task1执行后用户选择从当前的主任务中切出， 事件调度器开始调度 worker_1；

4、 worker_1 开始运行，  print 打印输出 'worker_1 start' ， 然后运行到 await asyncio.sleep(1) ， 从当前任务切出， 事件调度器开始调度 worker_2；

5、worker_2 开始运行，  print 打印输出 'worker_2 start' ， 然后运行到 await asyncio.sleep(2)从当前任务切出；

6、以上所有事件的运行时间， 都应该在 1ms 到 10ms 之间， 甚至可能更短， 事件调度器从这个时候开始暂停调度；

7、1秒钟后， worker_1 的 sleep 完成， 事件调度器将控制权重新传给 task_1， 输出'worker_1 done' ， task_1 完成任务， 从事件循环中退出；

8、await task1 完成， 事件调度器将控制器传给主任务， 输出 'awaited worker_1' ， ·然后在await task2 处继续等待；

9、2秒钟后， worker_2 的 sleep 完成， 事件调度器将控制权重新传给 task_2， 输出'worker_2 done' ， task_2 完成任务， 从事件循环中退出；

10、主任务输出 'awaited worker_2' ， 协程全任务结束， 事件循环结束

给某些协程任务限定运行时间，一旦超时就取消 或者 某些协程运行时出现错误， 该怎么处理呢？见如下代码：

```python
import time
import asyncio
async def worker_1():
    await asyncio.sleep(1)
    return 1
async def worker_2():
    await asyncio.sleep(2)
    return 2 / 0  # 运行时出现错误
async def worker_3():
    await asyncio.sleep(3)
    return 3
async def main():
    task_1 = asyncio.create_task(worker_1())
    task_2 = asyncio.create_task(worker_2())
    task_3 = asyncio.create_task(worker_3())
    await asyncio.sleep(2)
    task_3.cancel()  # 限定时间，取消协程任务
    res = await asyncio.gather(task_1, task_2, task_3, return_exceptions=True)
    print(res)

star = time.perf_counter()
asyncio.run(main())
print('Wait all time: {:.2f}s'.format(time.perf_counter() - star))
```
![协程运行时03.png](https://upload-images.jianshu.io/upload_images/16846478-77ebf24a9f199045.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

worker_1 正常运行， worker_2 运行中出现错误， worker_3 执行时间过长被cancel 掉了， 这些信息会全部体现在最终的返回结果 res 中。注意 return_exceptions=True，如果不设置这个参数， 错误就会完整地 throw 到执行层， 从而需要 try except 来捕捉， 这也就意味着其他还没被执行的任务会被全部取消掉。 

### 协程实现生产者消费者模型

```python
import time
import asyncio
import random
# 消费者
async def consumer(queue, my_id):
    while True:
        val = await queue.get()
        print('{} get a val: {}'.format(my_id, val))
        await asyncio.sleep(1)
# 生产者
async def producer(queue, my_id):
    for i in range(2):
        val = random.randint(1, 10)
        await queue.put(val)
        print('{} put a val: {}'.format(my_id, val))
        await asyncio.sleep(1)
async def main():
    queue = asyncio.Queue()
    consumer_1 = asyncio.create_task(consumer(queue, "consumer_1"))
    consumer_2 = asyncio.create_task(consumer(queue, "consumer_2"))
    producer_1 = asyncio.create_task(producer(queue, "producer_1"))
    producer_2 = asyncio.create_task(producer(queue, "producer_2"))
    await asyncio.sleep(10)
    consumer_1.cancel()
    consumer_2.cancel()
    await asyncio.gather(consumer_1, consumer_2, producer_1, producer_2, return_exceptions=True)
star = time.perf_counter()
asyncio.run(main())
print('Wait all time: {:.2f}s'.format(time.perf_counter() - star))
```

![协程实现生产者消费者模型.png](https://upload-images.jianshu.io/upload_images/16846478-ecba416f808ae06a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 参考资料：

*极客时间 Python核心技术与实战学习*

Python核心技术与实战（极客时间）链接：
http://gk.link/a/103Sv

asyncio--- 异步 I/O：
[https://docs.python.org/zh-cn/3/library/asyncio.html](https://docs.python.org/zh-cn/3/library/asyncio.html)


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
