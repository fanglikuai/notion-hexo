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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSAJYO3Y%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDndZ4gttarasSnwswoQUVuh48eyykLynN45GYRTgSEsAIhAKWw1sXPFh2ErtHKQvrsDCoU2O1%2FGVRaPcSQDF4R5eN1Kv8DCEsQABoMNjM3NDIzMTgzODA1IgzAN%2FToGuWQSK2%2FZ20q3AN5YHCO%2BQY%2BouMdWmRDnoTBZa6RSVPAZJd5BTdD%2FqB%2B5DT%2FAlr0HcYGD%2Bmn2GevHKSZ09jVHf6Dz1RArXBZHugOZxmEBYv5PIjOxvK1zjKJfYBkqifmJH2k7sDj0IGLD0q9YBSK5tMA9yH67Wbfi4riF6PLn5HFXQqcKN4ARqga2FdFdNv3daSfZaY%2B019V%2FmMpr%2FzoxZofP62H5pP%2BQOi%2Ft4oiR%2B7u2GTyuoKdgXj8UL9%2FfxyupDJbOXxcKa4bREjeN%2BK7SkUn2AL%2FKAYjC9kVk5xAr3R97YmxtU04w1TTeZ9bEoTMP01C7ke%2Fri7r4LHek%2FtXzt01WOXIKRJvFpsd8mL%2Bqr5Nnfdh0rN1RxKUAcNbs2mmbLu4b%2FVFxNDAlyWJEPevdnJXKZCJYc5GVvKB8n4E831k0oaKCyAmJCEB2Lg9f0BZ3VwBakBWEK4t6npWudKF19j%2FVyAjKUp5wBm6UgD2L1mX7GknQuX0kdlhvf5d8OGx6F9kIPXwqHQzS5Mc4%2BWSk9S1RK%2FrpW95nPguxUYj4JBoG5KNblE%2FhYldK78f2nsFqDuUYRtQQbxBCrs7keLuECacqpYnuUDtZ09D7b%2F6PtWyIHPicpA%2BTX26d9Apk7pbo3aroCgCzjD58bTHBjqkATswyvbf6GZS%2FlzPhLRyRwxs7gHvA6fF%2FwSLHGgIGZOG0BXSugOXUxYAOJvGKQ5UN918BgRavNgyKCPatE8HDu5O91JZO3q4yOA%2FVG5bldyQ6zGtCGuSX%2F63Vrew6R2ErxuNUvovgRwc%2BN0edtp8tOp4VObl%2Bo%2BTb1hf9zd6wMzVhihVnX4MiMTk8cBLNIp3AJTtjJQO%2BhwXEkWs7mbt%2FPN33YZG&X-Amz-Signature=c3e4feb65e91f733561c12059961d5a63f6493a21d306936afe5cc1471c349ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

