---
layout: post
title: "函数式 Swift 代码片段 #2: 函数组合"
date: 2015-07-01T10:51:41+08:00
comments: true
categories: [functional programming, swift, zh-Hans]
---
在函数式编程中，我们常常想把几个短小、独立的函数组成一个新的函数。举例来说，假设我们有个函数负责从 URL 获取内容，又有个函数负责把 `String` 按行转换成 `Array`，另有个函数负责计算元素的数目。我们可以把这些函数组合成一个可以计算 URL 对应内容中所含的行数的新函数：

```swift
func getContents(url: String) -> String {
    return NSString(contentsOfURL: NSURL(string: url),
                    encoding: NSUTF8StringEncoding, error: nil)
}

func lines(input: String) -> [String] {
    return input.componentsSeparatedByCharactersInSet(
            NSCharacterSet.newlineCharacterSet())
}

let linesInURL = { url in countElements(lines(getContents(url))) }
linesInURL("http://www.objc.io") // Returns 731
```

函数组合是如此常用，以至于编写函数式程序的人为其定义了一个自定义运算符。

```swift
infix operator >>> { associativity left }
func >>> <A, B, C>(f: B -> C, g: A -> B) -> A -> C {
    return { x in f(g(x)) }
}
```

现在，我们可以彻底摆脱嵌套的括号，把之前的例子写成这样：

```swift
let linesInURL2 = countElements >>> lines >>> getContents
linesInURL2("http://www.objc.io/books") // Returns 470
```

---
本文翻译自：http://www.objc.io/snippets/2.html
