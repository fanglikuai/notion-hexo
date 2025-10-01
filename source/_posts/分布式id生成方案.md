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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZBH7EYYK%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T160056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJHMEUCIAdBhKHJoOMsZVRiiV8hg%2Fq12ML9RkgYUOsJNtamnYw4AiEAj2rrkZieE3Wm7Hjl3GQqT6AJfhp5Klk4lQUclIW2F60q%2FwMIGBAAGgw2Mzc0MjMxODM4MDUiDDXK0iotR8ZxDjzzuSrcA%2Fd4ri%2BXBikK%2FkF22nHUdkTa77a6d2mhMeuwVNGWxB7bnktm66JGMGJ0lFVW%2F7yku55ct5LIZuYJWQSWt6ysgXQIJ3kmFzu1VSIfAEh8oPvtmOWZP0e5AcloRM7efUWd9JAv4OokLl5be%2FnreGXs6fnjvguxS7beipjkv4snEN2V27f0QHGTv%2FF%2B0mqKa16K79BenboQmqcX%2F56AU3VwbDtdlmCjjXWn2eyr8iZAkhWMIe5iJeGaJ%2F9AL5KOv8o8RY%2FKkwJQs0dAtiYAN2d0PZ%2FTCHXNlmlnSF6SCRrvWOMRAc61xkSS%2BfG3ozN%2BGbWgEWAvfXR56aMuipdr%2BjXqapGGWGu9o0r5SEvQw6UblzYBh%2Fd%2FV30OJhhBLLs2Z1pdl3AIifZI92hQh0zOK90qWKEXAYTjR%2BCTsexaf4M5NIn%2B%2FPabcajpsDkZWzPhJEQngZPqCCYSQdYwmvRKthSRIvMkbHUVAcj3fiVpAT0Q3u6IkUdzcsvtI8iv%2FjSXOspXj64czl12LX1CHaxWiFVOb0L5wu6640i3kD%2FfTB8sy58uhLQkLpmzJDom253svOKA9NCfnp9GvM4gHsOrOUxherX8UqF0EmFBJYcjs3WbffKEtKVuluQ4bpzjTB2XMPj%2F9MYGOqUBdfe58p70IqMMbyMgtTFtBpjYyP3p4r43Lu4aMQKzV67tBTy2m%2BkQQhy1QbJkFYlQAYpYiL19EkY%2FweD%2BByuq6kiwK9lcDgliHHUVlMF%2BNWGWxIYciLyC0jmy0UUHO4wpT%2BmxTklVrYvc1%2FgXWgTwJeZVVm%2BfAxw2ojsghk1S69BVMyXw7fEHYTOpuT8GtuY8MtN9y%2FdAHuOe9Jbvx9VOWS%2BY3MFf&X-Amz-Signature=78a7c0635c53dba84771072e7a093888e7dc0398aec7261e0cb33ff077f31725&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

