## Ruby 字符串从 1.8 到 2.5 的演变

### 介绍
在 Ruby 中，字符串被表示为 `String` 类的实例。`String` 类在 Ruby 1.8 和 Ruby 2.5 之间有很大的改变。

因此，本文的目的是详细介绍每个主要版本的主要更改。

如果你想分享更多的信息，请随意发表评论。

> 在开始之前，请随意看看我的 [最新项目](https://www.rubycademy.com/)

## 1.8 到 1.9

让我们看看在 1.8 和 1.9 之间的 Ruby 字符串类的主要区别。
```ruby
# in ruby 1.8.x
String.included_modules # => [String, Enumerable, Comparable, Object, Kernel]
ims_18 = String.instance_methods(false).map(&:to_sym)

# in ruby 1.9.x
String.ancestors # => [String, Comparable, Object, Kernel, BasicObject]
ims_19 = String.instance_methods(false)

diff = ims19 - ims18

diff # => [:===, :clear, :chr, :getbyte, :setbyte, :byteslice, :codepoints, :prepend, :ord, :each_codepoint, :encoding, :force_encoding, :valid_encoding?, :ascii_only?, :encode, :encode!, :to_r, :to_c]
```
第一个主要区别是，Ruby 1.8 中的 `String` 类包含 `Enumerable` 模块，Ruby 1.9 中的 `String` 类不包含 `Enumerable` 模块。

第二个不同之处在于，Ruby 1.9 中的 `String` 类有许多新的实例方法可用。

>如果你不熟悉 `Object` 类和祖先链，请阅读 [Ruby 对象模型](https://medium.com/@farsi_mehdi/ruby-object-model-part-1-4d06fa486bec) 这篇文章。

但最重要的变化仍然是，在 Ruby 1.8 中，字符串被视为`字节 bytes 序列`，而在 Ruby 1.9 中，字符串被视为`码点 codepoints 序列`。

与特定编码耦合的`码点序列`允许 `Ruby` 处理编码。

实际上，字符串在磁盘上被存储为`字节序列`。

编码只是指定如何获取这些字节并将它们转换为代码点 codepoints。

因此，在 `Ruby 1.9` 中，`Ruby` 本身就可以处理字符串编码，而在 `Ruby 1.8` 中需要 `iconv` 库来处理字符串编码。

注意：
- 每个字符串的默认编码是 `Binary` (读为字节序列)。
- 在 Ruby 1.9 中 `iconv` 库已经被弃用。

### 1.9 到 2.0
在 `Ruby 2.0` 中，`UTF8` 是一个正在运行程序中的每个字符串的默认编码。

实际上，在 `Ruby 2.0` 中，默认编码是 `UTF8`，而在 1.9 中是 `Binary`。

这种行为与使用 `UTF16` 作为默认编码的 `Java` 有点类似。

>注意，从 `Ruby 2.0` 开始，`iconv` 库不再是 `Ruby` 语言的一部分。

### 2.0 到 2.1
在 `Ruby 2.0` 中，将字符串从一种编码方式转换到相同种类的编码方式（例如：`UTF8 `到 `UTF8`）会导致空操作。

```ruby
# in ruby <= 2.0.x
content =  "Is your pl\xFFace available?".force_encoding("UTF-8")
content.encode("UTF-8", invalid: :replace) # => "Is your pl\xFFace available?"

# in ruby 2.1.x
content =  "Is your pl\xFFace available?".force_encoding("UTF-8")
content.encode("UTF-8", invalid: :replace) # => "Is your pl�ace available?"
```
这里我们可以看到，在 `Ruby 2.0` 中，我们显式用 `UTF8` 编码的 `UTF8 字符串`返回新字符串，返回字符串中未知的代码点 codepoint 不会被替换。因此，`invalid: :replace` 操作被省略。

在 `Ruby 2.1` 中，`invalid: :replace` 操作被处理，并且默认的字符 ` � ` 会替换序列中无效的代码点 codepoint。

### 2.1 到 2.5
自 `Ruby 2.1` 以来，除了提供了许多性能改进之外，`String` 类还增加了两个主要特性:

1. `frozen_string_literal: true` 魔法注释（从 Ruby 2.3 开始）。

```ruby
# frozen_string_literal: true

hash = { 'key' => 'value' }
hash['key'] # this will not allocate another String for 'key'
```

2. 非 `ASCII` 字符串的大小写转换（从 Ruby 2.4 开始）。

```ruby
'Türkiye'.upcase          # => "TÜRKIYE" - Full Unicode case mapping
'Türkiye'.upcase :ascii   # => "TüRKIYE" - ASCII only
'Türkiye'.upcase :turkic  # => "TÜRKİYE" - Full Unicode case mapping, adapted for Turkic languages
"Ačiū".upcase :lithuanian # => "AČIŪ"    - Full Unicode case mapping, adapted for Lithuanian
view raw
```

### 基准字符串分配
下面的基准是使用 `benchmark-ips gem` 做的。

```ruby
require 'benchmark/ips'

Benchmark.ips do |x|
  x.config(:time => 5, :warmup => 2)

  x.report("string allocation") { "aaa" }

  x.compare!
end
```
它为每个版本的 `Ruby` 生成如下结果——从 1.8 到 2.5。

![](https://ws4.sinaimg.cn/large/006tNbRwly1fwi1n62lxdj30m80ciaa6.jpg)

在这里，我们可以看到 `Ruby 2.5` 中的字符串分配效率大概是 `Ruby 1.8` 中的 4 倍。

我的上一篇文章的链接：[5个你可能不知道的 Ruby 技巧](https://medium.com/@farsi_mehdi/5-ruby-tips-you-probably-dont-know-76fee34cfd0c)

## 原文链接
https://medium.com/@farsi_mehdi/evolution-of-ruby-string-from-1-8-to-2-5-d47afd3505df
