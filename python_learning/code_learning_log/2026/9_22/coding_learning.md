# in 语法
```
club_mem = input().split()
mem = input()

print(mem in club_mem)
```

in 本质上是在判断某个元素是否存在于一个容器中。对于列表：
```
mem in club_mem
```

逻辑上相当于逐个比较：
```
for i in club_mem:
    if i == mem:
        ...
```
# map语法补充
- `map(function, iterable)`：对可迭代对象中的每个元素执行 `function`
- `iterable`：可迭代对象，可理解为能被 `for` 遍历的数据，如 `list`、`tuple`、`str`
- `str.lower('GURR')` 和 `'GURR'.lower()` 结果都是 `'gurr'`
- `str.lower`：表示 `lower` 方法本身，可传给 `map`
- `str.lower()`：表示立即调用，会因缺少字符串参数而报错
- `map(str.lower, current_users)`：依次将 `current_users` 中的字符串转成小写
- `map()` 返回迭代器，需要查看全部结果时可用 `list(map(...))`
