### 结构控制语句

#### 条件判断

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
