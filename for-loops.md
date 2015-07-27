# For 循环

## 问题

你想通过一个 for 循环来迭代数组、对象或范围。

## 解决方案

```
# for(i = 1; i<= 10; i++)
x for x in [1..10]
# => [ 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 ]

# To count by 2
# for(i=1; i<= 10; i=i+2)
x for x in [1..10] by 2
# => [ 1, 3, 5, 7, 9 ]

# Perform a simple operation like squaring each item.
x * x for x in [1..10]
# = > [1,4,9,16,25,36,49,64,81,100]
```

## 讨论

CoffeeScript 使用推导（comprehension）来代替 for 循环，这些推导最终会被编译成 JavaScript 中等价的 for 循环。
