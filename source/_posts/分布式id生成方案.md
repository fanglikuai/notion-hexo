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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DJ2JLY7%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJIMEYCIQCWOvtfJ%2BxuREM2h0i4nQWGsBBkYlYaD0no2JvDJMM4wQIhAOrzriEiFH7pix%2B7f%2BC6o7kGox24pjhssvJT2uF0SXD%2BKogECJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzwHIvkf%2BuX7xjo0vEq3ANMUfJFkRK3fWdWCp1QgQv6tfCUWXa19nxmlwYWQuX6zKeMn%2FqcblnZ%2BX87ZP8cazjo1lazLpTwzJVbPigpcYHvF49yQ%2BKSqht%2FXz6BwDeicgpJZn7u2zyRsyO%2FQEyjc4t8ud1BFQUN0Hww4KacHFFi6CauBQfTvD%2FxW2suN4XlNKe41eDz83QujotfGL6Hy7dXvUVaCnISmrjULBy1rI%2FihXlxX4g7x2PZ9USHubHG7Jzji0vJOVXtmMm5S%2FjswtvA6eJ%2FxZ%2B1cDWZ8c3H9b6vBHpFqa9gdiYLGM3GKI%2BVgCbTpX00y5NJYN65oN3Blt%2BFHUhS8sRK%2BbegXFZlHr7pjlm7syBEO05AOomk%2B59QFjR%2BJ43FsVMOIG2zo%2FTh%2BJupqwb9LP3%2B%2BxiYZpX7C4fLjGGtLJqYN0GYlpvYWRDKsZeu4vdKfbhLv4%2FzgOFLPdcbDnl0sCb6ok1J58E%2B%2BcLxswIt9%2B5tJqaDFnN%2BTII7OWjq%2BN%2BYyfAna9UTEF4kmybWuGtIIQoys%2F02dZMknkgB3LI25RXxpVQesSSXQkl7ZSqxHEHSvjtOxivqFfS7kKTvjddVBmz5L62fVT5WK%2BMcdam%2F%2FWjtT1v6PkfIRppzeCwNiGiHQjKyaqCcojDS46bGBjqkAToRxeDloxwO2KVOxBXoJYnHrLi5cm4FSNHA8FWUNCmGAiRrLgngjIon5RR91y1wtQkIJMi%2FZHccErp7TsHbMdhJc0r%2Bm5AcHVe8N43vHHpt0Y37g80cKwY2ZdmIOtUXmtZF%2FmBWw5tYBPwRk6sBuzQLEqKqdpoew946O%2FOGbRVGjgMm7RssoEGRPOpTM91wbC40ChL3VlcOG06vcExAqSw3k6dN&X-Amz-Signature=f32714160dd4a41d2231fa1c71cf01792416264c293adda911012b81ee884842&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

