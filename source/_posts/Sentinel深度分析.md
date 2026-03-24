---
categories: 源码阅读
tags:
  - 架构设计
  - 限流
sticky: ''
description: ''
permalink: ''
title: Sentinel深度分析
date: '2026-03-24 11:15:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/2d411fff-4df0-41f7-b283-309e73131555/142223454_p0-%E9%AF%A8%E3%81%AE%E5%A4%A2.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZVAJBQHJ%2F20260324%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260324T125107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDRRESIxO7PQJFnAEgCj9jaBiN5kb5p0Fm4%2BXIr8Xyv8gIhAKcnIFF6iAqseeUtOLoz0KyY5QajkY8aQqfj4P7%2FlxYzKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzkfEyD04%2B3fnm3qEAq3AMOlSUsYuV8UD7gOU6XQfL3ilkdvcr1U4O0BewG8RTUqf2kp%2BrM9sZbAv0ISMuYexNtZRJi7Ffh8YLyyIZs%2F3JWgLmO307XZlt2DGmfAV3wDcIYsn3ymy427sW1fwdUGzUU%2Fi9VSw3vTb%2FAi8aypbwJp7uKOSaxp1mDgjr18QA9ASKpXYgPMfIY2u2VZvhh6%2BvdQsGAXDazQs5jH1VBjxPyLqo4dugTELNJpz8E1ecFK1XLRnUz8sE4Ink6nYWBtthsMHPYX7%2BsOjJuoZgLQ9IjEZGfGtoCKS83mooH30TfQJobLB110h%2Bv0yxjikJKanY0eI%2BupnpMIMBMpOo1ldRbZKlVA1TgAQbV1GhklZ7gwaKiSkfHN6RBVrNHsG%2BmHVOjgrnWAGqlJYalapMxo%2Fwp81JaUv6OohIkQNSIx0Ggb1xspI9oIQmigUcnnsvP%2FdedJF3yo89egC5xj%2BHrOUB4QbwH0IdCAG2mam8S6IgU3QNEqtp6NexkC2lf3IR33aVZ9Vy5LiDNQ4OwYV6uog9CAcBsXCmpImo290c78UubJlZLmxnqlD33QysCi%2F45B96Td%2BB1%2FeuG4GHszqqDKvnoJF2jXXkK%2B6qMbCKjolj29Uq%2BldcJKcM%2FZ7%2F9WzCG8onOBjqkAWsVoK1AnJaLPCxmXmHVt49JUF9aUdgFguxK1s19CipEE9nQn2t0uKWIZJsSEm3wHUEBvUNm%2F3zGVZzJ%2FsIB8mpt1weB4qwRCzIP%2BacFtO%2Fk7v%2Fbx%2FJXku%2FMoa4IVo5P7bD1cZYRFNTAZz6%2Ff6D0fLG6xWcgXK8b%2F5wAhoCxUPYnmiNueH%2BNn%2FfMjYZ3Mh%2B7e4mhsxG9AzN7l9ROJ32h0NQCUGfr&X-Amz-Signature=c5cc83d4c8d0f192a1af1b6e7ffc68834b03c753bad470e3a80fea2ba2e9d2e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2026-03-24 15:34:00'
index_img: /images/5e7a19e301e3d312dc7b81be5aedddb7.png
banner_img: /images/5e7a19e301e3d312dc7b81be5aedddb7.png
---

# 基本概念


## 上下文


z


# 规则加载

- 通过`loadRules`
- 通过DataSource适配Nacos等数据源

## 源码分析


```java
//下面这个是实际的规则判断类
    private static volatile RuleManager<FlowRule> flowRules = new RuleManager<>();
//这个是监听器(就是别的配置更新了,会更新上面的)
    private static final FlowPropertyListener LISTENER = new FlowPropertyListener();
    //这个是实际的配置(map类型,缓存)
    private static SentinelProperty<List<FlowRule>> currentProperty = new DynamicSentinelProperty<List<FlowRule>>();
```


简单分析：就是一个观察者模式+配置更新

1. `Property`  更新完之后调用listener
2. 然后listener更新flowRules

# SentinelResource 拦截


filter则进行了拦截


## 切面源码分析



`SentinelResourceAspect`


block(限流)或者fallback(异常)


# 限流逻辑


![image.png](/images/151846f75b50e44a163b4108bff933f4.png)


```java
NodeSelectorSlot
    ↓
ClusterBuilderSlot
    ↓
StatisticSlot
    ↓
ParamFlowSlot
    ↓
SystemSlot
    ↓
AuthoritySlot
    ↓
FlowSlot
    ↓
DegradeSlot
```


todo：

- [ ] 每个限流算法的源码（手写一次）
    - [ ] 滑动窗口**ArrayMetric** 
- [ ] 为什么集群是集群节点（`static volatile`）来保证的吧 
