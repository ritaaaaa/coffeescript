# 平方根倒数快速算法

## 问题

你想[快速](https://en.wikipedia.org/wiki/Fast_inverse_square_root)计算某数的平方根倒数。

## 方案

在 Quake Ⅲ Arena 的[源代码](ftp://ftp.idsoftware.com/idstuff/source/quake3-1.32b-source.zip)中，这个奇怪的算法对一个幻数进行整数运算，来计算平方根倒数的浮点近似值。

在 CoffeeScript 中，我使用经典原始的变量，以及由 [Chris Lomont](http://www.lomont.org/Math/Papers/2003/InvSqrt.pdf) 发现的新的最优 32 位幻数。除此之外，我还使用 64 位大小的幻数。

另一特征是可以通过控制[牛顿迭代法](https://en.wikipedia.org/wiki/Newton%27s_method)的迭代次数来改变其精确度。

相比于传统的，该算法在性能上更胜一筹，归功于使用的机器及其精确度。

运行的时候使用 coffee 来编译 script： coffee -c script.coffee

然后复制粘贴编译的 JS 代码到浏览器的 JavaScript 控制台。

注意：你需要一个支持[类型数组](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Typed_arrays)的浏览器

参考文献：   
1. <ftp://ftp.idsoftware.com/idstuff/source/quake3-1.32b-source.zip>   
2. <http://www.lomont.org/Math/Papers/2003/InvSqrt.pdf>  
3. <http://en.wikipedia.org/wiki/Newton%27s_method>  
4. <https://developer.mozilla.org/en/JavaScripttypedarrays>  
5. <http://en.wikipedia.org/wiki/Fastinversesquare_root>  

以下的代码来源于：<https://gist.github.com/1036533>

```
###

Author: Jason Giedymin <jasong _a_t_ apache -dot- org>
        http://www.jasongiedymin.com
        https://github.com/JasonGiedymin

在 Quake Ⅲ Arena 的源代码 [1] 中，这个奇怪的算法对一个幻数进行整数运算，来计算平方根倒数的浮点近似值 [5]。

在 CoffeeScript 中，我使用经典原始的变量，以及由 Chris Lomont [2] 发现的新的最优 32 位幻数。除此之外，我还使用 64 位大小的幻数。

另一特征是可以通过控制牛顿迭代法 [3] 的迭代次数来改变其精确度。

相比于传统的，该算法在性能上更胜一筹，归功于使用的机器及其精确度。

运行的时候使用 coffee 来编译 script： 
coffee -c script.coffee

然后复制粘贴编译的 JS 代码到浏览器的 JavaScript 控制台。

注意：你需要一个支持类型数组 [4] 的浏览器

参考文献： 
[1] ftp://ftp.idsoftware.com/idstuff/source/quake3-1.32b-source.zip
[2] http://www.lomont.org/Math/Papers/2003/InvSqrt.pdf
[3] http://en.wikipedia.org/wiki/Newton%27s_method
[4] https://developer.mozilla.org/en/JavaScript_typed_arrays
[5] http://en.wikipedia.org/wiki/Fast_inverse_square_root

###

approx_const_quake_32 = 0x5f3759df # See [1]
approx_const_32 = 0x5f375a86 # See [2]
approx_const_64 = 0x5fe6eb50c7aa19f9 # See [2]

fastInvSqrt_typed = (n, precision=1) ->
    # 使用类型数组。现在只能在浏览器中操作。
    # Node.JS 的版本即将推出。

    y = new Float32Array(1)
    i = new Int32Array(y.buffer)

    y[0] = n
    i[0] = 0x5f375a86 - (i[0] >> 1)
    
    for iter in [1...precision]
        y[0] = y[0] * (1.5 - ((n * 0.5) * y[0] * y[0]))
    
    return y[0]

### 单次运行示例###
testSingle = () ->
    example_n = 10

    console.log("Fast InvSqrt of 10, precision 1: #{fastInvSqrt_typed(example_n)}")
    console.log("Fast InvSqrt of 10, precision 5: #{fastInvSqrt_typed(example_n, 5)}")
    console.log("Fast InvSqrt of 10, precision 10: #{fastInvSqrt_typed(example_n, 10)}")
    console.log("Fast InvSqrt of 10, precision 20: #{fastInvSqrt_typed(example_n, 20)}")
    console.log("Classic of 10: #{1.0 / Math.sqrt(example_n)}")

testSingle()
```
