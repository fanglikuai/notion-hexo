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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667G3LOTV4%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDdSNp6mMGKs%2FgKue7xUi02cizSD7Ftg%2FszGEzQXhAM7AIgTRI4Ueu0FiPm5jUzf8d9fJq5G0lK%2BqhwKyYUrAuhCc4qiAQIw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIgr1R1%2FFYQtlc3x9CrcAx3xeObIzE9E4e906bkfJD2fAnw70Wq7bqel9owwXsFP5%2FUGZ9eEkfwWQ3cKdFHOsRjiNL4dOvOx8Jg5Ci5Pn5DiHmnTuroC%2FrGnqguz1USt565ctbRBxIZ4nYI%2FMVAsXS3lpHh2v9EZKPT0aer%2BPlvYznSSubFxvNF5L7aZHH%2BQgxDlOv8vsPAT946MTtgfk0rjxW0GGIxOVj%2BlfNk2USbT6rctZ8OTuLLyuFA6h0CfnTSefSsSNX8lJ%2BFngqPTKI17VNFeQP4t6H9igKMw%2F%2B2RgonVrErElz9IDQSmD%2F2BhFdI8WTNeLtSs9LQ4zeRQunZLUqMK%2BpxAo%2FlAjRNz%2Bd9IctyyQYS6Tkl9kMS1FOq5ICZ%2FBXLjLZrEgldKRPqBbcExNDno%2B6KDCvmYyBBdPxBUyhOqbz6XqQ0U2dQGt88BmCDjRy4as6ZpSNqZrBFyz9spTDCyVyRhYjgqUgKrTO8R%2B08Ky9NO1JxX09zIosTz2a7JxJ%2FfxP7HdxgCQmDfU1cUkkhdPIziXKgSsPTiMc%2BKJ%2F5YvTc3BBSNlQywuGVfgRU6H9Qkn0qQHHma3j4yepyGGp%2BLpQ89lpP9i%2FDRXVtonLrGqnsEZNzY5Mk96jXcL4yJpu1OZf%2FH9MeMKvguMgGOqUBqSA9G3qwcj3yUpH%2B5eNXQRiEhmkvj3dntBUaP7cFAeYwLwnG2gPWJn2DknqkpsHP7vidRvlPpt3CcYYGnip1uuM3GCpNmjfHTmfksRPzQ0HgbHA1Xd5eZqZdhEI%2BrcfWxQHX3y%2BR33L1XK%2FU5T3ocvGmJLbrFiPt4oiIhiV4qjj5B2VMDqS6R8Cc3cYpMMtKraCswZiTVtOtSF72qQoivJyhh3kO&X-Amz-Signature=53de6e70fd23a82f087111d5d82ad61a6cc71c6bbc5892401c2df84428ca0f08&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

