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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46633NQSE4D%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAc1SXEt52kyj0%2BZS%2BX0xXh%2Fp6EZzQQur4Zdnfqmjfg3AiBPOIFTzbU7sxWMhOoHaIm3jS62LyVXEo67ODGroLYLBir%2FAwhEEAAaDDYzNzQyMzE4MzgwNSIMeIRzVUrjAp3lyNERKtwDfMrOx9cZVgoBjg7h7dCmia9GIhvHzF5xv%2FSZouI1uml8OrnMfGmY7PnBAUZDry%2B9ioo4n7oX%2F5L%2Fe0PaZaFW0IGx4sJwa%2FZwrvvyK%2F9pJDGMEMcqV24nmAg9WtEqkc6vb%2FLoleAQRS8fTlxsdsdfV%2Bkz1PicrdsIP4JXEW%2BBcl9w6oj6ysFKIdstCWvWDtK07SX%2BrshFuh9Hl70Ev%2FYt8lIl%2FpLFlKlH6rHzpv%2FrD4QxgGar5L8XCWdZvwsuJl1Iu%2BAk1EdWQdFjimXZ3ZPvhMDgcAdvqdXklnVHEDz%2BVOAweFqylqS0950Y68W3hib0Xtg90r3NlUjR4kxGsXUJSyTZBhTofRpemo43sgZou9pRZTPYfGnlGeDlwZiK9qA3gLOnkMVzmpjFj2GzuMWth0iMVwJSH%2BlmsP%2BcnvplKd8fIP2K1pO%2BFcOF0cyF8vDm6WQNc7jM1prhLzROEzKFQ0PDgFiMZOkwH5AF3rPwEYg6An%2BUUHp55NsmR8yP87jf6%2F34reeVSfQXIpAKNa56tDUWByj5g%2FBrbEmwoZpiLO%2FMXJ1ZFrfsvfc2ou5Mhvm7tCvps%2FWQuvRxfmX0JX0I0%2FGrgJ0bCrR%2BTdkNl5EkSTAnEo1BdhL698ks4TQwhc%2F%2BxgY6pgHTtWBW2eGbi2%2Fbsvm6MCMu28x5h81ur79XeIxO1m8uhxvpsun5nLk4%2F1EDgMvzBnF8OZMdpBbVlmGqhtquxFovIkuhlHATLpA0DRA0su%2FOMIqthFQq72ImVA9DTD%2B9xPTFhxnkTEKmUXE92zRcXsbJ1lJ%2FEMCTQ3rJB6yCAr%2FFr2wwttnmQZl6cjan%2Bk09q%2BmNQx7aeNjhBeS%2Fg%2B4J2dPS3R4vde5K&X-Amz-Signature=5dfbc1cb31a5dd778f6729b6ba71136e0ad58a7b027b0ad8bb54931634124e31&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

