*Python Standard Library based on Python 3.7.3 [https://docs.python.org/3/library/](https://links.jianshu.com/go?to=https%3A%2F%2Fdocs.python.org%2F3%2Flibrary%2F)*
>Python标准库 - 内置异常
>Link: [[https://docs.python.org/3/library/exceptions.html#built-in-exceptions](https://docs.python.org/3/library/exceptions.html#built-in-exceptions)
>说明 print 后的 # 注释为输出和相关说明，包含所有 Python3.7.3 官方文档中的内置异常
>部分异常未代码实现测试
>异常基本操作 可参考 http://www.runoob.com/python3/python3-errors-execptions.html
>GitHub Code : [Built-in Exceptions.py](https://github.com/lichangke/Python-Standard-Library-Learn/blob/master/Built-in%20Exceptions/Built-in%20Exceptions.py)

目录链接：https://www.jianshu.com/p/e1e201bea601

# Exception hierarchy
# 异常层次结构
![异常层次结构.png](https://upload-images.jianshu.io/upload_images/16846478-6c28425d66843ce3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Exception hierarchy End


# 基类
以下异常主要用作其他异常的基类。
### exception BaseException
```
# exception BaseException
'''
所有内置异常的基类,但并不意味这能被用户自定义类直接继承
'''
# exception BaseException End
```

### exception Exception
```
# exception Exception
'''
所有内置的，非系统退出的异常都派生自此类。所有用户定义的异常也应该从该类派生。
'''
# exception Exception End
```
### exception ArithmeticError
```
# exception ArithmeticError
'''
针对各种算术错误引发的内置异常的基类：OverflowError，ZeroDivisionError，FloatingPointError。
'''
# exception ArithmeticError End
```
### exception BufferError
```
# exception BufferError
'''
当不能进行缓冲相关操作引发。
'''
# exception BufferError End
```
### exception LookupError
```
# exception LookupError
'''
当映射或序列上使用的键或索引无效时引发的异常的基类：IndexError，KeyError。这可以通过codecs.lookup（）直接引发。
'''
# exception LookupError End
```
基类 End


# 具体异常
以下异常是常常被触发的异常
### exception AssertionError
```python
'''
@Description: 
    当assert语句失败时触发。
@Param: 
@Return: 
'''
# exception AssertionError
try:
    assert( -1 > 0 )
except AssertionError:
    print("exception AssertionError Test")  # exception AssertionError Test
# exception AssertionError End
```

### exception AttributeError
```python
'''
@Description: 
    当属性引用(参见 https://docs.python.org/3/reference/expressions.html#attribute-references )或赋值失败时引发。
    （当一个对象根本不支持属性引用或属性赋值时，会引发TypeError。）
@Param: 
@Return: 
'''
# exception AttributeError
class A:
    def __init__(self,x):
        self.x = x
a = A(5)
print(a.x)  # 5
try:
    print(a.y)
except AttributeError:
    print("exception AttributeError Test")  # exception AttributeError Test  
# exception AttributeError End
```
### exception EOFError
```python
'''
@Description: 
    当input（）函数在没有读取任何数据的情况下达到文件结束条件（EOF）时引发。
@Param: 
@Return: 
'''
# exception EOFError
try:
    s = input('ctrl+d to raise EOFError:\r\n')
except EOFError:
    print("exception EOFError Test")    # exception EOFError Test
# exception EOFError End
```

### exception FloatingPointError
```python
'''
@Description: 
    目前尚未使用。
@Param: 
@Return: 
'''
# exception FloatingPointError
# exception FloatingPointError End
```

### exception GeneratorExit
```python
'''
@Description: 
    当发generator或coroutine关闭时抬起;请参阅generator.close（）和coroutine.close（）。
    它直接继承自BaseException而不是Exception，因为它在技术上不是错误。
@Param: 
@Return: 
'''
# exception GeneratorExit
# 参看 https://stackoverflow.com/questions/30862196/generatorexit-in-python-generator
def myGenerator(n):  
    while n > 0:
        try:
            yield n
        except GeneratorExit:
            print("exception GeneratorExit Test")
        n -= 1
gen = myGenerator(1)
print(next(gen))    # exception GeneratorExit Test   
del gen
# exception GeneratorExit End
```
### exception ImportError
```python
'''
@Description: 
    导入语句尝试加载模块遇到问题时触发，或者 当 “from list” 在 from ... import中有个名称没有时触发
@Param: 
@Return: 
'''
# exception ImportError
try:
    from sys import nofunc
except ImportError:
    print("exception ImportError Test") # exception ImportError Test
# exception ImportError End
```

### exception ModuleNotFoundError
```python
'''
@Description: 
    ImportError的子类,当无法找到模块时由import触发，或者在sys.modules没找到时也会触发。
@Param: 
@Return: 
'''
# exception ModuleNotFoundError
try:
    import Module
except ModuleNotFoundError:
    print("exception ModuleNotFoundError Test") # exception ModuleNotFoundError Test

try:
    from sys.modules import nofunc
except ModuleNotFoundError:
    print("exception ModuleNotFoundError Test") # exception ModuleNotFoundError Test
# exception ModuleNotFoundError End
```

### exception IndexError
```python
'''
@Description: 
    当序列下标超出范围时触发，切片索引被默默截断以落在允许的范围内;如果索引不是整数，则会引发TypeError。
@Param: 
@Return: 
'''
# exception IndexError
listtmp = [0,1,2,3,4]
try:
    listtmp[6]
except IndexError:
    print("exception IndexError Test")  # exception IndexError Test

try:
    listtmp[0.1]
except TypeError:
    print("exception TypeError Test")   # exception TypeError Test
# exception IndexError End
```
### exception KeyError
```python
'''
@Description: 
    当一个map或者dict的key没有在key的集合中时触发
@Param: 
@Return: 
'''
# exception KeyError
dicttmp = {'a':1,'b':2}
try:
    print(dicttmp['a']) # 1
    print(dicttmp['c'])
except KeyError:
    print("exception KeyError Test")    # exception KeyError Test
# exception KeyError End
```

### exception KeyboardInterrupt
```python
'''
@Description: 
    当用户点击中断键（通常是Control-C或Delete）时触发，在执行期间，定期检查中断。
    该异常继承自BaseException，以免被捕获Exception的代码意外捕获，从而阻止解释器退出。
@Param: 
@Return: 
'''
# exception KeyboardInterrupt
try:
    s = input('ctrl+c to raise KeyboardInterrupt:\r\n')
except KeyboardInterrupt:
    print("exception KeyboardInterrupt Test")    # exception KeyboardInterrupt Test
# exception KeyboardInterrupt End
```

### exception MemoryError
```python
'''
@Description: 
    当一个操作耗尽内存但是情况仍然可能被挽救（通过删除一些对象）时触发，关联值是一个字符串，表示内存中耗尽了哪种（内部）操作。
    由于底层的内存管理架构（C的malloc（）函数），解释器可能无法始终从这种情况中完全恢复; 然而，它会引发异常，以便可以打印堆栈回溯，以防出现失控程序。
@Param: 
@Return: 
'''
# exception MemoryError
from sys import getsizeof
try:
    a = [0] * 1024 * 1024 * 1024
    print(getsizeof(a))
except MemoryError:
    print("exception MemoryError Test")     # exception MemoryError Test
# exception MemoryError End
```

### exception NameError
```python
'''
@Description: 
    当找不到局部或全局名称时触发，这仅适用于不合格的名称。关联的值是包含无法找到的名称的错误消息。
@Param: 
@Return: 
'''
# exception NameError
try:
    print(noname)
except NameError:
    print("exception NameError Test")   # exception NameError Test
# exception NameError End
```

### exception NotImplementedError
```python
'''
@Description: 
    此异常派生自RuntimeError。
    在用户定义的基类中，抽象方法在需要派生类重写方法时触发
@Param: 
@Return: 
'''
# exception NotImplementedError

class ClassDemo(object):
    def func(self):
        raise NotImplementedError

class ChildClassDemo(ClassDemo):
    def funcChild(self):
        print('def funcChild(self):')

try:
    demo = ChildClassDemo()
    demo.funcChild()
    demo.func()
except NotImplementedError:
    print("exception NotImplementedError Test") # exception NotImplementedError Test
# exception NotImplementedError End
```



### OSError([arg])
```python
'''
@Description: 
    exception OSError([arg])
    exception OSError(errno, strerror[, filename[, winerror[, filename2]]])   
    当系统函数返回与系统相关的错误时会引发此异常，包括I / O失败，例如“找不到文件”或“磁盘已满”（不是非法参数类型或其他偶然错误）。
    构造函数的第二种形式,属性默认为None
@Param: 
    errno -- 来自C变量errno的数字错误代码。
    winerror -- 在Windows下，这将为您提供本机Windows错误代码。在Windows下，如果winerror构造函数参数是整数，则根据Windows错误代码确定errno属性，并忽略errno参数。
    strerror -- 操作系统提供的相应错误消息。
    filename
    filename2
    对于涉及文件系统路径的异常（例如open（）或os.unlink（）），filename是传递给函数的文件名。 
    对于涉及两个文件系统路径（例如os.rename（））的函数，filename2对应于传递给函数的第二个文件名。
@Return: 
'''
# OSError([arg])
filename = 'nofile'
try:
    f = open(filename)
except OSError as e:
    print("exception OSError Test")     # exception OSError Test
    print("e.errno = {}".format(e.errno))   # e.errno = 2
    # print("e.winerror = {}".format(e.winerror)) #报错 因为不在windows下 object has no attribute 'winerror'
    print("e.strerror = {}".format(e.strerror)) # e.strerror = No such file or directory
    print("e.filename = {}".format(e.filename)) # e.filename = nofile
# OSError([arg]) End
```

### exception OverflowError
```python
'''
@Description: 
    当算术运算的结果太大而无法表示时触发，整数不会发生这种情况（更倾向于触发 MemoryError ）
    但是，由于历史原因，有时会在超出所需范围的整数时引发OverflowError。由于C中缺少浮点异常处理的标准化，因此不检查大多数浮点运算。
@Param: 
@Return: 
'''
# exception OverflowError
try:
    def pi(): 
        pi = 0 
        for k in range(350): 
            pi += (4./(8.*k+1.) - 2./(8.*k+4.) - 1./(8.*k+5.) - 1./(8.*k+6.)) / 16.**k 
        return pi 
    print(pi())
except OverflowError:
    print("exception OverflowError Test")  # exception OverflowError Test
# 例子 来自 https://stackoverflow.com/questions/20201706/overflowerror-34-result-too-large
# exception OverflowError End
```

### exception RecursionError
```python
'''
@Description: 
    此异常派生自RuntimeError。当解释器检测到超出最大递归深度触发
@Param: 
@Return: 
'''
# exception RecursionError
try:
    def recursion(n):
        if (n <= 1):
            return
        recursion(n - 1)
    recursion(998)  # RecursionError: maximum recursion depth exceeded in comparison
except RecursionError: # ?? 不知道为啥 捕获不到
    print("exception RecursionError Test")
# exception RecursionError End
```

### exception ReferenceError
```python
'''
@Description: 
    当weakref.proxy（）函数创建的弱引用代理用于在垃圾回收后访问引用的属性时触发
    
@Param: 
@Return: 
'''
# exception ReferenceError
import weakref
class Foo(object):
    pass
foo = Foo()
proxy = weakref.proxy(foo)
print(proxy)    # <__main__.Foo object at 0x7f3d469af128>
del foo
try:
    print(proxy)   
except ReferenceError:
    print("exception ReferenceError Test")  # exception ReferenceError Test
# exception ReferenceError End
```

### exception RuntimeError
```python
'''
@Description: 
    检测到的错误不属于任何其他类别触发。
@Param: 
@Return: 
'''
# exception RuntimeError
def checkthenum(num):
    if num > 0:
        pass
    elif num < 0:
        pass
    else:
        raise RuntimeError
try:
    checkthenum(0)
except RuntimeError as e:
    print("exception RuntimeError Test")    # exception RuntimeError Test
# exception RuntimeError End
```

### exception StopIteration
```python
'''
@Description: 
    由内置函数next（）和迭代器的__next __（）方法触发，表示迭代器没有其他项。
@Param: 
@Return: 
'''
# exception StopIteration
def funcStopIteration(n):
    while n > 0:
        yield n
        n -= 1
list1 = funcStopIteration(2) # <generator object funcStopIteration at 0x7fcc5e1d7c78>
print(list1)
try:
    next(list1) 
    next(list1) 
    next(list1) 
except StopIteration:
    print("exception StopIteration Test")   # exception StopIteration Test
# exception StopIteration End
```

### exception StopAsyncIteration
```python
'''
@Description: 
    必须由异步迭代器对象的__anext __（）方法引发以停止迭代。
@Param: 
@Return: 
'''
# exception StopAsyncIteration
class TestImplementation:
    def __aiter__(self):
        return self
    async def __anext__(self):
        raise StopAsyncIteration    

async def test_async_for(): 
    async for _ in TestImplementation():    # exception StopAsyncIteration Test
        pass

# exception StopAsyncIteration End
```

### exception SyntaxError
```python
'''
@Description: 
    解析器遇到语法错误触发
    这可能发生在import语句中，调用内置函数exec（）或eval（），或者在读取初始脚本或标准输入时触发。
@Param: 
@Return: 
'''
# exception SyntaxError
strtmp = "for i in range(0,10) print(i)" 
try:
    exec(strtmp)
except SyntaxError:
    print("exception SyntaxError Test") # exception SyntaxError Test
# exception SyntaxError End
```

### exception IndentationError
```python
'''
@Description: 
    与不正确的缩进相关的语法错误的基类。这是SyntaxError的子类。
@Param: 
@Return: 
'''
# exception IndentationError
# exception IndentationError End
```
### exception TabError
```python
'''
@Description: 
    缩进包含tabs和空格,Tab 和空格混用。这是IndentationError的子类。
@Param: 
@Return: 
'''
# exception TabError

# exception TabError End
```

### exception SystemError
```python
'''
@Description: 
    一般的解释器系统错误
@Param: 
@Return: 
'''
# exception SystemError

# exception SystemError End
```

### exception SystemExit
```python
'''
@Description: 
    解释器请求退出,sys.exit（）函数引发此异常。
@Param: 
@Return: 
'''
# exception SystemExit
import sys
try:
    sys.exit()
except SystemExit:
    print("exception SystemExit Test")  # exception SystemExit Test
# exception SystemExit End
```

### exception TypeError
```python
'''
@Description: 
    将不适当类型的对象用于操作或函数时触发
    关联值是一个字符串，提供有关类型不匹配的详细信息。
@Param: 
@Return: 
'''
# exception TypeError

try:
    num = 1
    next(num)
except TypeError as e:
    print("exception TypeError Test")   # exception TypeError Test
# exception TypeError End
```


### exception UnboundLocalError
```python
'''
@Description: 
    当引用函数或方法中的局部变量，但变量未初始化时触发。这是NameError的子类。
@Param: 
@Return: 
'''
# exception UnboundLocalError
def funcUnboundLocalError():
    test
    test += 1

try:
    funcUnboundLocalError()
except UnboundLocalError:
    print("exception UnboundLocalError Test")   # exception UnboundLocalError Test
# exception UnboundLocalError End
```

### exception UnicodeError
```python
'''
@Description: 
    发生与Unicode相关的 编码或解码 错误时触发。它是ValueError的子类。
    UnicodeError具有描述编码或解码错误的属性。例如，err.object [err.start：err.end]给出编解码器失败的特定无效输入。
@Param: 
    encoding - 引发错误的编码的名称
    reason  - 描述特定编解码器错误的字符串
    object - 编解码器试图编码或解码的对象。
    start - 对象中无效数据的第一个索引。
    end - 对象中最后一个无效数据后的索引。
@Return: 
'''
# exception UnicodeError
'''
exception UnicodeEncodeError: 在编码期间发生与Unicode相关的错误时引发。它是UnicodeError的子类。
exception UnicodeDecodeError: 在解码期间发生与Unicode相关的错误时引发。它是UnicodeError的子类。
exception UnicodeTranslateError: 在翻译期间发生与Unicode相关的错误时引发。它是UnicodeError的子类。
'''
# exception UnicodeError End
```

### exception ValueError
```python
'''
@Description: 
    当操作或函数接收到具有正确类型但值不合适的参数的情况时引发
@Param: 
@Return: 
'''
# exception ValueError
try:
    int("hello world")
except ValueError:
    print("exception ValueError Test")  # exception ValueError Test
# exception ValueError End
```

### exception ZeroDivisionError
```python
'''
@Description: 
    当除法或模运算的第二个参数为零时触发
@Param: 
@Return: 
'''
# exception ZeroDivisionError

try:
    1/0
except ZeroDivisionError:
    print("exception ZeroDivisionError Test")   # exception ZeroDivisionError Test
# exception ZeroDivisionError End
```



```python
'''
@Description: 
    保留以下异常以与先前版本兼容;从Python 3.3开始，它们是OSError的别名。
@Param: 
@Return: 
'''
# 保留以下异常以与先前版本兼容;从Python 3.3开始，它们是OSError的别名。
'''
exception EnvironmentError
exception IOError
exception WindowsError
'''
# 保留以下异常以与先前版本兼容;从Python 3.3开始，它们是OSError的别名。 End
```
具体异常 End



# OS exceptions
以下异常是OSError的子类，它们会根据系统错误代码而产生。
### exception BlockingIOError
```python
'''
@Description: 
    当一个操作阻塞一个设置为非阻塞操作的对象（例如套接字）时触发
@Param: 
@Return: 
'''
# exception BlockingIOError
import socket
TCP_IP = '192.168.1.10'
TCP_PORT = 7
class TcpClient:
    def __init__(self):
        self.s = socket.socket(socket.AF_INET,  socket.SOCK_STREAM)
        self.s.settimeout(5)

    def connect(self):
        self.s.connect((TCP_IP,  TCP_PORT))

client = TcpClient()
while True:
    try:
        client.connect()
    except socket.timeout:
        print("socket.timeout") # socket.timeout
    except BlockingIOError:
        print("exception BlockingIOError Test") # exception BlockingIOError Test
        break
           
# exception BlockingIOError End
```

### exception ChildProcessError
```python
'''
@Description: 
    在对子进程执行操作失败时触发。对应于errno ECHILD。
@Param: 
@Return: 
'''
# exception ChildProcessError
# exception ChildProcessError End
```
### exception ConnectionError
```python
'''
@Description:
    与连接相关问题的基类
    子类是BrokenPipeError，ConnectionAbortedError，ConnectionRefusedError和ConnectionResetError。
@Param: 
@Return: 
'''
# exception ConnectionError
'''
exception BrokenPipeError: ConnectionError的子类,试图在管道另一端关闭的情况下进行写入，或者试图写一个已经关闭写入的套接字触发 ，对应于errno EPIPE和ESHUTDOWN。
exception ConnectionAbortedError: ConnectionError的子类,当连接尝试被同伴中止时触发，对应于errno ECONNABORTED。
exception ConnectionRefusedError: ConnectionError的子类,当连接尝试被同伴拒绝时触发，对应于errno ECONNREFUSED。
exception ConnectionResetError: ConnectionError的子类,当连接被同伴重置时触发，对应于errno ECONNRESET。
'''
# exception ConnectionError End
```
### exception FileExistsError
```python
'''
@Description: 
    尝试创建已存在的文件或目录时触发。对应于errno EEXIST。
@Param: 
@Return: 
'''
# exception FileExistsError
import os
try:
    os.mknod("test.txt")  # test.txt已存在
except FileExistsError:
    print("exception FileExistsError Test") # exception FileExistsError Test

# exception FileExistsError　 End
```
### exception FileNotFoundError
```python
'''
@Description:
    在请求的文件或目录不存在时触发。对应于errno ENOENT。
@Param: 
@Return: 
'''
# exception FileNotFoundError
import os
print(os.stat("./test.txt")) # test.txt已存在 获取文件属性：os.stat（file）
# os.stat_result(st_mode=33152, st_ino=131091, st_dev=2049, st_nlink=1, st_uid=1000, st_gid=1000, st_size=0, st_atime=1556116677, st_mtime=1556116677, st_ctime=1556116677)
try:
    os.stat("nofile.txt")
except FileNotFoundError:
    print("exception FileNotFoundError Test")   # exception FileNotFoundError Test
# exception FileNotFoundError End
```

### exception InterruptedError
```python
'''
@Description: 
    当系统调用被传入信号中断时触发。对应于errno EINTR。
@Param: 
@Return: 
'''
# exception InterruptedError
# exception InterruptedError End
```

### exception IsADirectoryError
```python
'''
@Description: 
    在目录上请求文件操作（例如os.remove（））时引发。对应于errno EISDIR。
@Param: 
@Return: 
'''
# exception IsADirectoryError
import os
try:
    os.remove("./test") # 函数用来删除一个文件:os.remove()
    # 如过"./test" 不存在时会抛异常FileNotFoundError: [Errno 2] No such file or directory: './test'
except IsADirectoryError:
    print("exception IsADirectoryError Test")   # exception IsADirectoryError Test
# exception IsADirectoryError End
```

### exception NotADirectoryError
```python
'''
@Description: 
    在对非目录的事物请求目录操作（例如os.listdir（））时引发。对应于errno ENOTDIR。
@Param: 
@Return: 
'''
# exception NotADirectoryError
import os
try:
    os.listdir("./test.txt")  # test.txt已存在
except NotADirectoryError:
    print("exception NotADirectoryError Test")  # exception NotADirectoryError Test
# exception NotADirectoryError End
```

### exception PermissionError
```python
'''
@Description: 
    尝试在没有足够访问权限的情况下执行操作时触发 - 例如文件系统权限。对应于errno EACCES和EPERM。
@Param: 
@Return: 
'''
# exception PermissionError
try:
    fp = open("./test.txt",mode='w')     # 直接打开一个文件   手动设置 test.txt 为 只读 
    fp.write("leacoder")  
    fp.close()
except PermissionError:
    print("exception PermissionError Test") # exception PermissionError Test
   
# exception PermissionError End
```
### exception ProcessLookupError
```python
'''
@Description: 
    当给定进程不存在时触发。对应于errno ESRCH。
@Param: 
@Return: 
'''
# exception ProcessLookupError
import os
import signal
try:
    os.kill(123456,signal.SIGKILL) # 注意 123456 不是已存在的进程号
except ProcessLookupError:
    print("exception ProcessLookupError Test") # exception ProcessLookupError Test

# exception ProcessLookupError End
```

### exception TimeoutError
```python
'''
@Description: 
    当系统功能在系统级别超时时触发
@Param: 
@Return: 
'''
# exception TimeoutError
# exception TimeoutError End
```

OS exceptions End



# Warnings
以下异常用作警告类别;有关详细信息 https://docs.python.org/3/library/warnings.html#warning-categories
### exception Warning
```python
'''
@Description: 
    Base class for warning categories.
@Param: 
@Return: 
'''
# exception Warning
'''
Base class for warning categories.
'''
# exception Warning End
```

### exception UserWarning
```python
'''
@Description: 
    用户代码生成警告的基类
@Param: 
@Return: 
'''
# exception UserWarning
'''
用户代码生成警告的基类
'''
# exception UserWarning End
```

### exception DeprecationWarning
```python
'''
@Description: 
    弃用特性警告基类
@Param: 
@Return: 
'''
# exception DeprecationWarning
'''
弃用特性警告基类
'''
# exception DeprecationWarning End
```

### exception PendingDeprecationWarning
```python
'''
@Description: 
    将来会被弃用特性的警告基类
@Param: 
@Return: 
'''
# exception PendingDeprecationWarning
'''
将来会被弃用特性的警告基类
'''
# exception PendingDeprecationWarning End
```
### exception SyntaxWarning
```python
'''
@Description: 
    可疑句法警告基类
@Param: 
@Return: 
'''
# exception SyntaxWarning
'''
可疑句法警告基类
'''
# exception SyntaxWarning End
```

### exception RuntimeWarning
```python
'''
@Description: 
    可疑 Runtime 行为警告基类
@Param: 
@Return: 
'''
# exception RuntimeWarning
'''
可疑 Runtime 行为警告基类
'''
# exception RuntimeWarning End
```

### exception FutureWarning
```python
'''
@Description: 
    将来会改变语义结构的警告基类
@Param: 
@Return: 
'''
# exception FutureWarning
'''
将来会改变语义结构的警告基类
'''
# exception FutureWarning End
```

### exception ImportWarning
```python
'''
@Description: 
    可能弄错模块导入警告基类
@Param: 
@Return: 
'''
# exception ImportWarning
'''
可能弄错模块导入警告基类
'''
# exception ImportWarning End
```

### exception UnicodeWarning
```python
'''
@Description: 
    Unicode 相关的警告基类
@Param: 
@Return: 
'''
# exception UnicodeWarning
'''
Unicode 相关的警告基类
'''
# exception UnicodeWarning End
```

### exception BytesWarning
```python
'''
@Description: 
    与bytes和bytearray相关的警告的基类
@Param: 
@Return: 
'''
# exception BytesWarning
'''
与bytes和bytearray相关的警告的基类
'''
# exception BytesWarning End
```

### exception ResourceWarning
```python
'''
@Description: 
    与资源使用相关的警告的基类。被默认警告过滤器忽略。
@Param: 
@Return: 
'''
# exception ResourceWarning
'''
与资源使用相关的警告的基类。被默认警告过滤器忽略。
'''
# exception ResourceWarning End
```
Warnings End


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
