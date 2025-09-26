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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VV2PGU5L%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T170057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJIMEYCIQCAz%2B9bMq2XHkeF8a8caR58c9hpPbK2zP0nBZEk2zmrkwIhAIZiblhevLO8%2FdGqZX3QntpwUsvJXk1DVCTv3NKKsHlyKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwJxeRhLCmVdJ9g984q3AO8cBYogSC436p2Tx4c24IYJOOYtg4x6GNFjlLoI4C8TQt8R08n8Y%2FFfayZ1NzxQCOSzDzA3gX162dnzgFzZncgcO4Plz5tihKfmZTHdGWiwHbJ0fRWQ19EVuSay94SOQVbVDAShzVx%2B%2BXOT7P%2F4%2BqlvdTkJIkjoNKDiP8bYwEZTZLba2jCbIb2U4LyEOboFvTPWdDZb8pfG7PU8sGwbFccOk0Wa2LCI9dJpuiFNxW8zKcyzatUNUPFq02GNjWeXP6DMaSBXUeb8HD5mIyAboKP0RoC%2BZ6hL4iwcqqxzs8ojpilhX0xB2DqYlwSnSuftHD%2BGbGo2YHZTQAeKC9cbppsZ7pwI5iTGTHHEnPQOutVDBLF3hxljF1cz7Nu8Lud3W3Xn2FUo%2BDTiMlR5s1iKpdF3uGleKfcqQRj3ACVNP15nuvjIiyf5tAFOuvNyiDGJFaRyYpgc8HKKuwSbC2Whqz11%2Fysp7LKsTcdX4b9LgtEEQUtfyWKdelDAWYlpaXJMt3wWAXJKgRUN7e7m5EZ0BbPYrwuHcPfSfb1AVr6H35McURoMHTgowU0XUT2bwDRMj2xc7vsPs1vZtOK4Lh0prJqA7gMQRUX1PDik6Xari9WOnuuc6JVe8yRD0hk7TDsg9vGBjqkAfK%2BmH1%2FMyGQ1qGp4uVZIH1xb%2F3%2BRGaLcKXg%2B4PKU9HdU3dm1%2BqTKwqCMHoe0fTC5KA9%2BaOpfPz9WbmXZUsRSHyu%2Ft6n1FPYynuvHnsVsJ4FqvdyVdl0x4NPdWCJgR%2FbC1HY49UdURCiOQHwoY1F3eOGlsf9WWQCoEHVHRLbsheY4FOtoG%2FBGZahNDlRd5hWa%2BBooAYr9mD1cUwONnxjMFzOaH4w&X-Amz-Signature=496894c68597e9a863a230e4a8b26e4024588c16223368ee18ce4c950260fdb3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

