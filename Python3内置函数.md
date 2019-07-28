*Python Standard Library based on Python 3.7.3 [https://docs.python.org/3/library/](https://links.jianshu.com/go?to=https%3A%2F%2Fdocs.python.org%2F3%2Flibrary%2F)*
>Python标准库 - 内置函数
>Link: [https://docs.python.org/3/library/functions.html](https://docs.python.org/3/library/functions.html)
>说明 print 后的 # 注释为输出和相关说明，包含所有 Python3.7.3 官方文档中的内置函数
>GitHub Code : [Built-in Functions.py](https://github.com/lichangke/Python-Standard-Library-Learn/blob/master/Built-in%20Functions/Built-in%20Functions.py)

目录链接：https://www.jianshu.com/p/e1e201bea601

## abs(x) 
```python
'''
@Description: 
    abs(x) 函数返回数字的绝对值 
@Param: 
    x -- 数值表达式。
@Return: 
    函数返回x（数字）的绝对值。
'''
# abs(x) 
print ("abs(-45) : ", abs(-45)) # abs(-45) :  45
print ("abs(100.12) : ", abs(100.12))   # abs(-45) :  45
print ("abs(complex(2,-2)) : ", abs(complex(2,-2))) # abs(complex(2,-2)) :  2.8284271247461903
# abs(x)  End
```

## all(iterable)
```python
'''
@Description: 
    all() 函数用于判断给定的可迭代参数 iterable 中的所有元素是否都为 TRUE，如果是返回 True，否则返回 False。
    元素除了是 0、空、None、False 外都算 True。
@Param: 
    iterable -- 元组或列表。
@Return: 
    如果iterable的所有元素不为0、''、False或者iterable为空，all(iterable)返回True，否则返回False；
    注意：空元组、空列表返回值为True，这里要特别注意。
等价于
    def all(iterable):
        for element in iterable:
            if not element:
                return False
        return True
'''
# all(iterable)
print(all(['a', 'b', 'c', 'd']))    # True
print(all(['a', 'b', '', 'd']))     # False
print(all([0, 1, 2, 3]))    # False
print(all((0, 1, 2, 3)))    # False
print(all([]))  # True
print(all(()))  # True
# all(iterable) End
```

## any(iterable)
```python
'''
@Description: 
    any() 函数用于判断给定的可迭代参数 iterable 是否全部为 False，则返回 False，如果有一个为 True，则返回 True。
    元素除了是 0、空、FALSE 外都算 TRUE。
@Param: 
    iterable -- 元组或列表。
@Return: 
    如果都为空、0、false，则返回false，如果不都为空、0、false，则返回true。

等价于：
    def any(iterable):
        for element in iterable:
            if element:
                return True
        return False
'''
# any(iterable)
print(any(['a', 'b', '', 'd']) )    # True
print(any([0, '', False]) ) # False
print(any((0, '', False)))  # False
print(any([]))  # False
print(any(()))  # False
# any(iterable) End
```

## ascii(object)
```python
'''
@Description:  
    调用对象的__repr__()方法，获得该方法的返回值.
@Param: 
@Return: 
    返回一个包含对象的可打印表示的字符串
'''
# ascii(object)
print(ascii('!@#$%^1234abcvn\\\n')) # '!@#$%^1234abcvn\\\n'
# ascii(object) End
```

## bin(x)
```python
'''
@Description: 
    bin() 返回一个整数 int 或者长整数 long int 的二进制表示.。
@Param: 
    x -- int 或者 long int 数字
    如果x不是Python int对象，则必须定义一个返回整数的__index __（）方法。
@Return: 
    字符串。
'''
# bin(x)
print(bin(10))  # 0b1010
print(bin(-10)) # -0b1010
# print(bin(-3.1415926))  # TypeError: 'float' object cannot be interpreted as an integer
class Test:
    def __init__(self,x):
        self.intdata = x
    def __index__(self):
        return int(self.intdata)
test = Test(-3.1415926)
print(bin(test))    # -0b11
# bin(x) End
```

## class bool([x])
```python
'''
@Description: 
    bool() 函数用于将给定参数转换为布尔类型，如果没有参数，返回 False。
    bool 是 int 的子类。
@Param: 
    x -- 要进行转换的参数。
@Return: 
    返回 Ture 或 False。
'''
# class bool([x])
print(bool())   # False
print(bool(0))  # False
print(bool(2))  # True
print(issubclass(bool, int))    # True
# class bool([x]) End
```


## breakpoint(*args, **kws)
```python
'''
@Description: 
    3.7新增
    此函数会将您置于调试器中在你放置位置
    具体来说，它调用sys.breakpointhook（），直接传递args和kws。
    默认情况下，sys.breakpointhook（）调用pdb.set_trace（）期望没有参数。
@Param: 
@Return: 
'''
# breakpoint(*args, **kws)
breakpoint()
print("Here is a break point") # 在这里暂停 Here is a break point
# breakpoint(*args, **kws) End
```


## class bytearray([source[, encoding[, errors]]])
```python
'''
@Description: 
    bytearray() 方法返回一个新字节数组。这个数组里的元素是可变的，并且每个元素的值范围: 0 <= x < 256。
@Param: 
    如果 source 为整数，则返回一个长度为 source 的初始化数组；
    如果 source 为字符串，则按照指定的 encoding 将字符串转换为字节序列；
    如果 source 为可迭代类型，则元素必须为[0 ,255] 中的整数；
    如果 source 为与 buffer 接口一致的对象，则此对象也可以被用于初始化 bytearray。
    如果没有输入任何参数，默认就是初始化数组为0个元素。
@Return: 
    新字节数组。
'''
# class bytearray([source[, encoding[, errors]]])
print(bytearray())  # bytearray(b'')
print(bytearray([1,2,3]))   # bytearray(b'')
print(len(bytearray([1,2,3])))  # 3
tmpstr = bytearray('leacoder', 'utf-8')
print(tmpstr)   #bytearray(b'')
print(len(tmpstr)) # 8
tmpstr[0] = ord('d') # 它以一个字符（长度为1的字符串）作为参数，返回对应的 ASCII 数值
print(tmpstr) #  bytearray(b'deacoder')  d 的 ascii 为 100
# class bytearray([source[, encoding[, errors]]]) End
```


## class bytes([source[, encoding[, errors]]])
```python
'''
@Description:
    bytes是bytearray的不可变版本
@Param: 

@Return: 
    返回一个新的“字节”对象,它是一个不可变的整数序列，范围为0 <= x <256。
'''
# class bytes([source[, encoding[, errors]]])
print(bytes())  # bytearray(b'')
print(bytes([1,2,3]))   # bytearray(b'')
print(len(bytes([1,2,3])))  # 3
tmpstr = bytes('leacoder', 'utf-8')
print(tmpstr)   #bytearray(b'')
print(len(tmpstr)) # 8
# tmpstr[0] = ord('d') # 'bytes' object does not support item assignment  'bytes'对象不支持项目分配
# class bytes([source[, encoding[, errors]]]) End
```

## callable(object)
```python
'''
@Description:
    callable() 函数用于检查一个对象是否是可调用的。如果返回 True，object 仍然可能调用失败；但如果返回 False，调用对象 object 绝对不会成功。
    对于函数、方法、lambda 函式、 类以及实现了 __call__ 方法的类实例, 它都返回 True。
@Param: 
    object -- 对象
@Return: 
    可调用返回 True，否则返回 False。
'''
# callable(object)
print(callable(0))  # False
print(callable('leacock'))  # False
print(callable(lambda x, y: x + y)) # True
def add( x, y):
    return x + y
print(callable(add))    # True

class A:
    def func(self):
        return 0
print(callable(A))  # True
a = A() # A类的实例
print(callable(a))  # False
class B:
    def __call__(self):
        return
print(callable(B))  # True
b = B() # B类的实例
print(callable(b))  # True
# callable(object) End
```

## chr(i)
```python
'''
@Description: 
    chr(i) 返回表示Unicode代码点为整数i的字符的字符串。
    这是ord（）的反转。
@Param: 
    i -- 可以是10进制也可以是16进制的形式的数字。参数的有效范围是0到1,114,111（基数为16的0x10FFFF）。如果i超出该范围，则会引发ValueError。
@Return: 
    返回值是当前整数对应的 ASCII 字符。
'''
# chr(i)
print(chr(0x30), chr(0x31), chr(0x61))  # 0 1 a
print(chr(48), chr(49), chr(97) )   # 0 1 a
print(chr(0o60), chr(0o61), chr(0o141) )   # 0 1 a
print(chr(0b110000), chr(0b110001), chr(0b1100001) )   # 0 1 a
# print(chr(1114112)) # ValueError: chr() arg not in range(0x110000)
# chr(i) End
```


## @classmethod
```python
'''
@Description: 
    classmethod 修饰符对应的函数不需要实例化，不需要 self 参数，但第一个参数需要是表示自身类的 cls 参数，可以来调用类的属性，类的方法，实例化对象等。
@Param: 
@Return: 
    返回函数的类方法。
'''
# @classmethod
class C(object):
    bar = 1
    def func1(self):  
        print ('foo') 
        print (self.bar)
    @classmethod
    def func2(cls):
        print ('func2')
        print (cls.bar)
        cls().func1()   
 
C.func2() # 不需要实例化   func2    1   foo     1
# @classmethod End
```


## compile(source, filename, mode, flags=0, dont_inherit=False, optimize=-1)
```python
'''
@Description: 
    compile() 将source编译为代码或AST对象。代码对象可以由exec（）或eval（）执行。 source可以是普通字符串，字节字符串或AST对象
@Param: 
    source -- 字符串或者AST（Abstract Syntax Trees）对象。。
    filename -- 代码文件名称，如果不是从文件读取代码则传递一些可辨认的值。
    mode -- 指定编译代码的种类。可以指定为 exec, eval, single。
    flags -- 变量作用域，局部命名空间，如果被提供，可以是任何映射对象。。
    flags和dont_inherit是用来控制编译源码时的标志
    optimize  -- 参数optimize指定编译器的优化级别;
@Return: 
    返回表达式执行结果。
'''
# compile(source, filename, mode, flags=0, dont_inherit=False, optimize=-1)
strtmp = "for i in range(0,10): print(i)" 
c = compile(strtmp,'','exec')   # 编译为字节代码对象 
print(c)    # <code object <module> at 0x7f11505769c0, file "", line 1>
exec(c)  # 0 1 2 3 4 5 6 7 8 9
strtmp = "3 * 4 + 5"
a = compile(strtmp,'','eval')
print(eval(a))  # 17
# compile(source, filename, mode, flags=0, dont_inherit=False, optimize=-1) End
```

## class complex([real[, imag]])
```python
'''
@Description: 
    complex() 函数用于创建一个值为 real + imag * j 的复数或者转化一个字符串或数为复数。如果第一个参数为字符串，则不需要指定第二个参数。第二个参数永远不能是字符串。
    如果省略imag，则默认为零，构造函数用作int和float之类的数字转换。如果省略两个参数，则返回0j。
@Param: 
    real -- int, long, float或字符串；
    imag -- int, long, float；
@Return: 
    返回值为real + imag * 1j的复数或将字符串或数字转换为复数。
'''
# class complex([real[, imag]])
print(complex(1, 2))    # (1+2j)
print(complex(1))   # (1+0j)
print(complex())    # 0j
print(complex("10"))    # (10+0j)
# 注意：这个地方在"+"号两边不能有空格，也就是不能写成"1 + 2j"，应该是"1+2j"，否则会报错
print(complex("1+2j"))  # (1+2j)
# print(complex("1 + 2j"))    # ValueError: complex() arg is a malformed string
# class complex([real[, imag]]) End
```

## delattr(object, name)
```python
'''
@Description: 
    delattr 函数用于删除属性。delattr(x, 'foobar') 相等于 del x.foobar。
    与setattr()相对，
@Param: 
    object -- 对象。
    name -- 必须是对象的属性。
@Return: 
'''
# delattr(object, name)
class Coordinate:
    x = 10
    y = -5
    z = 0
point1 = Coordinate() 
delattr(Coordinate, 'z')
del Coordinate.y
print('x = ',point1.x)  # x =  10
# print('y = ',point1.y)  # 触发错误    AttributeError: 'Coordinate' object has no attribute 'y'
# print('z = ',point1.z)  # 触发错误  AttributeError: 'Coordinate' object has no attribute 'z'
# delattr(object, name) End
```


## dict() 
```python
'''
@Description: 
    dict() 函数用于创建一个字典。
    class dict(**kwarg)
    class dict(mapping, **kwarg)
    class dict(iterable, **kwarg)
@Param: 
    **kwargs -- 关键字
    mapping -- 元素的容器。
    iterable -- 可迭代对象。
@Return: 
    返回一个字典。
'''
# dict() 
print(dict())   # {}
# 关键字
print(dict(a='a', b='b', t='t'))    # {'a': 'a', 'b': 'b', 't': 't'}    
# 映射函数方式来构造字典 zip 返回 iterabl 
print(dict(zip(['one', 'two', 'three'], [1, 2, 3])))    # {'one': 1, 'two': 2, 'three': 3}  
# 可迭代对象方式来构造字典
print(dict([('one', 1), ('two', 2), ('three', 3)]))     # {'one': 1, 'two': 2, 'three': 3}
# 可迭代对象 + 关键字
print(dict([('one', 1), ('two', 2), ('three', 3)],a='a', b='b', t='t')) # {'one': 1, 'two': 2, 'three': 3, 'a': 'a', 'b': 'b', 't': 't'}
# dict()  End
```

## dir([object])
```python
'''
@Description: 
    dir() 函数不带参数时，返回当前范围内的变量、方法和定义的类型列表；带参数时，返回参数的属性、方法列表。
    如果参数包含方法__dir__()，该方法将被调用。如果参数不包含__dir__()，该方法将最大限度地收集参数信息。
@Param: 
    object -- 对象、变量、类型。
@Return: 
    返回模块的属性列表。
'''
# dir([object])
print(dir())    #  获得当前模块的属性列表
print(dir([ ]))     # 查看列表的方法
class Shape:
    def __dir__(self):
        return ['area', 'perimeter', 'location']
print(dir(Shape())) # ['area', 'location', 'perimeter']
# dir([object]) End
```


## divmod(a, b)
```python
'''
@Description: 
    divmod() 函数把除数和余数运算结果结合起来，返回一个包含商和余数的元组(a // b, a % b)。
@Param: 
    a: 数字非复数
    b: 数字非复数    
@Return: 
    返回一个包含商和余数的元组(a // b, a % b)。
'''
# divmod(a, b)
print(divmod(10, 3))    # (3, 1)
print(divmod(10.0, 3.0))    # (3.0, 1.0)
# print(divmod(1+2j,1+0.5j))  # 报错 TypeError: can't take floor or mod of complex number.
# divmod(a, b) End
```

## enumerate(iterable, start=0)
```python
'''
@Description: 
    enumerate() 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标，一般用在 for 循环当中。
@Param: 
    sequence -- 一个序列、迭代器或其他支持迭代对象。
    start -- 下标起始位置。
@Return: 
    返回 enumerate(枚举) 对象。
'''
# enumerate(iterable, start=0)
seasons = ['Spring', 'Summer', 'Fall', 'Winter']
print(enumerate(seasons))   # <enumerate object at 0x7f897c4ada20>
print(list(enumerate(seasons))) # [(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
print(list(enumerate(seasons, start=1)))    # [(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
# 相当于：
def enumerate(sequence, start=0):
    n = start
    for elem in sequence:
        yield n, elem
        n += 1
# enumerate(iterable, start=0) End
```

## eval(expression, globals=None, locals=None)
```python
'''
@Description: 
    eval() 函数用来执行一个字符串表达式，并返回表达式的值。
    只能是单个运算表达式（注意eval不支持任意形式的赋值操作），而不能是复杂的代码逻辑，这一点和lambda表达式比较相似。
    exec()动态执行Python代码。也就是说exec可以执行复杂的Python代码，而不像eval函数那么样只能计算一个表达式的值。
@Param: 
    expression -- 表达式。
    globals -- 变量作用域，全局命名空间，如果被提供，则必须是一个字典对象。
    locals -- 变量作用域，局部命名空间，如果被提供，可以是任何映射对象。
@Return: 
    返回表达式计算结果。
'''
# eval(expression, globals=None, locals=None)
x = 7
result = eval( '3 * x' )
print(result)   # 21
result = eval('pow(2,2)')
print(result)   # 4
strtmp = "for i in range(0,10): print(i)" 
# evaltest = compile(strtmp,'','eval')   # 报错 SyntaxError: invalid syntax 
evaltest = compile(strtmp,'','exec')
exec(evaltest)  # 0 1 2 3 4 5 6 7 8 9
result = exec('3 * x')
print(result)   # None
# eval(expression, globals=None, locals=None) End
# eval()函数与exec()函数的区别：
# eval()函数只能计算单个表达式的值，而exec()函数可以动态运行代码段。
# eval()函数可以有返回值，而exec()函数返回值永远为None。
```

## exec(object[, globals[, locals]])
```python
'''
@Description: 
    exec 执行储存在字符串或文件中的 Python 语句，相比于 eval，exec可以执行更复杂的 Python 代码。
@Param: 
    object：必选参数，表示需要被指定的Python代码。
    它必须是字符串或code对象。如果object是一个字符串，该字符串会先被解析为一组Python语句，然后在执行（除非发生语法错误）。
    如果object是一个code对象，那么它只是被简单的执行。
    globals：可选参数，表示全局命名空间（存放全局变量），如果被提供，则必须是一个字典对象。
    locals：可选参数，表示当前局部命名空间（存放局部变量），如果被提供，可以是任何映射对象。如果该参数被忽略，那么它将会取与globals相同的值。
@Return: 
    exec 返回值永远为 None。
'''
# exec(object[, globals[, locals]])
# 见 eval
# exec(object[, globals[, locals]])End 

## filter(function, iterable)
```python
'''
@Description: 
    filter() 函数用于过滤序列，过滤掉不符合条件的元素，返回一个迭代器对象，如果要转换为列表，可以使用 list() 来转换。
    该接收两个参数，第一个为函数，第二个为序列，序列的每个元素作为参数传递给函数进行判，然后返回 True 或 False，最后将返回 True 的元素放到新列表中。
@Param: 
    function -- 判断函数。
    iterable -- 可迭代对象。
@Return: 
    返回一个迭代器对象
'''
# filter(function, iterable)
tmplist = filter(lambda n : n % 2 == 1, range(1, 11))
print(tmplist)  # <filter object at 0x7f3494520940>
print(list(tmplist))    # [1, 3, 5, 7, 9]
# filter(function, iterable) End
```

## class float([x])
```python
'''
@Description: 
    float() 函数用于将整数和字符串转换成浮点数。
    对于一般Python对象x，float（x）委托给x .__ float __（）。
    如果没有给出参数，则返回0.0。
@Param: 
    x -- 整数或字符串
    如果参数是一个字符串，它应该包含一个十进制数字，可选地以符号开头，并且可选地嵌入在空格中。
    可选符号可以是“+”或“ - ”; “+”符号对产生的值没有影响。
    参数也可以是表示NaN（非数字）或正或负无穷大的字符串。
    在删除前导和尾随空格字符后，输入必须符合以下语法：
    sign           ::=  "+" | "-"
    infinity       ::=  "Infinity" | "inf"
    nan            ::=  "nan"
    numeric_value  ::=  floatnumber | infinity | nan
    numeric_string ::=  [sign] numeric_value
@Return: 
    返回浮点数。
'''
# class float([x])
print(float())  # 0.0
print(float('   -12345\n')) # -12345.0
print(float('-Infinity'))   # -inf
print(float('NAN')) # nan
print(float('+1E6'))    # 1000000.0
print(float('1e-003'))  # 0.001
# class float([x]) End
```

## format(value[, format_spec])
```python
'''
@Description: 
    将值转换为“格式化”表示，由format_spec控制。
@Param: 
@Return: 
'''
# format(value[, format_spec])
# 参见 string 库  格式字符串语法示例 Format String Syntax   https://www.jianshu.com/p/faaa48f4c511
# format(value[, format_spec]) End
```

## class frozenset([iterable])
```python
'''
@Description: 
    frozenset() 返回一个冻结的集合，冻结后集合不能再添加或删除任何元素。
    参见 https://blog.csdn.net/lilong117194/article/details/78522459
@Param: 
    iterable -- 可迭代的对象，比如列表、字典、元组等等。
@Return: 
    返回新的 frozenset 对象，如果不提供任何参数，默认会生成空集合。
'''
# class frozenset([iterable])
a = frozenset(range(10))     # 生成一个新的不可变集合
print(a)    # frozenset({0, 1, 2, 3, 4, 5, 6, 7, 8, 9})
# a.add(10) # AttributeError: 'frozenset' object has no attribute 'add'
# class frozenset([iterable]) End
```


## getattr(object, name[, default])
```python
'''
@Description: 
    getattr() 函数用于返回一个对象属性值。
@Param: 
    object -- 对象。
    name -- 字符串，对象属性。
    default -- 默认返回值，如果不提供该参数，在没有对应属性时，将触发 AttributeError。
@Return: 
    返回对象属性值。
'''
# getattr(object, name[, default])
class D(object):
    bar = 'leacoder'
d = D()
print(getattr(d, 'bar') )       # 获取属性 bar 值   
# print(getattr(a, 'bar2') )  #  报错 AttributeError: 'D' object has no attribute 'bar2'
print(getattr(d, 'bar2', '123456') )   # 属性 bar2 不存在，但设置了默认值
# getattr(object, name[, default]) End
```


## globals()
```python
'''
@Description: 
    globals() 函数会以字典类型返回当前位置的全部全局变量。
@Param: 
@Return: 
    返回全局变量的字典。
'''
# globals()
print(globals())
# globals() End
```

## hasattr(object, name)
```python
'''
@Description: 
    hasattr() 函数用于判断对象是否包含对应的属性。
@Param: 
    bject -- 对象。
    name -- 字符串，属性名。
@Return: 
    如果对象有该属性返回 True，否则返回 False。
'''
# hasattr(object, name)
class Coordinate1:
    x = 10
    y = -5
point1 = Coordinate1()   
print(hasattr(point1, 'x')) # True
print(hasattr(point1, 'y')) # True
print(hasattr(point1, 'z')) # False
# hasattr(object, name) End
```

## hash(object)
```python
'''
@Description: 
    hash() 用于获取取一个对象（字符串或者数值等）的哈希值。
@Param: 
    object -- 对象；
@Return: 
    返回对象的哈希值（如果有的话）。哈希值是整数。
    它们用于在字典查找期间快速比较字典键。比较相等的数字值具有相同的哈希值（即使它们具有不同的类型，如1和1.0的情况）。
'''
# hash(object)
print(hash('test'))            # 字符串 # 8918808336926749410
print(hash(str([1,2,3])))     # 集合    # -5376174432533403617
print(hash(300.0))      # 300
print(hash(300))    # 300
# hash(object) End
```


## help([object])
```python
'''
@Description: 
    help() 函数用于查看函数或模块用途的详细说明。
@Param: 
    object -- 对象；
@Return: 
    返回对象帮助信息。
'''
# help([object])
a = [1,2,3]
help(a.append)             # # 查看列表 list 帮助信息
# help([object]) End
```

## hex(x)
```python
'''
@Description: 
    hex() 函数用于将一个指定数字转换为 16 进制数。
    如果x不是Python int对象，则必须定义一个返回整数的__index __（）方法。参见 bin() 
@Param: 
    x -- 一个整数
@Return: 
    返回一个字符串，以 0x 开头。
'''
# hex(x)
result = hex(255)
print(result)   # 0xff
print(type(result)) # <class 'str'>
# hex(x) End
```

## id(object)
```python
'''
@Description: 
    id() 函数用于返回对象的“标识”,这是一个整数，在该生命周期内保证该对象是唯一且恒定的。
@Param: 
    object -- 对象。
@Return: 
    返回对象的“标识”
'''
# id(object)
a = 'leacoder'  
print(id(a))    # 140597046261744
b = 'leacoder'
print(id(b))    # 140597046261744
# id(object) End
```

## input([prompt])
```python
'''
@Description: 
    input() 函数接受一个标准输入数据，返回为 string 类型。
@Param: 
    prompt: 提示信息
    如果存在prompt参数，则将其写入标准输出而不带尾随换行符。该函数从输入中读取一行，将其转换为字符串（剥离尾部换行符），然后返回该行。
    读取EOF时，会引发EOFError。
@Return: 
'''
# input([prompt])
s = input('--> ')  # 输入 leacoder  # --> leacoder
# input([prompt])
```
## class int(x, base=10)
```python
'''
@Description: 
    int() 函数用于将一个字符串或数字转换为整型。
    如果x定义__int __（），则int（x）返回x .__ int __（）。如果x定义__trunc __（），则返回x .__ trunc __（）。对于浮点数，这会截断为零。
    如果x不是数字或者给定了base，则x必须是字符串，字节或bytearray实例，表示以radix为基数的整数文字。
    可选 文字可以在前面加+或 - （之间没有空格）并且用空格包围。
@Param: 
    x -- 字符串或数字。
    base -- 进制数，默认十进制。
@Return: 
    返回整型数据。
'''
# class int(x, base=10)
print(int())   # 不传入参数时，得到结果0  # 0
print(int(3))  # 3
print(int(3.6))     # 3
print(int('12',16))     # 18
# print(int(12,16))   # 报错 TypeError: int() can't convert non-string with explicit base
# class int(x, base=10) End
```


## isinstance(object, classinfo)
```python
'''
@Description: 
    如果object参数是classinfo参数的实例，或者是（直接，间接或虚拟）子类的实例，则返回true。
    如果object不是给定类型的对象，则该函数始终返回false。
    如果classinfo是类型对象的元组（或递归，其他此类元组），object是其中任何类型的实例，则返回true。
    如果classinfo不是类型和元组的类型或元组，则会引发TypeError异常。
@Param: 
    object -- 实例对象。
    classinfo -- 可以是直接或间接类名、基本类型或者由它们组成的元组。
    对于基本类型来说 classinfo 可以是： int，float，bool，complex，str(字符串)，list，dict(字典)，set，tuple
@Return: 
'''
# isinstance(object, classinfo)
a = 2
print(isinstance (a,int))   # True
print(isinstance (a,str))   # False
print(isinstance (a,(str,int,list)))    # True
'''
isinstance() 与 type() 区别：
    type() 不会认为子类是一种父类类型，不考虑继承关系。
    isinstance() 会认为子类是一种父类类型，考虑继承关系。
如果要判断两个类型是否相同推荐使用 isinstance()。
'''
class AClass:
    pass
class BClass(AClass):
    pass
print(isinstance(AClass(), AClass)) # True
print(type(AClass()) == AClass) # True     
print(isinstance(BClass(), AClass)) # True  
print(type(BClass()) == AClass) # False     
# isinstance(object, classinfo) End
```

## issubclass(class, classinfo)
```python
'''
@Description: 
    如果class是classinfo的子类（直接，间接或虚拟），则返回true。
    类被认为是其自身的子类。
@Param: 
    class -- 类。
    classinfo -- 类。
    classinfo可以是类对象的元组，在这种情况下，将检查classinfo中的每个条目。
@Return: 
    如果 class 是 classinfo 的子类返回 True，否则返回 False。
'''
# issubclass(class, classinfo)
class AClass1:
    pass
class BClass1(AClass1):
    pass
print(issubclass(AClass1,AClass1))  # True
print(issubclass(BClass1,AClass1))  # True
print(issubclass(BClass1,(str,int,list,AClass1)))  # True
# issubclass(class, classinfo) End
```

## iter(object[, sentinel])
```python
'''
@Description: 
    iter() 函数用来生成迭代器。
@Param: 
    object -- 支持迭代的集合对象。
    sentinel -- 如果传递了第二个参数，则参数 object 必须是一个可调用的对象（如，函数），
    此时，iter 创建了一个迭代器对象，每次调用这个迭代器对象的__next__()方法时，都会调用 object。
@Return: 
    迭代器对象。
'''
# iter(object[, sentinel])
settmp = {'a':1,'b':2,'c':3}
# next(settmp)    # TypeError: 'dict' object is not an iterator
print(next(iter(settmp)))   # a
# iter(object[, sentinel]) End
```


## len(s)
```python
'''
@Description: 
    len() 方法返回对象（字符、列表、元组等）长度或项目个数。
@Param: 
    s -- 对象。可以是序列（例如字符串，字节，元组，列表或范围）或集合（例如字典，集合或冻结集）。
@Return: 
    返回对象长度。
'''
# len(s)
strtmp = "leacdoer"
print(len(strtmp))  # 8
l = [1,2,3,4,5]
print(len(l))   # 5
# len(s) End
```

## class list([iterable])
```python
'''
@Description: 
    list() 方法用于将元组或字符串转换为列表。
@Param: 
    iterable -- 要转换为列表的元组或字符串。
@Return: 
    返回列表。
'''
# class list([iterable])
strtmp="leacoder"
list2=list(strtmp)
print ("列表元素 : ", list2)    # 列表元素 :  ['l', 'e', 'a', 'c', 'o', 'd', 'e', 'r']
# class list([iterable]) End
```


## locals()
```python
'''
@Description: 
    locals() 函数更新并返回表示当前本地符号表的字典。
    在模块级别，locals（）和globals（）是相同的字典。
@Param: 
@Return: 
'''
# locals()
def funtestlocals(arg):    # 两个局部变量：arg、z
    a = 2
    print (locals()) 
funtestlocals(123)  # {'arg': 123, 'a': 2}
# locals() End
```

## map(function, iterable, ...)
```python
'''
@Description: 
    map() 会根据提供的函数对指定序列做映射。
    第一个参数 function 以参数序列中的每一个元素调用 function 函数，返回包含每次 function 函数返回值的新列表。
@Param: 
    function -- 函数
    iterable -- 一个或多个序列
    对于多个迭代，迭代器在最短的iterable耗尽时停止。
@Return: 
    迭代器。
'''
# map(function, iterable, ...)
result = map(lambda x: x ** 2, [2, 3, 4, 5])
print(result)  # 使用 lambda 匿名函数   # <map object at 0x7ff9203c2438>
print(next(result)) # 4
result = map(lambda x, y: x + y, [1,3,5,7],[2,4])
print(next(result)) # 3
print(next(result)) # 7
# print(next(result)) # StopIteration
# map(function, iterable, ...) End
```


## max() 
```python
'''
@Description: 
    max(iterable, *[, key, default])
    max(arg1, arg2, *args[, key])
@Param: 
    如果提供了一个位置参数，则它应该是可迭代的。返回iterable中的最大项。
    如果提供了两个或多个位置参数，则返回最大的位置参数。
    当key参数不为空时，就以key的函数对象为判断的标准。
@Return: 
    返回可迭代中的最大项或两个或多个参数中的最大项。
    如果多个项是最大的，则该函数返回遇到的第一个项。
'''
# max() 
print ("max(80, 100, 1000) : ", max(80, 100, 1000)) # max(80, 100, 1000) :  1000
listtmp1 = [80, 100, 1000]
print ("max(listtmp1) : ", max(listtmp1)) # max(listtmp) :  1000
listtmp2 = [-20,200,5000,1]
print ("max(listtmp1,listtmp2) : ", max(listtmp1,listtmp2))   # max(listtmp,listtmp2) :  [80, 100, 1000]  默认以第一个进行比较
print ("max('123','32') : ", max('123','32'))   # max('123','32') :  32 以第一个进行比较
print ("max(listtmp1,listtmp2) : ", max(listtmp1,listtmp2,key = lambda x:x[1]))  #  max(listtmp1,listtmp2) :  [-20, 200, 5000, 1] key的函数对象为判断的标准。

def mymax(x):
    return -x
print (max(123, 321, key=mymax))    # 123
print (max(listtmp1, key=mymax))    # 80
# max()  End
```


## memoryview(obj)
```python
'''
@Description: 
    memoryview() 函数返回给定参数的内存查看对象(Momory view)。
    所谓内存查看对象，是指对支持缓冲区协议的数据进行包装，在不需要复制对象基础上允许Python代码访问。
@Param: 
    obj -- 对象
@Return: 
    返回元组列表
'''
# memoryview(obj)
# v = memoryview("abcefg")    # TypeError: memoryview: a bytes-like object is required, not 'str'
v = memoryview(bytes("abcefg",'utf-8'))
print(v[0]) # ascii   # 97  
print(v[0:1]) # <memory at 0x7f56a574c408>
print(v[0:1].tobytes()) # b'a'
print(v[1:4])   # <memory at 0x7f56a574c408>
print(v[1:4].tobytes()) # b'bce'
# memoryview(obj) End
```


## min() 
```python
'''
@Description: 
    min(iterable, *[, key, default])
    min(arg1, arg2, *args[, key])
@Param:
    如果提供了一个位置参数，则它应该是可迭代的。返回iterable中的最小项。
    如果提供了两个或多个位置参数，则返回最小的位置参数。
    当key参数不为空时，就以key的函数对象为判断的标准。
@Return: 
    返回可迭代中的最小项或两个或多个参数中的最小项。
    如果多个项目是最小的，则该函数返回遇到的第一个项目。
'''
# min() 
print ("min(80, 100, 1000) : ", min(80, 100, 1000))     # min(80, 100, 1000) :  80
listtmp1 = [80, 100, 1000]
print ("min(listtmp1) : ", min(listtmp1))   # min(listtmp1) :  80
listtmp2 = [-20,200,5000,1]
print ("min(listtmp1,listtmp2) : ", min(listtmp1,listtmp2))   #min(listtmp1,listtmp2) :  [-20, 200, 5000, 1]  默认以第一个进行比较
print ("min('123','32') : ", min('123','32'))   # min('123','32') :  123   以第一个进行比较
print ("min(listtmp1,listtmp2) : ", min(listtmp1,listtmp2,key = lambda x:x[1]))  #  min(listtmp1,listtmp2) :  [80, 100, 1000] key的函数对象为判断的标准。

def mymin(x):
    return -x
print (min(123, 321, key=mymin))    # 321
print (min(listtmp1, key=mymin))    # 1000
# min()  End
```


# next(iterator[, default])
```python
'''
@Description: 
    通过调用__next __（）方法从迭代器中检索下一个项。
@Param: 
    iterator -- 可迭代对象
    default -- 可选，用于设置在没有下一个元素时返回该默认值，如果不设置，又没有下一个元素则会触发 StopIteration 异常。
@Return: 
    迭代器中下一个项
'''
# next(iterator[, default])
it = iter([1, 2, 3])
# 循环:
for i in range(5):
    # 获得下一个值:
    x = next(it,'leacoder')
    print(x)    # 1 2 3 leacoder leacoder
# next(iterator[, default]) End
```

## class object
```python
'''
@Description:
    返回一个新的无特征对象。
    object是所有类的基础。它具有所有Python类实例共有的方法。此函数不接受任何参数。
    object没有__dict__，因此您无法将任意属性分配给对象类的实例
@Param: 
@Return: 
'''
# class object
objecttest = object()
print(objecttest)   # <object object at 0x7eff046bfa10>
print(dir(objecttest)) 
# ['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', 
# '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', 
# '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
# class object End
```

## oct(x)
```python
'''
@Description: 
    oct() 函数将一个整数转换成8进制字符串。
    将整数转换为前缀为“0o”的八进制字符串。结果是一个有效的Python表达式。如果x不是Python int对象，则必须定义一个返回整数的__index __（）方法。
    参见 bin()
@Param: 
    x -- 整数。
@Return: 
    返回一个字符串，以 0o 开头。
'''
# oct(x)
print(oct(9))   # 0o11
# oct(x) End
```

## open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
```python
'''
@Description: 
    open() 方法用于打开一个文件，并返回文件对象，在对文件进行处理过程都需要使用到这个函数，如果该文件无法被打开，会抛出 OSError。
    注意：使用 open() 方法一定要保证关闭文件对象，即调用 close() 方法。
    open() 函数常用形式是接收两个参数：文件名(file)和模式(mode)。
    pen(file, mode='r')
    参见 https://docs.python.org/3.7/library/functions.html#open
@Param: 
    file: 必需，文件路径（相对或者绝对路径）。
    mode: 可选，文件打开模式
    buffering: 设置缓冲
    encoding: 一般使用utf8
    errors: 报错级别
    newline: 区分换行符
    closefd: 传入的file参数类型
    opener:
@Return: 
'''
# open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
# f = open('test.txt')
# open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None) End
```


## ord(c)
```python
'''
@Description: 
    ord() 函数是 chr() 函数（对于 8 位的 ASCII 字符串）的配对函数，它以一个字符串（Unicode 字符）作为参数，返回对应的 ASCII 数值，或者 Unicode 数值。
@Param: 
    c -- 字符。
@Return: 
    返回值是对应的十进制整数。
'''
# ord(c)
print(ord('a')) #97
# ord(c) End
```

## pow(x, y[, z])
```python
'''
@Description: 
    返回x的y次方，如果z存在，返回 x的y次方再模z（比pow（x，y）％z更有效地计算）。两个参数形式pow（x，y）相当于使用幂运算符：x ** y。
    如果存在z，则x和y必须是整数类型，y必须是非负的
@Param: 
    x -- 数值表达式。
    y -- 数值表达式。
    z -- 数值表达式。
    参数必须具有数字类型
@Return:
'''
# pow(x, y[, z])
print( pow(100, 2))     # 10000
print( pow(100, -2))    # 0.0001
print( pow(100, 2, 3))  # 1
# print( pow(100, -2, 3))  # 报错 ValueError: pow() 2nd argument cannot be negative when 3rd argument specified
import math
print( math.pow(100, 2))    # 10000.0 和内置函数区别
# pow(x, y[, z]) End
```



## print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
```python
'''
@Description: 
    print() 方法用于打印输出，最常见的一个函数。
@Param: 
    objects -- 复数，表示可以一次输出多个对象。输出多个对象时，需要用 , 分隔。
    sep -- 用来间隔多个对象，默认值是一个空格。
    end -- 用来设定以什么结尾。默认值是换行符 \n，我们可以换成其他字符串。
    file -- 要写入的文件对象。
    flush -- 输出是否缓冲通常由文件确定，但如果flush关键字参数为true，则强制刷新流。
@Return: 
'''
# print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)
print("Hello World")    # Hello World
print("Hello","World",sep=' * ')    # Hello * World
print("Hello","World",sep=' * ',end='##\n')     # Hello * World##
print('https://docs', 'python' ,'org',sep='.')  # https://docs.python.org
# print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False) End
```


## class property(fget=None, fset=None, fdel=None, doc=None)
```python
'''
@Description: 
    property() 函数的作用是在新式类中返回属性值。
@Param: 
    fget -- 获取属性值的函数
    fset -- 设置属性值的函数
    fdel -- 删除属性值函数
    doc -- 属性描述信息
@Return: 
    新式类属性。
'''
# class property(fget=None, fset=None, fdel=None, doc=None)
class E(object):
    def __init__(self):
        self._x = None
    def getxnew(self):
        print("def getxnew(self):")
        return self._x
    def setxnew(self, value):
        print("def setxnew(self, value):")
        self._x = value
    def delxnew(self):
        print("def delxnew(self):")
        del self._x
    x = property(getxnew, setxnew, delxnew, "I'm the 'x' property.")
# 如果 e 是 E的实例化, e.x 将触发 getter,e.x = value 将触发 setter ， del e.x 触发 deleter。
e = E()
e.x = 123   # def setxnew(self, value):
e.x         # def getxnew(self):
del e.x     # def delxnew(self):
# 将 property 函数用作装饰器可以很方便的创建只读属性,只读属性的 getter 方法。
class F(object):
    def __init__(self):
        self._x = None
    @property
    def x(self):
        """I'm the 'x' property."""
        return self._x
    @x.setter
    def x(self, value):
        self._x = value
    @x.deleter
    def x(self):
        del self._x
# 这个代码和上个例子完全相同，但要注意这些额外函数的名字和 property 下的一样，例如这里的 x。
# class property(fget=None, fset=None, fdel=None, doc=None) End
```


## range()
```python
'''
@Description: 
    range(stop)
    range(start, stop[, step])
@Param: 
    start: 计数从 start 开始。默认是从 0 开始。例如range（5）等价于range（0， 5）;
    stop: 计数到 stop 结束，但不包括 stop。例如：range（0， 5） 是[0, 1, 2, 3, 4]没有5
    step：步长，默认为1。例如：range（0， 5） 等价于 range(0, 5, 1)   
@Return: 
    range() 函数返回的是一个可迭代对象（类型是对象），而不是列表类型， 所以打印的时候不会打印列表。
    list() 函数是对象迭代器，可以把range()返回的可迭代对象转为一个列表，返回的变量类型为列表。
'''
# range()
rangetmp = range(0,11,2)
print(rangetmp) # range(0, 11, 2)
print(list(rangetmp))   # [0, 2, 4, 6, 8, 10]
# range() End
```


## repr(object)
```python
'''
@Description: 
    repr() 函数将对象转化为供解释器读取的形式。
@Param:
    object -- 对象。
@Return: 
    返回一个对象的 string 格式。
'''
# repr(object)
s = 'leacoder'
print(s)    # leacoder
print(repr(s))  # 'leacoder'
dictmp = {'Author':'leacoder','Time':'2019/04/18 23:37:06'}
print(dictmp)   # {'Author': 'leacoder', 'Time': '2019/04/18 23:37:06'}
print(repr(dictmp))     # {'Author': 'leacoder', 'Time': '2019/04/18 23:37:06'}
# repr(object) End
```


## reversed(seq)
```python
'''
@Description: 
    reversed 函数返回一个反转的迭代器。
@Param: 
    seq -- 要转换的序列，可以是 tuple, string, list 或 range。
    seq必须是具有__reversed __（）方法的对象，或者支持序列协议（__len __（）方法和__getitem __（）方法，整数参数从0开始）。
@Return: 
    一个反转的迭代器。
'''
# reversed(seq)
seqstr = 'leacdoer'
print(list(reversed(seqstr)))   # ['r', 'e', 'o', 'd', 'c', 'a', 'e', 'l']
seqrange = range(5, 9)
print(list(reversed(seqrange))) # [8, 7, 6, 5
# reversed(seq) End
```

## round(number[, ndigits])
```python
'''
@Description: 
    round() 方法返回浮点数x的四舍五入值。
    对于支持round（）的内置类型，将值四舍五入为函数减去ndigits的最接近的10的倍数; 
    如果两个倍数相等，则向均匀选择进行舍入（例如，round（0.5）和round（-0.5）都是0，round（1.5）是2）。
    round（）对于浮点数的行为可能会令人惊讶：例如，round（2.675,2）给出2.67而不是预期的2.68。 这不是一个错误：这是因为大多数小数部分不能完全表示为浮点数。
    有关详细信息参见 ：https://docs.python.org/3.7/tutorial/floatingpoint.html#tut-fp-issues
@Param: 
    x -- 数字表达式。
    n -- 表示小数点位数，其中 x 需要四舍五入，默认值为 0。
@Return: 
    浮点数x的四舍五入值。
'''
# round(number[, ndigits])
print ("round(70.23456) : ", round(70.23456))   # round(70.23456) :  70
print ("round(0.5) : ", round(0.5)) # round(0.5) :  0
print ("round(-0.5) : ", round(-0.5))   # round(-0.5) :  0
print ("round(1.5) : ", round(1.5)) # round(1.5) :  2
print ("round(-1.5) : ", round(-1.5))   # round(-1.5) :  -2
print ("round(2.675, 2)  : ", round(2.675, 2) )     # round(2.675, 2)  :  2.67
print ("round(135.0, -1)  : ", round(135.0, -1) )   # round(135.0, -1)  :  140.0 
# round(number[, ndigits]) End
```

## class set([iterable])
```python
'''
@Description: 
    set() 函数创建一个无序不重复元素集，可进行关系测试，删除重复数据，还可以计算交集、差集、并集等。
@Param:
    iterable -- 可迭代对象对象；
@Return: 
    新的集合对象。
'''
# class set([iterable])
settmp = set('leacoder')
print(settmp)   # {'l', 'd', 'c', 'r', 'o', 'a', 'e'}
# class set([iterable]) End
```

## setattr(object, name, value)
```python
'''
@Description: 
    setattr() 函数对应函数 getattr()，用于设置属性值，该属性不一定是存在的。
@Param: 
    object -- 对象。
    name -- 字符串，对象属性。
    value -- 属性值。
@Return: 
'''
# setattr(object, name, value)
class G(object):
    bar = 'leacoder'
g = G()
setattr(g, 'bar',1234) 
print(g.bar)    # 1234
setattr(g, 'Author','leacoder') 
print(g.Author) # leacoder
# setattr(object, name, value) End
```


## slice()
```python
'''
@Description: 
    slice() 函数实现切片对象，主要用在切片操作函数里的参数传递。
    class slice(stop)
    class slice(start, stop[, step])
@Param: 
    start -- 起始位置
    stop -- 结束位置
    step -- 间距
    start和step参数默认为None。
@Return: 
    返回一个切片对象。
'''
# slice()
myslice = slice(None,9,2) 
print(myslice)  # slice(None, 9, 2)
arr = range(10)
print(list(arr))  # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(list(arr[myslice])) # [0, 2, 4, 6, 8]
# slice() End
```

## sorted(iterable, *, key=None, reverse=False)
```python
'''
@Description: 
    sorted() 函数对所有可迭代的对象进行排序操作。
    sort 与 sorted 区别：
    sort 是应用在 list 上的方法，sorted 可以对所有可迭代的对象进行排序操作。
    list 的 sort 方法返回的是对已经存在的列表进行操作，而内建函数 sorted 方法返回的是一个新的 list，而不是在原来的基础上进行的操作。
@Param: 
    terable -- 可迭代对象。
    key -- 主要是用来进行比较的元素，只有一个参数，具体的函数的参数就是取自于可迭代对象中，指定可迭代对象中的一个元素来进行排序。
    reverse -- 排序规则，reverse = True 降序 ， reverse = False 升序（默认）。
@Return:
    返回重新排序的列表。
'''
# sorted(iterable, *, key=None, reverse=False)
test = [3,5,2,1,4] 
sortedtest = sorted(test)   # [3, 5, 2, 1, 4]
print(test)
print(sortedtest)   # [1, 2, 3, 4, 5]
test.sort()
print(test) # [1, 2, 3, 4, 5]
example_list = [5, 0, 6, 1, 2, 7, 3, 4]
result_list = sorted(example_list, key = lambda x: -x)
print(result_list)  # [7, 6, 5, 4, 3, 2, 1, 0]
result_list = sorted(example_list, reverse=True)
print(result_list)  # [7, 6, 5, 4, 3, 2, 1, 0]
print(example_list)  # [5, 0, 6, 1, 2, 7, 3, 4]
# sorted(iterable, *, key=None, reverse=False) End
```

## @staticmethod
```python
'''
@Description: 
    将方法转换为静态方法。
@Param: 
@Return: 
'''
# @staticmethod
# 静态方法不会接收隐式的第一个参数。要声明静态方法，请使用此惯用法：
class H:
    @staticmethod
    def f():
        print('@staticmethod def f(arg1, arg2):')
        pass
# 以上实例声明了静态方法 f，类可以不用实例化就可以调用该方法 H.f()，当然也可以实例化后调用 H().f()。
H.f();       # @staticmethod def f(arg1, arg2):   # 静态方法无需实例化
cobj = H()
cobj.f()    # @staticmethod def f(arg1, arg2):    # 也可以实例化后调用
# @staticmethod End
```


## str()
```python
'''
@Description: 
    class str(object='')
    class str(object=b'', encoding='utf-8', errors='strict')
    返回对象的字符串版本。如果未提供object，则返回空字符串。否则，str（）的行为取决于是否给出编码或错误，
@Param: 
    object -- 对象。
    返回对象的字符串版本。如果未提供object，则返回空字符串。否则，str（）的行为取决于是否给出编码或错误
    1、如果既没有给出编码也没有给出错误，str（object）返回对象.__ str __（），它是对象的“非正式”或可打印良好的字符串表示。
    对于字符串对象，这是字符串本身。 如果object没有__str __（）方法，则str（）将返回 repr（object）。
    2、如果给出了编码或错误中的至少一个，则对象应该是类似字节的对象（例如，字节或字节数组）。
    在这种情况下，如果object是一个bytes（或bytearray）对象，则str（bytes，encoding，errors）等效于bytes.decode（encoding，errors）。
    3、将bytes对象传递给str（）而不使用encoding或errors参数属于第一种返回非正式字符串表示的情况
@Return:
'''
# str()
s = 'leacoder'
print(str(s))   # leacoder
s = b'leacoder'
print(str(s))   # b'leacoder'
print(str(s,encoding ='utf-8'))    # leacoder
# str() End
```


## sum(iterable[, start])
```python
'''
@Description: 
    sum() 方法对系列进行求和计算。
@Param: 
    iterable -- 可迭代对象，如：列表、元组、集合。
    start -- 指定相加的参数，如果没有设置这个值，默认为0。   
@Return: 
'''
# sum(iterable[, start])
print(sum([0,1,2])) # 3
print(sum([0,1,2],-1))  # 2 # 列表计算总和后再加 -1
# sum(iterable[, start]) End
```

## super([type[, object-or-type]])
```python
'''
@Description: 
    super() 函数是用于调用父类(超类)的一个方法。
    super 是用来解决多重继承问题的，直接用类名调用父类方法在使用单继承的时候没问题，但是如果使用多继承，会涉及到查找顺序（MRO）、重复调用（钻石继承）等种种问题。
    MRO 就是类的方法解析顺序表, 其实也就是继承父类方法时的顺序表。
    可以使用直接使用 super().xxx 代替 super(Class, self).xxx :
@Param: 
    type -- 类。
    object-or-type -- 类，一般是 self
@Return: 
'''
# super([type[, object-or-type]])
class Asuper:
     def add(self, x):
         y = x+1
         print(y)
class Bsuper(Asuper):
    def add(self, x):
        super().add(x)
b = Bsuper()
b.add(2)  # 3
# super([type[, object-or-type]]) End
```


## tuple([iterable])
```python
'''
@Description: 
    tuple 函数将列表转换为元组。
@Param: 
    seq -- 要转换为元组的序列。将字符串，列表，字典，集合转化为元组,将字典转换为元组时，只保留键！
@Return: 
    返回元组。
'''
# tuple([iterable])
list1= ['Google', 'Taobao', 'Baidu']
tuple1=tuple(list1)
print(tuple1)   # ('Google', 'Taobao', 'Baidu')
dict1 = {'Author':'leacoder','Time':'2019/04/18 23:37:06'}
tuple1 = tuple(dict1)
print(tuple1)   # ('Author', 'Time')
# tuple([iterable]) End
```

## type() 
```python
'''
@Description: 
    class type(object)
    class type(name, bases, dict)
    type() 函数如果你只有第一个参数则返回对象的类型，三个参数返回新的类型对象。

    isinstance() 与 type() 区别：
    type() 不会认为子类是一种父类类型，不考虑继承关系。
    isinstance() 会认为子类是一种父类类型，考虑继承关系。
    如果要判断两个类型是否相同推荐使用 isinstance()。
    参见 isinstance(object, classinfo)
@Param: 
    name -- 类的名称。
    bases -- 基类的元组。
    dict -- 字典，类内定义的命名空间变量。
@Return: 
    一个参数返回对象类型, 三个参数，返回新的类型对象。
'''
# type() 
# 一个参数实例
print(type(1))  # <class 'int'>
# 三个参数, 使用三个参数，返回一个新类型对象。这实际上是类申明的动态形式。
class X:
    a = 1
X = type('X', (object,), dict(a=1))
# 以上两个语句创建相同的类型对象
# type()  End
```

## vars([object])
```python
'''
@Description: 
    vars() 函数返回对象object的属性和属性值的字典对象。
    返回具有__dict__属性的模块，类，实例或任何其他对象的__dict__属性。
@Param: 
    object -- 对象
    没有参数，vars（）就像locals（）一样。locals字典仅对读取有用，因为忽略了对locals字典的更新。
@Return: 
'''
# vars([object])
print(vars())
class Xvars:
    a = 1
print(vars(Xvars))
# vars([object]) End
```


## zip(*iterables)
```python
'''
@Description: 
    创建一个迭代器，聚合每个迭代的元素。
    返回元组的迭代器，其中第i个元组包含来自每个参数序列或迭代的第i个元素。当最短输入可迭代用尽时，迭代器停止。
    zip（）与*运算符一起用于解压缩列表
@Param: 
    iterabl -- 一个或多个迭代器;
@Return: 
    一个对象。
'''
# zip(*iterables)
a = [1,2,3]
b = [4,5,6,7,8]
zipped = zip(a,b)     # 返回一个对象
print(zipped)   # <zip object at 0x7f6560237b88>
print(list(zipped)) # [(1, 4), (2, 5), (3, 6)]
a1, a2 = zip(*zip(a,b))  # 与 zip 相反，zip(*) 可理解为解压
print(a1)   # (1, 2, 3)
print(a2)   # (4, 5, 6)
print(type(a1)) # <class 'tuple'>
# zip(*iterables) End
```

## __import__(name, globals=None, locals=None, fromlist=(), level=0)
```python
'''
@Description: 
    __import__() 函数用于动态加载类和函数 。
    如果一个模块经常变化就可以使用 __import__() 来动态载入。
@Param: 
@Return: 
'''
# __import__(name, globals=None, locals=None, fromlist=(), level=0)

# __import__(name, globals=None, locals=None, fromlist=(), level=0) End
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
