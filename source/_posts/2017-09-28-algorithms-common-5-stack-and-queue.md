---
layout: post
title:  常见算法基础题思路简析(五)-队列和栈篇
date:   2017-09-28 21:45:07 +08:00
category: 算法和数据结构
tags: [算法, 数据结构]
comments: true
---

本文对和 **队列和栈** 有关的常见算法基础题思路分类进行分析和总结，并以 Java 为例，适当指出需要注意的编程细节

<!-- more -->

- 相关题目和代码在 GitHub: [https://github.com/brianway/algorithms-learning](https://github.com/brianway/algorithms-learning)
- 题目见 `com.brianway.learning.algorithms.lectures.stack`包

## 构造 MaxTree

题目见 `MaxTree`

思路：使用一个栈来储存某个元素左边比它大的元素的下标，该栈满足这样的特性：栈中每个元素 m 以及 m 出栈后的栈顶元素 peek，总有对应的数组值 `arr[peek]` 为 `arr[m]` 向左数第一个比它大的数组值。下面的 1～3 步只是为了使栈保持这个特性。

对数组中每个元素，执行如下操作：


1. 将当前元素与栈顶元素对应的数组值进行比较。如果栈非空且栈顶元素对应的数组值小于当前数组值，则出栈，并根据当前数组值与出栈后栈顶元素对应的数组值相对大小来赋值结果数组。一直执行该操作直到循环结束
2. 将当前元素下标对应的结果数组赋值
    - 栈为空则赋值为 -1
    - 栈非空则赋值为栈顶元素
3. 将当前元素下标入栈

注意：

- 第 1，2 步中，比较时注意岀栈后栈为空的情况



## 滑动窗口

题目见 `SlideWindow`

思路：使用一个双向队列来存储当前窗口里可能称为最大值的备选元素的下标。每次从头部取最大值，从尾部删除非备选项。

1. 对数组中每个元素，循环进行以下操作：
2. 从双向队列尾部依次删除对应数组值小于当前数组值的元素，并将当前元素下标加入队列尾部
3. 判断队列头部下标是否已经不在窗口内，若过期则删除
4. 此时的队列头部元素对应的数组值即为滑动窗口内的最大值

注意：

- 尾部的为最新的值，头部的为较老的值
- 删除时注意队列为空的情况

## 得到栈最小元素

题目见 `StackMin`

思路：使用两个栈，一个数据栈，一个最小栈，每次入栈和岀栈时，同时更新两个栈即可。

注意：

- 入栈：最小栈为空或者新元素 **小于等于** 其栈顶元素时才入栈
- 岀栈：最小栈栈顶元素等于数据栈栈顶元素时才出栈

## 栈的反转

题目见 `StackReverse`

思路：利用递归函数和栈本身的 pop 操作来实现反转，分三步

1. 取出栈底元素
    - 弹出栈顶元素 current
    - 若此时栈为空，说明已是栈底元素，则返回 current
    - 若此时栈不为空，则取剩下的栈的栈底元素 last，并将 current 压入栈中，返回 last
2. 反转剩下的栈
2. 将底部元素压入栈顶

注意：

- 反转栈和取出栈底元素这两个操作都是个递归调用

## 双栈实现队列

题目见 `TwoStack`

思路：使用一个数据栈，一个辅助栈。两次“后进先出”的最终结果就是“先进先出”

1. 先将数组元素按顺序压入数据栈
2. 再将数据栈中的数据全部依次弹出并压入辅助栈
3. 从辅助栈中依次弹出栈顶数据即可

注意：

- 数据需要在两个栈中全部腾挪一遍
- 因为先进先出，无论什么时候出队，结果一样，所以无需按输入顺序一个一个出队，只用统计出队次数，一次性出队即可

## 双栈排序

题目见 `TwoStacks`

思路：使用一个辅助栈，该辅助栈中的元素永远保持升序排列（即大的元素在栈顶）。简单来说就是每次将数据栈中的元素压入辅助栈，使得该元素位于辅助栈栈顶且最大，所以辅助栈若已有更大的元素，则回压给数据栈即可。进行如下的操作来是辅助栈保持这个特性，有点类似“插入排序”

1. 每次从数据栈弹出栈顶元素 m，依次和辅助栈的每个栈顶元素比较大小，若辅助栈栈顶元素较大，则将辅助栈栈顶元素压入数据栈，直到辅助栈为空或者 m 大于辅助栈栈顶元素为止
2. 将该元素 m 压入辅助栈中