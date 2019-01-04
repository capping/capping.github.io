---
title: 高效的php7数据结构
date: 2019-01-04 11:42:11
tags: [PHP, 数据结构]
categories: [PHP]
---

原文链接：https://medium.com/@rtheunissen/efficient-data-structures-for-php-7-9dda7af674cd

以下为译文：

PHP有一个数据结构来管理它们。PHP的数组是一个复杂的，灵活的，博而不精(master-of-none)的混合数据结构，结合了`list`(链表)和`linked map`的行为。
但是我们使用数组做任何事情，因为PHP是务实的：“以一种基于实际而非理论考虑的方式理性和现实地处理事物”。一个数组就能完成工作。不幸的是，灵活性导致了
复杂性。

最近发布的PHP7在PHP社区引起了大的轰动。我们迫不及待的开始使用[新功能](http://php.net/manual/en/migration70.new-features.php)尝试报告中提到
的两倍的性能提升。PHP7运行如此快的一个原因是数组被重新设计啦，但是它仍然是相同的结构，“适合一切；没有优化”，仍有改进的余地。

> “[SPL数据结构](http://php.net/manual/en/spl.datastructures.php)怎么样？”

不幸的是他们是糟糕的。他们确实在PHP7之前提供了一些好处，但后来被忽略到没有实际价值的程度。

> “为什么我们不能修复和改进它们？”

我们可以，但我相信他们的设计和实施非常的糟糕，用更新的东西替代它们会更好。

> “SPL data structures are horribly designed.” — Anthony Ferrara

介绍下`ds`，一个PHP7的扩展，提供了专门的数据结构，可用于替代数组。

本文简要介绍了每种数据结构的行为和性能优势。最后还有一系列预期问题的答案。

Github: https://github.com/php-ds

Namespace: Ds\\

Interfaces: Collection, Sequence, Hashable

Classes: Vector, Deque, Map, Set, Stack, Queue, PriorityQueue, Pair

## Collection

Collections 是基础接口，覆盖常见的功能，例如：foreach, echo, count, print_r, var_dump, serialize, json_encode, 和 clone.

## Sequence

Sequence描述了在单个线性维度中排列的值的行为。有些语言将此称为List。它类似于使用增量整数键的数组，但有一些特性除外：
- 值索引始终为[0, 1, 2, …, size - 1]
- 删除和插入操作更新所有连续值的位置
- 仅允许按[0，size  -  1]范围内的索引访问值

## Vector

Vector是连续缓冲区中Sequence值，可自动增长和收缩。它是最高效的顺序结构，因为值的索引是到缓冲区中其索引的直接映射，并且增长因子不绑定到特定的倍数或指数。

<iframe src="https://player.vimeo.com/video/154438958" width="640" height="360" frameborder="0" allowfullscreen></iframe>

**优势**

- 非常低的内存使用
- get, set, push 和 pop 的复杂度是O(1)

**弱点** 

- insert, remove, shift, 和 unshift 的复杂度是O(n)

> The number one data structure used in Photoshop was Vectors.” — Sean Parent, CppCon 2015

## Deque

Deque（发音为“deck”）是连续缓冲区中的值序列，它自动增长和收缩。该名称是“双端队列”的通用缩写，由Ds\Queue内部使用。

两个指针用于跟踪头部和尾部。指针可以“环绕”缓冲区的末端，这避免了移动其他值以腾出空间的需要。这使得移位和非移位非常快 - Vector无法与之竞争。

通过索引访问值需要索引与缓冲区中相应位置之间的转换：（（head + position）％capacity）。

<iframe src="https://player.vimeo.com/video/154438012" width="640" height="360" frameborder="0" allowfullscreen></iframe>

**优势**

- 低的内存使用
- get, set, push, pop, shift, and unshift 的复杂度都是O(1)

**弱点**

- insert, remove 的复杂度是O(n)
- 缓冲容量必须是2的幂


以下基准测试显示了用于推送2ⁿ随机整数所花费的总时间和内存。 PHP数组，Ds\Vector和Ds\Deque都很快，但SplDoublyLinkedList的速度始终慢了2倍。

SplDoublyLinkedList分别为每个值分配内存，因此预计会出现线性内存增长。数组和Ds \ Deque都有2.0增长因子来维持2ⁿ容量。 Ds\Vector的增长因子为1.5，这导致更多的分配，但整体内存使用率更低。

![](https://cdn-images-1.medium.com/max/1600/1*BZVzcscdpcUg8SZmvUEjQQ.gif)

![](https://cdn-images-1.medium.com/max/1600/1*FHxbwYbZ75l_pSEvWmNCig.gif)


以下基准测试显示将单个值取消移动到2ⁿ值序列所需的时间。设置样本所需的时间不包括在基准测试中。

它表明array_unshift是O（n）。每次样本量增加一倍，卸载所需的时间也会增加一倍。这是有道理的，因为必须更新[1，size  -  1]范围内的每个数字索引

![](https://cdn-images-1.medium.com/max/2000/1*7lF6nsm9MlvpHZ-IzFCLdw.gif)

但是Ds\Vector::unshift也是O（n），为什么它会这么快？请记住，数组会将每个值与其哈希和键一起存储在存储桶中。因此，如果索引是数字，我们必须检查每个桶并更新其哈希值。在内部，array_unshift实际上分配了一个全新的数组来执行此操作，并在复制了所有值时替换旧的数组。

Vector中值的索引是缓冲区中其索引的直接映射，因此我们需要做的就是将范围[1，size-1]中的每个值向右移动一个位置。在内部，这是使用单个memmove操作完成的。

Ds\Deque和SplDoublyLinkedList都非常快，因为取消移动值所花费的时间不受样本大小的影响，即O（1）


未完，可以提PR