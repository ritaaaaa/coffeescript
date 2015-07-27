# 对象的链式调用


## 问题

你想调用一个对象上的多个方法，但不想每次都引用该对象。

## 解决方案

在每次链式调用后返回 this（即@）对象

```

class CoffeeCup
    constructor:  ->
        @properties=
            strength: 'medium'
            cream: false
            sugar: false
    strength: (newStrength) ->
        @properties.strength = newStrength
        this
    cream: (newCream) ->
        @properties.cream = newCream
        this
    sugar: (newSugar) ->
        @properties.sugar = newSugar
        this

morningCup = new CoffeeCup()

morningCup.properties # => { strength: 'medium', cream: false, sugar: false }

eveningCup = new CoffeeCup().strength('dark').cream(true).sugar(true)

eveningCup.properties # => { strength: 'dark', cream: true, sugar: true }


```


## 讨论

jQuery 库使用类似的手段从每一个相关的方法中返回选择符对象，并在后续方法中通过调整选择的范围修改该对象：

```

	$('p').filter('.topic').first()

```

对我们自己对象而言，一点点元编程就可以自动设置这个过程并明确声明返回 this 的意图。

```

	addChainedAttributeAccessor = (obj, propertyAttr, attr) ->
    obj[attr] = (newValues...) ->
        if newValues.length == 0
            obj[propertyAttr][attr]
        else
            obj[propertyAttr][attr] = newValues[0]
            obj

	class TeaCup
    properties:
        size: 'medium'
        type: 'black'
        sugar: false
        cream: false

	addChainedAttributeAccessor(TeaCup.prototype, 'properties', attr) for attr of TeaCup.prototype.properties

	earlgrey = new TeaCup().size('small').type('Earl Grey').sugar('false')

	earlgrey.properties # => { size: 'small', type: 'Earl Grey', sugar: false }

	earlgrey.sugar true

	earlgrey.sugar() # => true

```

