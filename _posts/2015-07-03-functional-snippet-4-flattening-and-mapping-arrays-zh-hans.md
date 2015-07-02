---
layout: post
title: "函数式 Swift 代码片段 #4: 展开并映射数组"
date: 2015-07-03T06:45:01+08:00
comments: true
categories: [functional programming, swift, zh-Hans]
---
在函数式编程中，在数组上调用 `map` 是一项基本操作。常常令人惊讶的是，`map` 得到的是一个元素为数组的数组，需要将其展开。在许多函数式语言中，在 `map` 之后再展开被定义为一个运算符。这个运算符有时候被称作 _bind_。它使用 `map` 来调用一个函数 __f__，然后将得到的元素为数组的数组展开。

```swift
infix operator >>= {}
func >>=<A, B>(xs: [A], f: A -> [B]) -> [B] {
    return xs.map(f).reduce([], combine: +)
}
```

该运算符酷炫的用法之一就是组合多个列表。举例来说，假设我们有一个扑克牌点数的列表，以及一个扑克牌花色的列表：

```swift
let ranks = ["A", "K", "Q", "J", "10",
             "9", "8", "7", "6", "5",
             "4", "3", "2"]
let suits = ["♠", "♥", "♦", "♣"]
```

现在，我们可以通过枚举所有的点数，将其和每种可能的花色结合来生成一个所有扑克牌的列表：

```swift
let allCards =  ranks >>= { rank in
    suits >>= { suit in [(rank, suit)] }
}

// Prints: [(A, ♠), (A, ♥), (A, ♦),
//          (A, ♣), (K, ♠), ...
```

---
本文翻译自：[Functional Snippet #4: Flattening and Mapping Arrays](http://www.objc.io/snippets/4.html)
