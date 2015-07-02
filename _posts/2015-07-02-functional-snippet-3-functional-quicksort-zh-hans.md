---
layout: post
title: "函数式 Swift 代码片段 #3: 函数式快速排序"
date: 2015-07-02T00:08:58+08:00
comments: true
categories: [functional programming, swift, zh-Hans]
---
下面这种快速排序实现显然不会在速度上胜出。大多数现实中的快排实现只使用固定大小的内存。不过，这段代码确是最简短清晰的快速排序实现之一。

```swift
func qsort(input: [Int]) -> [Int] {
    if let (pivot, rest) = input.decompose {
        let lesser = rest.filter { $0 < pivot }
        let greater = rest.filter { $0 >= pivot }
        return qsort(lesser) + [pivot] + qsort(greater)
    } else {
        return []
    }
}
```

这段代码建立在之前的[分解 Array](http://blog.ztap.net/functional%20programming/swift/zh-hans/2015/06/30/functional-snippet-1-decomposing-arrays-zh-hans.html) 的代码片段之上：如果数组不为空，就取第一个元素作为基准，并将剩余的元素分为两个新的数组：小于基准的元素放进第一个数组，大于等于基准的元素放进第二个数组。接着就排序第一个数组，接上基准元素，最后再接上已被排序的第二个数组。

---
本文翻译自：http://www.objc.io/snippets/3.html
