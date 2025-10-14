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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2HUIN2M%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvZGmo4Bv5M1JqIcl0qmFkv1dfRH4NzaVRxkQyCWgDJAIgJuEdMuWc4iRx0PYVS22CP8Oh%2BtcIvVOtfHVn4sID66gq%2FwMIVRAAGgw2Mzc0MjMxODM4MDUiDBnaLklzKPnEMs4kkSrcA5%2FcIEi9ONE%2FAEvqYAdg%2BUH9oF05g4lEVM7Mz%2Brj1f8A4VErsjHwm8ZcSCdr11GU1cTbAWxhcgzBD6M2Bi0vauU2AWB2R7HhnAuikEEJ6vNr2U9kTZeI6XUCuKcfMHsbn7IFCnW%2BbIEr%2FkJax2JgVarLS0SG0Gl6JziyS8SQsrSBBSZo%2B1ZnSn2hYAPawXTgW0TADnRSV07liwYHlsmpMfemr%2BWoxhbhP67uVjcYbQkVPIPjfyfSZg%2FOevp0xNM3yxb%2FWc1p4i4V9%2F2oz25TfGy2VK8%2Br8DHzGlh0ZjTysa2xgvSEL%2FR42lM7WGhNusnrhiCc2puF9HnV9wBxvN4RGYIyAzs%2F2e912V2WVTXSGgj2Sov%2Byi68117Lm6IpM9TJdfkRl8Ka4uqMO%2Fcx1pTFQkcA%2BC4jqc4jf1HW3N9yt1BRXOt15OiVYV8Fb%2F0M7S7CJStnzrtb3G7PR3yBnIfKSoAcDozJ6uSuyN6kT8EMWU0bp0t1Cgl6WhVaL%2BPJLFFUY45zj9clo0M5Se3y2eG8xhkyOPMrhrWP4qU6wAXATTfeP%2BxmZL344fIs5P0nWYGgdJtnMO7kA%2F%2Fs7JBy1fkl5D7JXx7g9Eeg9W08Uyf5boduzdLKYoSqLWB4I6sMI%2Bct8cGOqUB3WUgSN5pp9RiTfXH5yMbhlN%2BiZM3buvbq%2FJ%2B9YEmU8cMnnsgbMSiTdX6PPnY%2FZIV9hQXEDXEvkwpdDJr%2BIGAhokaMCjdNRo1oHQh2QseqyUR2nyIb%2FHXw53XVK9%2Br3pze%2BacBCvq7c6xZvrXmu%2Bq9MjzdffdW9A%2FuI2vYEeiq4k4rxe8slLaHsMCtCNZK42Qq85F07A%2BZJWBn62%2B2V%2FPZqsCfPXo&X-Amz-Signature=923850e5a5f8a329bf54ca3f3aca5e07cedfcd7fe1596fc19c7994b280d71906&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

