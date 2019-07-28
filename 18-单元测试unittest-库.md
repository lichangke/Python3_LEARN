
目录链接：https://www.jianshu.com/p/e1e201bea601


## 什么是单元测试？

编写测试来验证某一个模块的功能正确性， 一般会指定输出， 验证输出是否符合预期。

单元测试，就不得不提 Python unittest 库（更多参看文章结尾中的参考资料）它提供了我们需要的大多数工具。

例子：
```python
import unittest
# 将要被测试的排序函数
def sort(arr):
    length = len(arr)
    for i in range(0, length):
        for j in range(i + 1, length):
            if arr[i] >= arr[j]:
                arr[i], arr[j] = arr[j], arr[i]
# 编写子类继承 unittest.TestCase
class TestSort(unittest.TestCase):
    # 以 test 开头的函数将会被测试
    def test_sort(self):
        arr = [3, 4, 1, 5, 6]
        sort(arr)
        # assert 结果跟我们期待的一样
        self.assertEqual(arr, [1, 3, 4, 5, 6])
if __name__ == '__main__':
    unittest.main()
```

![单元测试.png](https://upload-images.jianshu.io/upload_images/16846478-7f4f2efb6849317d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1、需要创建一个类 TestSort ， 继承类 ‘unittest.TestCase’ 
2、在这个类中定义相应的测试函数 test_sort()， 进行测试。 注意， 测试函数要以 ‘test’ 开头。
3、测试函数的内部， 通常使用 assertEqual()、 assertTrue()、 assertFalse() 和 assertRaise() 等 assert 语句对结果进行验证。

如果在 IPython 或者 Jupyter 环境下使用：
```python
unittest.main(argv=['first-arg-is-ignored'], exit=False)
```
## 组织TestSuite

使用TestSuite控制用例执行的顺序，添加到TestSuite中的case是会按照添加的顺序执行的。

```python
import unittest
def add(a, b):
    return a+b
def minus(a, b):
    return a-b
def multi(a, b):
    return a*b
def divide(a, b):
    return a/b
class TestMathFunc(unittest.TestCase):
    def test_add(self):
        """Test method add(a, b)"""
        self.assertEqual(3, add(1, 2))
        self.assertNotEqual(3, add(2, 2))
    def test_minus(self):
        """Test method minus(a, b)"""
        self.assertEqual(1, minus(3, 2))
    def test_multi(self):
        """Test method multi(a, b)"""
        self.assertEqual(6, multi(2, 3))
    def test_divide(self):
        """Test method divide(a, b)"""
        self.assertEqual(2, divide(6, 3))
        self.assertEqual(2, divide(5, 2))
if __name__ == '__main__':
    suite = unittest.TestSuite()
    tests = [TestMathFunc("test_minus"), TestMathFunc("test_add"), TestMathFunc("test_divide")]
    suite.addTests(tests)
    # verbosity 参数可以控制输出的错误报告的详细程度，默认是 1，如果设为 0，则不输出每一用例的执行结果，
    # 如果设为 2，则输出详细的执行结果
    runner = unittest.TextTestRunner(verbosity=2)
    runner.run(suite)
```
执行了三个case，并且顺序是按照我们添加进suite的顺序执行的。

![组织TestSuite.png](https://upload-images.jianshu.io/upload_images/16846478-547efc0d333c615c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
更多可参看 [https://blog.csdn.net/huilan_same/article/details/52944782](https://blog.csdn.net/huilan_same/article/details/52944782)


## 单元测试的几个技巧

Python 单元测试的几个技巧， 分别是 mock、 side_effect 和 patch。这三者用法不一样， 但都是一个核心思想， 即用**虚假的实现， 来替换掉被测试函数的一些依赖项， 让我们能把更多的精力放在需要被测试的功能上。**
可以参看 [https://docs.python.org/zh-cn/3/library/unittest.mock-examples.html](https://docs.python.org/zh-cn/3/library/unittest.mock-examples.html)

### mock

mock 是单元测试中最核心重要的一环。 mock 的意思， 便是通过一个虚假对象， 来代替被测试函数或模块需要的对象。
比如要测试个后端 API 逻辑的功能性， 但一般后端 API 都依赖于数据库、文件系统、网络等。 这样， 你就需要通过 mock， 来创建一些虚假的数据库层、文件系统层、 网络层对象， 以便可以简单地对核心后端逻辑单元进行测试。
Python mock 则主要使用 mock 或者 MagicMock 对象，一个简单示例：

```python
import unittest
from unittest.mock import MagicMock
class A(unittest.TestCase):
    def m1(self):
        val2 = self.m2()
        print('m1.val2=', val2)
        self.m3(val2)
    def m2(self):
        print('m2')
        pass
    def m3(self, val):
        print('m3.val=',val)
        pass
    def test_m1(self):
        a = A()
        # m2() 替换为一个返回具体数值的 value
        a.m2 = MagicMock(return_value="custom_val")
        # m3() 替换为另一个 mock（空函数）
        a.m3 = MagicMock()
        a.m1()
        self.assertTrue(a.m2.called)  # 验证 m2 被 call 过
        a.m3.assert_called_with("custom_val")  # 验证 m3 被指定参数 call 过
if __name__ == '__main__':
    unittest.main(verbosity=2)
```
![mock.png](https://upload-images.jianshu.io/upload_images/16846478-fd07ff6ce8d9fda6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

定义了一个类的三个方法 m1()、 m2()、 m3()。 我们需要对 m1() 进行单元测试， 但是 m1() 取决于 m2() 和 m3()。 如果 m2() 和 m3() 的内部比较复杂, 就不能只是简单地调m1() 函数来进行测试， 可能需要解决很多依赖项的问题。
使用mock，可以把 m2() 替换为一个返回具体数值的 value， 把 m3() 替换为另一个 mock（空函数） 。这样， 测试 m1() 就很容易了， 我们可以测试 m1() 调用 m2()， 并且用 m2() 的返回值调用 m3()。由于已被替换m2(),m3()中的print均未输出。

### Mock Side Effect

 mock 的函数， 属性是可以根据不同的输入， 返回不同的数值， 而不只是一个 return_value。

比如下面这个示例， 测试的是输入参数是否为负数， 输入小于 0 则输出为 1 ， 否则输出为 2。 
```python
from unittest.mock import MagicMock
def side_effect(arg):
    if arg < 0:
        return 1
    else:
        return 2
mock = MagicMock()
mock.side_effect = side_effect
print('mock(-1) = ', mock(-1))
print('mock(2) = ', mock(2))
```
![Mock Side Effect](https://upload-images.jianshu.io/upload_images/16846478-7a246bff6b0bffa8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### patch

patch， 给开发者提供了非常便利的函数 mock 方法。  它可以应用 Python 的 decoration 模式或是 context manager 概念， 快速自然地 mock 所需的函数。有些函数可能不属于你，你也不在意它的内部实现而只是想调用这个函数然后得到结果而已，这种时候就可以用 patch 方式来模拟。

有 2 个函数，其中一个 send_shell_cmd 是其他人写的，怎么做的不在乎。一个函数 check_cmd_response 用来检查send_shell_cmd 返回的结果，然后对自己写的 check_cmd_response 做单元测试。假如 send_shell_cmd 函数可能需要一个真实的 PC ，这需要设备，而且每次返回还可能与预期不符，比如设备无法连接，想检查的东西忘记配置所以取不回来等等，这些都会干扰我自己函数的行为，而且问题和自己函数无关，这种时候就可以用 mock 模拟 send_shell_cmd 函数而且把预期返回写到这个模拟过程中，这样每次都会正确处理。

```python
import re
from unittest import TestCase, TestSuite, TextTestRunner
from unittest.mock import patch
class LinuxTool(object):
    def __init__(self):
        pass
    def send_shell_cmd(self):
        return "Response from send_shell_cmd function"
    def check_cmd_response(self):
        response = self.send_shell_cmd()
        print("response: {}".format(response))
        return re.search(r"mock_send_shell_cmd", response)
class TestLinuxTool(TestCase):
    def setUp(self):
        self.linux_tool = LinuxTool()
    def tearDown(self):
        pass
    @patch("__main__.LinuxTool.send_shell_cmd")
    def test_check_cmd_response(self, mock_send_shell_cmd):
        mock_send_shell_cmd.return_value = "Response from emulated mock_send_shell_cmd function"
        status = self.linux_tool.send_shell_cmd()
        print("check result: {}" .format(status))
        self.assertTrue(status)
if __name__ == '__main__':
    suite = TestSuite()
    tests = [TestLinuxTool("test_check_cmd_response")]
    suite.addTests(tests)
    runner = TextTestRunner(verbosity=2)
    runner.run(suite)
```

![patch.png](https://upload-images.jianshu.io/upload_images/16846478-016f27e08c23295f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



```python
@patch("__main__.LinuxTool.send_shell_cmd")
def test_check_cmd_response(self, mock_send_shell_cmd):
    mock_send_shell_cmd.return_value = "Response from emulated mock_send_shell_cmd function"
...
```
mock_send_shell_cmd替代 LinuxTool.send_shell_cmd函数本身的存在，可以像之前提到的 mock
object 一样， 设置 return_value 和 side_effect。


如果 patch 多个外部函数，那么调用遵循自下而上的规则，比如：

```python
@patch("function_C")
@patch("function_B")
@patch("function_A")
def test_check_cmd_response(self, mock_function_A, mock_function_B, mock_function_C):
    mock_function_A.return_value = "Function A return"
    mock_function_B.return_value = "Function B return"
    mock_function_C.return_value = "Function C return"

    self.assertTrue(re.search("A", mock_function_A()))
    self.assertTrue(re.search("B", mock_function_B()))
    self.assertTrue(re.search("C", mock_function_C()))
```
注意官网说明 [https://docs.python.org/zh-cn/3/library/unittest.mock-examples.html#mocking-classes](https://docs.python.org/zh-cn/3/library/unittest.mock-examples.html#mocking-classes)

>mock provides three convenient decorators for this: [`patch()`](https://docs.python.org/zh-cn/3/library/unittest.mock.html#unittest.mock.patch "unittest.mock.patch"), [`patch.object()`](https://docs.python.org/zh-cn/3/library/unittest.mock.html#unittest.mock.patch.object "unittest.mock.patch.object") and [`patch.dict()`](https://docs.python.org/zh-cn/3/library/unittest.mock.html#unittest.mock.patch.dict "unittest.mock.patch.dict"). `patch` takes a single string, of the form `package.module.Class.attribute` to specify the attribute you are patching. It also optionally takes a value that you want the attribute (or class or whatever) to be replaced with. 'patch.object' takes an object and the name of the attribute you would like patched, plus optionally the value to patch it with.

上面例子中，可以有如下写法(测试 python 版本 3.7.3)
```python
@patch("__main__.LinuxTool.send_shell_cmd")
def test_check_cmd_response(self, mock_send_shell_cmd):
    mock_send_shell_cmd.return_value = "Response from emulated mock_send_shell_cmd function"
...
# Test 为文档名
@patch("Test.LinuxTool.send_shell_cmd")
def test_check_cmd_response(self, mock_send_shell_cmd):
    mock_send_shell_cmd.return_value = "Response from emulated mock_send_shell_cmd function"
...

@patch.object(LinuxTool,"send_shell_cmd")
def test_check_cmd_response(self, mock_send_shell_cmd):
    mock_send_shell_cmd.return_value = "Response from emulated mock_send_shell_cmd function"
...

```

![@patch.png](https://upload-images.jianshu.io/upload_images/16846478-07a499c3fe4ea8a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![@patch.object.png](https://upload-images.jianshu.io/upload_images/16846478-0a894be290ca491a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 参考资料：

*极客时间 Python核心技术与实战学习*

Python核心技术与实战（极客时间）链接：
http://gk.link/a/103Sv

Python必会的单元测试框架 —— unittest：
[https://blog.csdn.net/huilan_same/article/details/52944782](https://blog.csdn.net/huilan_same/article/details/52944782)

unittest--- 单元测试框架：
[https://docs.python.org/zh-cn/3/library/unittest.html](https://docs.python.org/zh-cn/3/library/unittest.html)

unittest.mock 上手指南：
[https://docs.python.org/zh-cn/3/library/unittest.mock-examples.html](https://docs.python.org/zh-cn/3/library/unittest.mock-examples.html)

Mock 模块使用说明：
[https://www.jianshu.com/p/55e5a6863c3f](https://www.jianshu.com/p/55e5a6863c3f)

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
