目录链接：https://www.jianshu.com/p/e1e201bea601


## 什么是上下文管理器？

在任何一门编程语言中， 文件的输入输出、 数据库的连接断开等， 都是很常见的资源管理操作。但资源都是有限的， 在写程序时， 我们必须保证这些资源在使用过后得到释放， 不然就容易造成资源泄露。
下面这一段代码：
```python
for x in range(10000000): 
    f = open('test.txt', 'w')
    f.write('hello') 
```
共打开了 10000000 个文件， 但是用完以后都没有关闭它们，这就是一个典型的资源泄露的例子。
 
为了解决这个问题， 不同的编程语言都引入了不同的机制。 在 Python 中， 对应的解决方式便是上下文管理器（context manager） 。

上下文管理器， 能够帮助你自动分配并且释放资源， 其中最典型的应用便是 with 语句。  

上面代码的正确写法如下：

```python
for x in range(10000000):
    with open('test.txt', 'w') as f:
        f.write('hello')
```
每次打开文件 “test.txt” ， 并写入 ‘hello’ 之后， 这个文件便会自动关闭， 相应的资源也可以得到释放， 防止资源泄露。

with 语句的代码， 也可以用下面的形式表示：

```python
f = open('test.txt', 'w')
try:
    f.write('hello')
finally:
    f.close()
```

另外一个典型的例子， 是 Python 中的 threading.lock 类。 

想要获取一个锁，执行相应的操作， 完成后再释放， 那么代码就可以写成下面这样：

```python
some_lock = threading.Lock()
some_lock.acquire()
try:
    ...
finally:
    some_lock.release()
```
对应的 with 语句:

```python
some_lock = threading.Lock()
with somelock:
    ...
```
## 上下问管理器的实现

### 基于类的上下文管理器

自定义了一个上下文管理类 FileManager， 模拟 Python 的打开、 关闭文件操作：

```python
class FileManager:
    def __init__(self, name, mode):
        print('calling __init__ method')
        self.name = name
        self.mode = mode 
        self.file = None
    def __enter__(self):
        print('calling __enter__ method')
        self.file = open(self.name, self.mode)
        return self.file
    def __exit__(self, exc_type, exc_val, exc_tb):
        print('calling __exit__ method')
        if self.file:
            self.file.close() 
with FileManager('test.txt', 'w') as f:
    print('ready to write to file')
    f.write('hello world')
```
![基于类的上下文管理器.png](https://upload-images.jianshu.io/upload_images/16846478-ba4b92858496ee00.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

必须保证这个类包括方法 ”__enter__()” 和方法 “__exit__()” 。 其中， 方法 “__enter__()” 返回需要被管理的资源， 方法 “__exit__()” 里通常会存在一些释放、 清理资源的操作， 

使用with 语句， 执行这个上下文管理器时：

```python
with FileManager('test.txt', 'w') as f:
    f.write('hello world')
```
下面这四步操作会依次发生：

1. 方法 “__init__()” 被调用， 程序初始化对象 FileManager， 使得文件名（name）是 "test.txt" ， 文件模式 (mode) 是 'w' ；
2. 方法 “__enter__()” 被调用， 文件 “test.txt” 以写入的模式被打开， 并且返回 FileManager对象赋予变量 f；
3. 字符串 “hello world” 被写入文件 “test.txt” ；
4. 方法 “__exit__()” 被调用， 负责关闭之前打开的文件流

方法 “__exit__()” 中的参数 “exc_type, exc_val, exc_tb” ， 分别表⽰exception_type、 exception_value 和 traceback。 当我们执行含有上下文管理器的 with 语句时， 如果有异常抛出， 异常的信息就会包含在这三个变量中， 传入方法 “__exit__()” 。

我们就可以在方法 “__exit__()”中添加相应的异常处理，例子如下：
```python
class Foo:
    def __init__(self):
        print('__init__ called')        
    def __enter__(self):
        print('__enter__ called')
        return self
    def __exit__(self, exc_type, exc_value, exc_tb):
        print('__exit__ called')
        if exc_type:
            print(f'exc_type: {exc_type}')
            print(f'exc_value: {exc_value}')
            print(f'exc_traceback: {exc_tb}')
            print('exception handled')
        return True
with Foo() as obj:
    raise Exception('exception raised').with_traceback(None)
```

![ “__exit__()”中添加相应的异常处理.png](https://upload-images.jianshu.io/upload_images/16846478-c055fc324cadaa23.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 基于生成器的上下文管理器

可以使用装饰器 contextlib.contextmanager， 来定义自己所需的基于生成器的上下文管理器， 用以支持with 语句。

前面的类上下文管理器 FileManager可以如下改写：
```python
from contextlib import contextmanager
@contextmanager
def file_manager(name, mode):
    try:
        f = open(name, mode)
        yield f
    finally:
        f.close()     
with file_manager('test.txt', 'w') as f:
    f.write('hello world')
```
![基于生成器的上下文管理器.png](https://upload-images.jianshu.io/upload_images/16846478-c8aee857e24d7464.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

函数 file_manager() 是一个生成器， 当我们执行 with 语句时， 便会打开文件， 并返回文件对象 f；当 with 语句执行完后， finally block 中的关闭文件操作便会执行。

不要忘记在方法 “__exit__()” 或者是 finally block 中释放资源


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
