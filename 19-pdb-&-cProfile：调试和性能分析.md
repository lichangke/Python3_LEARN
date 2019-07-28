目录链接：https://www.jianshu.com/p/e1e201bea601


## 用 pdb 进行代码调试

单步调试，通过在用户终端命令 python -m pdb xxx.py 启动脚本进入单步执行模式；或者在程序中， 加入 “import pdb” 和 “pdb.set_trace()” 这两行代码启动 pdb 调试。

### pdb命令行：
```
    1）进入命令行Debug模式，python -m pdb xxx.py  这个格式是固定的

          之所以可以这样做，主要是因为pdb.py 可以被当做一个脚本执行。

    2）h：（help）帮助

    3）w：（where）打印当前执行堆栈

    4）d：（down）执行跳转到在当前堆栈的深一层（个人没觉得有什么用处）

    5）u：（up）执行跳转到当前堆栈的上一层

    6）b：（break）添加断点

                 b 列出当前所有断点，和断点执行到统计次数

                 b line_number：当前脚本的line_no行添加断点

                 b filename:line_number：脚本filename的line_no行添加断点

                 b function：在函数function的第一条可执行语句处添加断点

    7）tbreak：（temporary break）临时断点

                 在第一次执行到这个断点之后，就自动删除这个断点，用法和b一样

    8）cl：（clear）清除断点

                cl 清除所有断点

                cl bpnumber1 bpnumber2... 清除断点号为bpnumber1,bpnumber2...的断点

                cl line_number 清除当前脚本line_number行的断点

                cl filename:line_number 清除脚本filename的line_number行的断点

    9）disable：停用断点，参数为bpnumber，和cl的区别是，断点依然存在，只是不启用

    10）enable：激活断点，参数为bpnumber(即哪一个断点，1，2，3，4......)

    11）s：（step）执行下一条命令

                如果本句是函数调用，则s会执行到函数的第一句

    12）n：（next）执行下一条语句

                如果本句是函数调用，则执行函数，接着执行当前执行语句的下一条。

    13）r：（return）执行当前运行函数到结束

    14）c：（continue）继续执行，直到遇到下一条断点，这个比较重要，常常和断点结合起来使用。

    15）l：（list）列出源码

                 l 列出当前执行语句周围11条代码

                 l first 列出first行周围11条代码

                 l first second 列出first--second范围的代码，如果second<first，second将被解析为行数

    16）a：（args）列出当前执行函数的函数

    17）p expression：（print）输出expression的值

           比如：(Pdb) p 1+2

                   这里会打印出 3

          再比如：(Pdb) p c    #这里用来查看某个变量c的值

    18）pp expression：好看一点的p expression

    19）run：重新启动debug，相当于restart

    20）q：（quit）退出debug

    21）j lineno：（jump）设置下条执行的语句函数

                只能在堆栈的最底层跳转，向后重新执行，向前可直接执行到行号

    22）unt：（until）执行到下一行（跳出循环），或者当前堆栈结束

    23）condition bpnumber conditon，给断点设置条件，当参数condition返回True的时候bpnumber断点有效，否则bpnumber断点无效
```
### 使用set_trace()设置断点

```python
import pdb
a = 1
b = 2
pdb.set_trace()
c = 3
print(a + b + c)
```
运行这个程序时，会在“pdb.set_trace()” 暂停，后面就可以通过上面的命令进行调试
![image.png](https://upload-images.jianshu.io/upload_images/16846478-43c6bb26bb7e7760.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## cProfile 进行性能分析
导入cProfile 这个模块， 并且在最后运行cProfile.run() ，或者python3 -m cProfile xxx.py

例子斐波拉契数列

```python
import cProfile
def fib1(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib1(n-1) + fib1(n-2)
def fib_seq1(n):
    res = []
    if n > 0:
        res.extend(fib_seq1(n-1))
    res.append(fib1(n))
    return res
cProfile.run('fib_seq1(30)')
```
![cProfile01.png](https://upload-images.jianshu.io/upload_images/16846478-1c470331929ea93d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ncalls， 是指相应代码 / 函数被调用的次数；
- tottime， 是指对应代码 / 函数总共执行所需要的时间（注意， 并不包括它调用的其他代码 /函数的执行时间） ；
- tottime percall， 就是上述两者相除的结果， 也就是 tottime / ncalls ；
- cumtime， 则是指对应代码 / 函数总共执行所需要的时间， 包括了它调用的其他代码 / 函数的执行时间；
- cumtime percall， 则是 cumtime 和 ncalls 相除的平均结果。

可以清晰看到函数 fib()， 被调用了 700 多万次。

优化：

```python
import cProfile
def fib1(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib1(n-1) + fib1(n-2)
def fib_seq1(n):
    res = []
    if n > 0:
        res.extend(fib_seq1(n-1))
    res.append(fib1(n))
    return res
cProfile.run('fib_seq1(30)')
def memoize(f):
    memo = {}
    def helper(x):
        if x not in memo:
            memo[x] = f(x)
        return memo[x]
    return helper
@memoize
def fib2(n):
    if n == 0:
        return 0
    elif n == 1:
        return 1
    else:
        return fib2(n-1) + fib2(n-2)
def fib_seq2(n):
    res = []
    if n > 0:
        res.extend(fib_seq2(n-1))
    res.append(fib2(n))
    return res
cProfile.run('fib_seq2(30)')
```
![cProfile02.png](https://upload-images.jianshu.io/upload_images/16846478-3e3ef236999e6344.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 参考资料：

*极客时间 Python核心技术与实战学习*

Python核心技术与实战（极客时间）链接：
http://gk.link/a/103Sv

pdb — The Python Debugger：
[https://docs.python.org/3/library/pdb.html#module-pdb](https://docs.python.org/3/library/pdb.html#module-pdb)

The Python Profilers:
[https://docs.python.org/3.7/library/profile.html](https://docs.python.org/3.7/library/profile.html)

python高级调试技巧（一）——原生态的pdb调试:
[https://blog.csdn.net/qq_27825451/article/details/85600992](https://blog.csdn.net/qq_27825451/article/details/85600992)

python模块-cProfile和line_profiler:
[https://blog.csdn.net/weixin_40304570/article/details/79459811](https://blog.csdn.net/weixin_40304570/article/details/79459811)


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
