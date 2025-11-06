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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQN5Z76S%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCRJPxZPWmoQb1tNI6sOG2NtukSvGIryYKgcehcv0r4vgIhAMgiPHcR1VzS9v%2FT6%2F3DBXqLIWEWuOHOiSAaGEsd3iSOKogECJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw1nsIWnTFIyeMbdU8q3AMNpiMP7pKpZ2wm5e9iS3KTj45LPLXmUkz66LO3hTx2AkGXoOTR%2Fn1byz14shlpswsl0M3OPsqNSLP3KQ3PwlvNU8cVLHmLcXyEG5W3JFhUmT4lBhLZS9l9IYaFqoZUlMuvJTBgJ9KbbUREZ5kCYZK0gNB3kxjv0gksk76q%2BaimSlMI4kK2VAw9CqSZgRjLKSbqSYZrHxfBHSNHMB1M0wSChrAkvApzpyFWRL%2FQkEjFE8dwdbv9bpEtR8S4BETAfLb7vlIErLIJXMP4klto7S71ci%2BSnBNpVI9O3b3%2FtNmfauaNBtHy3rME49RSvREROYb%2FVfRzzM3sj7wE55pjbA0pOSXolYL7tTkHNdpZeZezuc80tFlsh39nSvlNlEym3W4YUlDlUaM6EnQ5RYt9hmZV5dLMt16RztoIo9xeVOyCvlT7Y29lMgDP%2BxXkbwQyXXsfgRgn48IzxCkNUCsQuzRn9OLPeJyAT9xYrTr7QUglrzpueCeGwYWjSi1k1IuyUGLAqbrFPN0kJM0eCMVB6r58RJFHaVZ%2B4uXvdObBnFWRJ1GNfcCbi5gUkL5cZHG4wpxb2f16NQZlyJXxIj8NbczwBbkJUC%2BPUISY%2BOvZcLQ6EhLwoHUZGBpAPNng4DDarq%2FIBjqkAUvoPA0gJskuJulESedXdOXYoUpWVnnYTjnOuxy4yCxIbGxmXKzLBHhyOOte4TvExieya9rYpS4%2BQdGHFA28%2B4IwffaXGiL7xkxLmS6kGGk4SYuXi8CkoUcNfe5u4UlwmBzFnrcm4DiCsySlvuRiQgUMubS1HQEmld5phD5YDvw%2BkTZw05yuLChPxjwHptCNkCzEmGIB8eGYd4jqs2%2BJ0HEMlLrG&X-Amz-Signature=ec69f12c9a2e4bf794f7188d7e1b8b55895fb1bc7cf4aae0272bacce3a81c939&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

