---
title: Python的For/Else语句
tags:
  - python
  - 自学记录
abbrlink: 78b59d9f
date: 2021-05-21 18:05:39
---

## 知识点

首先我们知道可以像下面这样使用循环：

```python
fruits = ['apple', 'banana', 'mango']
for fruit in fruits:
    print(fruit.capitalize())
```

这是 for 循环的基本结构，现在让我们继续讨论 python 的 for 循环中的一个鲜为人知的特性—— else 子句。

for 循环也有一个大多数人都不熟悉 else 子句，该 else 子句在循环正常完成时执行，这意味着循环没有遇到任何 break 语句。当你明白了在哪里使用它们，就会非常有用。

常见的状况是运行一个循环并搜索一个项，如果找到该项目，我们使用 break 跳出循环。有两种情况可能会导致循环结束。第一个是找到该项并 break  ，第二种情况是循环自然结束。现在我们可能想知道其中的哪一个是循环完成的原因，一种方法是设置一个标志，然后在循环结束时检查它，另一个是使用该  else 子句。

下面是一个`for/else`循环的基本结构：

```python
for item in container:
    if search_something(item):
        # Found it!
        process(item)
        break
else:
    # Didn't find anything..
    not_found_in_container()
```

下面这个简单的例子来自官方文档：

```python
for n in range(2, 10):
    for x in range(2, n):
        if n % x == 0:
            print(n, 'equals', x, '*', n/x)
            break
```

它找出 2 到 10 之间因数。现在开始有趣的部分，我们可以添加一个额外的 else 子句块来捕捉素数并打印：

```python
for n in range(2, 10):
    for x in range(2, n):
        if n % x == 0:
            print( n, 'equals', x, '*', n/x)
            break
    else:
        # loop fell through without finding a factor
        print(n, 'is a prime number')
```

## 例题

输出指定范围内的素数：

```python
lower = int(input())
upper = int(input())

for num in range(lower, upper+1):
    if num>1:
        for i in range(2, num):
            if(num%i)==0:
                break
        else:
            print(num)
```



