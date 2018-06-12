### 函数式编程

简短地说，函数式编程就是把函数作为变量，在函数中操作函数调用函数的一种 **抽象编程范式**

`python`中语法支持函数式编程，函数名即为函数变量名
可用`函数变量.__name__`来取得声明时的函数名称

其包含几种情况：

1. 将函数作为变量
2. 将函数作为参数
3. 将函数作为返回值

`Javascript`中经常可以见到函数式编程的影子，从ES6开始使用了很多函数式编程的API，如：

```js
const dataArr = [1, 2, 3]
dataArr.map(x => x * x)
// dataArr => [1, 4, 9]
dataArr.reduce((acc, value) => acc + value, 0)
// 6
dataArr.forEach(x => console.log(x))
// => 1 2 3
```

以上几个例子中，函数均以参数的形式传入到另一个函数内部来执行，这就是一种函数式编程

#### 高阶函数及举例

##### map

`map`顾名思义是一种映射方式，将集合中的所有元素，按照某种映射出新值反映在集合中
`map`有两个参数，第一个参数为函数func，func必须有一个参数，第二个函数为要操作的对象集合arr
map执行时迭代arr，将arr中的每一个值作为参数传入func调用

```py
print([x for x in map(lambda x: x + 2, [1, 2, 3, 4])])
# [3, 4, 5 ,6]
```

##### reduce

`reduce`是将一组数据按照一定规则合并，引用廖老师的例子

```py
reduce(f, [x1, x2, x3, x4])
# 上下两种表达式为一个意思
f(f(f(x1, x2), x3), x4)
```

`reduce`有两个参数，第一个参数为函数func，func必须有两个参数，第二个函数为要操作的对象集合arr
`reduce`执行时同样迭代arr，将上一次执行的结果作为func的第一个参数，当前元素作为第二个参数调用func

```py
from functools import reduce
reduce(lambda acc, x: acc + x, [1, 2, 3, 4])
# 10
```

##### filter

`filter`即按照一定规则进行过滤操作
`filter`有两个参数，第一个为要执行的操作func，第二个为要执行的序列arr
`filter`运行时迭代序列arr，将每一个值作为参数传入func，并且需要返回一个bool值进行过滤

```py
print([x for x in filter(lambda x: x > 2, [1, 2, 3, 4])])
# [3, 4]
```

##### sorted

顾名思义，排序函数
`sorted`只有一个位置参数，序列arr。有两个可选的关键字参数，`key`和`reverse`
`key`为指定的排序key，是一个`function`。默认按照数字大小或者文字的`ascii`进行排序
`key`可以指定为一个`function`，参数序列arr的元素值，返回值为排序的key
`reverse`默认为False，指定为True是反向排序

```py
L = [('Bob', 75), ('Adam', 92), ('Bart', 66), ('Lisa', 88)]
def by_name(t):
    return t[0]

L2 = sorted(L, key=by_name)
print(L2)
# [('Adam', 92), ('Bart', 66), ('Bob', 75), ('Lisa', 88)]
```

### 返回函数

既函数的返回值是一个函数

```py
def foo(x):
  def func():
    print(x);
  return func

foo1 = foo(1)
foo1()    # => 1
```

### lambda关键字：匿名函数

匿名函数即没有函数名的函数

```py
lambda x: x + 1
```

与其他语言lambda表达式的比较：

```java
/* java */
List<String> strList = new ArrayList<>();
strList.add("a");
...;
strList.forEach(x -> System.out.println(x));
```

```js
/* Javascript */
cosnt func = () => {};
```

```py
# Python
fun = lambda x: x + 2
print(fun(2))
```

### 装饰器函数

装饰器基于装饰器设计模式，在不改变原有实现的基础上为其添加*新的功能*

与`JavaScript`的装饰器类似，以`@expr`的形式写在方法前面即可

无参装饰器原理：将被修饰的函数`func`作为参数传入装饰器函数`foo`中，返回新函数`wrapper`给原函数指针`func`

写法如下：

```py
def foo(func):
  def wrapper(*args, **kw):
    print('call %s' % func.__name__)  # 要执行的操作
    return func(*arg, **kw)
  return wrapper
```

有参装饰器原理：将装饰器的参数`'message'`传入装饰器函数`foo`，再返回一个新的装饰器函数`decorator`，
再将被修饰的函数`func`作为参数传入被返回的新的装饰器函数`decorator`中执行操作后，返回新的函数`wrapper`给原函数指针`func`

写法如下：

```py
def foo(msg):
  def decorator(func):
    def wrapper(*args, **kw):
      print('call ' + msg + ' %s' % func.__name__)  # 要执行的操作
      return func(*arg, **kw)
    return wrapper
  return decorator
```

作用等效写法：

```py
@foo
def func:
  ...
# 等同于
func = foo(func)

# 带参数的装饰器
@foo('message')
def func:
  ...
# 等同于
func = foo('message')(func)
```

实现一个下列功能的装饰器：

1. 在函数执行前后分别打log
2. 只用一个装饰器函数，使其既支持有参数的写法，同时也支持无参数写法

```py
# coding=utf-8
from functools import wraps
from types import FunctionType

def log(arg):
  if isinstance(arg, FunctionType):     # 没有参数，直接修饰函数，以函数为参的情况
    @wraps(arg)
    def wrapper(*args, **kw):
      print('begin call')
      result = arg(*args, **kw)
      print('end call')
      return result
    return wrapper
  else:                                 # 有参数，返回值修饰函数情况
    def decorator(func):
      @wraps(arg)
      def wrapper(*args, **kw):
        print('begin call' + arg)
        result = func(*args, **kw)
        print('end call' + arg)
        return result
      return wrapper
    return decorator

@log('function')
def f(*arg):
    print(arg)

f(1, 2, 3, 4)
```

functools.wrapsとは？

使用装饰器函数会导致原函数的`__name__`等元数据丢失，系统提供了`functools.wraps`装饰器，可以防止原函数的元数据丢失

关于[装饰器类](https://p-jiangh.github.io/python-notebook/docs/syntax/oop#装饰器类)

### 偏函数

在对函数进行柯里化时，可以使用环境内建函数`functools.partial`将被操作函数的一个参数进行固定，来生成一个缺省参数的新函数

新函数即为原函数的偏函数（与数学中的偏导数不是一回事）

```py
from functools import partial
int('12345', base=8)  # 5349
int('101', base=2)    # 5
# 创建偏函数
int8 = partial(int, base=8)
int2 = partial(int, base=2)
int8('12345')   # 5349
int2('101')     # 5
```
