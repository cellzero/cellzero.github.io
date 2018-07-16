# 读书笔记：Ruby基础教程第5版

> Ruby version: 2.3.0

#### 输出方法 print、puts 和 p 的区别

+ puts 和 p 方法输出每个字符串后一定换行，而 print 不会。
+ 使用 p 方法时，数值结果和字符串结果会以不同的形式输出。
+ 使用 p 方法时，换行符、制表符等不会转义，方便开发者调试。

```ruby
irb> print "hello\t", "world", 100, "100"
hello   world100100=> nil
```
```ruby
irb> puts  "hello\t", "world", 100, "100"
hello
world
100
100
=> nil
```
```ruby
irb> p     "hello\t", "world", 100, "100"
"hello\t"
"world"
100
"100"
=> ["hello\t", "world", 100, "100"]
```

#### 程序文件的编码

1. 程序的首行添加注释`# encoding: 编码方式`，这种注释被称为**魔法注释**。
2. 运行程序时，添加`-E 编码方式`选项，如`ruby -E hello.rb`。

#### 值嵌入

```ruby
x = 10
y = 10
print "面积 = #{(x * y)}\n"
```

#### 注释

+ 单行注释，使用`#`表示注释的开始。
+ 多行注释，使用`=begin`和`=end`表示注释的开始和结束。
