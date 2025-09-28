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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663Q6CLRFI%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQCtYaJgiBCt8FWRBADUtXflWjduV8loQjKX56%2BLbothIQIhAOP8OGwknFwAikz8IN54EFdFjNF8Mb505e8BtJrFmUvTKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzXo7TuTYtBySvBgf4q3AOD2YBS2drZe5TN%2BphrwlNGMA9F4dcv01zJbB%2FVItxguW9FIvKa5%2FwAYZegjjVNn5N9791W3z60ry0l5olYei2dWyo0U089ScCRHPulE0sxgjC6P1CPj5PkZ5yHo9y%2BRmGAb835SeTox4PvRjpQcIEAPwnaTHOPOFm%2B%2B2UFQCZr5WjNHGEji6P%2F8N%2B%2Fc4SZQktFUOVJsf6GUGmZ133A8WzWpK17lCI1yKFKUGuK35VCp1NPmsTpbFNQZRURchQeWueoXPall1dPgkk%2FIn%2BiDVOQs6y158Fq4DibDtPhPUfLdp76rVL6KE1D8zovfK5L5EaQUD6ZMKMiS8%2BjFW7utkAGkHnG%2FtVv1dm2j82vCg5GuOw9KmYH0bB3NFn6WvX9zZBmklTYkyMiXTwg04AAW76tgpCu0jDKYlcZ1VzXqcBZnJQyjiE8d9f6al8pdCu2mMlC88tx9YsyNFL%2F2LBeOyuCqa%2FOnvN7QC1%2B8zgtWQElr7SXGwaOY5VpP%2FQchw%2BiN0dISwdZP%2FFI2pOhqe6Z1%2F7NNro912BAuzBnfIrhl4okdsp03VC14PuzHlU6TpH%2FBMmJQHufZE95o1qmFSksfmxROEK7s6JNxYtUywS8Oav4jR5p9sPfZ9jj3QUC7jCRquHGBjqkAR6ca2%2BSeY%2BKXIlUMFmpFD6HSbuU%2FXip64iMyA%2BZDQoE9KVeIrCB73GDBJvvzoZ5YUyIWVsGUvAlfGc28Lld4UdZe%2F4Bk0NU0ef3oC9Ad8KOwez9NCEOCjBW3A%2F0n67ffa6yNuG%2BnOilQGAo7AeEyot8QgsfnQWdYvTEGTSy%2FW8nXCU4I%2FUXtGMDX%2Fgei96tPO42dXzbnI1Ugj3pTwz%2BNEAeJyeB&X-Amz-Signature=1ef3f2e514452f2a4be8d4bec2798a9ed8fe861c3fc3fb52fdfe2bce66eb3eda&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

