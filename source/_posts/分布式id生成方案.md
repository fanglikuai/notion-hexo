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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q2CC2HJE%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T010040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAGCUk4mxHpp77kZBVoT0fgjwQdtRxffIz9fBhMKJQFeAiEAt4idbxng%2B1hl8yMUmvM64t8CZ2u%2FRWZENRouoyi2H6Uq%2FwMIURAAGgw2Mzc0MjMxODM4MDUiDH%2FANPpwinA8N2XPPyrcA2%2F4H0Hz59XCSjh97atvXNFsHE0ndEaQIDHVK2jqlEV%2BDmE4uPxD1fhCM7Ong0s%2FRIXrEgDpXuvJMcaWJi7F5qa7u%2BMKARUZf%2BoV8VqUU2CG8SErOH0Kiqzdvc4csRdsU7l%2Fec06KfmRLK4kNcEBvDjW1hPb%2FSBboJdDYbFYft26iQvx7%2BFMc60scMKiS3mLDwS1hcDBodgR7MKwahiLclwO%2FYdZG5W8earj6luCBdWoh1%2BqBKepSzQcgDtlUysKR0%2Blz4Ir7MqdZtbLKOTFnUTWw2lg8KU9bsjewbLKk2AQnqMqjrQ1bstwO6ZBi9EoJNg7o%2BoFj7AKAp0C7BHyIJlqxB9ELM339dRIW8xinbCWYNIMur53rk9%2BMHttOjKwKSXWtp1Vf0PoJmIw%2F879p1ftjPRgiTA5uBNABG80SlV%2FPS2vaNO2Lg0UbQRJB%2BPwZh3r6EeIBt7uh5G2Wz4Ou%2FAJCL1GfrEzxPcxppfyIa02qvrbo8G3dj%2BKNonCRSI94khahbdk2QX3H%2FzzW0nINEMplERSTj38HPkKrlIkjWMEvXH4hDgkuH9tHJ4eYG9xoPONe0fp%2BZ81o%2FDLZQBz2blFGzs0ifHpGLEVm5I47EO0SFydWMN4GdWDkOBAMOHPgccGOqUBBn%2FU1jJgYkdcWuzQethfm4ZrfI6jGHki4jdPZ6nD74Jw%2FHnfzLYCb%2FsfmfvO6lFF1IMJzTOscKEWVxl09gwxMiO0lGs9IGlSWW3AYae%2FCOYQa01sskc7ePEuZFc2lulMSO5CW2Ah3mysTDPZf18yoocfSsM9Yd2sDTK3iDuLRSs4EZvRHRWjCyf08GrKlw9jIwSB%2BvrY4t0l5tEor6GgVaQ7KcOC&X-Amz-Signature=410b2648a4827f002797d978a251e9d4686cd158bf1fd0a2af4d0e80482bb4c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

