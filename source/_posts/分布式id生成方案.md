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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646ZWLPAY%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE3DwxT4UbApUYJiEatmbp8%2FOgr%2Fad%2FIdtrie%2F9pYocyAiBJr%2FxA%2FCcGZkBVNzHI4KDIyrjRC4ep9SopHpYKEFBI4ir%2FAwh%2BEAAaDDYzNzQyMzE4MzgwNSIMp1yDxyukphkluEbTKtwDyJMbD%2BDEiGeTZWLXkaM2B91RmTQ8qOiMN660m8sZJMj2%2FgPQjvUS1Q9gthwujRWMTZ%2FOvqzPK9nrks8L2vz5TupvVZABXHrIdgrgr5oeVupUkyXceWoGmzyvHWrkgDQlLgU7GKXS%2BGB3nqA%2B%2Fo6899OR7J3e6DUsGRgyB2X9AQ%2BDZVZGmcdSKl4inVc%2B5ufgJ1%2Bmn7xvW2p0of8CUTyyZLUjXGGZsEZ2wVzHi%2Frc0i09wYL18cGjEwK8TNcAKNZGmwz1bpKeOZ0bwG2VVmgiHmT6C5j%2F1pHeQ01XpdfeFKFdJwwtQVAwpGfNLHKkyTZ55irv9nCGN4Rrmd%2BkBOjHy8DIxEX%2F79F8UiXlEFQAs1zyf5k5Pj1MfQ9BY0FymgkIpJikGMxBNuAVWPevS6XCBqmtUc9DQhBIhNWNaRWaoEU%2FiUVDcrAMCkxUaubOxs13ucBGNA6qtLNvSI81XVvZ6vbW%2BqreQGqlqLVN3ZPLQatqbD93t5PMPPdcmuAoaWrHZFEowF8AcULvjr8ylEAPrREmWNv96nAt%2BF7sOZOG6lgavbWH06zpUCSWmJH9RlR6v8SBb2J%2Fgc%2By1FlGeUta%2BBRLdGz3tb%2Fr1lWunzf5pStdjjp4YGIKwEOP6Oow9o%2FAxwY6pgHtpMIarkmOHX3OtCq3SydMgmqXVAmdfgwMLqqgjQKlu0BNztEEYTizZjp4MkiOvWej4LeynXNUGiDp%2Bp5VV5dENDFJ%2BjIYPMWG4eYb0vXcut%2BkEaHUjSvjmYRL5FM9AZuFamuSzs72Diy8BeVbin8PmbJUsZcwY7YRc22aV91aTykcmS6Jsq3xc3xASjzWL%2FogRTyShLgsQiLmNB50SFXGjYNgoTjK&X-Amz-Signature=261adf21bd9e88f7c0cf499c465a7bf7e43fc721170e4ddf95ac1c54e59681d5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

