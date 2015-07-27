# 计算（美国和加拿大的）感恩节日期
## 问题
你需要在给出的年份中找到感恩节的月份和日期。
## 解决方案
下面的函数返回给出年份的感恩节的日期。如果没有给出任何参数，给出的结果是当前年份。  

美国的感恩节是十一月的第四个星期四。　　
```
thanksgivingDayUSA = (year = (new Date).getFullYear()) ->
  first = new Date year, 10, 1
  day_of_week = first.getDay()
  22 + (11 - day_of_week) % 7
```
加拿大的感恩节是在十月的第二个周一。
```
thanksgivingDayCA = (year = (new Date).getFullYear()) ->
    first = new Date year, 9, 1
    day_of_week = first.getDay()
    8 + (8 - day_of_week) % 7
```
## 讨论
```
thanksgivingDayUSA() #=> 24 (November 24th, 2011)

thanksgivingDayCA() # => 10 (October 10th, 2011)

thanksgivingDayUSA(2012) # => 22 (November 22nd)

thanksgivingDayCA(2012) # => 8 (October 8th)

```
这个想法很简单：  
1. 找出哪一天是以下各月份的第一天（美国十一月，加拿大十月）。  
2. 计算从那天起偏移到下一个工作日的量（美国星期四，加拿大星期一）。  
3. 将这个偏移量加上第一个可能的假期日期（第二十二个美国感恩节，第八个加拿大感恩节）。

