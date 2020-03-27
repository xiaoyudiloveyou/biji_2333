# biji_2333
## [教程](https://www.jb51.net/books/536708.html#downintro2)
## 其他自学网
>[1.其他教程 ](https://6so.so/t/256285/)

## 在linux下

>1. 添加头文件：
> #！usr/bin/env python3
> # -*- coding: utf-8 -*-

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
> 1.
encode = 'ABC'.encode('ascii')  
print(encode)  
>2.  
b = '中文'.encode('utf-8')  
print(b)  
>3.  
x_ = b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')  
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
>1. 例
print('hello, %s' %'world')
print('Hi, %s, you have $%d.' % ('Michael', 1000000))
>2.
//2表示宽度,0表示不够则0占位
print('%2d-%02d' % (31234, 11234))
>3.
//.3表示精度
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
