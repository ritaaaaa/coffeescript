# 匹配字符串

## 问题

你想要匹配两个或多个字符串。

## 解决方案

计算把一个字符串转换成另一个字符串所需的编辑距离或操作数目。

```

	levenshtein = (str1, str2) ->
    
    l1 = str1.length
    l2 = str2.length
    prevDist = [0..l2]
    nextDist = [0..l2]

    for i in [1..l1] by 1
      nextDist[0] = i
      for j in [1..l2] by 1
        if (str1.charAt i-1) == (str2.charAt j-1)
          nextDist[j] = prevDist[j-1]
        else
          nextDist[j] = 1 + Math.min prevDist[j], nextDist[j-1], prevDist[j-1]
        [prevDist,nextDist]=[nextDist, prevDist]
    
    prevDist[l2]

```

## 讨论

可以使用赫斯伯格（Hirschberg）或瓦格纳菲舍尔（Wagner–Fischer）的算法来计算来文史特（Levenshtein）距离。这个例子用的是瓦格纳菲舍尔算法。

这个版本的文史特算法和内存呈线性关系，和时间呈二次方关系。

在这里我们使用 str.charAt i 这种表示法而不用 str[i] 这种方式，是因为后者在某些浏览器（如 IE7）中不支持。

起初，"by 1" 在两次循环中看起来似乎是没用的。它在这里是用来避免一个 coffeescript [i..j] 语法的常见错误。如果 str1 或 str2 为空字符串，那么 [1..l1] 或 [1..l2] 将会返回 [1,0]。添加了 "by 1" 的循环也能编译出更加简洁高效的 javascript。

最后，循环结尾处对回收数组的优化在这里主要是为了演示 coffeescript 中交换两个变量的语法。
