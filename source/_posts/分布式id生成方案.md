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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SLLL4NGC%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJIMEYCIQCSMnS7ELCinKn7uSujjF0vsXWsBPX3ZPswbFOKETQFRQIhAL06DHsUEShARR%2BOOWosw60KYCKaDRcIC8yftIgyak4JKogECMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzyz%2FP2OmUGhr43zeUq3ANL4RpHvq98Xre%2F%2BW21gzDgyH2iR8JO%2BCz%2BNDwUkIHndWDRRgyeq%2BVtgadcNldZPSkFLWW8rDY31JRCJoc%2BRNO3syyiP7GAUCq4xjLvydh1HN8UO0%2BXytLZV6VkIWpbQM0%2B1cb1jB75vJ2%2FsgeYoUefcCQWpef%2BTqFK6h7HURexgQeW6K6%2BsScS0amoChYna1CHuVDMwZEdtq%2FEl%2FN0dh1EDmBdje8fb%2B9y5JmNxn%2BvcCOkyCw4T0ZvqDPLthJY3%2Bch%2B2iVcmsn0zVMySL74oJxzqm1hWkmerbtQNxCPu3r3BnB65jLzmvWMiCbhMpKC9Oro67en%2FsfNI2lHPlV0BINP0kV3KR1kpLoq7zm1pSc0M6fw1EkOhigz5PvlnJSFkWBmo%2Fk9la8KqmgWWRvz%2FumBIiZ2XQ%2FIf6xbR%2FSE13EJKVZ00i%2FiXdJawySyiGk5vBVBQfL39jw%2BvtJ7ArcMOqzYVAud3v4whFT0hX1tBQCvrtKpYI%2BqtVyI8LFnwj5dJhezm38r9b98IwPtBPz%2FkekL9V31r1Icr6w931jumPlzyNZjdJtuu13I7337Z9y%2FU5iIFL%2BuTjN%2FLd91Dygyh9xg8xtSmP0a6F4gDNJ9oShOVNxVzHYT2CjdPfy4TCmiPPIBjqkAfgb%2FEZEdqOsh7VqKhCoZNe295lT0tr9WlpIsHAm0b2u%2B66K35WFIf7rDW6YOkhjsCY0Iadq%2BKU0eMGq%2FYzro4khz58zdBoBGB98nehBzz1HbcNon2O5R3kku3pEN5a0yMvMaBVBn6Akj9cG8%2Bze9JybIJBgWYE%2FwbYb5O%2FAxye0fk%2BEl0p%2F85kKQOThp%2F0of1nG91loAX8sagbfviPXnKI6JZBF&X-Amz-Signature=ace4fe5002b37ad8b851117d31bda0d1938bdc19bd4b726cb7f72b36255eeaa1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

