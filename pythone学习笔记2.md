### 模块
&emsp;&emsp;一个.py 文件就称之为一个模块（Module）,使用模块还可以避免函数名和变量名冲突

&emsp;&emsp;假设我们的 abc 和 xyz 这两个模块名字与其他模块冲突了，于是
我们可以通过包来组织模块，避免冲突。方法是选择一个顶层包名;abc.py 模块的名字就变成了 mycompany.abc，类似的，xyz.py
的模块名变成了 mycompany.xyz。

### 使用模块

    sys 模块有一个 argv 变量，用 list 存储了命令行的所有参数。argv 至少
    有一个元素，因为第一个参数永远是该.py 文件的名称，例如：
    运行 python3 hello.py 获得的 sys.argv 就是['hello.py']；
    运行 python3 hello.py Michael 获得的 sys.argv 就是['hello.py',
    'Michael]。
    
```
> chcp 1252
Active code page: 1252

如果启动 Python 交互环境，再导入 hello 模块：
$ python3
Python 3.4.3 (v3.4.3:9b73f1c3e601, Feb 23 2015, 02:52:03)
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import hello
>>>
导入时，没有打印 Hello, word!，因为没有执行 test()函数。
调用 hello.test()时，才能打印出 Hello, word!：
>>> hello.test()
Hello, world!

```

#### 作用域

&emsp;&emsp;在一个模块中，我们可能会定义很多函数和变量，但有的函数和变量我
们希望给别人使用，有的函数和变量我们希望仅仅在模块内部使用。在
Python 中，是通过_前缀来实现的。
&emsp;&emsp;private 函数和变量“不应该”被直接引用，而不是“不能”
被直接引用，是因为 Python 并没有一种方法可以完全限制访问 private
函数或变量
```python
def __private_1(name):
    return 'Hello, %s' % name

def __private_2(name):
    return  'Hi, %s' % name

def greeting(name):
    if len(name) > 3:
        return __private_1(name)
    else:
        return __private_2(name)

if __name__ == '__main__':
    f = greeting('Sd')
    print(f)
```

#### 安装第三方模块

在 Python 中，安装第三方模块，是通过包管理工具 pip 完成的。
```
https://pypi.org/project/Pillow/#files
pip install Pillow
```
```python
from PIL import Image

im = Image.open('test.png')
print(im.format, im.size, im.mode)
#PNG (270, 352) RGBA
im.thumbnail((200, 200)) # 缩略图
im.save('thumb.png', 'PNG')
```

    其他常用的第三方库还有:   
        MySQL 的驱动：mysql-connector-python，  
        用于科学计算的 NumPy 库：numpy，  
        用于生成文本的模板工具 Jinja2，等等。


#### 模块搜索路径

默认情况下，Python 解释器会搜索当前目录、所有已安装的内置模块和
第三方模块，搜索路径存放在 sys 模块的 path 变量中：

```
>>> import sys
>>> sys.path

```
如果我们要添加自己的搜索目录，有两种方法：

一是直接修改 sys.path，添加要搜索的目录：

```
>>> import sys
>>> sys.path.append('/Users/michael/my_py_scripts')

```
这种方法是在运行时修改，运行结束后失效。 

第二种方法是设置环境变量 PYTHONPATH，该环境变量的内容会被自动添
加到模块搜索路径中。

### 面向对象编程

&emsp;&emsp;OOP 把对象作为程序的基本单元，一个对象包含了数据和
操作数据的函数。

```python
class Student(object):
    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_score(self):
        print('%s: %s' % (self.name, self.score))

bart = Student('Bart Simpson', 59)
lisa = Student('Lisa Simpson', 87)
bart.print_score()
lisa.print_score()
```

&emsp;&emsp;面向对象的设计思想是从自然界中来的，因为在自然界中，类（Class）
和实例（Instance）的概念是很自然的。Class 是一种抽象概念，比如我
们定义的 Class——Student，是指学生这个概念，而实例（Instance）则
是一个个具体的 Student，比如，Bart Simpson 和 Lisa Simpson 是两个具
体的 Student。  
&emsp;&emsp;所以，面向对象的设计思想是抽象出 Class，根据 Class 创建 Instance。
面向对象的抽象程度又比函数要高，因为一个 Class 既包含数据，又包
含操作数据的方法。

#### 小结
   数据封装、继承和多态是面向对象的三大特点

        和普通的函数相比，在类中定义的函数只有一点不同，就是第一个参数
    永远是实例变量 self，并且，调用时，不用传递该参数。除此之外，类
    的方法和普通函数没有什么区别，所以，你仍然可以用默认参数、可变
    参数、关键字参数和命名关键字参数。

#### 数据封装

```python
class Student(object):
    def __init__(self, name, score):
        self.name = name
        self.score = score
    def print_score(self):
        print('%s: %s' % (self.name, self.score))
```

和静态语言不同，Python 允许对实例变量绑定任何数据

```
>>> bart = Student('Bart Simpson', 59)
>>> lisa = Student('Lisa Simpson', 87)
>>> bart.age = 8
>>> bart.age
8
>>> lisa.age
```

### 访问限制

```python
class Student(object):
    def __init__(self, name, score):
        self.__name = name
        self.__score = score

    def print_score(self):
        print('%s: %s' % (self.__name, self.__score))

if __name__ == '__main__':
    bart = Student('Bart Simpson', 98)
    bart.print_score()
    bart.__name #AttributeError: 'Student' object has no attribute '__name'

```

### 继承和多态
&emsp;&emsp;在 OOP 程序设计中，当我们定义一个 class 的时候，可以从某个现有的
class 继承，新的 class 称为子类（Subclass），而被继承的 class 称为基
类、父类或超类（Base class、Super class）。

```python

class Animal(object):
    def run(self):
        print('Animal is running...')

def run_twice(animal):
    animal.run()


class Dog(Animal):

    def run(self):
        print('Dog is running...')

    def bit(self):
        print(" i bitting ...")

class Cat(Animal):

    def run(self):
        print('Cat is running...')

    def claw(self):
        print('i clawwing')


if __name__ == '__main__':
    dog = Dog()
    dog.run()
    dog.bit()
    cat = Cat()
    cat.run()
    cat.claw()
    # 判断一个变量是否是某个类型可以用 isinstance()判断
    print(isinstance(dog, Dog))      # True
    print(isinstance(dog, Animal))   # True

    run_twice(Dog()) #多态


#-------------------------------------
class Tortoise(Animal):
    def run(self):
        print('Tortoise is running slowly...')


if __name__ == '__main__':
    run_twice(Tortoise())

```
“开闭”原则：  
对扩展开放：允许新增 Animal 子类；  
对修改封闭：不需要修改依赖 Animal 类型的 run_twice()等函数

#### 鸭子类型 'file-like object'
```python
class Animal(object):
    def run(self):
        print('Animal is running...')

def run_twice(animal):
    animal.run()
    
class Timer(object):
    def run(self):
        print('Start ...')


if __name__ == '__main__':
    run_twice(Timer())
```
#### 静态语言 vs 动态语言

-
    &emsp;&emsp;对于静态语言（例如 Java）来说，如果需要传入 Animal 类型，则传入的
    对象必须是 Animal 类型或者它的子类，否则，将无法调用 run()方法。
    对于 Python 这样的动态语言来说，则不一定需要传入 Animal 类型。我
    们只需要保证传入的对象有一个 run()方法就可以了：
-
    &emsp;&emsp;这就是动态语言的“鸭子类型”，它并不要求严格的继承体系，一个对象
    只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。
-
    &emsp;&emsp;Python 的“file-like object“就是一种鸭子类型。对真正的文件对象，它有
    一个 read()方法，返回其内容。但是，许多对象，只要有 read()方法，
    都被视为“file-like object“。许多函数接收的参数就是“file-like object“，
    你不一定要传入真正的文件对象，完全可以传入任何实现了 read()方法
    的对象。

#### 小结
    继承可以把父类的所有功能都直接拿过来，这样就不必重零做起，子类
    只需要新增自己特有的方法，也可以把父类不适合的方法覆盖重写。
    动态语言的鸭子类型特点决定了继承不像静态语言那样是必须的。

### 获取对象信息

&emsp;&emsp;当我们拿到一个对象的引用时，如何知道这个对象是什么类型、有哪些
方法呢？

#### 使用 type()
```
>>> type(123)
<class 'int'>
>>> type('sdf')
<class 'str'>
>>> type(None)
<class 'NoneType'>

```
如果一个变量指向函数或者类，也可以用 type()判断：
```
>>> type(abs)
<class 'builtin_function_or_method'>

```
在 if 语句中判断，就需要比较两个变量的 type 类型是否相同：
```
>>> type(123)==type(456)
True
>>> type(123)==int
True
>>> type('abc')==type('123')
True
```
&emsp;&emsp;判断基本数据类型可以直接写 int，str 等，但如果要判断一个对象是否
是函数怎么办？可以使用 types 模块中定义的常量：
```python
import types
def fn():
    print(2333)

if __name__ == '__main__':
    print(type(fn) == types.FunctionType)
    print(type(abs) == types.BuiltinFunctionType)
    print(type(abs) == types.FunctionType)
    print(type(lambda x: x) == types.LambdaType)
    print(type(x for x in range(10)) == types.GeneratorType)
```

#### 使用 isinstance()
&emsp;&emsp;对于 class 的继承关系来说，使用 type()就很不方便。我们要判断 class
的类型，可以使用 isinstance()函数。

#### 使用 dir()

&emsp;&emsp;如果要获得一个对象的所有属性和方法，可以使用 dir()函数
```
>>> dir('adb')

['__add__', '__class__', '__contains__', '__delattr__', '__dir__',
'__doc__', '__eq__', '__format__', '__ge__', '__getattribute__',
'__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__',
'__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__',
'__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__',
'__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__',
'__subclasshook__', 'capitalize', 'casefold', 'center', 'count',
'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map',
'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier',
'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper',
'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace',
'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split',
'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate',
'upper', 'zfill']
```
- 重写len()
```python
class MyDog(object):
    def __len__(self):
        return 100

if __name__ == '__main__':
    dog = MyDog()
    i = len(dog)
    print(i)
    # 剩下的都是普通方法
    lower = 'ABC'.lower()
    print(lower)

```
- 通过失血模型操作对象：
```python
class MyObject(object):
    def __init__(self):
        self.x = 9
    def power(self):
        return self.x * self.x

if __name__ == '__main__':
    obj = MyObject()

    print(hasattr(obj, 'x'))  # 有属性'x'吗？
    print(obj.x)

    print(hasattr(obj, 'y'))  # 有属性'y'吗？

    setattr(obj, 'y', 19)  # 设置一个属性'y'
    print(getattr(obj, 'y'))
    print(obj.y)
```

    如果试图获取不存在的属性，会抛出 AttributeError 的错误：
    >>> getattr(obj, 'z') # 获取属性'z'
    Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
    AttributeError: 'MyObject' object has no attribute 'z'
    可以传入一个 default 参数，如果属性不存在，就返回默认值：

```python
class MyObject(object):
    def __init__(self):
        self.x = 9
    def power(self):
        return self.x * self.x

if __name__ == '__main__':
    obj = MyObject()

    print(getattr(obj, 'z', 404))  # 获取属性'z'，如果不存在，返回默认值 404
```

也可以获得对象的方法：
```
>>> hasattr(obj, 'power') # 有属性'power'吗？
True
>>> getattr(obj, 'power') # 获取属性'power'
<bound method MyObject.power of <__main__.MyObject object at
0x10077a6a0>>
>>> fn = getattr(obj, 'power') # 获取属性'power'并赋值到变量 fn
>>> fn # fn 指向 obj.power
<bound method MyObject.power of <__main__.MyObject object at
0x10077a6a0>>
>>> fn() # 调用 fn()与调用 obj.power()是一样的
81

```

#### 小结
    通过内置的一系列函数，我们可以对任意一个 Python 对象进行剖析，
    拿到其内部的数据。要注意的是，只有在不知道对象信息的时候，我们
    才会去获取对象信息。如果可以直接写：
    sum = obj.x + obj.y
    就不要写：
    sum = getattr(obj, 'x') + getattr(obj, 'y')
    一个正确的用法的例子如下：
```python
def readImage(fp):
 if hasattr(fp, 'read'):
    return readData(fp)
 return None
```
    假设我们希望从文件流 fp 中读取图像，我们首先要判断该 fp 对象是否
    存在 read 方法，如果存在，则该对象是一个流，如果不存在，则无法读
    取。hasattr()就派上了用场。
    请注意，在 Python 这类动态语言中，根据鸭子类型，有 read()方法，不
    代表该 fp 对象就是一个文件流，它也可能是网络流，也可能是内存中
    的一个字节流，但只要 read()方法返回的是有效的图像数据，就不影响
    读取图像的功能。

### 实例属性和类属性

给实例绑定属性的方法是通过实例变量，或者通过 self 变量：
```python
class Student(object):
    def __init__(self, name):
        self.name = name
        
if __name__ == '__main__':
    s = Student('Bob')
    s.score = 90
```
直接在 class 中定义属性，这种属性是类属性，归 Student 类所有：
```python
class Student(object):
    name = 'Student...'

if __name__ == '__main__':
    s = Student()
    print(s.name)
    print(Student.name)
```
```
>>> s.name = 'Michael' # 给实例绑定 name 属性
>>> print(s.name) # 由于实例属性优先级比类属性高，因此，它会屏蔽掉类的
name 属性
Michael
>>> print(Student.name) # 但是类属性并未消失，用 Student.name 仍然可以
访问
Student
>>> del s.name # 如果删除实例的 name 属性
>>> print(s.name) # 再次调用 s.name，由于实例的 name 属性没有找到，类的
name 属性就显示出来了
Student
```




