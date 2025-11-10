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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WBGN3DOI%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJIMEYCIQDLxSzgAdeSCJStTMx4BQXcWQIpII%2BxJJ5DdFqYxUy%2BsAIhAOSKMNA0JMFm4EMZzZcp%2BzErwwp2kquwD5Uh%2BHDGS1aUKv8DCAIQABoMNjM3NDIzMTgzODA1Igx2qX30h96Wd%2BaAsHUq3APGUq6aiEJsiTcFI7%2FEYhYi0TV5CPblZpF1bWbL9jEgUEavh9zHlaPbe%2B3gsV4v1T7%2BFZzOr13bNyk65v4FXfNLj%2FDARxWh9Dhey%2BSuvA3OwuC3ymbRUWsLqRX9p2ba0D76MaY2wMUB0BSLxhGCFscx8jlWdLM%2FI3Mb7U4ZmfJXebMQp3Jxlu%2FhoEELzSkjOPpN%2Fuj28z846rqev%2B%2BhA82DNowXggr2SXL6qqNYwTk5Tge7dCoV%2BqCUuaP7m20DG%2BDnFDYn%2B6PAzjpCHbx%2ByLSY4Kwc9Fuu0G%2BkAxBig9WKRNnbA%2FZfUPX1uzAj6zrBOpzHmajlkH35n%2BQtbvtZvjoOqH1qmaX2zQa%2B1ZZYLv4edag5AN1o%2BIyZ%2FCGrt9FVBqk2yG7zKacTn8TFvrkmZ48VHhKmJS1T1ex6vGo5CCDy1XYjE1yadgbMzf7fu9zHGIzLORp4dGbC6GpTdi6FfeQpV2sw8aIOmVaRHHGdCpdb%2BIL1Rv4GWgC9M1r61f8Kg9pCqGHfaWNP%2FVxBcnx1Rq5YjgzJw%2Bgwxrjnl88bZe1lkyE49brIwYM%2F8FWvae6y92MdsvLa77XY%2B3gVT3B0xzG9CXtJzhG6SjDEgaZtKK3TR%2BNzcX4M5uoNHeX6XjDY1sbIBjqkAfbuIASCcxsRHGuCztfVEKnE4Xb9HX6iimsHEXPmJ%2BZjYX043ALX2ekkUlyMX%2FkVm7UHelUgVWTLNR6Yibx1iZNDKROQ8oDCpocMfmqjo2su2pLTjnDcVTACnp4BY7KzeUJ8w5kGNmXiCYDoWC6lScQLSMb2vsH9STMAlEM3JOVItKETzQEwmmJva7uNUzqXMR02jXrhh6nc6EoYc4m5HTO2GUop&X-Amz-Signature=23828b2f7c0601e6684d14e3a0cd463d56b1f85db69723453f8afb2f8a2387f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

