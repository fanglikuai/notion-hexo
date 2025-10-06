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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663VT4KNQT%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDGw1iFQiF6b%2BzhdHTOKEvSzBBLGXNVfscpW8Z7f3VlNAIgLdTiitj0QemXNoHakcRlCDWEirCTyHKDAkLJNKU4AVQqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD8oo9pAIvZ8W6zdeCrcA2mUP%2F1L7epmXAAg4t3mNMzJk%2BHIsrA%2FeV%2Bc15seULjGetObY58iuR7MUq6cnsKWiK4mbOqu%2FzYW7naDF%2Bh3nf%2BNsXuuudJI1OukprxE8QEHwF45M%2FH9DmvYBioT%2BRsgjpudRSzdnN0IRszSwQbDD3JGRJyOydc%2FD05v3A0LnZBUe3hztFktjhk1Le%2BhTqQ%2FkG%2BIU913BmkAP0S9%2FNAq%2BtoRoE3bj%2B5v%2FlrM%2FB1voFXaJZvQVGBr6P05zRHBcHOA4X1k%2FPlp%2B%2FbP6YiZ4uprK18ZEK7yEsmRL4%2F4A5BCOezhkynKPuaCP%2FGzryMMrzJfHW7h9vohZrPY441S%2BE1NAnKTIcjbAaO%2FQGxM97Ik14mOKbUWbDPNl2OuMixupepM6n%2Fcci2kPi8JKsKgtdo6IFYxnTNEf8cl1ZeQUvVuiXNPCsxRGUtIxVOzpsKwXm96MU83KUke7ZQea6%2BTmKB1RbGhD1UX851tijvQzyYly54ImLU6Q9grXLxmdNiTK%2BeTwSwftXj%2F5v%2BCmIxjolB953kTKP8ucmp1XTEfe4qC8Drr1XPHOLdaD0vleqfJ9JKrkv34GMNtbJyHLim9jYHL%2BX%2Fjqgl2F3RzRa1vpx4Nl5e3dlQCx422KXEk47WEMP%2BNjccGOqUBpfCwuDsssyxeUhJF8Yr2APhsQBtoRZcV2W0bk2YxwMiIQ6uOMVN8ODHXSjp0B71xqrWPqoS%2B20wk4pG%2BgWJzZOG6gWOR1PDS3hxAuNa1k3927IjQCLQJkiOqEXHFQUEzQsCzaq%2B2ikpAd1G0i9WOqwjoeAYNrU%2Fa4ukawGuBI0oJxrHENNMVhZtf6Td2EhxE5N9q8K1gGJmKimu6sUDM%2FTEnnJ8p&X-Amz-Signature=aa0f57b122fc35e685ed4c4706fbda6b35a1a5123f9d87510b4eb79180d0e5c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

