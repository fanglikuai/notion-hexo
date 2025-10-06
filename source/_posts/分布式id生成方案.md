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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XBZ7TWHQ%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAiQ%2FLy7uUU4wpI1EQewzI%2BdbAs8gwBX1wMVWQbiB2rqAiEAnIpSH5NjEZ3h9x4mziPSpsOBgU4N4f7gv76kpUBXH2UqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLGeS4ZqCI1FEqIkYircAycOWIjy895sIUykMQ8sd4VmyA2ysL5ab8bJLT6OpNPnSk4Dt%2FQiKNaNcu0XIaPb4pfhD4oLge6CrD%2Fr2ZI0IvWPyBnLgvB4z7i8fcTVkRO%2FKUqOdmzmAzsLWRE3qRozjEb2CHJdDqaRJtOGQqTzwJ07o2MWasoyJezbsd1moVbGCzg3%2F5IrttTnqHbEFVUqT%2FUmGo%2FCbHqkbcViAZB4Ka5iokVkkU0vVI0e6aDbCPpHvVFWNTkbSXUpDBG%2BLtpu7XPOk2uL%2BKQQxnrsSk6O62M8eRcY1zyy4UkxNepP3LtB0L9V5rR3Rm3G3Za%2Ft9VqfFMM7Zu96xW5Mh5h3DDMPvr4IWHRgm16B4YksUclwbfaOkKGaHAjETiraRfg58hrY%2F6SgwUJFQffOEpGBia3lgS8brRjAspls7jfzJqpkvWegrTXxbvBpOFQKIdNQB%2FeOyjS04BVVjJLJfevQHQwZKCeYCwo1MQpdNsklxc61dv9rnQslcqLcvEEY6Ax%2FvxBcw9NwyowwzJfw0UaDk7za3XLTPuggm7g4%2BMNbDjn%2FO33H2UXABZkN9AkhEroPx%2F1dxU%2BSHtvJW7iXAnIpLt9Ddxw3%2B%2B%2B0A%2Funq99J3Phf%2BeiYXSV8MubnrxrkcLFMM3%2Bi8cGOqUB1jX8s7RUlKKZrd3yo5WYbWvkci9sk%2FnwONijJw88lbnkFBm9cFYTONUpHTjwBQ5IhBjVIbeee2dmCxdrS0cr6vA3EbWCpFNI5FCaUBbLwCGutN9ENb6QneOKzf%2FMg3KzQdpHK3Rcf3ldhN05wsXtDTCnK0izUzVHC%2BIXc5ZhEefdtvM%2FitWatyqtShLPHeiq814Ok8BTjJnFPnSYHvEBorR4AMF2&X-Amz-Signature=4a1c14e130bcdc7d75655deeae80b45a8bbed0e9e63effd2c87f5002ae3f600c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

