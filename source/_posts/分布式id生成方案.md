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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PV2Q7LM%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJIMEYCIQCGfcabprnaXWTE8WVMiS8BNnQkSXJSXXkdQ4pRCaE7LwIhAMnMgY2sujYoGNQV2nK8LXJVfPpDK1AukzCHIF%2B6vIZ%2FKogECLL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwaW1k7OKDRdGM7Bv0q3ANqdBuaGGjJ7iyNJNnzxaD2y%2B3j3J6%2FMXtfTzWLpxlVItaEth%2Fi7GYVoECebd3iAwYrRwAxzf7kbeGfgFmlcGX4UIDrLgR5M17UexnG2O19jlb7oJLAvkmznHD7V311EgCQMtU9eRw3LwOOZ6qCz5lf2aWFxzXAnIe6UbDZHYITtMrfrjHOrIiBwuxPYal18f4Y5VJyrJDbAc09d4lB%2B1IWaUbMlbNY8qcN0uI5DYHslzX%2BsL17mD82R33Hl80MCGJljeUJ5Rh2%2B93fse6rGBPHiJqOA2av08dFdYyzqVbbi5wMjnSviGg2uX%2BInlf0UEqofDnxJXqpelqI2sIIpfaamD5wLnXdntxWsauGw%2BD2pmntKtQUOd%2FhWkY3PGDOiIr6S4QjnMXQXhKT6FhCBvoV2yf0io7M96Cn8w1zSQ2sqbGBN8ymtQK64TvuskpOe87kE49%2B10NefuJUYORXjQgnZITcfcg47BPkbeL26qN4JKsVB%2Bf0e%2FQrf8WApcpG3LKa9bLBMvQ2PXBRKsc4h2zXzArVuUct09It5kp4iPcrll2PTiDS9VGfiQ3MFxvXdNIzE7WVfWO4kfTlvYXdtaB%2BDLPtMLqDTgyo7AulgsTbKoh9Qm9qDE7zZ7mpNTD%2BmeLGBjqkAURZim0qBYh5l1LsK3GVG%2B76X3z%2Fgay9hsjhWOOuscLMCtLI3eeMCSDJOaREz6xFDV%2FIfNcuJHSXm36M2cojfgJ%2FFqsHDucHKyfgdqf51DJsk9xzGQnE%2FBQm1b167K49I2tCgaXtYmItue7cqDIWwN%2Bh5xBTOwf8UTyLkzBAshYt5oxtX1lBCa61m48FKG6RZlpqH1lGyqQV%2BdWe0zWhaCTDxXja&X-Amz-Signature=6e28088a732fd316979171b66b8fc17dd8dac930f1c635fa06c91ac33c0d967f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

