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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EA6WK4M%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T170051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJIMEYCIQDI9akIpdC%2B7%2FUmqehFQmBpVFMWJPjjPHbDsvtHxxMcwQIhANoGEBtgBH%2FtHl3qIb3S%2B60wGc7OX20Daf1Etdg6e8HWKogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxJBFswWVvi0qbqNeEq3AOZthziLuN%2FRw1m06MYb8dmHcWkJL8c5PjRFujXWMAt82b4Bc9fCDVXvpAiaJfXgw3EBDNa8WwoU2zgLCMdUlY97ko%2FYm6%2BPPG1j19QTmFY%2BNzOZsXkKwLFEw5zNNyNkVtHIqQ49Z6lGY9s6xgFTB%2BDRpSiJg6%2BUa8x1mbKqkVTdcS8xhsnneZ7VFW7Sx%2Btrh%2F4dRHMgTXkEyYSaii7u7sQveUSbr1AWbaOwz%2Fvli80akSSPOwXLIZftsWfSZPCkTtZXVgZYYv86Zh01hkZuhbhPamMw5BkWOMLqUSdpbZGlqj63sEyUkuS4v7lx23ToaqoznpeQh%2BFJbBXs8WjYc%2FRC3ouP8Yk2MjlsPASakc1I3paGrM7%2FSdI3RHvhxSBhfPSu9t%2Fyv8bvuSjt77dNgOIuxQNWTFRGkj3SEdf1dQz3NDWNnv2GXcrhRuwRyL2wFSMaELRgAcsoz2DK%2BfXGlXZ3gfod59KheUAvXVs721lLfJ2d93Esp3dQkzWlqEYNqOlbBAnCjnzFE7kcEwlY5iun8vittTCQ2wrEtvYTMxP9plyjupVWDFBdf%2BTL%2FyIzZ0b8YaERYu5NSVoomCq4zc3MWnZx5SQYmkuwPFA2PAFnuzh49Hu%2FLEzAp2hRjCEg87HBjqkAZzEdsjza6gXvryLAA%2BnyuhwDievDPfmI8Ik9sdRRbTaQvEF3hxR4SePmXjPsMBk0%2FDLnQ56TYtSQW0D%2BOAnlGzgQHI0mPGAA%2BczI9Oo4jidtu97WjuHX73WN42sw5WH5%2BZaCIURj6jqt1h%2FLI3CISyVHvlELupKzvsbRCX9xbHYBDg5Uq5qroUjTk3ailORB5Gcxmzfd54%2FWZtrRnbOsPEnSXJJ&X-Amz-Signature=a6c6f3c5d162af9d27b250e52dc38bb92a4936e2eecfe3f58dfbb14724944285&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

