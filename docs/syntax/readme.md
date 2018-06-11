# 语法简记

### 数据类型

[基本数据类型](https://p-jiangh.github.io/python-notebook/docs/syntax/data-type#基本数据类型)

[集合数据类型](https://p-jiangh.github.io/python-notebook/docs/syntax/data-type#集合数据类型)

[不可变对象和可变对象](https://p-jiangh.github.io/python-notebook/docs/syntax/data-type#不可变对象和可变对象)

[切片操作](https://p-jiangh.github.io/python-notebook/docs/syntax/data-type#切片操作)

### 结构控制语句

#### 条件判断`if-elif-else`

```python
if condition1:
  ...
elif condition2:
  ...
elif condition3:
  ...
else:
  ...
```

#### 循环

`for` 循环：

```python
  for x in [1, 2, 3]:         # 迭代单值
    print(x)

  for i, x in [1, 2, 3]:      # 迭代单值， 索引-元素对
    print(x)

  for x, y in [(1, 2), (2, 3), (3, 4)]:      # 迭代多值
    print(x, y)
```

`while` 循环：

```py
  n = 0
  while n > 5:
    n += 1
    print(n)
```

`break` 与 `continue` 功能与其他语言相同

### 函数

[函数声明](https://p-jiangh.github.io/python-notebook/docs/syntax/function#函数声明)

[函数参数](https://p-jiangh.github.io/python-notebook/docs/syntax/function#函数参数)

[递归函数](https://p-jiangh.github.io/python-notebook/docs/syntax/function#递归函数)

### 列表生成式与生成器(generator)

[]
