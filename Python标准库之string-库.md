*Python Standard Library based on Python 3.7.3 [https://docs.python.org/3/library/](https://docs.python.org/3/library/)*

>Python标准库 - 文本处理服务 - string库    
>Source code:  [Lib/string.py](https://github.com/python/cpython/blob/3.7/Lib/string.py)
>Link: [https://docs.python.org/3/library/string.html#module-string](https://docs.python.org/3/library/string.html#module-string)

目录链接：https://www.jianshu.com/p/e1e201bea601

# 字符串常量 String constants : 
GitHub Code : [String constants.py](https://github.com/lichangke/Python-Standard-Library-Learn/blob/master/Text%20Processing%20Services/string%20%E2%80%94%20Common%20string%20operations/String%20constants.py)

## string.ascii_letters 大小写字母常数
```python
# ascii_letters     大小写字母常数
print(string.ascii_letters)	# abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ
```
## string.ascii_lowercase  小写字母常数 
```python
# ascii_lowercase  小写字母常数   
print(string.ascii_lowercase) # abcdefghijklmnopqrstuvwxyz
```
## string.ascii_uppercase  大写字母常数
```python
# ascii_uppercase   大写字母常数
print(string.ascii_uppercase)	# ABCDEFGHIJKLMNOPQRSTUVWXYZ
```
## string.digits  十进制数字常数
```python
# digits    十进制数字常数
print(string.digits)	# 0123456789
```
## string.hexdigits  十六进制数字常数
```python
# hexdigits     十六进制数字常数
print(string.hexdigits)	 # 0123456789abcdefABCDEF
```
## string.octdigits  八进制数字常数
```python
# octdigits     八进制数字常数
print(string.octdigits)	# 01234567
```
## string.punctuation  ASCII字符串，在C语言环境中被视为标点字符
```python
# punctuation   ASCII字符串，在C语言环境中被视为标点字符
print(string.punctuation)	# !"#$%&'()*+,-./:;<=>?@[\]^_`{|}~
```
## string.printable 能够被打印的ASCII字符串
```python
# printable     能够被打印的ASCII字符串
print(string.printable)
# 0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~  还要加上 字符空间，制表符，换行符，返回页面，换页符和垂直选项卡
```
## string.whitespace  字符空间，制表符，换行符，返回页面，换页符和垂直选项卡
```python
# whitespace    包含所有被视为空格的ASCII字符的字符串
print(string.whitespace) # 字符空间，制表符，换行符，返回页面，换页符和垂直选项卡
```
## 自定义字符串格式 Custom String Formatting
GitHub Code : [Custom String Formatting.py](https://github.com/lichangke/Python-Standard-Library-Learn/blob/master/Text%20Processing%20Services/string%20%E2%80%94%20Common%20string%20operations/Custom%20String%20Formatting.py)

介绍自定义字符串格式 class string.Formatter中主要的3个函数

format(format_string, *args, **kwargs)、vformat(format_string, args, kwargs)、parse(format_string)

### format(format_string, *args, **kwargs)
主要的API方法。它采用格式字符串和一组任意位置和关键字参数。它只是一个调用vformat（）的包装器。
```python
'''
@Description: 主要的API方法。它采用格式字符串和一组任意位置和关键字参数。它只是一个调用vformat（）的包装器。
@Param: 
format_string: 需要去格式化的目标字符串
*args: 任意位置 元组
**kwargs: 关键字参数 字典
@Return: 
'''
# string.Formatter.format(format_string, *args, **kwargs)
data = ("Pi = ",3.1415926)
strtmp = "This is a test:{}{:.4f}"
formatter  = string.Formatter()
strtmp = formatter.format(strtmp,*data) # 元组
print(strtmp)  # This is a test:Pi = 3.1416

data = {"Key1":3.1415926,"Key2":"Pi = "}
strtmp = "This is a test:{Key2}{Key1}"
formatter  = string.Formatter()
strtmp = formatter.format(strtmp,**data)  # 字典
print(strtmp)  # This is a test:Pi = 3.1415926

# string.Formatter.format(format_string, *args, **kwargs) End
```
### vformat(format_string, args, kwargs)
此函数执行格式化的实际工作
```python
'''
@Description: 此函数执行格式化的实际工作
@Param: 
format_string: 需要去格式化的目标字符串
args: 任意位置 元组
kwargs: 关键字参数 字典
@Return: 
'''
# string.Formatter.vformat(format_string, args, kwargs)
# 注意 Formatter.vformat的参数不是 (*args, **kwargs) 而是 (args, kwargs)
data = ("Pi = ",3.1415926)
strtmp = "This is a test:{}{:.4f}"
formatter  = string.Formatter()
strtmp = formatter.vformat(strtmp,data,{}) # 元组
print(strtmp)  # This is a test:Pi = 3.1416

data = {"Key1":3.1415926,"Key2":"Pi = "}
strtmp = "This is a test:{Key2}{Key1}"
formatter  = string.Formatter()
strtmp = formatter.vformat(strtmp,(),data)  # 字典
print(strtmp)  # This is a test:Pi = 3.1415926
# string.Formatter.vformat(format_string, args, kwargs) End
```
### parse(format_string)
循环遍历format_string并返回一个可迭代的元组（literal_text，field_name，format_spec，conversion）。
```python
'''
@Description: 循环遍历format_string并返回一个可迭代的元组（literal_text，field_name，format_spec，conversion）。 vformat（）使用它将字符串分解为文字文本或替换字段。
@Param: 
format_string：需要去格式化的目标字符串
@Return: 
tuples  元组
'''
# string.Formatter.parse(format_string)
strtmp = "This is a test:{}{:.4f}"
formatter  = string.Formatter()
strtuple = formatter.parse(strtmp)

for i, v in enumerate(strtuple):
    print(i, v)
    '''
    0 ('This is a test:', '', '', None)
    1 ('', '', '.4f', None)
    '''
strtmp = "This is a test:{Key2}{Key1}"
formatter  = string.Formatter()
strtuple = formatter.parse(strtmp)

for i, v in enumerate(strtuple):
    print(i, v)
    '''
    0 ('This is a test:', 'Key2', '', None)
    1 ('', 'Key1', '', None)
    '''
# string.Formatter.parse(format_string) End
```

# 格式字符串语法示例 Format String Syntax examples
GitHub Code : [Format String Syntax.py](https://github.com/lichangke/Python-Standard-Library-Learn/commit/faa2db0f93a8ad73db258db562bf115a4871c9ac "Create Format String Syntax.py")

包含str.format（）语法的示例以及与旧的％-formatting的比较。

在大多数情况下，语法类似于旧的％-formatting，添加了{}和with：而不是％。例如，'％03.2f'可以翻译为'{：03.2f}'。

## Accessing arguments by position: 按位置访问参数：
```python
tupdata = ("This","is","a","test") # 元组
formatstr = '{0} {1} {2} {3}'.format("This","is","a","test") 
print(formatstr)    # This is a test
formatstr = '{} {} {} {}'.format(*tupdata) # *data 解包参数序列
print(formatstr)    # This is a test
formatstr = '{3} {2} {1} {0}'.format(*tupdata) # *data 解包参数序列
print(formatstr)    # test a is This
formatstr = '{2} {3} {1} {2} {3}'.format(*tupdata)  # 参数可以重复
print(formatstr)    # a test is a test
```
## Accessing arguments by name：按关键字访问参数：
```python
dicdata = {'Author':'leacoder','Time':'2019/04/17'}
formatstr = 'The author is {Author}，The time is {Time}'.format(Author='leacoder',Time='2019/04/17')
print(formatstr)    # The author is leacoder，The time is 2019/04/17
formatstr = 'The author is {Author}，The time is {Time}'.format(**dicdata)
print(formatstr)    # The author is leacoder，The time is 2019/04/17
```
## Accessing arguments’ attributes: 访问参数的属性：
```python
class Point:
    def __init__(self,x,y):
        self.x ,self.y = x, y
point = Point(4,2)
formatstr = 'Thie point is ({key.x},{key.y})'.format(key = point) # key 可为其他 
print(formatstr)  # Thie point is (4,2)
formatstr = 'Thie point is ({point.x},{point.y})'.format(point = point) # point 可为其他 
print(formatstr)  # Thie point is (4,2)
```
## Accessing arguments’ items: 访问参数的各项：
```python
tupdata = ("leacoder","2019/04/17") # 元组
formatstr = 'The author is {0[0]},The time is {0[1]}'.format(tupdata)
print(formatstr)    # The author is leacoder,The time is 2019/04/17
formatstr = 'The author is {0[0]},The time is {0[1]}'.format(*tupdata)  # 注意区别
print(formatstr)    # The author is l,The time is e
```
## Replacing %s and %r: 替换％s和％r：  使用 !s !r
```python
formatstr = "repr() shows quotes: {!r}; str() doesn't: {!s}".format('test1', 'test2')
print(formatstr)    # repr() shows quotes: 'test1'; str() doesn't: test2
```
## Aligning the text and specifying a width: 对齐文本并指定宽度：
```python
formatstr = '{:<30}'.format('left aligned') # 左对齐 30位
print(formatstr)    # ‘left aligned                  ’  为了体现位数加了‘’
formatstr = '{:>30}'.format('right aligned')    # 右对齐 30位
print(formatstr)    # ‘                 right aligned’
formatstr = '{:^30}'.format('centered')  # 中间对齐 30位
print(formatstr)    # ‘           centered           ’
formatstr =  '{:*^30}'.format('centered')  # 使用* 作为填充字符
print(formatstr)    # ‘***********centered***********’
```
## Replacing %+f, %-f, and % f and specifying a sign: 替换％+ f，％ - f和％f并指定符号:
```python
formatstr = '{:+f}; {:+f}'.format(3.14, -3.14)  # 总是显示它符号
print(formatstr)    # ‘+3.140000; -3.140000’
formatstr = '{: f}; {: f}'.format(3.14, -3.14)  # 正数前显示空格
print(formatstr)    # ‘ 3.140000; -3.140000’
formatstr = '{:-f}; {:-f}'.format(3.14, -3.14)  # 只显示负号 同 '{:f}; {:f}'
print(formatstr)    # ‘3.140000; -3.140000’
```
## Replacing %x and %o and converting the value to different bases: 替换％x和％o并将值转换为不同的进制：
```python
formatstr = "int: {0:d};  hex: {0:x};  oct: {0:o};  bin: {0:b}".format(64)
print(formatstr)  # int: 64;  hex: 40;  oct: 100;  bin: 1000000
formatstr = "int: {0:d};  hex: {0:#x};  oct: {0:#o};  bin: {0:#b}".format(64)
print(formatstr)  # int: 64;  hex: 0x40;  oct: 0o100;  bin: 0b1000000
formatstr = "int: {0:d};  hex: {0:#x};  oct: {0:#o};  bin: {0:#b}".format(0b1000001) # 也支持其他进制
print(formatstr)  # int: 65;  hex: 0x41;  oct: 0o101;  bin: 0b1000001
```
##  Using the comma as a thousands separator: 使用逗号作为千位分隔符：
```python
formatstr = '{:,}'.format(1234567890)
print(formatstr)    # 1,234,567,890
```
## Expressing a percentage: 表达百分比：
```python
points = 1
total = 3
formatstr = 'points / total = {:.2%}'.format(points/total)
print(formatstr)    # points / total = 33.33%
```
## Using type-specific formatting: 使用特定类型的格式：
```python
import datetime
d = datetime.datetime(2019, 4, 17, 22, 49, 2) # 2019/04/17 22:49:02
formatstr = '{:%Y-%m-%d %H:%M:%S}'.format(d)
print(formatstr)    # 2019-04-17 22:49:02
```

# 模板字符串 Template strings
GitHub Code : [Template strings.py](https://github.com/lichangke/Python-Standard-Library-Learn/blob/master/Text%20Processing%20Services/string%20%E2%80%94%20Common%20string%20operations/Template%20strings.py)

## 模板字符串的规则
```python
'''
模板字符串提供更简单的字符串替换，如PEP 292中所述 https://www.python.org/dev/peps/pep-0292/
模板字符串支持基于$的替换，使用以下规则：
    1、$$是转义; 它被替换为单个$。
    2、$identifier 一个替换占位符，用于匹配映射关键字“identifier”默认情况下，
    “标识符”仅限于以下划线或ASCII字母开头的任何不区分大小写的ASCII字母数字字符串（包括下划线）。$字符后面的第一个非标识符字符结束此占位符。
    3、$ {identifier}相当于$ identifier。当有效标识符字符跟随占位符但不是占位符的一部分时，例如“$ {noun} ification”，则需要它。
    4、字符串中$的任何其他形式都将导致引发ValueError。
字符串模块提供实现这些规则的Template类。class string.Template(template)
'''
```
## class string.Template 类

###  substitute(mapping, **kwds)
```python
'''
@Description: 执行模板替换，返回一个新字符串。
@Param: 
mapping：任何类似字典的对象，其键与模板中的占位符匹配。
**kewds: 关键字参数，其中关键字是占位符。
当给出mapping和kwds并且存在重复时，来自kwds的占位符优先。
@Return: 返回一个新字符串
'''
# substitute(mapping, **kwds)
s = Template('The Author is $Author, The Time is $Time')    # 使用Template类构造函数
kewds = {'Author':'leacoder', 'Time':'2019/04/18 00:01:38'}
templatestr = s.substitute(Author='leacoder', Time='2019/04/18 00:01:38')  # **kewds
print(templatestr)  # The Author is leacoder, The Time is 2019/04/18 00:01:38
templatestr = s.substitute(**kewds)  # **kewds
print(templatestr)  # The Author is leacoder, The Time is 2019/04/18 00:01:38
templatestr = s.substitute(kewds)  # mapping
print(templatestr)  # The Author is leacoder, The Time is 2019/04/18 00:01:38
templatestr = s.substitute(kewds,Author='250',Time = 'No Time')  # mapping  **kewds
print(templatestr)  # The Author is 250, The Time is No Time

kewds1 = {'Author':'leacoder'}
templatestr = s.substitute(kewds1)
print(templatestr)  # KeyError: 'Time'
# substitute(mapping, **kwds) End
```
### safe_substitute(mapping, **kwds)
```python
'''
@Description:  
与substitute（）一样，如果map和kwds中缺少占位符，原始占位符将在结果字符串中完整显示，而不是引发KeyError异常
此外，与substitute（）不同，$的任何其他形式只会返回$而不是引发ValueError。
@Param: 
同 substitute（）
@Return: 
'''
# safe_substitute(mapping, **kwds)
kewds1 = {'Author':'leacoder'}
templatestr = s.safe_substitute(kewds1)
print(templatestr)  # The Author is leacoder, The Time is $Time
# safe_substitute(mapping, **kwds) End
```

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
