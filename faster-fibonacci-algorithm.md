# 更快的 Fibonacci 算法  

## 问题  

你想计算出 Fibonacci 数列中的数值 N ，但需迅速地算出结果。  

## 方案  

下面的方案（仍有需改进的地方）最初在 Robin Houston的博客上被提出来。  

这里给出一些关于该算法和改进方法的链接：  
*<http://bosker.wordpress.com/2011/04/29/the-worst-algorithm-in-the-world/>  
*<http://www.math.rutgers.edu/~erowland/fibonacci>  
*<http://jsfromhell.com/classes/bignumber>  
*<http://www.math.rutgers.edu/~erowland/fibonacci>  
*<http://bigintegers.blogspot.com/2010/11/square-division-power-square-root>  
*<http://bugs.python.org/issue3451>

以下的代码来源于：<https://gist.github.com/1032685>

```
###
Author: Jason Giedymin <jasong _a_t_ apache -dot- org>
        http://www.jasongiedymin.com
        https://github.com/JasonGiedymin

CoffeeScript Javascript 的快速 Fibonacci 代码是基于 Robin Houston 博客里的 python 代码。
见下面的链接。

我要介绍一下 Newtonian，Burnikel / Ziegle 和Binet 关于大数目框架算法的实现。

Todo:
- https://github.com/substack/node-bigint
- BZ and Newton mods.
- Timing

###

MAXIMUM_JS_FIB_N = 1476

fib_bits = (n) ->
    #代表一个作为二进制数字阵列的整数

    bits = []
    while n > 0
        [n, bit] = divmodBasic n, 2
        bits.push bit

    bits.reverse()
    return bits

fibFast = (n) ->
    #快速 Fibonacci

    if n < 0
        console.log "Choose an number >= 0"
        return

    [a, b, c] = [1, 0, 1]

    for bit in fib_bits n
        if bit
            [a, b] = [(a+c)*b, b*b + c*c]
        else
            [a, b] = [a*a + b*b, (a+c)*b]

        c = a + b
        return b

divmodNewton = (x, y) ->
    throw new Error "Method not yet implemented yet."

divmodBZ = () ->
    throw new Error "Method not yet implemented yet."

divmodBasic = (x, y) ->
    ###
   这里并没有什么特别的。如果可能的话，也许以后的版本将是Newtonian 或者 Burnikel / Ziegler 的。
   ###

    return [(q = Math.floor x/y), (r = if x < y then x else x % y)]

start = (new Date).getTime();
calc_value = fibFast(MAXIMUM_JS_FIB_N)
diff = (new Date).getTime() - start;
console.log "[#{calc_value}] took #{diff} ms."  
```

