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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VJLHNN4W%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T040049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCiH%2BD%2FUdpcMkn5GebDH8RmXIXzSEJrEydAMX2ieZDoCgIhAPoMf8fmgo2DFrUHVHfZc%2Blvz1Ts8X4BeJ9dhOw92rowKogECJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy4puKQNROmPvpcAAAq3AOfLj8c1XY%2BRJXcudAgHSaRYE%2F%2FEjEs2ELXRoazh0TewKJw%2BEUu9sWab1LbrAW5gTLlXfYtDBS3jVLTE7ggR6kfvwNV1bDuzXynUof%2FDatLNsaN6JiEtz7ZRlCP0yvl13sAzWYBF5ND5eucO043JFhISfkSqETXxSejodJlXgU0I%2FH3bssyCoSTW79BCsx2PgRvT8Hohta4seSV4HRWuT948Z9oet87lUFWqhqW7Qj3wgtSaHtXbt%2BIuaq%2BEaCWMwpRubDdVa0qMhaCB8iHq6%2FNHFQh8pCxRM8uoG1aOb2XTvuiiTdj7pcK1%2FUlAh5VzfMOORb7iwjIBCSO02ZelswZmZnHOQXSs35QuWShXmcFl%2BaaIvzv92XIoriWeav6v44BUB%2BaD7cPk5b443wghVK4%2FZ5gVwCPGaca2C1s6GLcPljebEvTIQL%2BGjRUkYe%2F6SD7Nf43StvVdwtYBIvYIiYHGLvGMzdpDlSBCw2t1MUzWPzxd7WUPWG9wKf88zilV5fWywZ1RH9IDIWrlN5Ejdt7KFEkZFshB7mhUYPbV7HWqIPoTOpj2F32dEfHqeCca7nMDtncz3%2FMp1p%2BPNIiRybv3XutU5e1ioijSuSKdTriGo7goPT1m9USXmXUWjCCurDIBjqkAcWWVFT33Fp1ApYi9sRS2Gr6LWJfwiMXmgaNUsOY6IGusL%2Fxp1PfZSwWFat5zv5u6gMfAt1advu%2BdrSWFP0%2FqXv1MbCwkSS03%2Bvs%2BIOvnFL5oNNr%2FVUt8dzB%2BwIzyIG44TcDSYEAdY4eBiKRBAEzfosFP9cYmHnDcFcCl17FOq45xUHlJ69AAOG4%2FYzOFJdXdsr%2F9%2FArL3NG%2F6LY%2F%2BTNV7cda1nK&X-Amz-Signature=b6baa03f40fa05444aa82f2e610c0354a39d01c6925a6dd63dabdfc38a18a0c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

