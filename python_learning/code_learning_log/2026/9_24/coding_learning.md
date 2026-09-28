# 2026-09-24 Python 学习笔记

## 1. 列表推导式

### 核心概念与语法

列表推导式遍历可迭代对象，计算表达式，并把结果放入新列表。

```python
[表达式 for 变量 in 可迭代对象]
```

它等价于以下循环结构（变量名仅用于说明）：

```python
结果列表 = []
for 变量 in 可迭代对象:
    结果列表.append(表达式)
```

### 嵌套列表推导式

原笔记中的代码：

```python
list=[[x*n for x in row] for row in list]
```

外层逐行取得 `row`，内层逐个取得 `x`，计算 `x * n` 并组成新的一行。展开为循环是：

```python
result=[]
for row in list:
    re=[]
    for x in row:
        re.append(x*n)
    result.append(re)
```

两段代码生成的列表内容相同。第一段把结果赋回 `list`，第二段把结果放在 `result`；若需连赋值也对应，还要执行 `list = result`。二者都构造新列表，没有逐项修改原内层列表。

**原代码问题：**代码要求 `list` 已是二维列表、`n` 已定义；把变量命名为 `list` 还会遮蔽内置的 `list()`。**原因：**这段代码是基于已有变量的片段。**修改后：**

```python
rows = [[1, 2], [3, 4]]
n = 2
scaled_rows = [[x * n for x in row] for row in rows]
print(scaled_rows)
```

输出：

```text
[[2, 4], [6, 8]]
```

## 2. 元组（tuple）

### 核心概念

元组是**有序、不可修改**的数据容器。列表 `list` 可以修改，常用 `[]` 表示；元组 `tuple` 不能给已有元素位置重新赋值，常用 `()` 表示。

```python
t = (1, 2, 3)
```

元组本身不可修改，但如果其中的某个元素是可变对象，该对象内部仍可改变。

### 索引与遍历

```python
t[0]    # 第一个元素
t[-1]   # 最后一个元素
```

```python
for x in t:
    print(x)
```

元组与列表一样支持索引、遍历。上述循环依次输出 `1`、`2`、`3`。

### 不可修改

列表的元素位置可以重新赋值：

```python
a = [1,2,3]
a[0] = 10   # 可以
```

元组的元素位置不能重新赋值：

```python
t = (1,2,3)
t[0] = 10   # 报错
```

**原代码问题：**`t[0] = 10` 抛出 `TypeError`。**原因：**元组不支持按索引赋值。**修改后：**如果需要修改元素，就使用列表，如上面的 `a[0] = 10`；如果需要固定数据，则保留元组并避免赋值。

### 单元素元组

```python
(1)
```

这是整数，不是元组。正确写法是：

```python
(1,)
```

单元素元组必须有逗号；决定元组类型的是逗号。

### 使用场景与列表对比

保存固定数据：

```python
point = (10,20)
```

函数返回多个值时，原笔记中的函数体代码片段是：

```python
return x,y
```

实际返回的是元组，相当于：

```python
(x,y)
```

| 特性 | `list` | `tuple` |
| --- | --- | --- |
| 常用符号 | `[]` | `()` |
| 元素位置可否重新赋值 | 可以 | 不可以 |
| 索引 | 支持 | 支持 |
| 遍历 | 支持 | 支持 |

记忆：元组可以像列表一样按顺序读取元素，但不能重新赋值其中的位置。

## 3. `try-except` 异常处理

### 作用与基本语法

`try-except` 捕获和处理程序运行中的异常。对预料到的错误进行处理，可以让程序按处理逻辑继续运行；不同类型的错误可以分别处理。

```python
try:
    可能发生错误的代码

except 错误类型:
    错误处理代码
```

这里的中文内容是语法占位说明，实际运行时须换成 Python 语句。执行流程：先执行 `try`；没有异常则跳过 `except`；出现匹配的异常时，停止执行 `try` 中剩余代码并进入对应的 `except`；不匹配的异常不会由该分支处理。

### 除零示例

```python
try:
    a = 10 / 0

except ZeroDivisionError:
    print("不能除以0")
```

执行 `10 / 0` → 产生 `ZeroDivisionError` → 进入对应的 `except`。输出：

```text
不能除以0
```

### 处理多种异常

```python
try:
    x = int(input())
    y = 10 / x

except ValueError:
    print("输入不是数字")

except ZeroDivisionError:
    print("不能输入0")
```

输入 `abc` 时，`int(input())` 抛出 `ValueError`，输出 `输入不是数字`；输入 `0` 时，`10 / x` 抛出 `ZeroDivisionError`，输出 `不能输入0`。不同错误进入不同的 `except`。

### 常见 Python 异常类型

| 异常类型 | 含义 | 常见情况 |
| --- | --- | --- |
| `TypeError` | 类型错误 | 不兼容的类型参与运算；修改不可变对象 |
| `ValueError` | 值错误 | 类型可接受，但值不符合要求 |
| `IndexError` | 索引错误 | 列表或元组索引超出范围 |
| `KeyError` | 键错误 | 字典中不存在对应键 |
| `NameError` | 名称未定义 | 使用未定义的变量 |
| `ZeroDivisionError` | 除零错误 | 数字除以 `0` |
| `AttributeError` | 属性错误 | 对象没有该属性或方法 |
| `FileNotFoundError` | 文件不存在 | 打开不存在的文件 |
| `ImportError` | 导入错误 | 模块或模块中的名称无法导入 |

### 常见错误示例

**1. `TypeError`：类型错误**

```python
a = "10"
b = 5

a + b
```

原代码问题：抛出 `TypeError`。原因：字符串不能和整数直接相加。修改后可先统一类型，如 `int(a) + b`，结果为 `15`。

**2. `ValueError`：值错误**

```python
int("abc")
```

原代码问题：抛出 `ValueError`。原因：`"abc"` 无法转换为整数。修改后可使用有效数字文本，如 `int("123")`，结果为 `123`；也可用 `try-except` 处理无效输入。

**3. `IndexError`：索引错误**

```python
a = [1,2,3]

a[5]
```

原代码问题：抛出 `IndexError`。原因：这个列表只有索引 `0`、`1`、`2`。修改后可用 `a[2]`，结果为 `3`。

**4. `KeyError`：键错误**

```python
d = {"name":"Tom"}

d["age"]
```

原代码问题：抛出 `KeyError`。原因：字典中没有 `"age"`。修改后可用已有键 `d["name"]`，或用 `d.get("age")` 处理可能缺失的键。

**5. `ZeroDivisionError`：除零错误**

```python
10 / 0
```

原代码问题：抛出 `ZeroDivisionError`。原因：除数不能为 `0`。修改后使用非零除数，或在除法前检查除数。

**6. `AttributeError`：属性错误**

```python
a = [1,2,3]

a.push(4)
```

原代码问题：抛出 `AttributeError`。原因：Python 列表没有 `push()` 方法。修改后使用 `a.append(4)`，列表变为 `[1, 2, 3, 4]`。

### 记忆与使用场景

原笔记中的记忆写法（中文内容是占位说明）：

```python
try:
    尝试执行
except:
    出错处理
```

`try` 负责尝试运行，`except` 负责错误发生后的处理。实际代码中优先指定预期的异常类型；不指定类型的 `except:` 捕获范围很广，容易掩盖其他问题。

常用于用户输入检查、文件读写、网络请求和数据转换。只有相应错误被正确捕获、处理，程序才能避免因该错误直接退出。

## 4. 字典（dict）、排序与格式化输出

### 创建字典

字典使用 `{}` 创建，存储键值对：

```python
operators = {
    '<': 'less than',
    '==': 'equal'
}
```

通用结构：

```python
{
    key: value
}
```

`key` 是用于查找的唯一键，`value` 是对应的数据。字典键还必须是可哈希对象，例如字符串或整数。

### 访问元素

通过键取得值：

```python
operators['<']
```

结果：

```text
less than
```

键可能不存在时，可以安全访问：

```python
operators.get('<')
```

这里也得到 `'less than'`。若键不存在且未提供默认值，`get()` 返回 `None`，而使用方括号会抛出 `KeyError`。

### 新增或修改元素

原笔记用下面的片段表示按键赋值：

```python
dict[key] = value
```

键不存在时新增：

```python
dict['>'] = 'greater than'
```

键已存在时修改：

```python
dict['<'] = 'smaller'
```

**原代码问题：**这些片段把 `dict` 当成已定义的字典变量；直接运行时若未定义会报错，且该名称会遮蔽内置类型 `dict`。**原因：**它们原本用于说明赋值规则。**修改后：**使用前面已创建的变量：

```python
operators['>'] = 'greater than'
operators['<'] = 'smaller'
```

此时 `operators['>']` 为 `'greater than'`，`operators['<']` 已更新为 `'smaller'`。

### 遍历字典

原笔记中遍历键的写法（假设 `dict` 是已定义的字典）：

```python
for key in dict.keys():
    print(key)
```

同时遍历键和值：

```python
for key, value in dict.items():
    print(key, value)
```

`keys()` 提供键，`items()` 提供键值对；第二段循环把每对中的键和值分别交给两个变量。实际编程可将 `dict` 改为 `operators`。

### `sorted()` 与排序遍历

`sorted()` 对可迭代对象中的元素排序，返回**新列表**，不改变原对象：

```python
sorted(['b', 'a', 'c'])
```

结果：

```python
['a', 'b', 'c']
```

原笔记中的字典排序遍历片段：

```python
for key in sorted(dict.keys()):
    print(key)
```

`dict.keys()` 取得键，`sorted()` 生成排序后的键列表，`for` 逐个遍历。这里的 `dict` 同样是示意名称，实际代码可用 `operators`。如需同时取得值，在循环内用 `operators[key]`。

### f-string 输出

f-string 用于在字符串中插入变量或表达式：

```python
f'{变量}'
```

示例：

```python
name = 'Tom'
print(f'Hello {name}')
```

输出：

```text
Hello Tom
```

构造字符串时，`{name}` 会替换成变量的值。

### 核心流程

创建字典 → `keys()` 取键 → `sorted()` 排序 → `for` 遍历 → `operators[key]` 取值；要新增或修改，则用 `operators[key] = value`。排序影响遍历顺序，不改变字典内容。
