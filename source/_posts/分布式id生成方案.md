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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2NDRGDW%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJGMEQCIFWICmlE7rvwrX2fYz7oBzwvKX%2BxiS5wf1n%2FNoknEUfjAiBQPr4VYhmCLPVvRWHD7WbGroNWYNM%2B8XaP7sMa9N%2BI0yr%2FAwhAEAAaDDYzNzQyMzE4MzgwNSIMpNUYlrpF7EflTXpfKtwDpfRYeNfnmlzvdnEhma1KJTxeIbXgWUGUmf92eyYMHhCYlpiFfUYbHuGeGojG5IqN2Vgp3krZ7jHpJ%2FKje22QyZpv8pDVr5GKl5yCK6fheCkGKtBGeUWaNxSwQ76B8EV2SoMfeeNcpXPejaFUMdMjLjijMik3WHCKzWpU0Mqzevpwyky9NaCirUZFBTPaohwl8jvBVwE%2F4kCiuGPzDdxxOOQpgK5jRrT3%2FV5T%2FXFZXujNXjWOUKGzk0arF%2BK6VWqVno43oBZuT9j0CPPUwJlcxxXvEbgc6P%2BlMPLopYBQMR1ozmoQ6T0E8%2BiDK%2BQghs0c1gggyPKsYy0RhPGLBsb51HMns4KA6HoDuDlJER2dgn56iLR7Wde9Bxp5peCLtaj%2F9bUWmHGYLUe2ng2pP8AuAHp8ZzZP5OVKvqrHYh%2BeQO79LYO9DJ66ZU83inT%2BzG%2BixIsY5TZ3tgv6f9e9nr7x0KHSX8FXAswc%2BUFXC4Gm%2FgpYPtOSGxRkxRlo%2BznrFJWAFv5CksXK4fzhZrgqEGtGA69mik6ZG2q%2BCZSTxQdhNiiI4yTemjjv4KDHzMhmli%2BudaafzuecZJCYipURC3aIwzXSoi1bWRy%2FDWOIrJo%2Fa01GPyHgcBqMto97QXgw%2FLqMyQY6pgG%2BEZuC9kSxkRXaQxoP1lhGk%2BcRBWdR8CWb%2B9W3ID7HMfHUhigOpvM4%2BJlk%2BR1DE6%2FC3zZtWkUex3cDOqZKOI%2FCiYU5MRNeQWPhJlcIrTdJZfa3hx1Za5ZlMF%2FWjSdzqAyqdSa12NHswYzc9HPiwlzGKrGH4qemp9eZHwBikFf5I2S2ORlhsxAQtsrMkCKyUtoH5iXd2LcEfuH1jpQyLi%2BKYe62fzF0&X-Amz-Signature=e9213f60d7a7b8acfed72c62043b13a2f1aa0450327fc15553a2058b15e6fbbb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

