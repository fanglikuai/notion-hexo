---
categories: 整理输出
tags:
  - 分布式
  - id
sticky: ''
description: ''
permalink: ''
title: 分布式id生成方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SF26PZXA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEID%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH4OL3QL64xRQD4RZCqKZwq4V5jw8SxSvtLdcQhPmIkXAiAla%2Bz2wLcWQxuXYLZKB4Wg%2BtOKM0Jcok69ZQ2LUlxQdSr%2FAwg5EAAaDDYzNzQyMzE4MzgwNSIMOMBHd0txP%2FJAHX%2BcKtwD45owEDE%2B%2Bq5lHpSBZ3p7RCWHFKIqx%2FcH2Koq5xcZyjdEq4n76T9AGE714SBKg1iN3kmwWpntWPvw3YLq5oVdV98yx%2FStOGKaH6htwMCaunnQHl8Eq4La5V42JtazlPzha6jXebf4N9OdRp0tY094NG0JIbG%2Fl%2B5E2do3or%2B%2Fd%2FAX7TcuxVFfULPnCy6wcN9%2Fl2z%2B8zpQ%2F6vNvfwpvdoyhw1DRZF%2BqBWXcS2tbxOBck1BnsH0GiK9m3yOZPxGtCi8JqMqW8ZZeHnZCQCKB66Ddeq3jsQN6SlgNRFd8NRKSd9DE5WJQYCwUX5vcz9XAexUTKs8B56rs%2BULLhgnAPZa77mu57as5NRLk1yuDDCdoK%2F87AKZfSYBQv6BfHZNErMk8XBFGLmxo13D2rpXx7dkZmDMBCT4BKxYGm9LZO0pwOepoZmNa4QKkU4bZUG47wQYVe7AQK3ds4I7R377ujcKn7ISN3N7SYZUcVC0vie2VO71z%2BQRTrQjmaf0NV5euu49qo30fjp73cIV5uC5LY9wbKZytNr5aIiWtRpzsShXjoBpYou6%2BtWopNL1WD%2BUawXNlTUiNI%2BP3U%2Fec4NV5R36WAmtfOG%2Beqe%2B0lNo5gikAG6X3fiLgUEALZLKJREw5trlxwY6pgGGBWDf7IxBjmLic81Oqy7ZLIU8y%2BouYrG0ueg%2Fcjapwsk16aoHVKJUmWUSgk05VxBpy1gJPhKWxA%2F0xaoe4HICxYublamvXqNdNM8sFesh9Kp147AildDlrM6Cwe%2BNhBGb7Qc%2FnDB224RSyFIC8H7yTmfKzGQTQ%2FkLLQ7BGt6HfMoKHdns4dSqwpn%2FelL5MPGGyxVhVgrAg5XbgZbFkimviNnJ7%2FMh&X-Amz-Signature=740d6721ef92eaf96bad0bc042e3da6fccd838ead6d7a18cc356ae5fcb740f4a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:55:00'
index_img: /images/62c2d2c32218b2c2c1ec4c8cfc657330.jpg
banner_img: /images/62c2d2c32218b2c2c1ec4c8cfc657330.jpg
---

要求：

1. 全局唯一性
2. 趋势递增
3. 单调递增
4. 信息安全

# 雪花算法

> 0 - 0000000000 0000000000 0000000000 0000000000 0 - 0000000000 - 000000000000
>
> 符号位 时间戳 机器码 序列号
>
>

12位存储序列号，当同一毫秒有多个请求访问到了同一台机器后，此时序列号就派上了用场，为这些请求进行第三次创建，最多每毫秒每台机器产生2的12次方也就是4096个id，满足了大部分场景的需求。


## seata版本


将时间戳和序列号连在一起，然后每个微服务开始只获取一次时间，后面直接递增+1就行了


并发量大大提升。


问题：


如果超前消费很多，然后系统又重启了，那么是不是就重复了。


不会出现这个问题：因为这样的并发量得很大才行


![imagesb91ceb2795145be127635d50c861bbfa.png](/images/c686239b1567bd4f1ee7f6da809063a0.png)


最终收敛，防止页分裂


# leaf方案


## 数据库方案


去数据库修改字段，每次获取setp缓存在服务中，减少并发量


重要字段说明：biz_tag用来区分业务，max_id表示该biz_tag目前所被分配的ID号段的最大值，step表示每次分配的号段长度。原来获取ID每次都需要写数据库，现在只需要把step设置得足够大，比如1000。那么只有当1000个号被消耗完了之后才会去重新读写一次数据库。读写数据库的频率从1减小到了1/step，大致架构如下图所示：


![imagesa73fdbbf512d967222aafea71d8485df.png](/images/6ba8f71fc9902de8f6ead17de802a727.png)


### 优化


就是监控使用的id量，到达阈值的时候，发起线程去更新


采用双buffer的方式，Leaf服务内部有两个号段缓存区segment。当前号段已下发10%时，如果下一个号段未更新，则另启一个更新线程去更新下一个号段。当前号段全部下发完后，如果下个号段准备好了则切换到下个号段为当前segment接着下发，循环往复。

- 每个biz-tag都有消费速度监控，通常推荐segment长度设置为服务高峰期发号QPS的600倍（10分钟），这样即使DB宕机，Leaf仍能持续发号10-20分钟不受影响。
- 每次请求来临时都会判断下个号段的状态，从而更新此号段，所以偶尔的网络抖动不会影响下个号段的更新。

## 雪花方案


使用zookeeper，在第一次的时候进行注册，然后后续缓存起来机器id。


后面进行周期性检查：看时钟是否回拨

