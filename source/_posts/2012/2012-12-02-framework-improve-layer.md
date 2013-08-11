---
layout: post
comments: true
title: "从某维护系统的架构改造谈起–分层的理解(旧)"
description: ""
categories: ["总结"]
---

这是刚到公司时写的~

公司的项目是一个维护有好几年的大规模电信项目，并且经过无数人的摧残，代码极其混乱。我们想改变这个层次不清代码混成一团的现状，引入业界广为使用的三层架构开发模式。如下图所示:
![三层结构][1]

最近小组一直在探讨如何从现有架构上逐渐迁移为分层清晰的状况，虽然我们主要的精力还是放在新增的功能上，但是在这个整个过程还是令我对分层有了更深的理解。这里主要说说对于通用分层的理解：

1. 虽然大家都或多或少知道Action,Service,Dao干的是什么，不过实践起来的时候，有很多细节问题需要考虑(我们的系统有些比较蹩脚的调用接口，又不能抛弃)
2. Action主要关注大的流程处理，其中可能会包括多个Service的处理(像我们需要与大量外围系统交互更是如此)，一个Service方法代表一个具有完整事务边界的流程。
3. Action和Service的边界主要由参数来决定，Service不需要知道调用方是一个GUI还是web请求，所以调用参数不应该出现request/response/session之类的对象，
应该限制为java基本类型，基本集合类型，简单的值对象(用于避免长参数调用)，或者是具有特殊业务意义的变量(例如用户等信息)。
4. Service和Dao的区分主要是业务相关性，Service不需要关心Dao究竟调用DB还是其他的数据源获得的(多数据源在大型系统中非常常见)，
所以从调用参数和返回值上看，参数和返回值都避免具体的Dao实现有关。针对某些Dao调用返回值需要进一步处理，这部分的工作放在Dao还是Service取决于处理工作更靠近业务还是靠近具体Dao实现协议。
5. 从关注对象上看，Action关注request、response、session等; Service关注java基本类型，java集合类型，值对象等；Dao关注Sql，ResultSet等底层数据结构。
6. 从关注的异常处理看，Action关注特定的业务异常(BusinessException)，并统一业务异常处理流程，
Service只关心Dao是否发生异常(例如统一的Dao异常接口DataAccessException)，记录并转化为相应的业务异常交个业务处理；Dao关注底层的异常，通常也无法恢复，只好转化成统一接口交给上层处理。

晚了，下一次就谈谈关于这个项目的异常处理的改造或者Dao的改造情况吧~~

 [1]: /assets/images/tiger.jpg