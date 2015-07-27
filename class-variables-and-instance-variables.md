# 类变量和实例变量

## 问题

你想创建类变量和实例变量（属性）。


## 解决方案

### 类变量

```

  class Zoo
  	@MAX_ANIMALS: 50
  	MAX_ZOOKEEPERS: 3
  
  	helpfulInfo: =>
       "Zoos may contain a maximum of #{@constructor.MAX_ANIMALS} animals and #{@MAX_ZOOKEEPERS} zoo
       keepers."

	Zoo.MAX_ANIMALS
	# => 50

	Zoo.MAX_ZOOKEEPERS
	# => undefined (it is a prototype member)

	Zoo::MAX_ZOOKEEPERS
	# => 3

	zoo = new Zoo
	zoo.MAX_ZOOKEEPERS
	# => 3
	zoo.helpfulInfo()
	# => "Zoos may contain a maximum of 50 animals and 3 zoo keepers."

	zoo.MAX_ZOOKEEPERS = "smelly"
	zoo.MAX_ANIMALS = "seventeen"
	zoo.helpfulInfo()
	# => "Zoos may contain a maximum of 50 animals and smelly zoo keepers."

```

### 实例变量

你必须在一个类的方法中才能定义实例变量（例如属性），在 constructor 结构中初始化你的默认值。

```

	class Zoo
  	constructor: ->
    @animals = [] # Here the instance variable is defined
    
  	addAnimal: (name) ->
    @animals.push name


	zoo = new Zoo()
	zoo.addAnimal 'elephant'

	otherZoo = new Zoo()
	otherZoo.addAnimal 'lion'

	zoo.animals
	# => ['elephant']

	otherZoo.animals
	# => ['lion']

```

## 警告！

不要试图在 constructor 外部添加变量（即使在 [elsewhere](http://arcturo.github.io/library/coffeescript/03_classes.html#content) 中提到了，由于潜在的 JavaScript 的原型概念，这不会像预期那样运行正确，）。

```

	class BadZoo
  	animals: []           # Translates to BadZoo.prototype.animals = []; and is thus shared between instances
    
  	addAnimal: (name) ->
    @animals.push name  # Works due to the prototype concept of Javascript


	zoo = new BadZoo()
	zoo.addAnimal 'elephant'

	otherZoo = new BadZoo()
	otherZoo.addAnimal 'lion'

	zoo.animals
	# => ['elephant','lion'] # Oops...

	otherZoo.animals
	# => ['elephant','lion'] # Oops...

	BadZoo::animals
	# => ['elephant','lion'] # The value is stored in the prototype

```

## 讨论

Coffeescript 会将类变量的值保存在类中而不是它定义的原型中。这在定义类中的变量时是十分有用的，因为这不会被实体属性变量重写。
