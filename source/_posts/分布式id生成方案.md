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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663XRQNOFR%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICj3tszmmkkQtgyJb1puE76S0DMPZlH6WFX78Qq7ZIG8AiASbIlLdERVZChYvPCcbMryajCGPJEUJwR9e3U9GTvX%2Fir%2FAwhGEAAaDDYzNzQyMzE4MzgwNSIMYlYvuD3C7mVtP%2BrhKtwDdJV%2BqC%2FD9hTxVoiXuckj5e7lyUf4d2oEoFq2%2FbnKUkwUtZsbfvd7Y8T76c4ddzQXALYQ18nMBzaZ5wDUVnczBHBLaNIecu47YjcM3K7yGRSr%2FzLj6MSxL6fvQdifdWDwKSUReEqR%2FIzqdXtFII69mRoWj4goAZ7RutndoUa1oESpLqqSDcEyI%2BPsh8H97pA9AeXwtX3Nb89OoLlecPA%2F%2FOknDRHMHZsjdoJL1%2Ft4JtP7S1yjVWpPb1jKAHZgbY3XIAs3fxcjUTKdBz8GJz6e9Dj7mIifXwmg%2BkcZGSk7mzZWoP22wrpHuSyXcdV4Zlnz0PwNtO8NjGGU20fAMVdFLycb4LJaAN1tmhpX7znv7BXJ0V9CqvC8%2FsN9n%2F7EJHHLfHMNMqYsuIzOn%2FBlwpuyhmkbAdG0j%2FmwrnHLTrzbd8webqEsjnaKRc%2FbIK3nBx9oUqcLpKnTR17%2BliZhYGJk3YIzwCG4p8zo3q8XtB8VywiRoji84%2FxNpy5rKXNAwB8B2ggnIcqJdioY24lf05x4JaATIZRYWLQVE85e07JcyMSxFOQduzb7Q14Ai5bv72e902zE0UdtyqxeXxzB3GSNdqnLPJOcxLMprcpc9Nc9zX3LLy%2Fz%2BaslZNcKKAQwmZL%2FxgY6pgHO2P%2ByrZeBhqfK6vYH1MaVNhK5Eeo0PTDUsuBdHGGeYdPqQrIA42QMk5KqY9SLw%2F6Ki5xQYXCb64r9LUyhzFCIoXOrC8UmwEU2M5UeJHAy%2BnKNSqCEYXuYpfMiyLE3QIZ6zB5DahKSkylML8JIuQKEIeoTRabifI8%2FrZSWgs3FjpcxgCFeVjkmr%2F8P4xuqATDMtwcftOBTzGErsEpisU5ZoY7SU8eT&X-Amz-Signature=36da67ae26522ee141dfcac4dd0f2295ad97e2e00c9fe238c9ba84c2beff1585&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

