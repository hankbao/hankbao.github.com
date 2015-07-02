---
layout: post
title: "函数式 Swift 代码片段 #1: 分解 Array"
date: 2015-06-30T23:45:34+08:00
comments: true
categories: [functional programming, swift, zh-Hans]
---
下面的代码把一个 `Array` 分解为 `Array` 第一个元素和剩余部分，并放入一个 `Tuple`。如果 `Array` 为空，则返回 `nil`:

```swift
extension Array {
    var decompose : (head: T, tail: [T])? {
        return (count > 0) ? (self[0], Array(self[1..<count])) : nil
    }
}
```

当上述代码和`if let`语句或者模式匹配结合时会非常有用。举例来说，这是递归形式的`sum`：

```swift
func sum(xs: [Int]) -> Int {
    if let (head, tail) = xs.decompose {
        return head + sum(tail)
    } else {
        return 0
    }
}
```

你可以非常容易地为其他类型（比如 `Dictionary` 或 `String`）定义自己的 `decompose` 函数。

---
本文翻译自：[Functional Snippet #1: Decomposing Arrays](http://www.objc.io/snippets/1.html)
