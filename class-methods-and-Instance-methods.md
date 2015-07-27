# 类方法和实例方法

## 问题

你想创建类和实例的方法。


## 解决方案

### 类方法

```

	class Songs
  	@_titles: 0    # Although it's directly accessible, the leading _ defines it by convention as private property.

  	@get_count: ->
    	@_titles

  	constructor: (@artist, @title) ->
    @constructor._titles++     # Refers to <Classname>._titles, in this case Songs.titles

	Songs.get_count()
	# => 0

	song = new Songs("Rick Astley", "Never Gonna Give You Up")
	Songs.get_count()
	# => 1

	song.get_count()
	# => TypeError: Object <Songs> has no method 'get_count'

```

### 实例方法

```

	class Songs
  	_titles: 0    # Although it's directly accessible, the leading _ defines it by convention as private property.

  	get_count: ->
    @_titles

  	constructor: (@artist, @title) ->
    @_titles++

	song = new Songs("Rick Astley", "Never Gonna Give You Up")
	song.get_count()
	# => 1

	Songs.get_count()
	# => TypeError: Object function Songs(artist, title) ... has no method 'get_count'

```

## 讨论

Coffeescript 会在对象本身中保存类方法（也叫静态方法），而不是在对象原型中（以及单一的对象实例），在保存了记录的同时也将类级的变量保存在中心位置。
