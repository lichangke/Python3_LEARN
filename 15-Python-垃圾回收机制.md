目录链接：https://www.jianshu.com/p/e1e201bea601


## 计数引用

Python 中一切皆对象，因此你所看到的一切变量， 本质上都是对象的一个指针。

当一个对象的引用计数（指针数） 为 0 的时候， 说明这个对象永不可达， 它也就成为了垃圾需要被回收。

例子：
```python
import sys
a = []
# 两次引用，一次来自 a，一次来自 getrefcount
print(sys.getrefcount(a))
def func(intmp):
    # 四次引用，a，python 的函数调用栈，函数参数，和 getrefcount
    print(sys.getrefcount(intmp))
func(a)
# 两次引用，一次来自 a，一次来自 getrefcount，函数 func 调用已经不存在
print(sys.getrefcount(a))
```
![计数引用01.png](https://upload-images.jianshu.io/upload_images/16846478-eed477c80b3ac37c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

sys.getrefcount() 这个函数， 可以查看一个变量的引用次数，getrefcount 本身也会引用一次计数。

在函数调用发生的时候， 会产生额外的两次引用，一次来自函数栈， 另一个是函数参数。
例子：
```python
import sys
a = []
print(sys.getrefcount(a))  # 两次
b = a
print(sys.getrefcount(a))  # 三次
c = b
d = b
e = c
f = e
g = d
print(sys.getrefcount(a))  # 八次
```
![计数引用02.png](https://upload-images.jianshu.io/upload_images/16846478-2984a10970925bac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

a、 b、 c、 d、 e、 f、 g 这些变量全部指代的是同一个对象，最后一共会有8次引用。

python里每一个东西都是对象，它们的核心就是一个结构体：PyObject
```c
 typedef struct_object {
 int ob_refcnt;
 struct_typeobject *ob_type;
} PyObject;
```
PyObject是每个对象必有的内容，其中ob_refcnt就是做为引用计数。当一个对象有新的引用时，它的ob_refcnt就会增加，当引用它的对象被删除，它的ob_refcnt就会减少
```c
#define Py_INCREF(op)   ((op)->ob_refcnt++) //增加计数
#define Py_DECREF(op) \ //减少计数
    if (--(op)->ob_refcnt != 0) \
        ; \
    else \
        __Py_Dealloc((PyObject *)(op))
```
当引用计数为0时，该对象生命就结束了。

引用计数机制的优点：
1、简单
2、实时性：一旦没有引用，内存就直接释放了。不用像其他机制等到特定时机。实时性还带来一个好处：处理回收内存的时间分摊到了平时。

引用计数机制的缺点：

1、维护引用计数消耗资源
2、循环引用

引用计数机制无法处理循环引用的情况，如下代码
```python
list1 = []
list2 = []
list1.append(list2)
list2.append(list1)
```
list1与list2相互引用，如果不存在其他对象对它们的引用，list1与list2的引用计数也仍然为1，所占用的内存永远无法被回收，这将是致命的。注定python还将引入新的回收机制。(标记清除（mark-sweep） 和分代收集（generational） )

![循环引用.png](https://upload-images.jianshu.io/upload_images/16846478-a30f7dcc2e2c456c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


如果想手动释放内存，可以先调用del a 来删除一个对象；然后强制调用 gc.collect()， 即可手动启动垃圾回收。

## 标记-清除算法

对于一个有向图， 如果从一个节点出发进行遍历， 并标记其经过的所有节点；那么， 在遍历结束后， 所有没有被标记的节点， 我们就称之为不可达节点。 显而易见， 这些节点的存在是没有任何意义的，我们就需要对它们进行垃圾回收。当然， 每次都遍历全图， 对于 Python 而言是一种巨大的性能浪费。 所以， 在 Python 的垃圾回收实现中， mark-sweep 使用双向链表维护了一个数据结构， 并且只考虑容器类的对象（只有容器类对象才有可能产生循环引用） 。
 
现在有两种情况：
A:

```python
a=[1,3]
b=[2,4]
a.append(b)
b.append(a)
del a
del b
```

B:

```python
a=[1,3]
b=[2,4]
a.append(b)
b.append(a)
del a
```

在标记-清除算法中，有两个集中营，一个是root链表(root object)，另外一个是unreachable链表。

- 对于情景A，原来再未执行DEL语句的时候，a,b的引用计数都为2（init+append=2），但是在DEL执行完以后，a,b引用次数互相减1。a,b陷入循环引用的圈子中，然后**标记-清除算法**开始出来做事，找到其中一端a,开始拆这个a,b的引用环（我们从A出发，因为它有一个对B的引用，则将B的引用计数减1；然后顺着引用达到B，因为B有一个对A的引用，同样将A的引用减1，这样，就完成了循环引用对象间环摘除。），去掉以后发现，a,b循环引用变为了0，所以a,b就被处理到unreachable链表中直接被做掉。

- 对于情景B,简单一看那b取环后引用计数还为1，但是a取环就为0了（其实不是的）。这个时候a已经进入unreachable链表中，但是这个时候，root链表中有b。如果a被做掉，在root链表中的b会被进行引用检测引用了a，如果a被做掉了，那么b就也就要被做掉这时不对，一审完事，二审a无罪，所以a又被拉回到了root链表中。

QA： 为什么要搞这两个链表

之所以要剖成两个链表，是基于这样的一种考虑：**现在的unreachable可能存在被root链表中的对象直接或间接引用的对象，这些对象是不能被回收的，一旦在标记的过程中，发现这样的对象，就将其从unreachable链表中移到root链表中；当完成标记后，unreachable链表中剩下的所有对象就是名副其实的垃圾对象了，接下来的垃圾回收只需限制在unreachable链表中即可。**


## 分代收集算法

Python 将所有对象分为三代。 刚刚创立的对象是第 0 代；经过一次垃圾回收后， 依然存在的对象， 便会依次从上一代挪到下一代。 而每一代启动自动垃圾回收的阈值， 则是可以单独指定的。当垃圾回收器中新增对象减去删除对象达到相应的阈值时， 就会对这一代对象启动垃圾回收。分代收集基于的思想是， 新⽣的对象更有可能被垃圾回收， 而存活更久的对象也有更高的概率继续存活。 

## 调试内存泄漏

虽然有了自动回收机制， 但这也不是万能的， 难免还是会有漏网之鱼。 内存泄漏是我们不想见到的， ⽽且还会严重影响性能。 objgraph， 一个非常好用的可视化引用关系的包。 

第一个有用的函数是 show_refs()， 它可以生成清晰的引用关系图。

通过下面这段代码和生成的引用调用图， 能非常直观地发现有两个 list 互相引用， 说明这里极有可能引起内存泄露。 这样， 再去代码层排查就容易多了。
```python
import objgraph
a = [1, 2, 3]
b = [4, 5, 6]
a.append(b)
b.append(a)
objgraph.show_refs([a])
```
![ show_refs().png](https://upload-images.jianshu.io/upload_images/16846478-354c474d42b00661.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要安装第三方库：pip install objgraph
并使用[https://graphviz.gitlab.io/_pages/Download/Download_windows.html](https://graphviz.gitlab.io/_pages/Download/Download_windows.html)连接中工具graphviz，并用gvedit.exe查看.dot文件

![调试内存泄漏.png](https://upload-images.jianshu.io/upload_images/16846478-3b66636cbace04e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



第二个非常有用的函数是 show_backrefs()

```python
import objgraph
a = [1, 2, 3]
b = [4, 5, 6]
a.append(b)
b.append(a)
objgraph.show_backrefs([a])
```
![ show_backrefs().png](https://upload-images.jianshu.io/upload_images/16846478-212b454c06a91164.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![调试内存泄漏.png](https://upload-images.jianshu.io/upload_images/16846478-340b50d8c9331739.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个 API有很多有用的参数， 比如层数限制（max_depth） 、 宽度限制（too_many） 、 输出格式控制
（filename output） 、 节点过滤（filter, extra_ignore） 等。 可参看：[https://mg.pov.lt/objgraph/](https://mg.pov.lt/objgraph/)


## 参考资料：

*极客时间 Python核心技术与实战学习*

Python核心技术与实战（极客时间）链接：
http://gk.link/a/103Sv

Python垃圾回收（GC）三层心法,你了解到第几层？：
[https://juejin.im/post/5b34b117f265da59a50b2fbe](https://juejin.im/post/5b34b117f265da59a50b2fbe)


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
