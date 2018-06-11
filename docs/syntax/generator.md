### 列表生成式与生成器(generator)

#### 列表生成**式**

* 使用内部函数创建`list`

```py
print(list(range(10)))
# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

* 利用一个可迭代对象，使用生成式语句创建新的`list`

使用`for`迭代生成

```py
[ x * x for x in range(1, 11) ]
# [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

使用`if`过滤

```py
[ x * x for x in range(1, 11) if x > 3 ]
# [16, 25, 36, 49, 64, 81, 100]
```

使用两层循环（嵌套循环）

```py
[ x * y for x in range(1, 4) for y in range(2, 5) ]
```

迭代中的`for`与循环的关键字作用相同，因此可以在生成表达式中使用两个参数迭代dict对象

```py
dict = { 'x': 1, 'y': 2 }
[ key + '=' + value for key, value in dict.items() ]
# [16, 25, 36, 49, 64, 81, 100]
```

#### 列表生成**器**

将生成式中的`'[]'`换为`'()'`即为列表生成器，生成器是惰性的，
只有使用`next()`方法调用生成器才会返回生成的新值，占用内存小。
全部元素都输出之后再next()会抛出异常

```py
dict = { 'x': 'a', 'y': 'b' }
gen = ( key + '=' + value for key, value in dict.items() )
print(gen)
print(next(gen))
print(next(gen))
print(next(gen))
# <generator object <genexpr> at 0x01738120>
# x=a
# y=b
# Traceback (most recent call last):
#   File "src\app.py", line 9, in <module>
#     print(next(gen))
# StopIteration
```
