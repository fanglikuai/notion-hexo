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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYUS3WBG%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T010043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJGMEQCIGHviEQsEu3xdD56ZD4oF45UCP7W147uowOSXJf%2BecIpAiAIBLot3NxQzmLfbdcqszNaUimEmZUe5Tn738u61mjaiyqIBAiZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWEL7PkCy3eNkCWN0KtwD3LVYE0S1ZgdF8W2CmjYJQA1lRqX%2BHRx6p4C9Je4iAwaS2Y875N6aoUxcMkAVnx4i%2FxxmLdeZzsdjaqwFI6btJ7XwTOIUlRTG4QGMdchkagieAerVI9vFZ%2FLBmd%2ByD7j2G3oZPeyjEE%2BwGtbXD0HmcOuIdCq8V1hy0hTI8kBUXtqhegLzSoraBUZ81hCJvkGnKvj7FAjEzIqCbIDyM33vOh2iOsZLWE0ECu0IRi4O5upmNNL1HPNBYiL%2FuWLHjIvRQ6QoVK7BYLHB3NjE4mQ%2Fo0AflpQyuTpqwtw0Y4Oe71OGbHs6kld%2Fi6b53QZfvWmAR3fVtk86ELH09bhUE8BZfc4wkD6ZLYONCnOoM762BA50%2Fl%2FmsQ5sDVHFXNhFYnbLDp4Y774uIRF8Q%2B3Bl2sJbP4is5elLCPQzel2IY1J88%2FH6lFwzOPwIAteHiKl18rRpnKxIMgeit0tUULZwz9KLybf%2FQzGxcSi5167Nz9z%2F579glMtBiunoUTbC2LgmEoreEojzBZjQnzTLBXkXcbOg9QNf6LbWyo1DQ3YrZ8WhA5x96%2F3%2F9T4eQpEwepcLnERZcKEY0%2BXM3N7i66DvXAGXeF0ofW4qkwLJYZNTIePo%2FhBi4UKXBZWaLo5Z80w9rSRxwY6pgF4cCIMIzw5ke4fjOCQLTC5hLb%2FdEDlj4gQQ9CieO6PzzuceSKfeQqR9XvbNP2IL%2FvDL1EHCj0dLi1aQR2essSy3CA0hLgYKW168wjwYyAnwjQL%2FirnPQiwhKzxwDSvWvbURiQtBbXU7yuyRHDroXbNQGPx8axUXBlqEW8vreh6WRAqJuH6Wosr9wFM1%2Fcg8fanH%2F%2FZtNcxA1H5sllRgQnk7PsKkv3f&X-Amz-Signature=3dd3570001130462966003305e13295b044341c3ffd2e3acb09f2f767e597d9a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

