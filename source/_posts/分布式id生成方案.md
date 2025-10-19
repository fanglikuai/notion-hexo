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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQXINEFQ%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T090047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIQC1ExpW93gOgl28pcKLXqvZVv3A8zoahByuTZE%2BigMtIwIgdcpkuY0jj57FIZgPRhaPBak5lnncfQ4HOk3sKHZWny8qiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFHNcuB9TGEQTt2%2BNCrcAzGwA23CPLcOKnt9UtDdMDQjo4z6zLdnil5yrH3%2B2u1ndnypjsoTmJXRvlHT7eJ4T24S0Mqn9mjmtHTOlgis%2FALKN%2Ft5dV17GUDUKN07zQxJco%2BHHqSj5aGoJueCmgzBvjTCaD4w36KJJD%2FWl%2B%2Fyj%2F6cxGbvU1BbIBzcxZKgKwcxtgE%2BIg%2BM2I1q2D6KRpG2xWYC5gdTzsC9IdcY7yR2VSktsrHM44MfD2pV8SjCc7lHtMwT9zjMcD9iuSjplapAk0JoNXfx9paV08L5fEODN%2FEj0sKf796UALPdqtBAgNp%2FIZZb91zBjmLQXrCcu9Xb8qRq%2Fp0x8UT0Wox3AcTCjqqDkTYIpITIMMNiS6M2PkrLY2tvIYMlDWtLhuVUKIaw4Tn%2B5SwXztxgqdGlZcIOb79qW4vF8qVIMBWxg25HG3V3mjh72fmSi9tpSao508gZ3UNYOrgcSlUxiK1E67Nw9J8qgPPhjri5sFUpEof7%2F6MRbZ%2FgrsYOobZnmMmsCZe3JGf2%2BIS%2BWv0cgm7PLBk8V0PkxQB0xj9qrx3QxBmtLHSKQf24CUlwOv7MaE5JzKzLKY4MdZAv3xS%2BoDskI4WTcONKu5nBiwn9S6HF9cqmkWj%2FTlHmR0BjsL88w%2FFCMICK0scGOqUBCfj%2FCE5b%2F1NDk7Wrkq%2FWJeVvE1OgEZppM%2Ft4DJi13sixfit91GJCr%2ButkzOlZNKi2uhl%2FCv9s8McsajuSIXUqPxmDqA4EP1eyiHm0fJZ%2BuzJzIJnLU2a3CcSqTny6xNfVKdqOjyp9G25K8WzWRAd%2FaTof1%2BsqYljjIdgcIQd7rl0BciAQyubvavAANdU8F%2B9F8KEDnJVX28BHSikxfhtDpRcOhSR&X-Amz-Signature=6525627c89c16470c54fa448dbb16e775d2146786966de9ff22de19f8251c8b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

