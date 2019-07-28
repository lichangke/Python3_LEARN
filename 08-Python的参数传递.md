目录链接：https://www.jianshu.com/p/e1e201bea601

## 什么是值传递和引用传递

### 值传递

所谓值传递， 通常就是拷贝参数的值， 然后传递给函数里的新变量，原变量和新变量之间互相独立， 互不影响。

```c++
#include <iostream>
using namespace std;
 
// 交换 2 个变量的值
void swap(int x, int y) {
    int temp;
    temp = x; // 交换 x 和 y 的值
    x = y;
    y = temp;
    return;
}

int main () {
    int a = 1;
    int b = 2;
    cout << "Before swap, value of a :" << a << endl;
    cout << "Before swap, value of b :" << b << endl;
    swap(a, b); 
    cout << "After swap, value of a :" << a << endl;
    cout << "After swap, value of b :" << b << endl;  
    return 0;
}
```
![值传递.png](https://upload-images.jianshu.io/upload_images/16846478-9e785f09e3d0f5d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 swap() 函数， 把 a 和 b 的值拷贝给了 x 和 y， 然后再交换 x 和 y 的值。 这样， x 和 y的值发生了改变， 但是 a 和 b 不受其影响， 所以值不变。 这种方式， 就是我们所说的值传递。

### 引用传递

所谓引用传递， 通常是指把参数的引用传给新的变量， 这样， 原变量和新变量就会指向同一块内存地址。 如果改变了其中任何一个变量的值， 那么另外一个变量也会相应地随之改变。


```c++
#include <iostream>
using namespace std;
 
// 交换 2 个变量的值
void swap(int& x, int& y) {
   int temp;
   temp = x; // 交换 x 和 y 的值
   x = y;
   y = temp;
   return;
}


int main () {
    int a = 1;
    int b = 2;
    cout << "Before swap, value of a :" << a << endl;
    cout << "Before swap, value of b :" << b << endl;
    swap(a, b); 
    cout << "After swap, value of a :" << a << endl;
    cout << "After swap, value of b :" << b << endl;  
    return 0;
}
```
![引用传递.png](https://upload-images.jianshu.io/upload_images/16846478-4eead2eb91d298e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Python 变量及其赋值 

```python
a = 1
b = a
a = a + 1
```

首先将 1 赋值于 a， 即 a 指向了 1 这个对象：

![图片来自SQL必知必会专栏（极客时间）.png](https://upload-images.jianshu.io/upload_images/16846478-49a5d306aef9d33f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接着 b = a 则表示让变量 b 也同时指向 1 这个对象， Python 里的对象可以被多个变量所指向或引用。

![图片来自SQL必知必会专栏（极客时间）.png](https://upload-images.jianshu.io/upload_images/16846478-65760f8d7ee57d91.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行 a = a + 1，由于在Python中整型（int） 是不可变的。***所以， a = a + 1， 并不是让 a 的值增加 1，而是表示重新创建了一个新的值为 2 的对象， 并让 a 指向它。***

![图片来自SQL必知必会专栏（极客时间）.png](https://upload-images.jianshu.io/upload_images/16846478-927f7733164b9a11.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后的结果是， a 的值变成了 2，而 b 的值不变仍然是 1。

同时， 指向同一个对象， 也并不意味着两个变量就被绑定到了一起。 如果你给其中一个变量重新赋值， 并不会影响其他变量的值。

可以参看：[07 Python对象的比较、 拷贝](https://www.jianshu.com/p/eacff7a05f8b)

```python
l1 = [1, 2, 3]
l2 = l1
'由于列表是可变的，l1.append(4)  不会重新新建一个对象，而是直接在对象[1, 2, 3]改变，故l1 l2均改变'
l1.append(4)  
print('l1=',l1)
print('l2=',l2)
'而l1 = l1 + [-1] 会新建一个对象[1, 2, 3, 4, -1]并赋值与l1，而l2未变'
l1 = l1 + [-1]
print('l1=',l1)
print('l2=',l2)
```
![Python 变量及其赋值01.png](https://upload-images.jianshu.io/upload_images/16846478-5e8d77c4de072bd2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Python 中的变量可以被删除， 但是对象无法被删除。

```python
l1=[1,2,3]
del l1
```
del l1 删除了 l1这个变量， 从此以后你无法访问 l1， 但是对象 [1, 2, 3] 仍然存在。Python 程序运⾏时， 其垃圾回收系统会跟踪每个对象的引用。 如果 [1, 2, 3] 除了 l1 外， 还在其他地方被引用， 那就不会被回收， 反之则会被回收。

- 变量的赋值， 只是表示让变量指向了某个对象， 并不表示拷贝对象给变量；一个对象， 可以被多个变量所指向。

- 可变对象（列表， 字典， 集合等等） 的改变， 会影响所有指向该对象的变量。

- 对于不可变对象（字符串， 整型， 元祖等等） ， 所有指向该对象的变量的值总是一样的， 也不会改变。 但是通过某些操作（+= 等等） 更新不可变对象的值时， 会返回一个新的对象。

- 变量可以被删除， 但是对象无法被删除。

```python
l1 = [1, 2, 3]
l2 = [1, 2, 3]
l3 = l2
print('id(l1)={0},id(l2)={1},id(l3)={2}'.format(id(l1),id(l2),id(l3)))
print('l1={0},l2={1},l3={2}'.format(l1,l2,l3))
```

![Python 变量及其赋值02.png](https://upload-images.jianshu.io/upload_images/16846478-13bdc2f29a048e2d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

虽然l1,l2,l3的值相同，但是通过id()函数可知，l2和l3是指向同一个对象因为两者之间用等号赋值了，l1并不是，l1所指向的[1, 2, 3]是另外一块内存空间

## Python 函数的参数传递

Python 的参数传递是赋值传递 （pass by assignment） ， 或者叫作对象的引用传递（pass by object reference） 。 Python 里所有的数据类型都是对象， 所以参数传递时， 只是让新变量与原变量指向相同的对象而已， 并不存在值传递或是引用传递一说。赋值或对象的引用传递， 不是指向一个具体的内存地址，而是指向一个具体的对象。

```python
def my_func1(b):
    b = 2

a = 1
my_func1(a)
print('a=',a)
```
![Python 函数的参数传递01.png](https://upload-images.jianshu.io/upload_images/16846478-9d690c3f47062dc9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

参数传递时变量 a 和 b 同时指向了 1 这个对象。 但当我们执行到 b = 2 时， 系统会重新创建一个值为 2 的新对象， 并让 b 指向它；而 a 仍然指向 1 这个对象。 所以， a 的值不变， 仍然为 1。

要改变a的值可采用如下方法：

```python
def my_func2(b):
    b = 2
    return b

a = 1
a = my_func2(a)
print('a=',a)
```
![Python 函数的参数传递02.png](https://upload-images.jianshu.io/upload_images/16846478-473ee5b0044035ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当可变对象当作参数传入函数里的时候， 改变可变对象的值， 就会影响所有指向它的变量。

```python
def my_func3(l2):
    l2.append(4)

l1 = [1, 2, 3]
my_func3(l1)
print('l1=',l1)
```
![Python 函数的参数传递03.png](https://upload-images.jianshu.io/upload_images/16846478-f53d0c7be4a916ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是如果 采用 l2 = l2 + [4]的方式改变值，l1是不会变得，原因 l2 = l2 + [4]是新建一个对象，并让l2指向这个对象。

```python
def my_func4(l2):
    l2 = l2 + [4]

l1 = [1, 2, 3]
my_func4(l1)
print('l1=',l1)
```

![Python 函数的参数传递04.png](https://upload-images.jianshu.io/upload_images/16846478-69a5d7b55efe8a3f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果要改变l1可以采用如下方法

```python
def my_func5(l2):
    l2 = l2 + [4]
    return l2

l1 = [1, 2, 3]
l1 = my_func5(l1)
print('l1=',l1)
```
![Python 函数的参数传递05.png](https://upload-images.jianshu.io/upload_images/16846478-c729e86c7557784e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- my_func3() 中单纯地改变了对象的值并没有新建对象， 因此函数返回后， 所有指向该对象的变量都会被改变；但 my_func4() 中则创建了新的对象， 并赋值给一个本地变量， 因此原变量仍然不变。




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
