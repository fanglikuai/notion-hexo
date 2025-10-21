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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLBPEIEE%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJHMEUCIQCaYY9dy4ti9WPXGqV6n1sNM2QOfrTJgip6glo2o0Ag5AIgGkPRbLxyo4Kr7wXIOP2mFttY9ANlbknzIFCyH%2BVTZe0q%2FwMIFBAAGgw2Mzc0MjMxODM4MDUiDKNwsPhrNPHiL7GcVSrcA5UTJVw7lkJTdXfoChhApMJA4lP73dDPK5td1qn%2FokFhQyYca3Sb5Z755BxdM5ONXRzaIp%2B7qC4%2BZqvBwoP8qlDk52HfB%2FZ%2B9AFn77Y8R%2FI4D3bVUmJj%2BrgRfjx7oNOVBcJoOnusOUhrG3I6nxefZT0AJ70I1EzgLKHAPyziRSlSLdIEyrfGD3j%2Fc6fA8PnyPxqJQCode%2FgrVpfYy9Yde%2F5vQ7TUanzOTXC%2BzHVIX3YnGR9nvCBSlekHv0ISFuPzkseaDc4Oz8P9qPEYX2SmHraVC5KfoJAzunPucgkDgylA%2Fe%2FWno2WhrAes0pBq5Tt%2F20pZQbRY6JFYguXl6J1brgN%2BYy%2FprUrERlutqPCbsOIr2XZKtWa8EoiZ18NL9ftmy2ieiAyz5hHl1Bmpwri7J6u2%2Bsca3b5tfvO8avUs%2BwKFT1j%2BZIhjhW5hqXbArBD1kmEL%2FGv0OnzzKWlR9NxBToupZv3Ks8VGKWbhBSen40OmitkzBhhADrOQJiHxQGXwjUBEmON8oXXWeYnZH5PLD2Q8sPcyceaZWHp59DQWB5Qy%2FnAtXSTolTiRqhyKB2SJjq93gYch98UX4Q5kPrjm1K2QxS7X6od%2BWvJKzMROsZeqi%2BkA3I5yqvzOCdTMK3Z3ccGOqUBQm7OxcAZh1J8opv8BE6Zm0ged%2BJic9md8hphV0%2BZPKNdbaIGKbOZwodArify45guS5o7pCO0GPmneLwciwnagyRxeLO7M9z6VAnLkpjcbJmkqEiRazUk0F7hUwXxTgikoZjLLw3G0tJK0vRGaacYjkcSW1Z8L%2BfiAx%2F5f6%2B6fJK0ekiA3LZSgY%2Fgn60KlwipGAo92qKG%2B7iH%2FKt53DWR8QZTjaQ1&X-Amz-Signature=ede2521831e9483483a8781b022a7dee60d1086e00c82c58c2059365d054d3c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

