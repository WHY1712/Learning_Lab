
# range() 和 list()

## 1. range()

`range()` 是 Python 内置函数，用于生成整数序列, 返回对象是一个类型为range的可迭代对象

```python
range(start, stop, step)
```

参数：
- `start`：起始值，包含，默认 0
- `stop`：结束值，不包含
- `step`：步长，默认 1

```python
range(10, 51)
```
表示生成：
```text
10, 11, 12, ..., 50
```

注意：
- `stop` 不包含，所以 `range(10,50)` 最大只能到 49。

它是可迭代对象（iterable），可以用于：

```python
for i in range(5):
    print(i)
```

range 不会提前保存所有数字，而是保存：

```python
start
stop
step
```
需要元素时再计算，因此节省内存。

# list()

## 1. 基本概念

`list()` 是 Python 内置函数，用于将可迭代对象转换为列表。

格式：

```python
list(iterable)
```

例如：

```python
r = range(5)

a = list(r)

print(a)
```

输出：

```text
[0, 1, 2, 3, 4]
```

## 2. list转换逻辑

本质类似：

```python
result = []

for element in iterable:
    result.append(element)

return result
```
---

## 3. range 和 list 的区别

| | range | list |
|-|-|-|
| 类型 | range对象 | 列表对象 |
| 是否存储元素 | 否 | 是 |
| 是否可迭代 | 是 | 是 |
| 是否可修改 | 否 | 是 |
| 内存占用 | 小 | 较大 |

关系：

```python
range(10,51)
```

得到：

```text
range对象
```

转换：

```python
list(range(10,51))
```

得到：

```text
[10,11,12,...,50]
```

记忆：

- `range()`：生成数字规则，适合循环
- `list()`：把可迭代对象中的元素全部取出保存

# map和list长度问题

- map(function, iterable) 返回一个迭代器
- 迭代器不会保存所有元素，只负责逐个生成元素
- 因此 map 对象不能使用 len()

例：

map(int, input().split())

如果需要列表：

list(map(int, input().split()))

转换为列表后：
- 可以 len()
- 可以索引
- 会保存所有数据

如果不转换：
- 可以 for 遍历
- 需要自己统计数量

# f-string格式化

格式：
f'{变量:格式}'

常用：
:.2f  保留2位小数
:.3f  保留3位小数

f-string格式：

f'文本 {表达式:格式} 文本'

- 外层必须有引号，因为它本质是字符串
- {}里面可以放变量、计算表达式
- :.2f只是控制输出形式
- 格式化后的结果一定是字符串

# 列表解析

格式：
[表达式 for 变量 in 可迭代对象]

```
[i for i in range(10)]
```

等价于：

```
list = []
for i in range(10):
    list.append(i)
```

# list.pop()

- pop() 默认删除列表最后一个元素
- pop(index) 删除指定位置元素
- pop() 会改变原列表

例：
a = [1,2,3]

a.pop()

结果：
a = [1,2]


# join
案例：输出希望用空格相间
```
result = []

for i in range(1, 16):
    if i == 13:
        continue
    result.append(str(i))

print(' '.join(result))
```
直接用print()，末尾end默认有一个'\n'，难以改成空格且最后一位没有空格，因此用join方法

格式：
'分隔符'.join(字符串列表)

作用：
用指定字符连接列表元素。

例：
```
a = ['1','2','3']

' '.join(a)
```
结果：
'1 2 3'

- 注意：join只能连接字符串，所以数字需要：str(number)