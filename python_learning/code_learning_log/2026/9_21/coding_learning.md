# 除法与取模运算学习
详见[牛客网](https://www.nowcoder.com/practice/9fe22ac9e55448f4920886f84efeac58?tpId=314&tqId=10055876&sourceUrl=%2Fexam%2Foj%3Fpage%3D1%26tab%3D%25E8%25AF%25AD%25E6%25B3%2595%25E7%25AF%2587%26topicId%3D314)

```
x=int(input())
y=int(input())

quotient=x // y
remainder=x%y

print(quotient,remainder)

float_result = x/y
print(f"{float_result:.2f}")
```

- input()输入为字符型，如果用作数字，必须加类型转换 int()

- // ；整除结果；% ：除余结果；/ ：除法结果，存在小数
  
- f-string（格式化字符串字面量）：在字符串引号前添加 `f` 或 `F`，Python 会解析字符串中的 `{}` 内容，并将其中的变量或表达式结果替换到对应位置。

- `{}`：用于包裹需要插入字符串中的变量或表达式，Python 会先计算其中的内容，再将结果转换为字符串输出。例如：`f"{x+1}"`。

- `:`：用于指定格式化规则，位于表达式之后，用于分隔数据内容和格式说明符。例如：`f"{x:.2f}"`。

- `.2f`：格式说明符，其中 `.2` 表示保留小数点后两位，`f` 表示使用浮点数格式输出。对于浮点数会自动进行四舍五入。


# map(int, input().split())

用于一次输入多个数据，并将输入内容转换为指定类型。

执行过程：

1. `input()`：读取一行输入，返回字符串。
2. `split()`：按照空格切分字符串，得到多个字符串元素。
3. `map(int, ...)`：将每个字符串转换为整数。
4. 使用多个变量接收转换后的结果。

例如：

```python
a, b = map(int, input().split())
···