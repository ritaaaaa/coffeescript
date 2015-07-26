# 适配器模式

## 问题

想象你去国外旅行，一旦你意识到你的电源线插座与酒店房间墙上的插座不兼容。幸运的是，你记得带你的电源适配器。它将一边连接你的电源线插座另一边连接墙壁插座，允许它们之间进行通信。

同样的情况也可能会出现在代码中，当两个(或更多)实例(类、模块等)想跟对方通信，但其通信协议(e.i.他们所使用的语言交流)不同。在这种情况下，[Adapter](http://en.wikipedia.org/wiki/Adapter_pattern) 模式更方便。它会充当翻译，从一边到另一边。

## 方案
```
# a fragment of 3-rd party grid component
class AwesomeGrid
    constructor: (@datasource)->
        @sort_order = 'ASC' 
        @sorter = new NullSorter # in this place we use NullObject pattern (another useful pattern)
    setCustomSorter: (@customSorter) ->
        @sorter = customSorter
    sort: () ->
        @datasource = @sorter.sort @datasource, @sort_order
        # don't forget to change sort order


class NullSorter
    sort: (data, order) -> # do nothing; it is just a stub
    
class RandomSorter
    sort: (data)->
        for i in [data.length-1..1] #let's shuffle the data a bit
                j = Math.floor Math.random() * (i + 1)
                [data[i], data[j]] = [data[j], data[i]]
        return data

class RandomSorterAdapter
    constructor: (@sorter) ->
    sort: (data, order) ->
        @sorter.sort data

agrid = new AwesomeGrid ['a','b','c','d','e','f']
agrid.setCustomSorter new RandomSorterAdapter(new RandomSorter)
agrid.sort() # sort data with custom sorter through adapter
```

## 讨论

当你要组织两个具有不同接口的对象之间的交互时，适配器是有用的。它可以发生当你使用第三方库或者使用遗留代码。在任何情况下小心使用适配器：它可以是有用的，但它也可以导致设计错误。
