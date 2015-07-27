# 类的混合

## 问题

你有一些通用方法，你想把他们包含到很多不同的类中。

## 解决方案

使用 mixOf 库函数，它会生成一个混合父类。

```
mixOf = (base, mixins...) ->
  class Mixed extends base
  for mixin in mixins by -1 #earlier mixins override later ones
    for name, method of mixin::
      Mixed::[name] = method
  Mixed

...

class DeepThought
  answer: ->
    42
    
class PhilosopherMixin
  pontificate: ->
    console.log "hmm..."
    @wise = yes

class DeeperThought extends mixOf DeepThought, PhilosopherMixin
  answer: ->
    @pontificate()
    super()
    
earth = new DeeperThought
earth.answer()
# hmm...
# => 42

```

## 讨论

这适用于轻量级的混合。因此你可以从基类和基类的祖先中继承方法，也可以从混合类的基类和祖先中继承，但是不能从混合类的祖先中继承。与此同时，在声明了一个混合类后，此后的对这个混合类进行的改变是不会反应出来的。

