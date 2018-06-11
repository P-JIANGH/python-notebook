### 函数

#### 函数声明

```py
def func_name(params):
  if...
  ...any function body
  return
print(func_name('a'))
```

python的函数允许返回多个值，返回多个值时返回的是一个`tuple`

抛出异常使用`raise`关键字

空函数：

```py
def func():
  pass
```

pass语句将不执行任何代码，什么也不做，if等结构中也可以使用，可用来做占位符，解除运行`error`

#### 函数参数

* 位置参数 “正常的参数”

```py
def func(a):
  pass
```

* 默认参数

不可变对象

```py
def func(a, b = 1):
  pass
```

* 可变参数

在函数内部是一个tuple

```py
def func(*c):
  pass
func(1,2,3,4)
# (1,2,3,4)
# 或者
arr = [1,2,3]
func(*arr)
#可以将'*'运算符理解为javascript的数组拓展运算符'...'
```

* 关键字参数

```py
def func(a, b, **c):
  pass
func(1,2, name='HJiang', age=23)
# 或者
kv = { 'name': '', 'age': 23 }
func(1,2, **kv)
# 可以将'**'运算符理解为js中的对象拓展运算符
```

* 命名关键字参数

以*为参数的分隔符，限制关键字参数的键

```py
def func(a, b, *, c, d):
  print(a, b, c, d)
func(1, 2, c='HJiang', d='123')
# 可以将'**'运算符理解为js中的对象拓展运算符
```

命名关键字参数可以有缺省值，把其中一个参数设默认值

```py
def func(a, b, *, c='HJiang', d):
  print(a, b, c, d)
func(1, 2, d='123')
```

命名关键字参数在调用时，只能传入设定好key的参数，且，在没有默认值时不可缺省

```py
def func(a, b, *, c='HJiang', d):
  print(a, b, c, d)
func(1, 2, **{ 'c':'HAO', 'd': 123, 'e': 456 })
# Traceback (most recent call last):
#   File "src\app.py", line 6, in <module>
#     func(1, 2, **{ 'c':'HAO', 'd': 123, 'e': 456 })
# TypeError: func() got an unexpected keyword argument 'e'

def func(a, b, *, c='HJiang', d):
  print(a, b, c, d)
func(1, 2, **{ 'd': 123, 'e': 456 })
# Traceback (most recent call last):
#   File "src\app.py", line 6, in <module>
#     func(1, 2, **{ 'd': 123, 'e': 456 })
# TypeError: func() got an unexpected keyword argument 'e'

def func(a, b, *, c, d):
  print(a, b, c, d)
func(1, 2, **{ 'd': 123 })
# Traceback (most recent call last):
#   File "src\app.py", line 6, in <module>
#     func(1, 2, **{ 'd': 123 })
# TypeError: func() missing 1 required keyword-only argument: 'c'
```

对于任意函数，都可以通过类似`func(*args, **kw)`的形式调用它，无论它的参数是如何定义的。

位置参数，默认参数，可变参数可以用`tuple`传值

关键字参数和命名关键字参数可以用`dict`传入
