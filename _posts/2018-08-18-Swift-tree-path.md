---
layout: post
title: Swift算法实战——二叉树路径
description: Swift算法实战——二叉树路径
categories: 数据结构与算法
keywords: algorithm
---


##### 先从一道题开始

> 给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。
 给定如下二叉树，以及目标和 sum = 22，

```

                    5
                   / \
                  4   8
                 /   / \
                11  13  4
               /  \      \
              7    2      1

```

> 返回 `true`, 因为存在目标和为 `22` 的根节点到叶子节点的路径 `5->4->11->2`。

这是这道题的原型，其本质还是要找二叉树路径。所以在这里我们就实现打印路径的算法。

##### 分析

咋一看，这与先序遍历有些像，那么我们可以参考先序遍历的算法。另外，由于要打印路径，所以需要用一个`栈`来保存路径。

```

// 定义二叉树节点
indirect enum BinaryTree<T: Comparable> {
  case empty
  case node(BinaryTree<T>, T, BinaryTree<T>)
}

var stack: [Int] = []
func path(root: BinaryTree<Int>) {
  if case let .node(left, value, right) = root {
    stack.append(value)
    if case .empty = left, case .empty = right {
      print("path: \(stack)")
    }
    path(root: left)
    path(root: right)
    stack.removeLast()
  }
}

```

> 这里我用枚举定义了一个二叉树节点。在打印路径的算法中，
  1.首先让根节点入栈，
  2.递归打印左右子树路径，
  3.打印完左右子树路径便让栈顶元素出栈，
  4.如果入栈的节点是叶子节点，便打印栈（这就是一条路径）。

##### 测试

```

let tree: BinaryTree<Int> = .node(
    .node(.node(.node(.empty,
                      7,
                      .empty),
                11,
                .node(.empty,
                      2,
                      .empty)),
        4,
        .empty),
    5,
    .node(.node(.empty,
                13,
                .empty),
        8,
        .node(.empty,
              4,
              .node(.empty,
                    1,
                    .empty)
        ))
)
path(treee)

//path: [5, 4, 11, 7]
//path: [5, 4, 11, 2]
//path: [5, 8, 13]
//path: [5, 8, 4, 1]

```
