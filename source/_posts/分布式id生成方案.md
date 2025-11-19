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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OCE5DVV%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T080056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQClpqPeSTkWTf81h1YwzM9amJy%2FRt4asf9Mlrv5bD8ZoQIhAPy3Das9UUMgf%2FaUsp0Zfq2YMLSqe1ZKBiIJNTXywx5oKogECNj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyHfVlOuIWxVZIGP9Aq3AM3vdpZb7Bx6IikC3%2FM2Xn8RT2AlZzRlnvCrbWuosnbgXUnFyFsHVnUkFjoapbAnnBsLJvoYq5djR8adzs5feyTMIl79%2BpXBA6iRg6hJEf0YlGoGebDH7sXURUt7aPj6TFWsmL82U0GJYO3fH6xdy%2Br2lsmR5Q9oW%2Fo1eCOcjjddMtG7fgNlGIZyQdwvJbYN4K73k%2FKXwLkdeL%2FCV4lJ43tffiqjxg2XaesVjExyJCWvZv8ABSsAFGkxv1d8pMvb29QsBj9uZTGkber%2FKpG9XaGMqrghoe4BRy5nghrKkrSMtgvEgFzE8uZB5KMffTjx%2BC0DGo785S3S6%2FtRrEy%2BNx1KmIKjGK01b8k946qTj7F5tuWpHnN7To9Jtc%2Bhysw762z%2Fyd8T5d%2F4ShTXbuM1xJY2AqHCu2UTkXjwQnwiV4a%2F39TuslVcvINQQ1bMn%2Bog2Ut5m5705TwY1QnixrihmiBNPMgBCGmLK5sGaN4Ymi48eC6gZ7zKruj9CMcRAC%2F00dqqA2WEXvQfxlnLM%2BKKJUqcgwxn09RndEZ1bXm4zpEYF5rDDw53WYhu8XjHjTHKZZ86YhpJVagBfaZ7z7R5Bt4Tyt%2F6326cbnyiSSZ0PNrzwUCq3TbR2C2zV9hVDD%2F0vXIBjqkARnbUGYt98hqB2aM45MubTe5DZ8iuBeVthXCfOhUAemB5QC%2B8z%2FDsERJOZa3SY3gsnH6TYT0pL%2FA8vHYjyiTD5zsYO7cp9qxbcZKZqFTRDUD2uZNXvDIslkVKtmqy0ym7NeIad3Rraa11NUYTSDfdm5%2FBDcXVzd9xJguK0dg%2FDd1qLFqrs3iiYr6Yz2CQby2VF6ohIibg2eQP84Y41LFqB4PClft&X-Amz-Signature=b59b73bf89a20c2aac1d57b1bc2b5a725485811744e567522c6bf8bb1fc04053&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

