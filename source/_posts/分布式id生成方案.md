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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHG45VRB%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T100039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFjerAtq2f%2Bw95MzS4BHqIzXvVB8tVvGWTNZAzG8t7FHAiEAoT9PdTI2CEhPGb0GuyNwEyj1ZearXsgAgKBTrn5RtQsq%2FwMIQxAAGgw2Mzc0MjMxODM4MDUiDG09s2RmhCd22egJFSrcAx11d8f2fRo9l4IT4%2B6umBpHrzxiTMWIrq65cXaFnjHgo2uaN7krKq5Se2VT7UGoUpdfYsm6DBkO%2FxaMVBVaVkmNmHRoL94dKiiPzlxX8ijiWkq1fupex2l4dM2yNB27mG7XVCCYsSs02nSoFErZNekH1O1Ei74y8bdd3bvIUvMWPEgZqZ0VmvOfjhc9nWhRMEI5NmqJG8kbeXa6QiowjjCpRxBvOTKC7MVSg16EnADmyezJ4tPfe%2FkO1rsqYIUnwWL88kGJVgjpyIkHumje0z%2Fv83aNZvTDGjELhS7efzPUUKsaJ5%2B4d2f3F%2F9BAwNEX9Yqkvn6ehiAaqCo9RkKZ8kijD1A%2BnZKtrgLRd26COdbgUiVIoZL5gOxzF2BpjO4jGnu1WZxxLURJNFpRxPMPZdifr6rMQTvQEndModOy%2BK%2FU0PasMuQxYjF1%2B56R3duvRNp0uOM8v3vZM9yYMVoJFlSrnjEmfG4Ewlfz4EXFyCacN0GRspA1F0QaarEG3lC14H12Z9XTdly7oaeE%2F%2FnKmiTjtGO6XYirZLOzp1zicPBzufwD13HymDQ3IDS%2FBnw%2BvGgKF%2FSwk3wfXOXDkdF%2BzNZRB5NgZaOLa6dTu7qb4fu0FuF4aVSY%2BA9ZBMqMP3aycYGOqUBpTQZZpkkuISABXhQcaYzBXfcBtoRjYk4j0I0wW4YFAYlrKn8y9Ay2%2FGIDuv5Wbe8VET1%2BKPRatVXyweev%2FpNl3HnkJsuTFK9t186SSWVH3GxvG%2ByKblyD2LP5tWszb5SpHDBItXmY%2Bh%2BolnKoaDUA4V245KCyPvCACOTShgz08mTxQ1wf2sXtLUt3Io%2FFOiLBghCEJkI61QkQ8ebblKskzdNH1lP&X-Amz-Signature=3c2b792bab5ac484eb91a6c4e51060b2d421eb4cfa7cb1534e39d355b6db7b71&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

