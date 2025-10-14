---
categories: 整理输出
tags:
  - 算法
  - id
sticky: 100
description: 常见的id生成算法
permalink: ''
title: id 生成算法大杂烩（todo）
date: '2025-09-14 16:24:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/66bdab08-2c95-4215-a279-9a664fd9f37b/wallhaven-85lly2.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ZRY7JRV%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICbKvsBr6to503fZI%2BZqX389N2TiWgzBa43I8V4UZJ25AiAUzC%2BEeh%2FJkf0l3WT85ITqgbs1peD7vb3OlB0WY%2BPTvir%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMfAtzrDO8QNXJZ9SyKtwDUKD2vE%2FzFAxbO3R5lAS1FLzkop%2BqdGKytEHsFzdqKTx9qb90S1ZJLRNBIzja7Tu0zxUYQ4NbhZ7m1xKaiY06BomLaBgfKMQxccggEmOKtM59Eqg%2FH89M5XDzoL1oFg6peazVMVgesubUVoJWpawMmodzNanOexO%2BM1iopfdbw%2FcpMiCQ3ZUbNuQtynqCFEHO2Ctn1dYnDCDHIdwOXtUvm0hCE%2BRSGt1LE5s0Bht2L4hS4E5Qnrg5HUySOJ5MR%2BZxg6NNN7CWBDUo1yF7sdYjtxPgTQ%2B7GxrsHSp%2B3GCEaSp6i7ETwxhCkSHF0t6xg%2Fe0uFabNDlxA2ItN9dYF6MSl61yJaudlalDjL3AEBtPe0%2BMvF6q1Go9QB%2FebyAcqjjxYWvNxLsFSI7JI0iUeCo%2BIjAvcFUEennZDa4YkrqVpaalJCy4I%2BKqKwIDf0MQBFfeN%2BUhSja7aaXBuzjv7n3CsI0w%2FR%2FOtNnXszIYuxWToJ6ezUm8TaL2bG8UNdvcMX%2ByNxlI1o%2B0t767lMxN29ZWH0yzln8iqaVt7X02sFi80v8872su9MIASKXqyIFSuIC1FK26s%2FIJKxi9foMBeHYEQnLdyYG1VkpNXFaxz%2BWf8Sj3wudxSijYmMl1mRgwptm5xwY6pgEa5Ly9LUrSIKlx3TD5wsZyBJGGgLtisdVukUr9OJRchS8Sg0jS4zdJXofmLsMBI2nHHbxUyo%2FNtFNYaqhW72O6sUIAd%2FdVA%2Fz9mzbb%2Fl4nQoCnXNHeTXz7N8rKM5rOjcLjR%2Bi2VjuBxBgijzW96OZrZynREZZrW%2BZuGhZZOmSTzyqbO6WLG7MO2GT5PQN0uaxfRh%2FKGkMuRJ6NsHNO%2Fp7%2F21IFftId&X-Amz-Signature=8e9c82d86bb3553e3852b9a1de108608a282b74b83ee7a415185b802966d84b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 20:20:00'
index_img: /images/652dbf567bc4f106d704263e430a2fc1.jpg
banner_img: /images/652dbf567bc4f106d704263e430a2fc1.jpg
---

分布式情况下，id 一般就不是自增了


# 幸运码设计

> https://juejin.cn/post/7522392183367385130

## 背景/要求

1. 每期一百万唯一无重复的 6 位数字幸运码
2. 人数到达上限的时候，可以追加 100 万

特征：

- **随机性**，发给每个用户的幸运码都是随机的，同时每个用户领取的多个幸运码也是随机的。
- **唯一性**，每一组的幸运码中，各幸运码都是唯一的。
- **范围性**，幸运码严格限定在000000到999999区间内。
- **高并发**，幸运码的生成和发放需要支持高并发，能够至少达到300QPS。
- **可追加**，在当期活动非常火爆时，需要可临时追加一组幸运码库存。

## 方案设计


### 实时随机生成

> 随机生成，然后看数据有没有，有再生成一个

`已经不像人类的方案了`


### 预生成库存方案


离线生成一百万个幸运码，随机打乱，写入数据库，每个幸运码对应一个从 1 自增的序列号，并使用 redis 记录幸运码序列号索引，初始值为 1


数据库表结构


id code


1 ddj2


这种就是提前生成了，存在数据库里面，然后在 redis 记录一个当前用到了哪里的索引


### 号段+子码模式


子码串为什么是一千位？


因为你还要进行二级随机，所以在这里生成了一个0-1000 的随机数，然后去这个 01 串（也就是 bitmap 中）查看有没有被使用，如果被使用，再进行一次随机。


这里应该还有设计，就是用一次之后看看是不是已经全部用完了，然后 redis 继续进行增加，文章并没有说明


采用号段+子码机制：

- **号段管理**：将10^6号码划分为1000个号段（号段值：0-999）
- **子码管理**：每个号段维护1000个可用子码（子码值：0-999）
- **生成规则**：幸运码=随机号段_1000+随机子码（示例：129358=129_1000+358）

![images3e79ee85f517a24b6dd17c49517d6021.webp](/images/00898df9e2516fa4f0f181649bb7126d.webp)


### 具体设计


将 100 万注幸运码分为了 1000 个号段（每段 1000 注）

- 号段 id：唯一且不重复，范围介于 0-999
- 子码串：1000 位字符串，采用`01`标记，0 表示 未使用，1 表示使用

幸运码生成公式：号段 id*1000+ 子码位置


保留了幸运码的随机性（号段 id 随机+子码随机）又通过子码的类比特存储方式提升了存储效率


### 4.2.1 多级缓存策略


Redis存储可用号段集合，若号段的子码使用完，该号段会从Redis集合中剔除，同时本地缓存也会预加载可用号段，确保发码时能更高效地获取候选号段。


### 4.2.2 高效锁抢占策略


系统为每个号段分配了分布式锁，当执行发放幸运码时，会从本地缓存随机获取15个候选号段。然后在遍历获取号段时，将等待锁的超时时间设置成30ms，确保号段被占用的情况下能够快速遍历到下一个号段（根据实际场景统计，等待锁的情况很少发生，一般最多遍历到第二个号段即可成功抢占）。一旦成功获得号段的分布式锁后，便可进一步随机获取该号段下的可用子码。


### 4.2.3 动态库存策略


要追加库存，只需再创建一组幸运码号段，并写入Redis，后续发放时获取该组的可用号段生成幸运码即可。从性能和存储空间上远优于预生成方式。


### 幸运码发放流程


**发放步骤**：

1. 随机获取至多15个可用号段
2. 遍历号段
3. 抢占号段的分布式锁
4. 若号段的分布式锁抢占成功，则随机获取号段中可用的子码，再根据号段和子码生成幸运码
5. 若号段的分布式锁抢占失败，则遍历下一个号段，并重复上述步骤

![image.png](/images/035399511e7a9d2be97ad9c7b0b1c6d7.png)


![image.png](/images/26032740c6d1a88a70a626c31b95f6fc.png)

