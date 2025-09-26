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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMKIUBKA%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T010045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDichzHq%2Bc1C7aSVYIGqWa5q8CGYacXCnooYpdvt8C5XwIgeKvd0oNBaigBQJMUcHCJcJlWfHdF%2FBf7v2rAGRF2ew0qiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDP065v6zw4hnXu57WircAx4ckn7tu4HUPdVjZ1np%2BEmVVWNnTKer0pHxz5GMx6xOZczxxB8IAOL%2B9zcEKgDcUZaf%2BeCm5snAZFuGxf4KjDRYaXK3Rxi8b8M9BQVfvSQhyfPCyS6Z9dg%2Bt6uxMuBRdfwrABq9nVBLeOL%2FOUVIFcVV%2FMPjVSWp0JRUptY4lnSGUrvY%2BFrFbxWFhZDZtSnrQtsdJGWqM50ymlUdGOKBaeHUeRZkY6bpqxnDyjipTzUc5b%2B4jHuoQEYjw94LKGoYwZqQSNQdWSuYU1Ci6QlcCEyUyzQrwCFWmM3D4On5%2FwY5tbgu5iIcg4HxiOyWTtOsMphBzZv9DSfIf%2Fv%2F23PC4wUOG%2Fd6WPj7aM%2BaVc%2BgPyqlDpalNyldgJ0MJJQce5zxxNFIReIVARk2inNgoPiJcEKqo2kdP9%2FYtI5hHaJF6rXyyxR9qM4xb9OMRG8kD52COZ%2B72Tod1DKdhZBhYs%2F69ohoV8VXtbLOnERsv3YpIUAl%2Fgbx%2FUakXXMLVJvEpJbB8Nh1BJMa71L0%2BkB48Vx5XOsAedtlJLNQ19s%2FJL2C6wG0dTBWlSqBfYG1kr7tMN5CxZZdqFggS9E796%2FcVVHU1Bb0euptkbp1GhVh8NpYyQ9VUOj3Zh7W6PIjjq1OMKXD18YGOqUBDj8av5P4QQHSWBCugHr%2BGDmhW5LsIbDH%2Bp3ouLWn93t%2FbymuqLJr2S3e5WimwuCOq4xNjh9jJ8Wx8Vkxj3UegJR1rlU7ExtDKsQH4NyxDdw9zpJMMGZ9w0Use4caE2aFlKMrQRN%2Fv6CcaOhYcX2ESjevbi0BuagGqQPLzFPz%2FenforcHOlr4g%2F1hYeAuqDKgGhRU2VsA2ng5iuP2Fm5aOvh4cMBL&X-Amz-Signature=d391f38f8c2443a332c2d318f14fffe779693174159d7af2ea8ae84f442c1ea6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

