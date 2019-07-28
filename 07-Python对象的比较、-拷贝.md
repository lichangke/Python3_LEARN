目录链接：https://www.jianshu.com/p/e1e201bea601

## '==' VS 'is'

等于（==） 和 is 是 Python 中对象比较常用的两种方式。

'==' 操作符比较对象之间的值是否相等：

```python
a == b  # 表示比较变量 a 和 b 所指向的值是否相等。
```
 'is' 操作符比较的是对象的身份标识是否相等， 即它们是否是同一个对象， 是否指向同一个内存地址。

在 Python 中， 每个对象的身份标识， 都能通过函数 id(object) 获得。 因此， 'is' 操作符， 相当于比较对象之间的 ID 是否相等

```python
a1 = 10
b1 = 10
print('a1 == b1:',a1 == b1)
print('id(a1):',id(a1))
print('id(b1):',id(b1))
print('a1 is b1:',a1 is b1)
a2 = 257
b2 = 257
print('a2 == b2:',a2 == b2)
print('id(a2):',id(a2))
print('id(b2):',id(b2))
print('a2 is b2:',a2 is b2)
```
!['==' VS 'is'.png](https://upload-images.jianshu.io/upload_images/16846478-fc8c253a49ae61ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对于整型数字来说， 以上 a1 is b1 为 True 的结论， 只适用于 -5 到 256 范围内的数字。 

事实上， 出于对性能优化的考虑， Python [CPython（Python 的 C 实现）]内部会对 -5 到 256 的整型维持一个数组， 起到一个缓存的作用。 这样， 每次你试图创建一个 -5 到 256 范围内的整型数字时， Python 都会从这个数组中返回相对应的引用， 而不是重新开辟一块新的内存空间，这样a1 is b1: True。如果整型数字超过了这个范围， 比如上述例子中的 257， Python 则会为两个 257 开辟两块内存区域， 因此 a 和 b 的 ID 不一样，这样a2 is b2: False。

'is' 的速度效率 通常要优于 '==' ，因为 'is' 操作符不能被重载， Python 就不需要去寻找， 程序中是否有其他地方重载了比较操作符。执行比较操作符 'is' ， 就仅仅是比较两个变量的 ID 而已。 '==' 操作符却不同，执行 a == b 相当于是去执行 a.\_\_eq\_\_(b) ， Python大部分的数据类型都会去重载 \_\_eq\_\_ 这个函数。

```python
t1 = (1, 2, [3, 4])
t2 = (1, 2, [3, 4])
print('t1 == t2:',t1 == t2)

t1[-1].append(5)
print('t1 == t2:',t1 == t2)
```
!['=='.png](https://upload-images.jianshu.io/upload_images/16846478-ded4a1c411bc798a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们知道元组是不可变的， 但元组可以嵌套， 它里面的元素可以是列表类型， 列表是可变的， 所以如果我们修改了元组中的某个可变元素， 那么元组本身也就改变了。

## 浅拷贝和深度拷贝

浅拷贝（shallow copy） 和深度拷贝（deep copy）

### 浅拷贝 

浅拷贝指重新分配一块内存， 创建一个新的对象， 里面的元素是原对象中子对象的引用。
Python 中可以用 copy.copy() 来实现对象的浅拷贝，适用于任何数据类型。

如果原对象中的元素不可变， 那倒无问题；但如果元素可变， 浅拷贝通常会带来一些副作用：


```python
'''对 l1 执行浅拷贝， 赋予 l2。浅拷贝里的元素是对原对象元素的引用， 因此 l2 中的元素和 l1 指向同一个
列表和元组对象，但l1和l2本身是不同对象。'''
l1 = [[1, 2], (30, 40)]
l2 = list(l1)    

l1.append(100)    'l1和l2本身是不同对象， l1 的列表新增元素 100而l2不会'
'l1 中的第一个列表新增元素 3，因为 l2 是 l1 的浅拷贝，l2 中的第一个元素和 l1 中的第一个元素， 共同指向同一个列表，l2 中的第一个列表也会相对应的新增元素 3'
l1[0].append(3)   

print('l1:',l1)
print('l2:',l2)

 '元组是不可变的，这里对 l1 中的第二个元组拼接， 然后重新创建了一个新元组作为 l1 中的第二个元素，与l2的第二个元素不再是用一个对象的引用，操作后 l2 不变， l1 发生改变'
l1[1] += (50, 60)

print('l1:',l1)
print('l2:',l2)
```
![浅拷贝副作用.png](https://upload-images.jianshu.io/upload_images/16846478-5d49d02ff7f69439.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 深度拷贝

深度拷贝， 是指重新分配一块内存， 创建一个新的对象， 并且将原对象中的元素， 以递归的方式， 通过创建新的子对象拷贝到新对象中。 因此， 新对象和原对象没有任何关联。

Python 中以 copy.deepcopy() 来实现对象的深度拷贝。
```python
import copy
l1 = [[1, 2], (30, 40)]
l2 = copy.deepcopy(l1)

l1.append(100)
l1[0].append(3)

print('l1:',l1)
print('l2:',l2)
```
![深度拷贝.png](https://upload-images.jianshu.io/upload_images/16846478-d2bf2c79fbf9a4ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要注意的是，如果被拷贝对象中存在指向自身的引用， 那么程序很容易陷入无限循环：

```python
import copy
x = [1]
x.append(x)

print('x:',x)

y = copy.deepcopy(x)
print('y:',y)
```

![被拷贝对象中存在指向自身的引用.png](https://upload-images.jianshu.io/upload_images/16846478-8944b5b2d032e84e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**为什么深度拷贝 x 到 y 后， 程序并没有出现 stack overflow？**

深度拷贝函数 deepcopy 中会维护⼀个字典， 记录已经拷⻉的对象与其 ID。 拷贝过程中， 如果字典里已经存储了将要拷贝的对象， 则会从字典直接返回。

源码地址：[https://github.com/python/cpython/blob/3.7/Lib/copy.py](https://github.com/python/cpython/blob/3.7/Lib/copy.py)

```python
def deepcopy(x, memo=None, _nil=[]):
    """Deep copy operation on arbitrary Python objects.
    	
	See the module's __doc__ string for more info.
	"""
	
    if memo is None:
        memo = {}
    d = id(x) # 查询被拷贝对象 x 的 id
	y = memo.get(d, _nil) # 查询字典里是否已经存储了该对象
	if y is not _nil:
	    return y # 如果字典里已经存储了将要拷贝的对象，则直接返回
        ...    
```

如果使用 x == y 比较上述 y = copy.deepcopy(x) 的 y 与x

![RecursionError](https://upload-images.jianshu.io/upload_images/16846478-4bc4f5668b354921.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


x==y内部执行是会递归遍历列表x和y中每一个元素的值，由于x和y是无限嵌套的，因此会stack overflow报错

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
