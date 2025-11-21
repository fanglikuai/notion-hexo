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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CQ3B4NV%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T180054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIQDP52q9PNTe8zWZ6dpYR45PtV8vTX%2BxEDhGsbHizYgpjAIgGqvlYnkH18hPXaOpjkt%2FCvjJ0kaOG4%2FUi%2BtIPJPSnHEq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDLutctyKGLIP0xFxFCrcA%2B2SWAIqoZE43qq0hh9bKKMSTf%2FgWElERQ8PBsvGiWm389ztF%2BxcOIJM6xFw%2B5Uhgax9wOqtvgxTu6kerq59uzNNwN0NtGBguojEABx3KeHbocAkI5EkZshfPIXrhG%2BQKK5URFJNlbuU9PytANlHbysUDwztaaTKeVqccJ7WJ8WxQ0IGPtxjlAK9bZPG%2Fa7xMk8VFMdMHxwrOORQtIVCPwI0keXrJtIo6D5X6VsHI5ts23tY%2FySQP7SwVywZC85Vqql%2BCfC1jU7JpoTaROHFfRicEsg%2BiEeSBORo4blCUPTl5XQRHftTSly7fmQfVDN8Q61zWzh0lLNM5RH6O6aziaM%2BaVoZTCbe4GPTAJdWIlQ51A6If0gAD4zMnZarpkQddrAT5%2B80%2BvTDdnrKYNHmzbgnWKvz1Qo2KDyDuST2VlRLbVVKJXt9eP%2FdzZejSOxMPvuW2mHj7kOqZKUbXmAE%2F%2BIorH8c3L6C428lohG4edQnWk%2FnCDu40aGYAfzfFjvXgV2mJEdIJmJFUvzFEk%2FsWYDT%2F8%2B48gBFu2LpRgEkixOxYR4D6LvVXMstgMgbK4rgS5lgIUXRBI2EbcJE2hSxAYVz7vbtATsQa4%2F2IDlOCtP3a3JCR5dtZcNqozs1MIvJgskGOqUBSj%2BqVURJyVXjLhV2RFOf%2F131gcvqQnkQCSr1UVwjW%2BViftDiJSpD7uy%2ByN9BAOMgQ4JcErKe1ZBfKtu8yOzEqli6Ni6kRoQkPxBikgyfVwb%2B4hbK9dbfIi2qbrJJRjUmziVdTq6%2FOOJ%2FctdOlwGAV6uPjgcosXWxaQsMxNVnq%2BUMvQ8fDWw%2BqgDNdHVniu0dy2MxIIY4aG7k42hHswtDNSozdYkh&X-Amz-Signature=91ba3fe11143fd953251b45ed58460850b6a8282d3788b61f835c2517e2015b5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

