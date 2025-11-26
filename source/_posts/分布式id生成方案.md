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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46647J47XWG%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCbbByXY5qQ0Yp4YTYCMzDXJSzQeGTCYQOKzlh7fvryMQIgJqUEl8ix7gWxwXudqofh00UT0sp4QW21L2q9nBuuJJoqiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKBvEQHXv5KtJ6DO2ircA3Id7ybQRIm52x8xJDMvhimMf0dwrIj3eErKe9KYWW8p2UVLtdXV2E6BIqO0O2sFNHDIyfYGwUJWV2NsxbIY%2FpJZ%2BJ%2FMoK5rxLDmFmY64oVgfs8pdUmdDLKUjSw1Rhc58oJOE81FtUoW2FZWg9iVWV8VWv5c3HhTYSWI28jD5%2BKba9X8hkb%2B0s6NlUWC%2BbDifuK9k%2FtJAqMINZBXN7qbXTkKW365a9jhBRuvG1pzyemVRkryqXw8U9KXiFpsdTOnkThliDcHPlkGRx0C1ey8hMQNBZvOhA23npDTYDYzIEv75PJyMdfXYpnDSwMYt8EmOsdxhgVfw83uj%2Fe5jhSMhjBAeUqSUro6RmXRZW5NL34qo32tZX5UB5Q8C5APEBcE5c%2FDsAIndXyenitTayn4pMW1nYnrQMpsR0hiW%2FUD2GsrnyXJX0XtSZLWeItNqDzJj49mai03cmLkgXhbL20upBfImMulOW%2FRu1PrqiZ05Qv26GFanbdzTWTBTvLBadAeImKZMkcVQ6TLHOG2hD16lkxZpJP2jI3V5axDNQ5zHtl1yw%2BEUD5hHMYy5wWRGuknBmWZ9y1fcuPZegC0SIZJ%2FBR8cYwC1hsK%2B%2BvjtVHtLcccIDkY9dJ6KqICSomUMIeNm8kGOqUBieLnblKx8pFNWgz4uxo9DNq8ISEFF1Pv9gyFt8ZRjk0cCEkhoB7Dx5jPgNQSprpC1ag2nY0iPyb5TPOGnKLgS1dch%2BRaIJWoPbVv2KQ0R9KB%2B9Xetke4N0YHNAjhRSl%2BUJYWF0xhZFS2xXyFYVhhUJwp3OdyN%2BqyFaD4Fkqg7%2Fy%2BvK1KgyxoFR3scRQOwHp5hc22miKo3ZmSw3%2FjpT7EcN7VkE6c&X-Amz-Signature=de1179e734f66d2014853cb9a6256400886005cc37762e5735e43021b99dc65f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

