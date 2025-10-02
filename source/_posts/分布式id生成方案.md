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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VN4VL35N%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T050038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDQiGw%2ByZI8RIfl%2BC%2FLwA7K%2F2mnmC0tPkhFhTKaMcPzIAiAuZKv0jVyhY6LiDcCv3XFyTToy%2B47K2%2FOWJHASWSlDjyr%2FAwglEAAaDDYzNzQyMzE4MzgwNSIME%2Fn%2Bpiz8OYhOmtRYKtwDj9RF%2FRwjw%2FY8eqfFRJPB%2BGgJ25vQqEXD2dTMRQ1oIJEYemntDdtL6j8QM3ejQvz7SjKb1NtQRpJ7gduz4BopeRNj0HHtuYxBUkdiD%2BbQKpLZ9YStYldjXet3sICkW37fQfpIwaq58erSjfSIsMtQwct4EYHmLKE8oyjU6wOSLr%2BQBZ%2BRR5dLJwJa4Hcm0r5H5YsIfGtMdY%2FCBv7gEiMn59UC4CPfo6SxufufqW%2BRwXGeoucoqmoaq1Uqjq8nUrmfiyno7fxdxuqTOeQIuBgRmJG2S51dbvVOYUm03AZlQVlWxQ1VF4wG55oRpOd3xZtvzuxK%2Fiqs24C7w7E4J5Mmn7SYa8cKnT5JQAD9c%2Bv1HnG2%2BTIgW%2BCUkE1KMDUR7iGmc%2FsarVwPxAumfJewd%2Ff3VDNuRvYjBeGM0X%2BasRx1P81FDaTexuszaSKic2yOBz5CZGyVxXpD7B8av4UiuKq%2F3gCO9BbqIy0i%2BCC0%2B7QIkczi2vU9%2B2se1Sccd4FMo9fI8w%2F%2Ftuy3vJMteW2rdWOD2vI6yk8pknv3FiGUQg88a7Om5YbUeLJAU8omj9Ab%2BMs9Wbwt%2FJC%2Fg%2BDQV57V%2FbgGntM0XgBWq7g6dsVsJ3avn%2F1a8je9AtiixmzcUrIwtvz3xgY6pgFYjDwJnElGtf0Dy0ix9s96Kwapta1BprPJXLypZR%2BqjD3G%2Fob05dPF7F9i27F8TEeZQcVmUzXUONYhWVDXTXmJShmQH7i2m8SPTv9ddyx9SSfof6TFpvrv9%2BMBfrrLheGud4JGcrvA6xmGmha6q%2F4UC3sRLyQXpoiTIGheh5okuDbSDTp8DkjI2xeYWZXX%2FqX0f41vB%2Blh%2Fsn831nOf1%2B%2BnWw05vgz&X-Amz-Signature=64bd7a3c78d0fe653d8952d1f445382995dedae5214e57de8bad6bab1e3ea3da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

