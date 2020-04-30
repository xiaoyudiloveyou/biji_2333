### 面向对象高级编程

&emsp;&emsp;Python 中，面向对象还有很多高级特性，允许我们写出非常强大的功能。
我们会讨论多重继承、定制类、元类等概念。

#### 使用__slots__

```python
from types import MethodType

class Student(object):
    name = 'Student'


if __name__ == '__main__':
    s = Student()
    s.name = 'Michael'
    print(s.name)   #Michael

# 给实例绑定一个方法：
    def set_age(self, age):
        self.age = age

    s.set_age = MethodType(set_age, s)
    s.set_age(25)
    print(s.age)    #25

    s1 = Student()  #但是，给一个实例绑定的方法，对另一个实例是不起作用的：

# 为了给所有实例都绑定方法，可以给 class 绑定方法：
    def set_score(self, score):
        self.score = score

    Student.set_score = MethodType(set_score, Student)

    s.set_score(100) # 给类绑定方法后，前边创建过的实例也会拥有该方法
    print(s.score)
```

#### 使用__slots__ 实例的变量
```python
from types import MethodType

class Student(object):
    __slots__ = ('name', 'age')  # 用 tuple 定义允许绑定的属性名称


class GraduateStudent(Student):
    __slots__ = Student.__slots__
    # pass

if __name__ == '__main__':
    s = Student()
    s.name = 'Michael'
    s.age = 25
    # s.sore = 100
 # AttributeError: 'Student' object has no attribute 'sore'

# 使用__slots__要注意，__slots__定义的属性仅对当前类实例起作用，对继承的子类是不起作用的：
# 除非在子类中也定义__slots__，这样，子类实例允许定义的属性就是自身的__slots__加上父类的__slots__。
    class GraduateStudent(Student):
        __slots__ = Student.__slots__
        
    g = GraduateStudent()
    g.score = 9999
    

```

#### 使用@property 类的变量
```python
# 利用getter/setter对属性进行检查
class Student(object):
    __slots__ = ('name', 'age')  # 用 tuple 定义允许绑定的属性名称

class GraduateStudent(Student):
    __slots__ = Student.__slots__ + ('_score',)

    def get_score(self):
        return self._score

    def set_score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0~100')
        self._score = value

if __name__ == '__main__':
    s = GraduateStudent()
    s.set_score(60)
    print(s.get_score())
    s.set_score(600)    # 抛异常
    print(s.get_score())
```
&emsp;&emsp;但是，上面的调用方法又略显复杂，没有直接用属性这么直接简单  
&emsp;&emsp;有没有既能检查参数，又可以用类似属性这样简单的方式来访问类的变量呢？对于追求完美的 Python 程序员来说，这是必须要做到的！

    @property 装饰器就是负责把一个方法变成属性调用的：
```python
# 利用getter/setter对属性进行检查
class Student(object):
    __slots__ = ('name', 'age')  # 用 tuple 定义允许绑定的属性名称

class GraduateStudent(Student):
    __slots__ = Student.__slots__ + ('_score',)

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0~100')
        self._score = value

if __name__ == '__main__':
    s = GraduateStudent()
    s.score = 60     # OK，实际转化为 s.set_score(60)
    print(s.score)   # OK，实际转化为 s.get_score()

```
- 只定义 getter 方法，不定义 setter 方法就是一个
只读属性：

#### 小结
&emsp;&emsp;@property 广泛应用在类的定义中，可以让调用者写出简短的代码，同时
保证对参数进行必要的检查，这样，程序运行时就减少了出错的可能性

```python
from types import MethodType


# 利用getter/setter对属性进行检查
class Screen(object):
    __slots__ = ('_width', '_height')
    pass
    @property
    def width(self):
        return self._width

    @width.setter
    def width(self, value):
        if not isinstance(value, int):
            raise ValueError('number must be Integer!')
        self._width = value

    @property
    def height(self):
        return self._height

    @height.setter
    def height(self, value):
        if not isinstance(value, int):
            raise ValueError('number must be Integer!')
        self._height = value

    @property
    def resolution(self):
        print(self.width)
        return self.width * self.height


if __name__ == '__main__':
    # Screen.resolution = 324 类属性被重新覆盖，@perpety的类属性就不起作用了
    s = Screen()
    s.width = 100
    s.height = 100
    # s.resolution = 1 没有setter方法，实例属性不可赋值
    print(s.resolution)
    print(Screen.resolution)
assert s.resolution == 10000, '100 * 100 = %d ?' % s.resolution

```

### 多重继承
```python
class Animal(object):
 pass
# 大类:
class Mammal(Animal):
 pass
class Bird(Animal):
 pass
# 各种动物:
class Dog(Mammal):
 pass
class Bat(Mammal):
 pass
class Parrot(Bird):
 pass
class Ostrich(Bird):
 pass
 #  Runnable 和 Flyable 的类
class Runnable(object):
 def run(self):
    print('Running...')
class Flyable(object):
 def fly(self):
    print('Flying...')

# 对于需要 Runnable 功能的动物，就多继承一个 Runnable，例如 Dog：
class Dog(Mammal, Runnable):
 pass
# 对于需要 Flyable 功能的动物，就多继承一个 Flyable，例如 Bat：
class Bat(Mammal, Flyable):
 pass

```
#### MixIn

&emsp;&emsp;如果需要“混入”额外的功能，通过多重继承就可以
实现，比如，让 Ostrich 除了继承自 Bird 外，再同时继承 Runnable。这
种设计通常称之为 MixIn

    MixIn 的目的就是给一个类增加多个功能，这样，在设计类的时候，我
    们优先考虑通过多重继承来组合多个 MixIn 的功能，而不是设计多层次
    的复杂的继承关系。
    Python自带的很多库也使用了MixIn。举个例子，Python自带了TCPServer
    和 UDPServer 这两类网络服务，而要同时服务多个用户就必须使用多进
    程或多线程模型，这两种模型由 ForkingMixIn 和 ThreadingMixIn 提供。
    通过组合，我们就可以创造出合适的服务来。

```
比如，编写一个多进程模式的 TCP 服务，定义如下：
class MyTCPServer(TCPServer, ForkingMixIn):
 pass
编写一个多线程模式的 UDP 服务，定义如下：
class MyUDPServer(UDPServer, ThreadingMixIn):
 pass
 
如果你打算搞一个更先进的协程模型，可以编写一个 CoroutineMixIn：
class MyTCPServer(TCPServer, CoroutineMixIn):
 pass

```

#### 小结
&emsp;&emsp;由于 Python 允许使用多重继承，因此，MixIn 就是一种常见的设计。
只允许单一继承的语言（如 Java）不能使用 MixIn 的设计。

### 定制类
1 . \_\_str__和__repr__
```python
class Student(object):
    def __init__(self, name):   # <__main__.Student object at 0x00000140842CA790>
        self.name = name
    def __str__(self):
        return 'Student object (name: %s)' % self.name  # Student object (name: Michael)

    __repr__ = __str__  #用于调试，可直接显示，格式化后的，不用print打印

if __name__ == '__main__':
    print(Student('Michael'))
```
2 . \_\_iter__  
&emsp;&emsp;如果一个类想被用于 for ... in 循环，类似 list 或 tuple 那样，就必须实
现一个__iter__()方法，该方法返回一个迭代对象，然后，Python 的 for
循环就会不断调用该迭代对象的__next__()方法拿到循环的下一个值，
直到遇到 StopIteration 错误时退出循环。

斐波那契数列为例，写一个 Fib 类:

```python
class Fib(object):
    def __init__(self):
        self.a, self.b = 0, 1 # 初始化两个计数器a, b

    def __iter__(self):
        return self # 示例本身就是迭代对象，故返回自己

    def __next__(self):
        self.a, self.b = self.b, self.a + self.b
        if self.a > 100000:
            raise StopIteration()
        return self.a

if __name__ == '__main__':
    for n in Fib():
        print(n)
```

3 . \_\_getitem__

&emsp;&emsp;Fib 实例虽然能作用于 for 循环，看起来和 list 有点像，但是，把它当成
list 来使用还是不行，比如，取第 5 个元素：

    >>> Fib()[5]
    Traceback (most recent call last):
     File "<stdin>", line 1, in <module>
    TypeError: 'Fib' object does not support indexing

```python
class Fib(object):

    def __getitem__(self, item):
        print(item,'item')
        for x in range(item):
            a, b = b, a+b
        return a

if __name__ == '__main__':
    for n in Fib():
        print(n)
    fib = Fib()
    print(fib[0])
    
```
4 . 但是 list 有个神奇的切片方法

    对于 Fib 却报错。原因是__getitem__()传入的参数可能是一个 int，也可
    能是一个切片对象 slice，所以要做判断：

```python
class Fib(object):

    def __getitem__(self, n):
        if isinstance(n, int):
            a, b = 1, 1
            for x in range(n):
                a, b = b, a+b
            return a
        if isinstance(n, slice):
            start = n.start
            stop = n.stop
            if start is None:
                start = 0
            a, b = 1, 1
            L = []
            for x in range(stop):
                if x >= start:
                    L.append(a)
                a, b = b, a + b
            return L

if __name__ == '__main__':
    fib = Fib()
    print(fib[1:10])
```

5 . 

-
    但是没有对 step 参数作处理：
    
    f[:10:2]
    [1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]

    也没有对负数作处理，所以，要正确实现一个__getitem__()还是有很多
    工作要做的。
-
    此外，如果把对象看成 dict，__getitem__()的参数也可能是一个可以作
    key 的 object，例如 str。
    与之对应的是__setitem__()方法，把对象视作 list 或 dict 来对集合赋值。
    最后，还有一个__delitem__()方法，用于删除某个元素。

- 
    总之，通过上面的方法，我们自己定义的类表现得和 Python 自带的 list、
    tuple、dict 没什么区别，这完全归功于动态语言的“鸭子类型”，不需要
    强制继承某个接口。
```python
class Fib(object):

    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
            raise AttributeError('\'Student\' object has no attribute\'%s\'' % attr)


if __name__ == '__main__':
    fib = Fib()
    print(fib[1:10])

```
完全动态调用的用处：

    这实际上可以把一个类的所有属性和方法调用全部动态化处理了，不需
    要任何特殊手段。
    这种完全动态调用的特性有什么实际作用呢？作用就是，可以针对完全
    动态的情况作调用。

    举个例子：
    现在很多网站都搞 REST API，比如新浪微博、豆瓣啥的，调用 API 的
    URL 类似：
     http://api.server/user/friends
     http://api.server/user/timeline/list
    如果要写 SDK，给每个 URL 对应的 API 都写一个方法，那得累死，而
    且，API 一旦改动，SDK 也要改。
```python
class Chain(object):

    def __init__(self, path=''):
        self._path = path

    def __getattr__(self, path):
        return Chain('%s/%s' % (self._path, path))

    def __str__(self):
        return self._path

    __repr__ = __str__


if __name__ == '__main__':
    print(Chain().status.user.timeline.list)
```
    这样，无论 API 怎么变，SDK 都可以根据 URL 实现完全动态的调用，
    而且，不随 API 的增加而改变！
    还有些 REST API 会把参数放到 URL 中，比如 GitHub 的 API：
    GET /users/:user/repos
    调用时，需要把:user 替换为实际用户名。如果我们能写出这样的链式
    调用：
Chain().users('michael').repos
```python
class Chain(object):
    def __init__(self,path=''):
        self._path = path

    def __getattr__(self, path):
        return Chain('%s/%s' % (self._path, path))

    def __str__(self):
        return self._path

    def __call__(self, user):
        return Chain('%s/%s' % (self._path, user))

if __name__ == '__main__':
    print('GET', Chain().user('michael').repos('repos'))
```

\_\_call__()

只需要定义一个__call__()方法，就可以直接对实例进行调用。
```python
class Student(object):
    def __init__(self, name):
        self.name = name
    def __call__(self):
        print('My name is %s.' % self.name)

if __name__ == '__main__':
    s = Student('Michael')
    s()

```
    __call__()还可以定义参数。对实例进行直接调用就好比对一个函数进
    行调用一样，所以你完全可以把对象看成函数，把函数看成对象，因为
    这两者之间本来就没啥根本的区别。
    如果你把对象看成函数，那么函数本身其实也可以在运行期动态创建出
    来，因为类的实例都是运行期创建出来的，这么一来，我们就模糊了对
    象和函数的界限。
通过 callable()函数，我们就可以判断一个对象是否是“可调用”对象。
```
>>> callable(Student())
True
>>> callable(max)
True
>>> callable([1, 2, 3])
False
>>> callable(None)
False
>>> callable('str')
False
```

#### 小结
Python 的 class 允许定义许多定制方法，可以让我们非常方便地生成特
定的类

### 使用枚举类

```python
from enum import Enum

Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May',  'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))

if __name__ == '__main__':
   for name, member in Month.__members__.items():
       print(name, '=>', member, ',', member.value)
```
@unique 装饰器可以帮助我们检查保证没有重复值
```python

from enum import Enum, unique

@unique
class Weekday(Enum):
    Sun = 0
    Mon = 1
    Tue = 2
    Wed = 3
    Thu = 4
    Fri = 5
    Sat = 6

if __name__ == '__main__':
    day1 = Weekday.Fri
    print(day1.value)

```
#### 小结
Enum 可以把一组相关常量定义在一个 class 中，且 class 不可变，而且成
员可以直接比较。

### 使用元类

__type()__  
动态语言和静态语言最大的不同，就是函数和类的定义，不是编译时定
义的，而是运行时动态创建的。 
```python
class Hello(object):
    def hello(self, name='world'):
        print('Hello, %s.' % name)
```
```
>>> from Ping import Hello
>>> h = Hello()
>>> h.hello()
Hello, world.
>>> type(Hello)
<class 'type'>
>>> type(h)
<class 'Ping.Hello'>

```
class 的定义是运行时动态创建的，而创建 class 的方法就是使用type()函数
```python
def fn(self, name='world'):
    print('Hello1, %s.' % name)

if __name__ == '__main__':
    Hello1 = type('Hello', (object,), dict(hello=fn))
    hello_ = Hello1()
    hello_.hello()
```

要创建一个 class 对象，type()函数依次传入 3 个参数：
1. class 的名称；
Python3 基础教程【完整版】 http://www.yeayee.com/
211/531
2. 继承的父类集合，注意 Python 支持多重继承，如果只有一个父类，
别忘了 tuple 的单元素写法；
3. class 的方法名称与函数绑定，这里我们把函数 fn 绑定到方法名
hello 上。

    通过 type()函数创建的类和直接写 class 是完全一样的，因为 Python 解
    释器遇到class定义时，仅仅是扫描一下class定义的语法，然后调用type()
    函数创建出 class。
    正常情况下，我们都用 class Xxx...来定义类，但是，type()函数也允许
    我们动态创建出类来，也就是说，动态语言本身支持运行期动态创建类，
    这和静态语言有非常大的不同，要在静态语言运行期创建类，必须构造
    源代码字符串再调用编译器，或者借助一些工具生成字节码实现，本质
    上都是动态编译，会非常复杂。

#### metaclass
除了使用 type()动态创建类以外，要控制类的创建行为，还可以使用metaclass。

    metaclass，直译为元类，简单的解释就是：
    当我们定义了类以后，就可以根据这个类创建出实例，所以：先定义类，
    然后创建实例。
    
    先定义 metaclass，就可以创建类，最后创建实例。
    所以，metaclass 允许你创建类或者修改类。换句话说，你可以把类看成
    是 metaclass 创建出来的“实例”。

我们先看一个简单的例子，这个 metaclass 可以给我们自定义的 MyList增加一个 add 方法：

```python
class ListMetaclass(type):
    def __new__(cls, name, bases, attrs):
        attrs['add'] = lambda self, value: self.append(value) # <class '__main__.MyList'> 1
        print(type(cls), type(name), type(bases), type(attrs)) # <class 'type'> <class 'str'> <class 'tuple'> <class 'dict'>
        print(cls)      # <class '__main__.ListMetaclass'>
        print(name)     # MyList
        print(bases)    # (<class 'list'>,)
        print(attrs)    # {'__module__': '__main__', '__qualname__': 'MyList', 'add': <function ListMetaclass.__new__.<locals>.<lambda> at 0x000001AFA93CC1F0>}
        return type.__new__(cls, name, bases, attrs)

class MyList(list, metaclass=ListMetaclass):
    pass

if __name__ == '__main__':
   L = MyList()
   L.add(1)
   print(L)
```
\_\_new__()方法接收到的参数依次是：
1. 当前准备创建的类的对象；
2. 类的名字；
3. 类继承的父类集合；
4. 类的方法集合。

metaclass 修改类定义的。ORM 就是一个典型的例子。

```python
# 定义 Field 类，它负责保存数据库表的字段名和字段类型：
class Field(object):
    def __init__(self, name, column_type):
        self.name = name
        self.column_type = column_type

    def __str__(self):
        return '<%s:%s>' % (self.__class__.__name__, self.name)


class StringField(Field):
    def __init__(self, name):
        super(StringField, self).__init__(name, 'varchar(100)')


class IntegerField(Field):
    def __init__(self, name):
        super(IntegerField, self).__init__(name, 'bigint')

class ModelMetaclass(type):
    def __new__(cls, name, bases, attrs):
        print(cls,'====',name,'===',bases,'===',attrs,'===',i)
        # <class '__main__.ModelMetaclass'> ==== Model === (<class 'dict'>,) === {'__module__': '__main__', '__qualname__': 'Model'} === 0
        # <class '__main__.ModelMetaclass'> ==== User === (<class '__main__.Model'>,) === {'__module__': '__main__', '__qualname__': 'User', 'id': <__main__.IntegerField object at 0x00000268D19B2250>, 'name': <__main__.StringField object at 0x00000268D19B2520>, 'email': <__main__.StringField object at 0x00000268D19B2490>, 'password': <__main__.StringField object at 0x00000268D19B24C0>} === 0
        if name == 'Model':
            return type.__new__(cls, name, bases, attrs)
        print('Found model: %s' % name)
        mappings = dict()
        for k, v in attrs.items():
            if isinstance(v, Field):
                print('Found mapping: %s ==> %s' % (k, v))
                mappings[k] = v
        for k in mappings.keys():
            attrs.pop(k)
        attrs['__mappings__'] = mappings # 保存属性和列的映射关系
        attrs['__table__'] = name  # 假设表名和类名一致
        return type.__new__(cls, name, bases, attrs)

class Model(dict, metaclass=ModelMetaclass):
    # def __int__(self, **kw):
    #     super(Model, self).__init__(**kw)
    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"Model' object has no attribute '%s'" % key)

    def __setattr__(self, key, value):
        self[key] = value

    def save(self):
        fields = []
        params = []
        args = []
        for k, v in self.__mappings__.items():
            fields.append(v.name)
            params.append('?')
            args.append(getattr(self, k, None))
        sql = 'insert into %s (%s) values (%s)' % (self.__table__, ','.join(fields), ','.join(params))
        print('SQL: %s' % sql)
        print('ARGS: %s' % str(args))
    pass


# ORM 框架，想定义一个 User 类来操作对应的数据库表 User，
# 我们期待他写出这样的代码：
class User(Model):
    # 定义类的属性到列的映射：

    id = IntegerField('id')
    name = StringField('username')
    email = StringField('email')
    password = StringField('password')

if __name__ == '__main__':
    # 创建一个实例：
    u = User(id=12345, name='Michael', email='test@orm.org', password='my-pwd')
    # 保存到数据库
    u.save()

# 父类 Model 和属性类型 StringField、IntegerField 是由 ORM 框架提供的，剩下的魔术方法比如 save()全部由 metaclass 自动完成

```
在 ModelMetaclass 中，一共做了几件事情：
1. 排除掉对 Model 类的修改；
2. 在当前类（比如 User）中查找定义的类的所有属性，如果找到一
个 Field 属性，就把它保存到一个__mappings__的 dict 中，同时从
类属性中删除该 Field 属性，否则，容易造成运行时错误（实例的
属性会遮盖类的同名属性）；
3. 把表名保存到__table__中，这里简化为表名默认为类名。

#### 小结
metaclass 是 Python 中非常具有魔术性的对象，它可以改变类创建时的
行为。这种强大的功能使用起来务必小心

[python with关键字学习](https://www.cnblogs.com/Xjng/p/3927794.html)
hello.py
```python
class Dict(dict):

    def __init__(self, **kw):
        super().__init__(**kw)

    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError("'Dict' object has no attribute'%s'" % key)

    def __setattr__(self, key, value):
        self[key] = value
```

hello_test.py
```python
import unittest
from hello import Dict

class TestDict(unittest.TestCase):

    def test_init(self):
        d = Dict(a=1, b='test')
        self.assertEqual(d.a, 1)
        self.assertEqual(d.b, 'test')
        self.assertTrue(isinstance(d, dict))

    def test_key(self):
        d = Dict()
        d['key'] = 'value'
        self.assertEqual(d.key, 'value')

    def test_attr(self):
        d = Dict()
        d.key = 'value'
        self.assertTrue('key' in d)
        self.assertEqual(d['key'], 'value')


    def test_keyerror(self):
        d = Dict()
        with self.assertRaises(KeyError):
            value = d['empty']

    def test_attrerror(self):
        d = Dict()
        with self.assertRaises(AttributeError):
            value = d.empty


if __name__ == '__main__':
    unittest.main()

```

#### setUp 与 tearDow
```python
import unittest
class TestDict(unittest.TestCase):
     def setUp(self):
         print('setUp...')
     def tearDown(self):
         print('tearDown...')
```

    小结
    单元测试可以有效地测试某个程序模块的行为，是未来重构代码的信心
    保证。
    单元测试的测试用例要覆盖常用的输入组合、边界条件和异常。
    单元测试代码要非常简单，如果测试代码太复杂，那么测试代码本身就
    可能有 bug。
    单元测试通过了并不意味着程序就没有 bug 了，但是不通过程序肯定有
    bug。

### 文档测试

[正则表达式](https://www.cnblogs.com/shenjianping/p/11647473.html)
[python中re.group()简介](https://blog.csdn.net/shujuelin/article/details/90409097)

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

class Dict(dict):
    '''
    Simple dict but also support access as x.y style.
    >>> d1 = Dict()
    >>> d1['x'] = 100
    >>> d1.x
    100
    >>> d1.y = 200
    >>> d1['y']
    200
    >>> d2 = Dict(a=1, b=2, c='3')
    >>> d2.c
    '3'
    >>> d2['empty']
    Traceback (most recent call last):
        ...
    KeyError: 'empty'
    >>> d2.empty
    Traceback (most recent call last):
        ...
    AttributeError: 'Dict' object has no attribute 'empty'
    '''
    def __init__(self, **kw):
        super(Dict, self).__init__(**kw)

    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Dict' object has no attribute '%s'" % key)

    def __setattr__(self, key, value):
        self[key] = value

if __name__=='__main__':
    import doctest
    doctest.testmod() 
```
对函数fact(n)编写doctest并执行：
```python
def fact(n):
    '''
    Function to get n!
    Example:
    >>> fact(1)
    1
    >>> fact(2)
    2
    >>> fact(3)
    6
    >>> fact('a')
    Traceback(most recent call last)
        ...
    ValueError: 'empty'
    >>> fact(-2)
    Traceback (most recent call last):
        ...
    ValueError
    '''
    if n < 1 :
        raise ValueError()
    if n == 1 :
        return 1
    return n * fact(n - 1)

if __name__ == 'main' :
    import doctest
    doctest.testmod()
    # print(fact(5))
    
```



















