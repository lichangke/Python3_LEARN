目录链接：https://www.jianshu.com/p/e1e201bea601

## 什么是 assert？


Python 的 assert 语句， 可以说是一个 debug 的好工具， 主要用于测试一个条件是否满足。 如果测试的条件满足， 则什么也不做， 相当于执行了 pass 语句；如果测试条件不满足， 便会抛出异常AssertionError， 并返回具体的错误信息（optional） 。

一个简单形式的assert expression：

```python
assert 1 == 2
```
它就相当于下面这两行代码：

```python
if __debug__:
    if not expression: raise AssertionError
```

assert expression1, expression2 的形式例子：

```python
assert 1 == 2,  'assertion is wrong'
```
相当于：
```python
if __debug__:
    if not expression1: raise AssertionError(expression2)
```

__debug__ 是一个常数。 如果 Python 程序执行时附带了 -O 这个选项， 比如 Python test.py -O ， 那么程序中所有的 assert 语句都会失效， 常数 __debug__ 便为 False；反之 __debug__ 则为 True。

不要在使用 assert 时加入括号：
```python
assert(1 == 2, 'This should fail')
```
![不要在使用 assert 时加入括号.png](https://upload-images.jianshu.io/upload_images/16846478-c5f34eed754a0431.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

无论表达式对与错（这里的 1 == 2 显然是错误的） ， assert 检查永远不会 fail





## 参考资料：

*极客时间 Python核心技术与实战学习*

Python核心技术与实战（极客时间）链接：
http://gk.link/a/103Sv


What is the use of “assert” in Python?：
[https://stackoverflow.com/questions/5142418/what-is-the-use-of-assert-in-python](https://stackoverflow.com/questions/5142418/what-is-the-use-of-assert-in-python)

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
