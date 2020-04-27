# biji_2333
## [教程](https://www.jb51.net/books/536708.html#downintro2)
## 其他自学网
>[1.其他教程 ](https://6so.so/t/256285/)

## 在linux下

>1. 添加头文件：   
> `#！usr/bin/env python3`  
`# -*- coding: utf-8 -*-`

---

## API

### input：
>1. input输出为str，如果要输出数字，则可int()包裹
>2. input('这里可直接插入提示符')

### 关于字符转义：
>1. 屁眼允许 **r‘’** 来限制内容自动转义

### 关于bool
>1. not 单目运算符  
n = not True  
print(n)  
print(not 1 > 2)  
  
### 关于int和float数据的上限
>1. 都没有上线，float超过一定值，会直接用inf来表示无限

### decode用于字符编码的转换
>1.  
>>encode = 'ABC'.encode('ascii')  
print(encode)
>2.  
>>b = '中文'.encode('utf-8')  
print(b)
>3.    
>>x_ = b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')  
print(x_)  

### 用**len()**求中英文字符个数和字节个数,**encode()**和**decode()**
>1. 求字符个数  
i = len('ACC')<br>
print(i)
  
>2. 求字节个数  
len1 = len('ACC'.encode("ASCII"))  
print(len1)<br>
len2 = len('我的妈'.encode("utf-8"))  
print(len2)    

### 格式化字符串
>1. 
>>print('hello, %s' %'world')  
print('Hi, %s, you have $%d.' % ('Michael', 1000000))  
>2.
>>//表示宽度,0表示不够则0占位 
print('%2d-%02d' % (31234, 11234))  
>3.
>>//表示精度  
print('%.3f' % 3.1415926)

>2. 其他: 如不知用何表示，可完全用%s

### list
>1.
>>    tracy_ = ['Michael', 'Bob', 'Tracy']   
      print(tracy_)
>2.
>>  print(len(tracy_))
    可使用负数做索引
    list是一个可变的有序表，会在末尾追加
    list中的元素是可以不同的  
    list可以嵌套
>3.
>>    //删除list末尾的元素，用pop()  
      print(tracy_.pop())  
>4.
>>  //二维数组  
    p = ['asp', 'php']  
    s = ['python', 'java', p, 'scheme']  
    print(p[1])  
    print(s[2][1])  
    //为'p' 会按python的字符串顺序索引  
    print(s[0][1])

 ### tuple是长度不可变的list 
>1. 当tuple只想定义一个元素的时候：t(1,) 而不是t(1)
>2. 可变的tuple则是在t中包含可变的list,因为list可变
>>  t = ('a', 'b', ['A', 'B'])  
    t[2][0] = 'C'   
    print(t[2][0])  
    t[0][0] = 'Z'  这个会报错，不能改  
    print([0][0])  
  
### for循环: 
```
names = ['Michael', 'Bob', 'Tracy']
for name in names:
    print(name)
```

```
#range()函数  
print(list(range(100)))

sum = 0  
for x in range(101):  
    sum += x  
print(sum)
```

### while循环
```$xslt
#算出100以内奇数和
sum = 0
n = 99
while n > 0:
    sum += n
    n = n - 2
print(sum)
```

### dict
```$xslt
#dict
d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
print(d['Michael'])

#删除key
d.pop('Michael')
```

>1. 和list比较，dict有一下几个特点  
>    1. 查找和插入的速度极快，不会随着key的增加而增加
>    2. 需要占用大量的内存，内存浪费多。 
>2. 而list相反:
>    1. 查找和插入的时间随着元素的增加而增加
>    2. 占用空间小，浪费内存很少
>3. 总而言之，dict就是典型的空间换时间
>
>4. 注意: 可变的都不可作为"key",例如list


### 单集合set
```$xslt
s = set([1, 1, 2, 2, 3, 3])

s.add(4)
for s1 in s:
    print(s1)

s.remove(4)
for s2 in s:
    print(s2)
```

### 字符串replace
```$xslt
a = 'abc'
a = a.replace('a', 'A') 
print(a)
```


## 函数
### 函数简介，*一些基本API*
```python
# 绝对值函数 abs(n)、最大值函数max(n1,n2)、数据类型转换int() float() str() bool() 、 16进制转换hex() 、 

```

### 定义默认参数必须指向不变对象
```python
def add_end(L=[]):
    L.append('END')
    return L


print(add_end([1, 2, 3]))
print(add_end(['x', 'y', 'z']))
print(add_end())
print(add_end())


# 改：
def add_endd(L=None):
    if L is None:
        L = []
    L.append('END')
    return L


print(add_endd())
print(add_endd())

```
### 函数的参数
```python
import math

#返回多个参数
print(math.sqrt(2))

#可变参数：允许传入0个或任意个参数，在函数调用时自动组装为一个tuple，
def calc(numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum

print(calc([1,2,3]))


def cacl1(*numbers):
    sum = 0
    print("===")
    for n in numbers:
        sum = sum + n * n
    return sum

print(cacl1(1, 2, 3))

#如果已经有一个 list 或者 tuple，要调用一个可变参数怎么办？可以这样
#1. 你可以将list或tuple的元素变成可变参数传进去
nums = [1,2,3]
print(cacl1(nums[0], nums[1], nums[2]))
print(cacl1(*nums))

#关键字参数:允许传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict
def person(name,age,**kw):
    print("===")
    print('name',name,'age',age,'other',kw)

person('Michael',30)
person('Bob',35,city='Beijing')

extra = {'city': 'Beijing', 'job': 'Engineer'}
person('Jack', 24, city=extra['city'], job=extra['job'])
#简化
person('Jack',24,**extra)

#命名关键字参数：必须传入参数名，否则报错
def dog(name, age, **kw):
    if 'city' in kw:
        # 有ctiy参数
        pass
    if 'job' in kw:
        #有job参数
        pass
    print("===1")
    print('name:', name,'age:',age,'other:',kw)
dog('Jack',24,city='Beijing',addr='Chaoyang',zipcode=123456)

def dog1(name,age,*,city,job):
    print()
    print(name,age,city,job)


# 综上：共有必选参数、默认参数、可变参数：{关键字参数，命名关键字参数和关键字参数}

def f1(a,b,c=0,*args,**kw):
    print("f1===")
    print('a=',a,'b=',b,'c=',c,'args=',args,'kw=',kw)

def f2(a,b,c=0,*,d,**kw):
    print("f2===")
    print('a=',a,'b=',b,'c=',c,'d=',d,'kw=',kw)

f1(1,2)
f1(1,2,c=3)
f1(1,2,3,'a','b')
f1(1,2,3,'a','b',x=99)

f2(1,2,d=99,ext=None)

args=(1,2,3,4)
args1=(1,2,3)
kw={'d': 99, 'x': '#'}
kw1={'d': 88, 'x': '？'}
f1(*args, **kw)
f1(*args1,**kw1)

# 总结：
#     *args是可变参数，args接收的是一个tuple;
#     **kw是关键字参数,kw接收的是一个dict。
```
### 练习：
```python
# 练习：汉诺塔
def move(n, a, b, c):
    print("===")
    if n == 1:
        print("move", a, '-->', c)
    else:
        move(n - 1, a, c, b)
        move(1, a, b, c)
        move(n - 1, b, a, c)


move(3, 'A', 'B', 'C')
```

### 切片
取一个 list 或 tuple 的部分元素是非常常见的操作。比如，一个 list 如下：
```python
L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']
```
取前 3 个元素，应该怎么做？
笨办法：   
```
[L[0], L[1], L[2]]
['Michael', 'Sarah', 'Tracy']   
```
之所以是笨办法是因为扩展一下，取前 N 个元素就没辙了。
取前 N 个元素，也就是索引为 0-(N-1)的元素，可以用循环：
```
r = []
n = 3
>>> for i in range(n):
... r.append(L[i])
...
>>> r
: ['Michael', 'Sarah', 'Tracy']   
```
对这种经常取指定索引范围的操作，用循环十分繁琐，因此，Python 提
供了切片（Slice）操作符，能大大简化这种操作。

1 . 对应上面的问题，取前 3 个元素，用一行代码就可以完成切片：
```
>>> L[0:3]
['Michael', 'Sarah', 'Tracy']
```

2 . 取倒数 (倒数第一个元素的索引是-1。)
```python
L = ['Michael', 'Sarah', 'Tracy', 'Bob', 'Jack']

S = L[-3:]
print(S)
# ['Tracy', 'Bob', 'Jack']
```
3 . tuple 也是一种 list，唯一区别是 tuple 不可变。因此，tuple 也可以用切片操作，只是操作的结果仍是 tuple：
```
T = (0, 1, 2, 3, 4, 5)[:3]
print(T)
```

4 . 字符串'xxx'也可以看成是一种 list，每个元素就是一个字符。因此，字符串也可以用切片操作，只是操作结果仍是字符串：
```python
S = 'ABCDEFG'[:3]
print(S)
```

### 迭代
&emsp;如果给定一个 list 或 tuple，我们可以通过 for 循环来遍历这个 list 或
tuple，这种遍历我们称为迭代（Iteration）。

&emsp;无论对象是否有下标，只要是可迭代对象，便可遍历

```python
# 遍历键
d = {'a': 1, 'b': 2, 'c': 3}
for key in d:
    print(key)
# 遍历值
for value in d.values():
    print(value)
```
```python
for ch in 'ABC':
    print(ch)
```

1 . 如何判断一个对象是否可迭代，方法是collections模块的Iterable类型判断：
```python
from collections.abc import Iterable
# import collections.abc
# print(isinstance('abc',collections.abc.Iterable))
print(isinstance('abc',Iterable))
print(isinstance([1,2,3],Iterable))
print(isinstance(123,Iterable))

True
True
False

```
2 . 实现下表遍历
```python
# 对 list 实现类似 Java 那样的下标循环
for i, value in enumerate(['A','B','C']):
    print(i,value)

# 同时引用了两个变量，
for x, y in [(1,1), (2,4), (3,9)]:
    print(x, y)

```

3 .  列表生成式
```python
L = list(range(1,11))
print(L)

L1 = []
for x in range(1,11):
    L1.append(x * x)

print(L1)

# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
# [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
# 一行语句代替循环生成上面的

L3 = [x * x for x in range(1,11)]
print(L3)

# for 循环后面还可以加上 if 判断，这样我们就可以筛选出仅偶数的平方：
L4 = [x * x for x in range(1,11) if x % 2 == 0]
print(L4)

# 还可以使用两层循环，可以生成全排列：
L5 = [m + n for m in 'ABC' for n in 'XYZ']
print(L5)

# os.listdir 可以列出文件和目录
from os import listdir
L6 = [d for d in listdir('.')]
print(L6)

# for 循环其实可以同时使用两个甚至多个变量，比如 dict 的 items()可以同时迭代 key 和 value：
d = {'x': 'A', 'y': 'B', 'z': 'C' }
for k,v in d.items():
    print(k, '=', v)

# 列表生成式也可以使用两个变量来生成 list：
L7 = [k + '=' + v for k,v in d.items()]
print(L7)

# 最后把一个 list 中所有的字符串变成小写：
S = ['Hello', 'World', 'IBM', 'Apple']
L8 = [s.lower() for s in S]
print(L8)
```

#### 练习
```
如果 list 中既包含字符串，又包含整数，由于非字符串类型没有 lower()  
方法，所以列表生成式会报错：  
>>> L = ['Hello', 'World', 18, 'Apple', None]  
>>> [s.lower() for s in L]  
Traceback (most recent call last):  
File "<stdin>", line 1, in <module>  
File "<stdin>", line 1, in <listcomp>  
AttributeError: 'int' object has no attribute 'lower'  
使用内建的 isinstance 函数可以判断一个变量是不是字符串：  
```
```python
L =  ['Hello', 'World', 18, 'Apple', None]

S = [ s.lower() for s in L if(isinstance(s,str))]

print(S)
```

#### 小结
&emsp;运用列表生成式，可以快速生成list，可以通过一个list推导出另一个list，
而代码却十分简洁。

### 生成器
&emsp;&emsp;通过列表生成式，我们可以直接创建一个列表。但是，受到内存限制，
列表容量肯定是有限的。而且，创建一个包含 100 万个元素的列表，不
仅占用很大的存储空间，如果我们仅仅需要访问前面几个元素，那后面
绝大多数元素占用的空间都白白浪费了。
所以，如果列表元素可以按照某种算法推算出来，那我们是否可以在循
环的过程中不断推算出后续的元素呢？这样就不必创建完整的 list，从
而节省大量的空间。在 Python 中，这种一边循环一边计算的机制，称
为生成器：generator。   
&emsp;&emsp;要创建一个 generator，有很多种方法。第一种方法很简单，只要把一个
列表生成式的[]改成()，就创建了一个 generator：

```python
L = [x * x for x in range(10)]
print(L)
g = (x * x for x in range(10))
print(g)

print(next(g))
print(next(g))
```
    我们讲过，generator 保存的是算法，每次调用 next(g)，就计算出 g 的
    下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出
    StopIteration 的错误。
    generator 非常强大。如果推算的算法比较复杂，用类似列表生成式的 for
        循环无法实现的时候，还可以用函数来实现。
    比如，著名的斐波拉契数列（Fibonacci），除第一个和第二个数外，任
    意一个数都可由前两个数相加得到：
    1, 1, 2, 3, 5, 8, 13, 21, 34, ...
    斐波拉契数列用列表生成式写不出来，但是，用函数把它打印出来却很
    容易：
```python
def fib(max):
    n, a, b = 0,0,1
    while n < max:
        print(b)
        # yield b
        a, b = b, a+b
        n = n + 1
    return 'done'

fib(6)
```
```python
# 举个简单的例子，定义一个 generator，依次返回数字 1，3，5：
def odd():
    print('step 1')
    yield 1
    print('step 2')
    yield 3
    print('step 3')
    yield 5

o = odd()
print(next(o))
print(next(o))
print(next(o))
```

用异常捕获来获取generator的返回值:
```python
def fib(max):
    n, a, b = 0,0,1
    while n < max:
        # print(b)
        yield b
        a, b = b, a+b
        n = n + 1
    return 'done'

# for n in fib(6):
#     print(n)

g = fib(6)
while True:
    try:
        x = next(g)
        print('g:',x)
    except StopIteration as e:
        print('Generator return value:',e.value)
        break
```
### 迭代器
1 . 两类可迭代的数据类型：集合与generator

    我们已经知道，可以直接作用于 for 循环的数据类型有以下几种：
    一类是集合数据类型，如 list、tuple、dict、set、str 等；
    一类是 generator，包括生成器和带 yield 的 generator function。
    这些可以直接作用于 for 循环的对象统称为可迭代对象：Iterable。
    可以使用 isinstance()判断一个对象是否是 Iterable 对象：
```
>>> from collections import Iterable
>>> isinstance([], Iterable)
True
>>> isinstance({}, Iterable)
True
>>> isinstance('abc', Iterable)
True
>>> isinstance((x for x in range(10)), Iterable)
True
>>> isinstance(100, Iterable)
False
```
    而生成器不但可以作用于 for 循环，还可以被 next()函数不断调用并返
    回下一个值，直到最后抛出 StopIteration 错误表示无法继续返回下一个
    值了。
    可以被 next()函数调用并不断返回下一个值的对象称为迭代器：
    Iterator。
    可以使用 isinstance()判断一个对象是否是 Iterator 对象：
```
>>> from collections import Iterator
>>> isinstance((x for x in range(10)), Iterator)
True
>>> isinstance([], Iterator)
False
>>> isinstance({}, Iterator)
False
>>> isinstance('abc', Iterator)
False
```

    生成器都是 Iterator 对象，但 list、dict、str 虽然是 Iterable，却不是
    Iterator。
2 . 把 list、dict、str 等 Iterable 变成 Iterator 可以使用 iter()函数：
```
>>> isinstance(iter([]), Iterator)
True
>>> isinstance(iter('abc'), Iterator)
True
```

    你可能会问，为什么 list、dict、str 等数据类型不是 Iterator？
    Python3 基础教程【完整版】 http://www.yeayee.com/
    121/531
    这是因为 Python 的 Iterator 对象表示的是一个数据流，Iterator 对象可
    以被 next()函数调用并不断返回下一个数据，直到没有数据时抛出
    StopIteration 错误。可以把这个数据流看做是一个有序序列，但我们却
    不能提前知道序列的长度，只能不断通过 next()函数实现按需计算下一
    个数据，所以 Iterator 的计算是惰性的，只有在需要返回下一个数据时
    它才会计算。
    Iterator 甚至可以表示一个无限大的数据流，例如全体自然数。而使用
    list 是永远不可能存储全体自然数的。

3 . 小结:

    凡是可作用于 for 循环的对象都是 Iterable 类型；
    凡是可作用于 next()函数的对象都是 Iterator 类型，它们表示一个惰性
    计算的序列；
    集合数据类型如 list、dict、str 等是 Iterable 但不是 Iterator，不过可
    以通过 iter()函数获得一个 Iterator 对象。

示例地址：
[do_iter.py](https://github.com/michaelliao/learn-python3/blob/master/samples/advance/do_iter.py)

### 函数式编程
    1. 函数就是面向过程的程序设计的基本单元。
    
    2. 函数式编程就是一种抽象程度很高的编程范式，纯粹的函数式编程语言
    编写的函数没有变量，因此，任意一个函数，只要输入是确定的，输出
    就是确定的，这种纯函数我们称之为没有副作用。而允许使用变量的程
    序设计语言，由于函数内部的变量状态不确定，同样的输入，可能得到
    不同的输出，因此，这种函数是有副作用的。
    函数式编程的一个特点就是，允许把函数本身作为参数传入另一个函
    数，还允许返回一个函数！
    
    3. Python 对函数式编程提供部分支持。由于 Python 允许使用变量，因此，Python 不是纯函数式编程语言。


### 高阶函数
#### 变量可以指向函数
```python
abs(-10)
# >>> 10
f = abs
f(-10)
# >>> 10
```
    变量 f 现在已经指向了 abs 函数本身。直接调用 abs()函数和调用变量 f()完全相同。

#### 函数名也是变量

    那么函数名是什么呢？函数名其实就是指向函数的变量！对于 abs()这
    个函数，完全可以把函数名 abs 看成变量，它指向一个可以计算绝对值
    的函数！
    如果把 abs 指向其他对象，会有什么情况发生？
```
>>> abs = 10
>>> abs(-10)
Traceback (most recent call last):
 File "<stdin>", line 1, in <module>
TypeError: 'int' object is not callable
```
#### 传入函数

&emsp;&emsp;既然变量可以指向函数，函数的参数能接收变量，那么一个函数就可以接收另一个函数作为参数，这种函数就称之为高阶函数。
```python
def add(x, y, f):
    return f(x) + f(y)

print(add(1,2,abs))
```
小结:

    把函数作为参数传入，这样的函数称为高阶函数，函数式编程就是指这种高度抽象的编程范式。

### map/reduce
Python 内建了 map()和 reduce()函数。

#### map
&emsp;&emsp;map()函数接收两个参数，一个是函数，一个是 Iterable，
map 将传入的函数依次作用到序列的每个元素，并把结果作为新的
Iterator 返回。

```python
def f(x):
    return x * x

r = map(f,[1,2,3,4,5])
l = list(r)
print(l)

# 你可能会想，不需要 map()函数，写一个循环，也可以计算出结果：

L = []
for n in [1, 2, 3, 4, 5]:
    L.append(f(n))
print(L)
```
&emsp;&emsp;的确可以，但是，从上面的循环代码，能一眼看明白“把 f(x)作用在 list
的每一个元素并把结果生成一个新的 list”吗？
所以，map()作为高阶函数，事实上它把运算规则抽象了，因此，我们不
但可以计算简单的 f(x)=x2，还可以计算任意复杂的函数，比如，把这个
list 所有数字转为字符串：

```python
i = map(str, [1, 2, 3, 4, 5])
str = list(i)
print(str)
```

#### reduce
reduce 把结果继续和序列的下一个元素做累积计算，其效果就是：  
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)

对一个序列求和，就可以用 reduce 实现：
```python
from functools import reduce

def add(x, y):
    return x + y

l = reduce(add, [1, 3, 5, 7, 9])
print(l)
```
&emsp;&emsp;当然求和运算可以直接用 Python 内建函数 sum()，没必要动用 reduce。
但是如果要把序列[1, 3, 5, 7, 9]变换成整数 13579，reduce 就可以派上
用场：

```python
from functools import reduce

def fn(x, y):
    return x * 10 + y

l = reduce(fn, [1, 3, 5, 7, 9])
print(l)

```

&emsp;&emsp;这个例子本身没多大用处，但是，如果考虑到字符串 str 也是一个序列，
对上面的例子稍加改动，配合 map()，我们就可以写出把 str 转换为 int
的函数：
```python
from functools import reduce

def fn(x, y):
    return x * 10 + y

def char2num(s):
    return {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6,'7': 7, '8': 8, '9': 9}[s]

l = reduce(fn,map(char2num, '13579'))
print(l)

#整理

def str2int(s):
    def fn(x, y):
        return x * 10 + y
    def char2num(s):
        return {'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6,'7': 7, '8': 8, '9': 9}[s]
    return reduce(fn, map(char2num, s))

# 还可以用 lambda 函数进一步简化成：

def str2int_2(s):
    return reduce(lambda x, y: x * 10 + y, map(char2num, s))
```

#### 练习:
1 . 利用 map()函数，把用户输入的不规范的英文名字，变为首字母大写，
其他小写的规范名字。输入：['adam', 'LISA', 'barT']，输出：['Adam',
'Lisa', 'Bart']：

```python
from functools import reduce

str_list = ['adam', 'LISA', 'barT23']

#方法一：

def normalize(name):

    return list(map(fn, str_list))

def fn(S):
    def fn(x, y):
        return x + y
    S = S[0].upper() + reduce(fn,[ s.lower() for s in S[1:] if(isinstance(S,str))])
    # print(S)
    return S

print(normalize(str_list))

# 方法二：
def normalize(name):
    result = name[0].upper() + name[1:].lower()
    return result

L1 = ["adam", "LISA", "barT"]
L2 = list(map(normalize, L1))

print(L2)

```

2 . Python 提供的 sum()函数可以接受一个 list 并求和，请编写一个 prod()
   函数，可以接受一个 list 并利用 reduce()求积：

```python
from functools import reduce

def prod(L):
    def fn(x, y):
        return x * y

    N = reduce(fn, L)
    return N

print('3 * 5 * 7 * 9 =', prod([3, 5, 7, 9]))

```

3 . 利用 map 和 reduce 编写一个 str2float 函数，把字符串'123.456'转换成
    浮点数 123.456：

```python
from functools import reduce

CHAR_TO_FLOAT = {
    '0': 0,
    '1': 1,
    '2': 2,
    '3': 3,
    '4': 4,
    '5': 5,
    '6': 6,
    '7': 7,
    '8': 8,
    '9': 9,
    '.': -1
}

def str2float(s):
    nums = map(lambda ch: CHAR_TO_FLOAT[ch], s)
    # print(list(nums))
    point = 0
    def to_float(f, n):
        print(f,n)
        nonlocal point
        if n == -1:
            point = 1
            return f
        if point == 0:
            return f * 10 + n
        else:
            point = point * 10
            return f + n / point
    return reduce(to_float, nums, 0.0)

print(str2float('123.23'))
```

### filter

Python 内建的 filter()函数用于过滤序列。

    和 map()类似，filter()也接收一个函数和一个序列。和 map()不同的时，
    filter()把传入的函数依次作用于每个元素，然后根据返回值是 True 还
    是 False 决定保留还是丢弃该元素。

例如，在一个 list 中，删掉偶数，只保留奇数，可以这么写：
```python
def is_odd(n):
    return n % 2 == 1

l = list(filter(is_odd, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
print(l)

```

把一个序列中的空字符串删掉，可以这么写：   
[逻辑运算符and和or到底该怎么用](https://baijiahao.baidu.com/s?id=1645995216613969947&wfr=spider&for=pc)
```python
def not_empty(s):
    return s and s.strip()
l = list(filter(not_empty, ['A', '', 'B', None, 'C', ' ']))
print(l)

```
&emsp;&emsp;注意到 filter()函数返回的是一个 Iterator，也就是一个惰性序列，所
以要强迫 filter()完成计算结果，需要用 list()函数获得所有结果并返
回 list。

#### 练习1. 
埃氏筛选法 
#### 练习2. 
回数是指从左向右读和从右向左读都是一样的数，例如 12321，909。请利用 filter()滤掉非回数：

1 . 方法一：
```python
#埃氏筛法计算素数
#生成一个奇数序列
def _odd_iter():
    n = 1
    while True:
        n = n + 2
        yield n

#筛选函数，保留不能整除的，筛掉可以整除的，即倍数。
def _not_divisible(n):
    return lambda x: x % n > 0

#求素数函数。
def primes():
    yield 2
    it = _odd_iter() #初始序列
    while True:
        n = next(it) #3,5,7,9,11
        yield n
        it = filter(_not_divisible(n), it) #it在不断的next调用中，进行筛选。

# 由于 primes()也是一个无限序列，所以调用时需要设置一个退出循环的条件：
#输出1000以内的所有素数：
for n in primes():
    if n < 1000:
        print(n)
    else:
        break
```

利用 filter()滤掉非回数：
```python
def is_palindrome(n):

    return str(n) == str(n)[::-1]

output = filter(is_palindrome, range(1, 1000))
print(list(output))

```

#### 小结
&emsp;&emsp;filter()的作用是从一个序列中筛出符合条件的元素。由于 filter()使
用了惰性计算，所以只有在取 filter()结果的时候，才会真正筛选并每
次返回下一个筛出的元素。

### sorted

#### 排序算法：
&emsp;&emsp;排序也是在程序中经常用到的算法。无论使用冒泡排序还是快速排序，
排序的核心是比较两个元素的大小。如果是数字，我们可以直接比较，
但如果是字符串或者两个 dict 呢？直接比较数学上的大小是没有意义
的，因此，比较的过程必须通过函数抽象出来。通常规定，对于两个元
素 x 和 y，如果认为 x < y，则返回-1，如果认为 x == y，则返回 0，如
果认为 x > y，则返回 1，这样，排序算法就不用关心具体的比较过程，
而是根据比较结果直接排序。

1 . Python 内置的 sorted()函数就可以对 list 进行排序：
```python
sorted([36, 5, -12, 9, -21])
```

2 . sorted()函数也是一个高阶函数，它还可以接收一个 key 函数来
    实现自定义的排序，例如按绝对值大小排序：
```python
sorted([36, 5, -12, 9, -21], key=abs)
```

        key 指定的函数将作用于 list 的每一个元素上，并根据 key 函数返回的
    结果进行排序。对比原始的 list 和经过 key=abs 处理过的 list：
    list = [36, 5, -12, 9, -21]
    keys = [36, 5, 12, 9, 21]
        然后 sorted()函数按照 keys 进行排序，并按照对应关系返回 list 相应的
    元素：
        keys 排序结果 => [5, 9, 12, 21, 36]
         | | | | |
        最终结果 => [5, 9, -12, -21, 36]

&emsp;&emsp;默认情况下，对字符串排序，是按照 ASCII 的大小比较的，由于'Z' < 'a'，
结果，大写字母 Z 会排在小写字母 a 的前面。

3 . 这样，我们给 sorted 传入 key 函数，即可实现忽略大小写的排序：
```python
sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower)
    # ['about', 'bob', 'Credit', 'Zoo']
```
要进行反向排序，不必改动key函数，可以传入第三个参数reverse=True：
```python
sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower,reverse=True)
    # ['Zoo', 'Credit', 'bob', 'about']
```


```python
sorted(['bob', 'about', 'Zoo', 'Credit'])
['Credit', 'Zoo', 'about', 'bob']
```
&emsp;&emsp;给 sorted 传入 key 函数，即可实现忽略大小写的排序：

#### 小结:
&emsp;&emsp;sorted()也是一个高阶函数。用 sorted()排序的关键在于实现一个映射
函数。

#### 练习:
&emsp;&emsp;假设我们用一组 tuple 表示学生名字和成绩：
L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]
请用 sorted()对上述列表分别按名字排序：

```python
from operator import itemgetter
students  = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]
L2 = sorted(students, key=itemgetter(1), reverse=True)
print(L2)

```

eg:　　itemgetter()
```python
from operator import itemgetter

rows = [
    {'fname': 'Brian', 'lname': 'Jones', 'uid': 1003},
    {'fname': 'David', 'lname': 'Beazley', 'uid': 1002},
    {'fname': 'John', 'lname': 'Cleese', 'uid': 1001},
    {'fname': 'Big', 'lname': 'Jones', 'uid': 1004}
]

rows_by_fname = sorted(rows, key=itemgetter('fname'))
print(rows_by_fname)
```

### 返回函数

#### 函数作为返回值

&emsp;&emsp;如果不需要立刻求和，而是在后面的代码中，根据需要再计算怎
么办？可以不返回求和的结果，而是返回求和的函数  
&emsp;&emsp;相关参数和变量都保存在返回的函数中，这种称为“闭包（Closure）”的程序结构拥有极大的威力。  
&emsp;&emsp;注意一点，当我们调用 lazy_sum()时，每次调用都会返回一个新的
函数，即使传入相同的参数：
>f1 = lazy_sum(1, 3, 5, 7, 9)  
f2 = lazy_sum(1, 3, 5, 7, 9)  
 f1==f2  
False  

#### 闭包

&emsp;&emsp;返回闭包时牢记的一点就是：返回函数不要引用任何循环变量，或者后
续会发生变化的变量。  
eg: 结果全是9，而不是 1 ，4 ，9的原因
```python
def count():
    fs = []
    for i in range(1,4):
        def f():
            return i*i
        fs.append(f)
    return fs

f1,f2,f3 = count()

print(f1())
print(f2())
print(f3())
```
如果一定要引用循环变量怎么办？  
&emsp;&emsp;方法是再创建一个函数，用该函数的
参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到
函数参数的值不变：
```python
def count():
    def f(j):
        def g():       #return lambda j=j :  j * j
            return j*j
        return g
    fs = []
    for i in range(1,4):

        fs.append(f(i))# f(i)立刻被执行，因此 i 的当前值被传入 f()
    return fs

f1,f2,f3 = count()

print(f1())
print(f2())
print(f3())
```

### 匿名函数

eg:
```python
L = list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
```
eg: 匿名函数也是一个函数对象，也可以把匿名函数赋值给一个变量
```python
f = lambda  x: x * x
```

eg: 同样，也可以把匿名函数作为返回值返回

```python
def buid(x, y):
    return lambda : x * x + y * y
```

#### 小结
&emsp;&emsp;Python 对匿名函数的支持有限，只有一些简单的情况下可以使用匿名函数。

### 装饰器
&emsp;&emsp;增强 now()函数的功能，比如，在函数调用前后自动
            打印日志，但又不希望修改 now()函数的定义，这种在代码运行期间动
            态增加功能的方式，称之为“装饰器”（Decorator）。

```python
def log(func):
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper

@log    # now = log(now)
def now():
    print('2015-3-25')

now()

def log1(text):
    def decorator(func):
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator

@log1('execute') # now = log('execute')(now)

def now1():
    print('2015-3-25')

now1()

print(now1.__name__) # wrapper
```

&emsp;&emsp;返回的那个 wrapper()函数名字就是'wrapper'，所以，需要把原始函
            数的__name__等属性复制到 wrapper()函数中，否则，有些依赖函数签名
            的代码执行就会出错。   
&emsp;&emsp; wrapper()的前面加上@functools.wraps(func)即可         
```python
import functools

def log2(func):
    @functools.wraps(func) # wrapper.__name__ = func.__name__
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper

#或者针对带参数的 decorator：

def log3(text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print('%s %s():' % (text, func.__name__))
            return func(*args, **kw)
        return wrapper
    return decorator

```

#### 小结
&emsp;&emsp;在面向对象（OOP）的设计模式中，decorator 被称为装饰模式。OOP
的装饰模式需要通过继承和组合来实现，而 Python 除了能支持 OOP 的
decorator 外，直接从语法层次支持 decorator。Python 的 decorator 可以
用函数实现，也可以用类实现。

&emsp;&emsp;decorator 可以增强函数的功能，定义起来虽然有点复杂，但使用起来非
常灵活和方便。

#### 练习：
    请编写一个 decorator，能在函数调用的前后打印出'begin call'和'end
    call'的日志。
    再思考一下能否写出一个@log 的 decorator，使它既支持：
    @log
    def f():
     pass
    又支持：
    @log('execute')
    def f():
     pass

```python
import functools

def log(*text):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kw):
            print(func.__name__, ' begin ', *text)
            func(*args, **kw)
            print(func.__name__, ' end ', *text)

        return wrapper
    return decorator

@log('execute') #or@log()
def foo():
    print('foo is running... ')

if __name__ == '__main__':
    f=foo
    f()
```

### 偏函数
```python
i = int('12345', base=8)
#或 int('12345', 8)
print(i)

def int2(x, base=2):
    return int(x, base)

print(int2('1000000'))
```
functools.partial 就是帮助我们创建一个偏函数
```python
from functools import partial

p = partial(int, base=2)
print(p('1000000'))
```
&emsp;&emsp;简单总结 functools.partial 的作用就是，把一个函数的某些参数
给固定住（也就是设置默认值），返回一个新的函数，调用这个新函数
会更简单。

    int2 = functools.partial(int, base=2)
    实际上固定了 int()函数的关键字参数 base，也就是：
    int2('10010')
    相当于：
    kw = { 'base': 2 }
    int('10010', **kw)
    
    当传入：
    max2 = functools.partial(max, 10)
    实际上会把 10 作为*args 的一部分自动加到左边，也就是：
    max2(5, 6, 7)
    相当于：
    args = (10, 5, 6, 7)
    max(*args)
    结果为 10。

#### 小结
&emsp;&emsp;当函数的参数个数太多，需要简化时，使用 functools.partial 可以创建
一个新的函数，这个新函数可以固定住原函数的部分参数，从而在调用
时更简单。















