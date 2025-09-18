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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYFKQUZZ%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIATOLgUMIExknlduCGzVp2sfbLncktcapfkza8d97CrSAiEAs%2BhHTlcUwgAAchdFDLoT8cDJ7TgTUhFcEshuNWtnbZwqiAQIsf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJaC8FjoMMSYR9dDQCrcA70wNTNnrIMCk2nHHMiYoDLyKQ9JfJnIz%2BmzwQzg6M%2BkjEZDGlrgVH%2B8Y9xG6FJHEBK75h4WJZVWtTCL1V3TV1Dz8ItMhcsLl8ooyCVRRgSWu86PDiVa7t1QOR8UYHGhfcOJ0fyUj72FdzNNP0Hyy7cnTJfRHfh4iM4VmrELlDXK8YrWckg3yCbqkEoWWXGqJjABpdAAlBFnZtQTsy02xYkSMPw%2BJlY5gtj3IAYXzCCJ8mYxNuQnHmob2H9wcpuQrviMRuc%2F3d4PPAQilGjrJUoJ3UgnzcntW3FctmmRCaLW6PVz9RAjvKqhZ9TnD5t7fHGdO2z4Ph8Txzf3r4Uvy646ZM7wwOnKV0boBfn0bLTPA2vvpKbAuERhybrhRyyIxQM9fvn0xZHFch4qTaoEWo6BI3tY5jgyaODGVXAJoLF%2B0Y1M0vA8gGnXKrMNNxsAD89YAUMaK04kVTSQLq8xvX0slz9wBVr6qF7caGXWboanIT2naK%2FduEB432pUIkyIJdaO5%2F679iDB5Pf8Dtou6%2Bi1I88eW31JS3Q1gsfmDAuxcel%2FNB%2B9vssszvtYbtsRcRaVwfgbaGDTLNQ%2FTfSy60aUIhYKZyIIvb2h858KBhzptH7jzP7QpcGdPvzSMKyZrcYGOqUBt1wqL%2Fj4V1A%2Fh%2BrTz0yzdz9mqPWtY1ajGpY5unJMKIg3LlxYuTVKHFi60O9JQxhcudtyy0IYz5Jfdjf0LE%2BhZEvO5B8FPYjojzt0is5hFZG4UD%2FLTVN5DYB6klT%2BWqOM1ZsxohPH3MeqlN25YZBqIyGL6ec67Mo%2FAcablQ5h2Ndve8QXVWPUyqoAldP0J8UGj6%2FcObmCgTJ4NlWOZq8oInBgOP%2F9&X-Amz-Signature=d1f0b499678f017f6854420174dd30ef8b179e974b2c649371e0f7d6bc02ef69&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

