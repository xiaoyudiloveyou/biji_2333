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
