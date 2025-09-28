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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YPMNT4CG%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIQDTv3GEQ3VUPwE0S7eKQKqKJJpUbJvqox9nTd1McU7bAgIgWTT0yPqprtHRSN4xX25do2V6s5lwE%2BJog55hgznXM4sqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNK%2BD%2BjFLtDsmn%2F8PircA3%2B0ObGOcma8IM3r3F5XreBC4Bi65QOuRyghgvPmmKivHbt9qh8XGIXZ9p%2BZzfME62rjpvlKYdZw9CO7mEDlszkVXJeRBtSpMc6h4aVavhKGoGIt%2B0KAIGZzk3YERe6l2c1dtmmhO5aLFWapjbBY0v%2BygrWiJmuvH2yFEFpMVs3YDNjWgg0iWvkLYC%2F5jJy26TtALj%2FF84beTt4VZ%2FkI8YRdr82ScOYMJeQvA4j1Qlbplu9mHduFfiHXF%2B08s3Thtq6EysV%2Fo993OleXrf4%2B%2B6Ui3Y0ZcAmabNh5Pbd6Cx46rZ40m7mRlDcA0tUCcpTi6HDIl4At%2B2wFKSXsWN5sXYeaT1Ih9orzmxdCoNg49tuimfWLhQ9pg2SK72xkzRTjYa0cm2pJpPRAu%2BUfj9rrB7ksWgFjpEyWs73Myyh5aaebLVPjYBb3or1OQYURuvguKeCocLINc6o59L7V1bg8f4MLQoiUYYOzDTfnQvXv%2FSo%2BSsrEKuROx3%2BzJvC%2BPjT5pujXcr135gNulBO6yHdDPf8zZ1X0106jZO%2FpIYnxW%2FA1mlP9y76gvqu9W9VMupaezQqVIjtXYFQvihvMGeA2eyNQImFTz9Lkxow4y2s7BTEj8Yvf2auTgm2nkiAIMNDX5cYGOqUB9UHUUUTJx0HWJ5t8%2Bv0tJ%2FlWeViz6Nnlg%2BfIoHOe0hkzLI6RZQh4OkUPVn97PcgOYHKYNUVWKm%2FUQnNbx5ov5JW4mJcya5JwcTvcbJh1tjJ59RI%2BWISJ0qTNQcG2zX0CrTGB%2FaFg31Yc30X04P4ZR0D2V%2BjxhzU1z%2Fq55W4t6UKqMZR%2BC9DSPDek0ZIrNLnr0N8cx13AkWg2ir8q%2FvNJ3JO9fgkC&X-Amz-Signature=d20ae545dd4de4dbbb776e333d23250f2030a33fee69a307baf14b978df48bd3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

