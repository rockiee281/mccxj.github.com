---
layout: post
comments: true
title: "判断循环菜单的思考"
categories: ["算法", "菜单", "循环", "树", "指针"]
---

今天下午的时候，在执行gem clean命令看到包依赖的时候，突然想起关于循环菜单的老问题。

### 无限级菜单的问题

很多系统的菜单都要求是无限级的，也就是可以很多层的父子菜单关系。
在进行数据存储的设计的时候，以数据库为例，通常都会给菜单表增加一个父菜单的字段，用来标识从属于哪个菜单。

这里就要求菜单的配置不能出现循环，例如某个菜单的父父菜单是它的子菜单，这是不允许的。
但有时候难免出现一些错误，特别不是通过可视化界面增加菜单的时候。我们的系统也曾经出现过，例如见[定位数据问题](/blog/20120804_improve-bitter-code-7.html)。

### 如何判断循环

就拿[定位数据问题](/blog/20120804_improve-bitter-code-7.html)来说，当时时间比较急就没多想，直接参考了项目中已有的逻辑。

考虑整个菜单结构是一棵树的话，它的判断逻辑的思路是，第一次把根结点去掉(最顶层的菜单)，
第二次把剩余的树的根结点去掉，以此类推，直到最后没有结点(没有循环)，或没有结点可以删除(出现循环)。
写成伪代码的话，大约是这样的:

``` java
hasDel = true

do
  hasDel = false
  for 结点 in 树
    if 结点 has not 父节点
      删除结点 and hasDel = true
    end
  end
while hasDel

if 还剩下结点
  出现循环
else
  没有循环
end
```
因为每次操作都要循环剩下的整个树，循环的次数很关键，如果树有3层，扫描3次即可，如果层次很深，扫描的次数就比较可观了。

### 另一个思路

以其中某个结点为例，它有父节点..一直到根节点，或者出现循环。
如果不是循环，则整结点都可以去掉。这样的话，只需要遍历整棵树，对未处理的结点，进行循环判断即可，而处理过的(去掉的)可以忽略。

伪代码如下:
``` java
for 结点 in 树
  if 结点已经被标识为已处理
    continue
  else 
    判断结点到根结点是否存在循环(如在路径上遇到已处理的直接返回false)
    if 存在循环
       出现循环
    else
       标记结点到根结点为已处理
    end
  end
end
```

**问题现在就回到如何判断循环链表**，这其实是个经典的面试笔试题了，我在网上挑了其中一个解答:[判断循环链表](http://blog.csdn.net/splendour/article/details/7701449)
对于我们的情况，在遇到已处理结点的时候，直接就可以说明没有循环了，这里可以小优化一下。

### 效率分析

我们粗略的分析一下，外层循环的数量级是整个树的结点数，判断循环的逻辑是和结点的深度有关系的，
以极端的情况为例，只有1层的话，很明显，相当于扫描一次结点。如果是个非常深的结点(都是单子结点)，就最后一个的结点的话，循环判断的次数也是和结点数成线性的。

**结点越深，循环判断的次数就越多，跟结点到根节点的长度成正比，但一次排除的结点也越多。总体还是跟总的结点数成线性正比的。**
相对于原来的处理方式，平均效率就是它的最好效率。

### 小结

判断链表是否存在循环这种面试题，还是有点实际用处的，以前真的没特别留意。
算法这东西，真的是无处不在的呀。