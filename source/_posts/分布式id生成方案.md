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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKK4T7KD%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE7PBIQyzfQ4ok25InepgAgVEqFvWqmgHhMI2CmNUZieAiB1FLSzZpsmg9LvpubdeWLTVQ9EQatfbitlOJ%2FbiRh7OSqIBAj5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6FQJnC22ooVOKUweKtwDz%2BHjMRuzOVJW%2BWNzbUr%2BZDoylTVIC0z91TJOUvxzOeo2VspEs4ZRljRauv1TBgTVnzbPEGhF6I2m58qm4GsKAwpXVnyyMuUkAG6Flh11AQoM196x3ln98fySFl0oCx%2BzQYkobEm5whmqMmdrkK62XrE0I9JrJ8Y5glf24UtpW9%2FeShvKNGpAsYjS5ZzqG61SZpP9M6sLM7UZ5oNGiPEaPcFnwexoVlqxgaZNXJfEG%2F3%2FNUjXnn1q6Z0WBts4JjdP9nvdsr8QxqlnhTqZ%2BpG6k5%2BC%2FkxDoE28De6MPY1X59DAeR3XrJaGGa8NAG%2BnnsbxlD96DQv1eebVsTT7ybdDZif6QLOxMTGGVKqIRLKY7qEeX4QZIiNvkD59TJVzeR5ImMFDcuyAiw2tUnFgdDrGuosdjOqebep%2BXMNFTSm43FGHkwZuiTHH9oHL9iNuDuuc72pd6KLHAsruqkb4GxcH0m%2Bby9f1B6l9Ct5yUdvvq0R8YrlMBdUrOTSqsxl4y5T9y6EqSeItIAzqrWfiSyHrwAddKqNsYHzGOS%2Bn9OGMYxJrlA0R7gU88rZe9uCxQfunGBTnE4wJq%2BZ%2FTe59KG8ZbLVNE%2BXtd5lBbQMuaRkZDrzKXAE51kSTp0V%2Fwbgw%2FIq9xgY6pgEmX4XhSLU36CXXy9LfMUPkrCfHUsnfO08zvhWCuIvzpDJnHZcmcCIPIbCN9TcululF9Kt%2B%2FnP4URgHgVh1WWMRqCP%2BWFQxhw6XRqF%2Ba80Ssc9PBvBceu%2F%2FJFGo99xogNvzpox5fwEttuFTh2rJcwCs5GO5B519I71nWZ%2B%2BHWGumU3K75B2A7uPQWTPbu2Qf8lXpGfOpxCoXsGANzRuYwgyuFN%2Ftn3Y&X-Amz-Signature=3edd31ca39efb63d08095e15ead2b08d537bf28fb686cb189b11c61f39132ef3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

