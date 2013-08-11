---
layout: post
comments: true
title: "连连看中如何判断连通"
description: "以前弄的连连看中使用的算法,用js实现的"
categories: ["算法", "连连看", "连通"]
---

## 连连看基本玩法
连连看的玩法非常简单，判断两个块是否可以消除的规则是：必须是同一类型的块，可以通过至多
三条直线相连，并且直线没有被其他的块所阻拦。

## 数据结构表示
可以用一个二维数组来表示整个界面，不同类型的点用不同的值k来表示，那么一个点可以用k和(x, y)来表示。
空白处的值设置为0。另外为了方便处理只在边边上可以连通的情况，可以加上边界（二维数组加上两行两列，并设置为0）。
判断点a和b是否可以连通，如果是不同类型的点很好判断，判断a.k==b.k即可。
那么，问题就变成判断两个点a(a.x, a.y)和b(b.x, b.y)是否可连通。

## 简单的算法
我用的是一种比较直观的算法，当然还有其他很多算法(在网上资料搜一下就有很多了)。

首先为了方便理解，先不考虑两个点在同一线上的情况。那么两个点能够连上的情况只有就是横竖横或竖横竖两个情况
先考虑两个点是否可以横竖横连接的情况，参考下面的算法步骤:

1. 给整个边界加上一圈空点(0)（方便后面计算）。
2. 计算x轴上a，b两点左右两边的空点，这样可以算出两者公共的空点(x坐标相同)，只有通过这些空点才可能联通达到横竖横的情况。
3. 对每个公共的点，假设x坐标为c,那么就是点(c,a.y)和(c,b.y)，再判断这两个点能否联通。
4. 假设a.y小于b.y，那么就是看从(c,a.y+1)到(c,b.y-1)这些点是否都为空。
5. 如果是空(0)的，表示最初的两个点是可以联通的。

到这里，一切已经ok了。另外一种情况跟这个是一样的，
而在一条直线上的情况只是这种判断的特殊情况而已。
