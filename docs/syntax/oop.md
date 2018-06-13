### 面向对象编程

#### 类

##### 类定义

`python`定义一个类的写法：使用`class`关键字声明一个类，关键字后面紧跟着类名`Foo`，类名后有一个参数`(object)`，表示从那个类继承下来

```py
class Foo(object):
  pass
```

##### 构造器

类中必须包含`__init__(self, [params])`方法来创建实例，`__init__`方法必须有一个参数，代表实例本身。调用时，调用的方法名为类名，*且不需要传递self参数*。
这个方法也可以添加除了`self`以外的参数，用于给实例添加属性

可以将`__init__`方法理解为构造器

```py
class Foo(object):

  def __init__(self):
    pass

foo = Foo()
```

实际上，类当中的所有self参数都是不需要传的，指向实例本身

##### 私有属性

在构造函数中把设置的属性的前面加上两个下划线，就变成了一个私有属性，私有属性会被python编译器编译为其他名称以防止从外部访问私有属性

```py
class Foo(object):

  def __init__(self, name):
    self.__name = name

foo = Foo('jianghao')
foo.__name
# Traceback (most recent call last):
#   File "src\app.py", line 11, in <module>
#     foo.__name
# AttributeError: 'Foo' object has no attribute '__name'
```

外部想要访问实例的私有属性时，可以给私有属性设置`get`和`set`方法，

```py
class Student(object):

  def __init__(self, name):
    self.__name = name

  def get_name(self):
    return self.__name

  def set_name(self, name):
    if name == '':
      raise ValueError('name must have value')
    else:
      self.__name = name

stu = Student('Bob')
stu.set_name('HJ')
```

##### property

python同样也支持c#的`property`属性写法，下面例子使用装饰器`@property`为类添加一个属性。
`@property`会为类新加两个构造器，分别为：`@属性名.setter`和`@属性名.deleter`

```py
class Student(object):

  @property
  def name(self):
    if (hasattr(self, '_name')):
      return self._name
    else:
      return ''

  @name.setter
  def name(self, name):
    self._name = name

stu = Student()
stu.name = 'HJ'
print(stu.name)
```

##### 实例属性和类属性

前面提到的在`__init__`方法中设置的属性为实例属性

类中直接写的属性即为类属性

```py
class Foo(object):
  name = ''
  ...
```

name 即为类属性

##### 动态添加属性和方法

给实例动态添加属性和方法

```py
from types import MethodType

class Student(object):
  pass

def set_name(self, name):
  self.name = name

student = Student()
student.name = 'mick'
student.set_name = MethodType(set_name, student)
student.set_name('jiang hao')
print(student.name)
# >>> jiang hao
```

为了给所有实例都添加上属性和方法，可以对类添加属性或方法

```py
from types import MethodType

class Student(object):
  pass

def set_name(self, name):
  self.name = name

Student.name = 'mick'
Student.set_name = MethodType(set_name, Student)
student = Student()
print(student.name)
student.set_name('jiang hao')
print(student.name)
# >>> mick
# >>> jiang hao
```

动态设置属性时收到类内部的静态属性`__slots__`限制，`__slots__`是一个可拓展的属性名的`tuple`

```py
from types import MethodType
class Student(object):
  __slots = ('name', 'age')
```

#### 继承与多态

##### 继承

同前面提到的一样，类在定义时，类名会带着一个括号，括号里面就是要继承的类

但不太一样的是，python支持多继承，可以在原有的继承链中混入`mixin`新的功能

比如，编写一个多进程模式的TCP服务，定义如下：

```py
class MyTCPServer(TCPServer, ForkingMixin):
  pass
```

##### 多态

python同样存在多态，在子类中写一个与父类*同名*的方法可以覆盖父类方法（注意不是相同签名的方法）

```py
class Foo(object):

  def __init__(self, name):
    self.__name = name

  def run(self):
    print('foo %s' % self.__name)

class Dtas(Foo):

  def __init__(self, name):
    self.__name = name

  def run(self, msg):
    print('foo %s %s' % (self.__name, msg))

da = Dtas('jiangh')

da.run()
# foo jiangh ms123
```

##### isinstance关键字

`isinstance`可以判断实例是否是某个类的实例

```py
class Foo(object):
  pass

foo = Foo()
isinstance(foo, Foo)
# >>> true
```

isinstance的第二个参数可以为一个`tuple`，判断实例是否为指定类中的某一个类的实例

```py
class Foo(object):
  pass

foo = Foo()
isinstance(foo, (Foo, object))
# >>> true
```

`type()`同样可以获得实例的所属类信息

```py
print(type(123))
# >>> <class 'int'>
print(type(lambda x: x + 1) == LambdaType)
# >>> True
```

使用`dir()`可以获得一个对象的所有属性和方法。同样也有`getattr()`、`setattr()`以及`hasattr()`等方法来获取或设置对象的属性和状态

#### 定制类

定制类即重写类的系统方法，类似之前看到的`__init__`，`__new__`等方法。

##### \_\_str\_\_与\_\_repr\_\_

重写`__str__`可以定制`print()`输出出来的内容
重写`__repr__`可以定制直接在交互环境中输出变量的内容

```py
class Student(object):

  def __str__(self):
    return 'print Student'

stu = Student()
print(stu)
# >>> print Student
```

##### \_\_iter\_\_与\_\_next\_\_

如果想让一个类可以使用for...in循环迭代，就需要实现`__iter__`方法和`__next__`

```py
class Fib(object):

  def __init__(self):
    self.a, self.b = 0, 1

  def __iter__(self):
    return self

  def __next__(self):
    self.a, self.b = self.b, self.a + self.b
    if self.a > 5000:
      raise StopIteration
    return self.a

f = Fib()
print('First: %d' % next(f))
for x in f:
  print(x)
```

##### \_\_getitem\_\_

如果想让一个类是可索引的，就需要实现`__getitem__`方法

类似`TypeScript`的可索引接口：

```typescript
export interface indexable {
  [number]: string;
}
```

在`Python`当中：

```py
class Powerable(object):

  def __init__(self):
    self.__data = 1

  def __getitem__(self, index):
    return self.__data << index

  def __setitem__(self, index, value):
    self.__data = value >> index

d = Powerable()
print(d[3])
d[5] = 1024
print(d[6])
# >>> 8
# >>> 2048
```

与它对应的有`__setitem__`设置一个元素和`__delitem__`删除一个元素

##### \_\_getattr\_\_

在取得对象的属性时，调用的是`__getattr__`方法，当要取得一个不存在的属性时，可以先实现`__getattr__`来动态返回值

实现一个可以链式调用，动态生成url的类

```py
class Chainable(object):

  def __init__(self, path='/'):
    self.__path = path

  def __getattr__(self, attr):
    return Chainable(self.__path + attr + '/')

  def __str__(self):
    return self.__path

  __repr__ = __str__

url = Chainable()
url.api.home.docs.xxx
print(url.api.home.docs.xxx)
# >>> /api/home/docs/xxx/
```

##### \_\_call\_\_

如果想让类的实例都像一个函数一样可以被调用，就需要实现`__call__`方法

对于实例是否是可调用的实例，可以用`callable(instance)`测试

更多定制方法需要参着[Python官方文档](https://docs.python.org/3/reference/datamodel.html#special-method-names)

#### 装饰器类

定义类中提到的`__call__`方法可以让一个类的实例能够被调用，装饰器类就是使用了这种做法实现的

```py
class Logable(object):
  def __init__(self, func):
    self.__func = func

  def __call__(self, *args, **kw):
    print('call %s start' % self.__func.__name__)
    result = self.__func(*args, **kw)
    print('call %s end' % self.__func.__name__)
    return result

@Logable
def add(x, y):
  print('add result %d' % (x + y))
  return x + y

# 等同于：
# def add(x, y):
#   print('add result %d' % (x + y))
#   return x + y

# add = Logable(add)

print(add(3, 4))
# >>> call add start
# >>> add result 7
# >>> call add end
# >>> 7
```

但是，被装饰器类修饰过的方法会变成该类的一个实例

#### 类装饰器

##### 无参类装饰器

我们会遇到使用装饰器来装饰类的情况，类装饰器和函数装饰器稍有不同，因为执行时传入的参数是一个类的构造器

```py
class Singleton(object):
  def __init__(self, clazz):
    self.__class = clazz
    self.__instance = None

  def __call__(self, *args, **kw):
    if self.__instance == None:
      print('No instance, create new one')
      self.__instance = self.__class(*args, **kw)
    return self.__instance

@Singleton
class Student(object):
  pass

# 等同于：
# class Student(object):
#   pass

# Student = Singleton(Student)

a = Student()
b = Student()
c = Student()

print(a, b, c, Student)
```

以上是一个单例模式的代码，可以在`__init__`的方法中调用被修饰类的构造器，来更简洁的实现单例模式

关键代码：`Student = Singleton(Student)`是创建一个类`Singleton`的实例，执行`Singleton`的`__init__`方法。
使得`Student`本身是`Singleton`的一个实例。所以执行`a = Student()`时才会进入`Singleton`的`__call__`方法。

##### 有参类装饰器

与无参类装饰器思路类似，看代码：

```py
class Singleton(object):
  def __init__(self, msg):
    self.msg = msg
  def __call__(self, clazz):
    msg = self.msg

    class decorator(object):
      def __init__(self, clazz):
        self.clazz = clazz
        self.instance = None

      def __call__(self, *args, **kw):
        if self.instance == None:
          print('Create a New Instance with msg: %s' % msg)
          self.instance = self.clazz(*args, *kw)
        return self.instance

    return decorator(clazz)

@Singleton('Singleton')
class Student(object):
  pass

# 等同于
# class Student(object):
#   pass
# # Student = Singleton('Singleton')(Student)

a = Student()
b = Student()
c = Student()

print(a, b, c, Student)
# >>> Create a New Instance with msg: Singleton
# >>><__main__.Student object at 0x01BD07F0> <__main__.Student object at 0x01BD07F0> <__main__.Student object at 0x01BD07F0> <__main__.Singleton.__call__.<locals>.decorator object at 0x01BD07D0>
```

#### 枚举类

枚举类有`enum`包提供，使用时先`import`进来。在声明自己的类继承`Enum`。使用时可以引入`enum`包的`unique`装饰器

```py
from enum import Enum, unique

@unique
class BUTTON_TYPE(Enum):
  START = 1
  BACK = 2
  QUIT = 3
```
