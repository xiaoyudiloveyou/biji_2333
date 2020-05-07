### IO 编程

```python
try:
    f = open('zg.md', 'r') # r 表示只读
    print(f.read())
finally:
    if f:
        f.close()

# with语句简化
with open('zg.md', 'r') as f:
    print(f.read())

with open('zg.md', 'r') as f:
    for line in f.readlines():
        print(line.strip())  # 把末尾的'\n'删掉
```

#### file-like Object

    像 open()函数返回的这种有个 read()方法的对象，在 Python 中统称为
    file-like Object。除了 file 外，还可以是内存的字节流，网络流，自定义
    流等等。file-like Object 不要求从特定类继承，只要写个 read()方法就行。
    StringIO 就是在内存中创建的 file-like Object，常用作临时缓冲。

#### 二进制文件

    前面讲的默认都是读取文本文件，并且是 UTF-8 编码的文本文件。要读
    取二进制文件，比如图片、视频等等，用'rb'模式打开文件即可：

```python
with open('test.PNG', 'rb') as f:
    for line in f.readlines():
        print(line.strip())
```

#### 字符编码

读取 GBK 编码的文件：

```python
f = open('/Users/michael/gbk.txt', 'r', encoding='gbk')
f.read()

```

    遇到有些编码不规范的文件，你可能会遇到 UnicodeDecodeError，因为在
    文本文件中可能夹杂了一些非法编码的字符。遇到这种情况，open()函
    数还接收一个 errors 参数，表示如果遇到编码错误后如何处理。最简单
    的方式是直接忽略：

```python
f = open('/Users/michael/gbk.txt', 'r', encoding='gbk', errors='ignore')
```

#### 写文件

    操作系统往往不会立刻把数据写入磁盘，而是
    放到内存缓存起来，空闲的时候再慢慢写入。只有调用 close()方法时，
    操作系统才保证把没有写入的数据全部写入磁盘。忘记调用 close()的
    后果是数据可能只写了一部分到磁盘，剩下的丢失了。所以，还是用 with
        语句来得保险：

```python
with open('zg.md', 'w') as f: # 重写
    f.write('hello world!')
```

#### 小结
 在 Python 中，文件读写是通过 open()函数打开的文件对象完成的。使用
 with 语句操作文件 IO 是个好习惯。

### StringIO 和 BytesIO

    StringIO
    很多时候，数据读写不一定是文件，也可以在内存中读写。
    StringIO 顾名思义就是在内存中读写 str。

1 . 要把 str 写入 StringIO，我们需要先创建一个 StringIO，然后，像文件一
样写入即可：

```python
from io import StringIO

f = StringIO()
f.write('hello')
f.write(' ')
f.write('world!')

print(f.getvalue()) # getvalue()方法用于获得写入后的 str。

f = StringIO('Hello!\nHi!\nGoodbye!')
while True:
    s = f.readline()
    if s == '':
        break
    print(s.strip())

```

#### BytesIO

    StringIO操作的只能是str，如果要操作二进制数据，就需要使用BytesIO。
    BytesIO 实现了在内存中读写 bytes，我们创建一个 BytesIO，然后写入
    一些 bytes：

```python
from io import BytesIO

f = BytesIO()
f.write('中文'.encode('utf-8'))
print(f.getvalue())

f = BytesIO(b'\xe4\xb8\xad\xe6\x96\x87')
print(f.read())
```

#### 小结
     StringIO 和 BytesIO 是在内存中操作 str 和 bytes 的方法，使得和读写文
     件具有一致的接口。

### 操作文件和目录

```python
import os

print(os.name)

# 如果是 posix，说明系统是 Linux、Unix 或 Mac OS X，如果是 nt，就是 Windows系统。

os.uname()

# uname()函数在 Windows 上不提供，也就是说，os 模块的某些函数是跟操作系统相关的。
```

#### 环境变量

```python
import os

print(os.environ)
print(os.environ.get('PATH'))
print(os.environ.get('x', 'default')) # 要获取某个环境变量的值，可以调用 os.environ.get('key')：
```

#### 操作文件和目录

```python
import os

print(os.path.abspath('.')) # 查看当前目录的绝对路径:

path = os.path.join('D:\workspace_idea_python\demo1', 'testdir')

# 在某个目录下创建一个新目录，首先把新目录的完整路径表示出来
# 然后创建一个目录:
os.mkdir(path)

# 删掉一个目录
os.rmdir(os.path.join(path))

# 拆分路径时，也不要直接去拆字符串，而要通过os.path.split()函数
# str = os.path.split(path)
for i in os.path.split(path):
    print(i)

# 得到文件扩展名
print(os.path.splitext(os.path.join(path, 'zg.md')))

# 对文件重命名:
os.rename('test.md', 'zg.md')
# 删掉文件:
os.remove('test.py')
```

     shutil 模块提供了 copyfile()的函数，你还可以在 shutil 模块
    中找到很多实用函数，它们可以看做是 os 模块的补充。

```python
import os

# 利用 Python 的特性来过滤文件。比如我们要列出当前目录下的所有目录，只需要一行代码：
[print(x) for x in os.listdir('.') if os.path.isdir(x)]

# 要列出所有的.py 文件，也只需一行代码：\
[print(x) for x in os.listdir('.') if os.path.isdir(x) and os.path.splitext(x)[1] == '.py']
```

#### 小结
     Python 的 os 模块封装了操作系统的目录和文件操作，要注意这些函数
     有的在 os 模块中，有的在 os.path 模块中。

### 序列化
    我们把变量从内存中变成可存储或传输的过程称之为序列化  
    在 Python中叫 pickling，在其他语言中也被称之为 serialization，marshalling，
    flattening 等等，都是一个意思。
    序列化之后，就可以把序列化后的内容写入磁盘，或者通过网络传输到
    别的机器上。
    反过来，把变量内容从序列化的对象重新读到内存里称之为反序列化，
    即 unpickling。
    Python 提供了 pickle 模块来实现序列化。

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

import pickle
d = dict(name='Bob', age=20, score=88)
a=pickle.dumps(d,2)#序列化二进制
print(a)

# 方法1
f=open("dump.txt","wb")
f.write(a)
f.close()
# 方法2
f = open('dump.txt', 'wb')
pickle.dump(d, f)
f.close()

with open('dump.txt', 'wb') as f:
    pickle.dump(d, f)

```

反序列化刚才保存的对象
```python
import pickle

f = open('dump.txt', 'rb')
d = pickle.load(f)
f.close()
print(d)

```

    Pickle 的问题和所有其他编程语言特有的序列化问题一样，就是它只能
    用于 Python，并且可能不同版本的 Python 彼此都不兼容，因此，只能
    用 Pickle 保存那些不重要的数据，不能成功地反序列化也没关系。

#### JSON

    如果我们要在不同的编程语言之间传递对象，就必须把对象序列化为标
    准格式，比如 XML，但更好的方法是序列化为 JSON，因为 JSON 表示
    出来就是一个字符串，可以被所有语言读取，也可以方便地存储到磁盘
    Python3 基础教程【完整版】 http://www.yeayee.com/
    260/531
    或者通过网络传输。JSON 不仅是标准格式，并且比 XML 更快，而且
    可以直接在 Web 页面中读取，非常方便。

    JSON 表示的对象就是标准的 JavaScript 语言的对象
    
    内置的数据类型对应如下：
    JSON 类型 Python 类型
    {} dict
    [] list
    "string" str
    1234.56 int 或 float
    true/false True/False
    null None

```python
import json

d =  dict(name='Bob', age=20, score=88)
dumps = json.dumps(d)
print(dumps)

```
dumps()方法返回一个 str，内容就是标准的 JSON。  
dump()方法可以直接把 JSON 写入一个 file-like Object。

    JSON 反序列化为 Python 对象，用 loads()或者对应的 load()方法，
    前者把 JSON 的字符串反序列化，后者从 file-like Object 中读取字符
    串并反序列化：
    
```python
import json

json_str = '{"age": 20, "score": 88, "name": "Bob"}'
loads = json.loads(json_str)
print(isinstance(loads, dict))

# 由于 JSON 标准规定 JSON 编码是 UTF-8，所以我们总是能正确地在
# Python 的 str 与 JSON 的字符串之间转换。
```
[json.dump(), json.dumps()与json.load(), json.loads()区别](https://blog.csdn.net/wf592523813/article/details/95002133)

#### JSON 进阶

```python
import json

class Student(object):
    def __init__(self, name, age, score):
        self.name = name
        self.age = age
        self.score = score

s = Student('Bob', 20, 88)
# print(json.dumps(s))    # TypeError: Object of type Student is not JSON serializable

def student2dict(std):
    return {
        'name': std.name,
        'age': std.age,
        'score': std.score
    }


# 把任意 class 的实例变为 dict：
print(json.dumps(s, default=lambda obj: obj.__dict__))

# 反序列化
def dict2student(d):
    return Student(d['name'], d['age'], d['score'])

json_str = '{"age": 20, "score": 88, "name": "Bob"}'
print(json.loads(json_str, object_hook=dict2student)) # <__main__.Student object at 0x000001AB021EF340>
# <__main__.Student object at 0x000001AB021EF340>

```

#### 小结
     Python语言特定的序列化模块是pickle，但如果要把序列化搞得更通用、
     更符合 Web 标准，就可以使用 json 模块。

    json 模块的 dumps()和 loads()函数是定义得非常好的接口的典范。当我
    们使用时，只需要传入一个必须的参数。但是，当默认的序列化或反序
    列机制不满足我们的要求时，我们又可以传入更多的参数来定制序列化
    或反序列化的规则，既做到了接口简单易用，又做到了充分的扩展性和
    灵活性。




