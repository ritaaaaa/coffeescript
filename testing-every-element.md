# 检测每个元素
## 问题
你希望能够在特定的情况下检测出在数组中的每个元素。
## 解决方案
使用 Array.every (ECMAScript 5):
```
evens = (x for x in [0..10] by 2)

evens.every (x)-> x % 2 == 0
# => true
```
Array.every 被加入到 Mozilla 的 Javascript 1.6 ，ECMAScript 5 标准。如果你的浏览器支持，但仍无法实施 EC5 ，那么请检查 [ _.all from underscore.js]( http://documentcloud.github.io/underscore/) 。  

对于一个真实例子，假设你有一个多选择列表，如下：
```
<select multiple id="my-select-list">
  <option>1</option>
  <option>2</option>
  <option>Red Car</option>
  <option>Blue Car</option>
</select>
```
现在你要验证用户只选择了数字。让我们利用 array.every ：
```
validateNumeric = (item)->
  parseFloat(item) == parseInt(item) && !isNaN(item)

values = $("#my-select-list").val()

values.every validateNumeric
```
## 讨论
这与 Ruby 中的 Array #all? 的方法很相似。


