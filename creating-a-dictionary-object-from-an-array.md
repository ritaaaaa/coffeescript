# 由数组创建一个对象词典

## 问题

你有一组对象，例如：

```

	cats = [
  	{
    	name: "Bubbles"
    	age: 1
  	},
  	{
    	name: "Sparkle"
    	favoriteFood: "tuna"
  	}
	]

```

但是你想让它像词典一样，可以通过关键字访问它，就像使用 cats["Bubbles"]。

## 解决方案

你需要将你的数组转换为一个对象。通过这样使用 reduce：

```

	# key = The key by which to index the dictionary
	Array::toDict = (key) ->
  	@reduce ((dict, obj) -> dict[ obj[key] ] = obj if obj[key]?; return dict), {}

```

使用它时像下面这样：

```

	catsDict = cats.toDict('name')
  	catsDict["Bubbles"]
  	# => { age: 1, name: "Bubbles" }

```

## 讨论

另一种方法是使用数组推导：

```

	Array::toDict = (key) ->
  	dict = {}
  	dict[obj[key]] = obj for obj in this when obj[key]?
  	dict

```

如果你使用 Underscore.js，你可以创建一个 mixin：

```

	_.mixin toDict: (arr, key) ->
    	throw new Error('_.toDict takes an Array') unless _.isArray arr
    _.reduce arr, ((dict, obj) -> dict[ obj[key] ] = obj if obj[key]?; return dict), {}
	catsDict = _.toDict(cats, 'name')
	catsDict["Sparkle"]
	# => { favoriteFood: "tuna", name: "Sparkle" }

```

