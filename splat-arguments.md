# 提示参数

## 问题

你的函数将会被可变数量的参数所调用。

## 方案

使用*提示*。

```
loadTruck = (firstDibs, secondDibs, tooSlow...) ->
    truck:
        driversSeat: firstDibs
        passengerSeat: secondDibs
        trunkBed: tooSlow

loadTruck("Amanda", "Joel")
# => { truck: { driversSeat: "Amanda", passengerSeat: "Joel", trunkBed: [] } }

loadTruck("Amanda", "Joel", "Bob", "Mary", "Phillip")
# => { truck: { driversSeat: "Amanda", passengerSeat: "Joel", trunkBed: ["Bob", "Mary", "Phillip"] } }
```

使用尾部参数：

```
loadTruck = (firstDibs, secondDibs, tooSlow..., leftAtHome) ->
    truck:
        driversSeat: firstDibs
        passengerSeat: secondDibs
        trunkBed: tooSlow
    taxi:
        passengerSeat: leftAtHome

loadTruck("Amanda", "Joel", "Bob", "Mary", "Phillip", "Austin")
# => { truck: { driversSeat: 'Amanda', passengerSeat: 'Joel', trunkBed: [ 'Bob', 'Mary', 'Phillip' ] }, taxi: { passengerSeat: 'Austin' } }

loadTruck("Amanda")
# => { truck: { driversSeat: "Amanda", passengerSeat: undefined, trunkBed: [] }, taxi: undefined }
```

## 讨论

通过在函数其中的（不多于）一个参数之后添加一个省略号（...），CoffeeScript 能把所有不被其他命名参数采用的参数值整合进一个列表中。就算并没有提供命名参数，它也会制造一个空列表。

