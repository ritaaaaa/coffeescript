# 产生可预测的随机数

## 问题

你需要生成在一定范围内的随机数，但你也需要对发生器进行“生成种子”操作来提供可预测的值。

## 方案

编写你自己的随机数生成器。当然有很多方法可以做到这一点，这里给出一个简单的示例。 *该发生器绝对不可以以加密为目的！*

```
class Rand
  # 如果没有种子创建，使用当前时间作为种子
  constructor: (@seed) ->
    # Knuth and Lewis' improvements to Park and Miller's LCPRNG
    @multiplier = 1664525
    @modulo = 4294967296 # 2**32-1;
    @offset = 1013904223
    unless @seed? && 0 <= seed < @modulo
      @seed = (new Date().valueOf() * new Date().getMilliseconds()) % @modulo

  # 设置新的种子值
  seed: (seed) ->
    @seed = seed

  # 返回一个随机整数满足 0 <= n < @modulo
  randn: ->
    # new_seed = (a * seed + c) % m
    @seed = (@multiplier*@seed + @offset) % @modulo

 # 返回一个随机浮点满足 0 <= f < 1.0
  randf: ->
    this.randn() / @modulo

  # 返回一个随机的整数满足 0 <= f < n
  rand: (n) ->
    Math.floor(this.randf() * n)

  #返回一个随机的整数满足min <= f < max
  rand2: (min, max) ->
    min + this.rand(max-min)
```

## 讨论

JavaScript 和 CoffeeScript 都不提供可产生随机数的发生器。编写发生器对于我们来说将是一个挑战，在于权衡量的随机性与发生器的简单性。对随机性的全面讨论已超出了本书的范围。如需进一步阅读，可参考 Donald Kunth 的 *The Art of Computer Programming* 第 Ⅱ 卷第 3 章的 “ Random Numbers ” ，以及 *Numerical Recipes in C* 第二版本第 7 章的“ Random Numbers ”。

但是，对于这个随机数发生器只有简单的解释。这是一个线性同余伪随机数发生器，其运行源于一条数学公式 I<sub>j+1</sub> = (aI<sub>j</sub>+c) % m，其中 a 是乘数，c 是加法偏移量，m 是模数。每次请求随机数时就会执行很大的乘法和加法运算——这里的“很大”与密钥空间有关——得到的结果将以模数的形式被返回密钥空间。

这个发生器的周期为 232。虽然它绝对不能以加密为目的，但是对于最简单的随机性要求来说，它是相当足够的。randn() 在循环之前将遍历整个密钥空间，下一个数由上一个来确定。

如果你想修补这个发生器，强烈建议你去阅读 Knuth 的 * The Art of Computer Programming * 中的第 3 章。随机数生成是件很容易弄糟的事情，然而 Knuth 会解释如何区分好的和坏的随机数生成。

不要把发生器的输出结果变成模数。如果你需要一个整数的范围，应使用分割的方法。线性同余发生器的低位是不具有随机性的。特别的是，它总是从偶数种子产生奇数，反之亦然。所以如果你需要一个随机的 0 或者 1，不要使用：

```
# NOT random! Do not do this!
r.randn() % 2
```

因为你肯定得不到随机数字。反而，你应该使用 r.rand(2)。
