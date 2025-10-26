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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y3OS44A3%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBCerCjW3602lzhdyoVfxey6NPMlZW7Bbj04Af1csxqMAiAgWCq9UMAurNJ4KzfipAs%2BhF1A3jBhyihyTpDgxoMNliqIBAiT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMuywDyOOyPC0B2xChKtwDTbunwH1yQUkRWaJuPNUU7tKESqsk%2BzL3VkcHqZYz0XXES5yJFjzbVt%2FQqL5UomZizA5nJ6R0WS%2FYqMb2hAlpBkMVIOSvOAuxP1t4oROrk1f%2FyVvI%2B6EKTaoeH5G4SZE6%2BiGqDwchTlRlz0ZewaIETX6ldESr7iFig84O0o0Vw3%2Bh1YN5gcjHVhSkoc%2Faj5X3Dc63Vv7MlWmPuNrS6cR0zBECbNTr4IpOd0WnHnTq%2FfLEM2XKOOIPNAoFFzacKP1nli5JWCLonSsT43HohCF9AXqCntTIj1C4k6EO9ON2hfojJTbDWLA3XOi6ml8IRCeUAEs7qJIbv6%2BZufKCIpBQqqhFqEFwM4HivrCbZdARFS1CpTEB5UZnhIk%2BVX%2BE7zU4%2FeFXwtWct7HR6Sb52Q3BAYdWbOSQTsSd0vssz4cEJ7GVePYaq3Mbai%2F2N39JrjI8KMGO7CASpEehLZ5pR9fKhatSm%2BF7PO6aKSvuWvhIfY9IYyblYIxRDxQcs6kRM6fNIivRz6XpYpORlN%2Br0cLmR7qggSqdURsREbZZNd7D4C%2FWS6YwxFgz%2F21bYw6rY4IHbzS1zLPR1n%2FFcz2Zf4tV90Vs0CP0cg0s4mO3ZBos5j12Qoh%2BLSGDI5MHHM4w%2Fs75xwY6pgHPb%2Fk0X4kW8jwVsztVvSAaGZW6NmZVKbdvInMrJxioI%2BFYy%2Bx%2Fx3HIiiyE5BO3yQ1wd9b5tbJ3QpJOQva06Ir2z5wEKQaiDnGroYPyQTKPSDxr2a7HBbCoYtT7nNuBOpsr4yZ2pNEBFHxL%2F9cWwoZEtZYTLoW30JZP9fxCArJNxxffT%2BB%2BVmV%2BxF4odPIIbTHm%2Ba2UtO1%2F8nXPiDcx7xBlFQd%2FTGKQ&X-Amz-Signature=eeba6d3f0ca985707c90ee6a809e025cfe33396ad7aca785c32e2781c2926dfb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

