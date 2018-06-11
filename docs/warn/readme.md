# 注意点

* 很大或很小的浮点数需要使用科学计数法表示

* 字符串是一个集合（数组），由每一个UNICODE组成，python中没有char型

* 声明长度为1的tuple时写法为：

```python
tuple = (1,)
```

* python的代码结构是用缩进控制的，块语句别忘了冒号

* input()的返回类型是str

* 默认参数必须使用不可变值，如果要声明为可变值，需要使用下面代码的方式check

```python
def func(param=None):
  if param is None:
    param = []
  ..operations
  return
```

* dict型与JSON类似，注意避免混淆

* 赋值操作中支持将多个值赋值给多个变量

```py
a, b = 1, 2
```
