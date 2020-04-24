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
