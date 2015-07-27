# 回调绑定

## 问题

你想要把一个回调与一个对象绑定在一起。

## 方案

```
$ ->
  class Basket
    constructor: () ->
      @products = []

      $('.product').click (event) =>
        @add $(event.currentTarget).attr 'id'

    add: (product) ->
      @products.push product
      console.log @products

  new Basket()
```

## 讨论

通过使用等号箭头（=>）取代正常箭头（->），函数将会自动与对象绑定，并可以访问 @- 可变量。

