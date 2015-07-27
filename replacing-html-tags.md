# 使用 HTML 命名实体替换 HTML 标签

## 问题

你需要使用命名实体来替代 HTML 标签：  
<br/> => &lt;br/&gt;

## 方案

```
htmlEncode = (str) ->
  str.replace /[&<>"']/g, ($0) ->
    "&" + {"&":"amp", "<":"lt", ">":"gt", '"':"quot", "'":"#39"}[$0] + ";"

htmlEncode('<a href="http://bn.com">Barnes & Noble</a>')
# => '&lt;a href=&quot;http://bn.com&quot;&gt;Barnes &amp; Noble&lt;/a&gt;'
```

## 讨论

可能有更好的途径去执行上述方法。

