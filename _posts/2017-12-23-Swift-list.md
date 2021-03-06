---
layout: post
title: Swift 算法实战 — 链表
description: Swift 算法实战 — 链表
categories: 数据结构与算法
keywords: algorithm,算法,swift,链表
---

## 前言

在表示一大波数据的时候，通常我们会选择数组，但有时候数组显得不够灵活，比如：
在一个有序数组中插入一个数，使原数组仍然有序。

> 给定一个从小到大的数组 [2, 3, 5, 8, 9, 10, 18, 26, 32]，要求往数组中插入6，使原数组依然从小到大排序。

如果使用数组，8以后的数都得往后移，显然链表就要方便许多。

## 实现链表

- 链表节点

```swift
class ListNode {
  var value: Int
  var next: ListNode?
  init(_ value: Int) {
    self.value = value
  }
}
```

- 有了节点，就可以创建链表了

```swift
//创建链表，返回头节点
func createList(_ values: [Int]) -> ListNode? {
  var head: ListNode? = nil
  var trail: ListNode? = nil
  values.forEach { (value) in
    let node = ListNode(value)
    if head == nil {
      head = node
    } else {
      trail!.next = node
    }
    trail = node
  }
  return head
}
```

- 查询节点

```swift
//根据下标查询节点
func getNode(from head: ListNode, at index: Int) -> ListNode? {
  var i = 0
  var node: ListNode? = head
  while i < index && node?.next != nil {
    node = node?.next
    i = i + 1
  }
  if i != index {
    return nil
  }
  return node
}
```

- 插入节点

```swift
//插入节点
func insert(node: ListNode?, to head: ListNode, at index: Int) -> ListNode? {
  guard let node = node else { return head }
  guard index > 0 else {
    node.next = head
    return node
  }
  if let pNode = getNode(from: head, at: index - 1) {
    node.next = pNode.next
    pNode.next = node
  }
  return head
}
```

- 删除节点

```swift
//删除节点
func deleteNode(from head: ListNode, at index: Int) -> ListNode? {
  guard index > 0 else {
    let next = head.next
    head.next = nil
    return next
  }
  if let pNode = getNode(from: head, at: index - 1) {
    let node = pNode.next
    pNode.next = node?.next
    node?.next = nil
  }
  return head
}
```

- 修改节点

```swift
//修改节点
func updateNode(_ head: ListNode, at index: Int, use value: Int) -> ListNode {
  getNode(from: head, at: index)?.value = value
  return head
}
```

#### 我们来做开头那道题

```swift
func modify() {
  let array = [2, 3, 5, 8, 9, 10, 18, 26, 32]
  let value = 6
  let head = createList(array)
  var i = 0
  var node = head!
  var v = node.value
  while node.next != nil && v < value {
    node = node.next!
    v = node.value
    i = i + 1
  }
  insert(node: ListNode(6), to: head!, at: i)
  printList(head)
}
modify()

// [2, 3, 5, 6, 8, 9, 10, 18, 26, 32]
```


附上 [Demo](https://github.com/DouKing/Structure.playground)



