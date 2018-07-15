# 读书笔记：Ruby基础教程第5版

> Ruby version: 2.3.0

#### 输出方法 print、puts 和 p 的区别

+ puts 和 p 方法输出每个字符串后一定换行，而 print 不会。
+ 使用 p 方法时，数值结果和字符串结果会以不同的形式输出。
+ 使用 p 方法时，换行符、制表符等不会转义，方便开发者调试。

```
irb> print "hello\t", "world", 100, "100"
hello   world100100=> nil
irb> puts  "hello\t", "world", 100, "100"
hello
world
100
100
=> nil
irb> p     "hello\t", "world", 100, "100"
"hello\t"
"world"
100
"100"
=> ["hello\t", "world", 100, "100"]
```

#### 
