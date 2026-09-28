# zip()

## 1. 基本概念

`zip()` 是 Python 内置函数，返回一个 `zip` 类型的迭代器。


它会将多个可迭代对象中**相同位置的元素进行配对**。

```python
zip([1, 2, 3], ['a', 'b', 'c'])
```

依次产生：

```text
(1, 'a')
(2, 'b')
(3, 'c')
```

---

## 2. zip本质

`zip` 返回的是迭代器，因此可以使用 `next()` 逐个获取元素：

```python
z = zip([1, 2, 3], ['a', 'b', 'c'])

print(next(z))  # (1, 'a')
print(next(z))  # (2, 'b')
print(next(z))  # (3, 'c')
```

也可以被 `for` 遍历：

```python
for item in zip([1, 2], ['a', 'b']):
    print(item)
```


## 3. 核心记忆

- `zip()` → 配对相同位置的元素
- 返回 `zip` 类型的迭代器
- `list(zip(...))` → 转成列表
- `dict(zip(keys, values))` → 两个列表直接组成字典
- `zip` 通常按照最短的可迭代对象结束

# Python 可迭代对象与迭代协议

## 1. 可迭代对象

可迭代对象（iterable）是可以被 `for` 循环遍历的对象，如：

```python
list
tuple
str
dict
set
range
```

它们之所以能使用 `for`，是因为实现了 Python 的**迭代协议**。

---

## 2. `for` 循环本质

```python
for x in a:
    print(x)
```

内部近似等价于：

```python
it = iter(a)

while True:
    try:
        x = next(it)
        print(x)
    except StopIteration:
        break
```

其中：

```python
iter(a)      # 获取迭代器
next(it)     # 获取下一个元素
```

---

## 3. 迭代协议

`iter(a)` 会调用：

```python
a.__iter__()
```

`next(it)` 会调用：

```python
it.__next__()
```

所以整体关系是：

```text
可迭代对象
   ↓ __iter__()
迭代器
   ↓ __next__()
元素
   ↓
StopIteration
```

可迭代对象负责**提供迭代器**，迭代器负责**记录遍历位置并逐个返回元素**。

---

## 4. 自造一个可迭代类型

```python
class MyList:
    def __init__(self, data):
        self.data = data

    def __iter__(self):
        return MyIterator(self.data)


class MyIterator:
    def __init__(self, data):
        self.data = data
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.index >= len(self.data):
            raise StopIteration

        value = self.data[self.index]
        self.index += 1
        return value
```

使用：

```python
a = MyList([10, 20, 30])

for x in a:
    print(x)
```

输出：

```text
10
20
30
```

核心：

```text
MyList
  ↓ __iter__()
MyIterator
  ↓ __next__()
10 → 20 → 30
  ↓
StopIteration
```

## 5. 关键结论

Python 的 `for` 循环并不关心对象是不是 `list`、`str` 等具体类型，只关心对象是否实现了迭代协议：

```text
__iter__() + __next__() + StopIteration
```