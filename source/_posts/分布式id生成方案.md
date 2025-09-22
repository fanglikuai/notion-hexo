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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RD2LLHZ2%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDKGrLRalaOc3tj9ioh30c556tIHZfMV1812iOR7tkFOAiBkL2dtPj1ZHXi1JyYJiKhYSLdW1hYqMRbJSWxgYOZ4nyr%2FAwgnEAAaDDYzNzQyMzE4MzgwNSIMY1GrHbpPh%2Fv7bDppKtwDuiOZrVqRiiAFvwimDaFGUGOsgCCsvFw7amCO6A7OserGIlE4dKXYEctYaxxG8vcx%2Bj7nPybglmkBBW63LSUeNRzh%2BshiGkYtn7mRTKqgFvRX60335ggwIHzMbGbEFecQ4NEgXdlkG6w%2FAE9o2g0crpx5ShAhfs94bVAB4fDKwMS3tQz1N39Sj3ie7rbqifjmx5kUlx52G05RYuN59uWgvf45Zvs3nylYJOiFhU5qDKa%2B0CwWOkkTa4jhK7th1M%2BpuyEOvSCBxQNWv5XbjPe4mVaOaFWdchzpogCg0AiUxE8G0YK8KfrVoaYU03Y8kHulHPC8%2Buo2n3OrOSR7D%2B5My8BNe7Ml1cFyBlNtlFkcbEuB%2B5hbu6KGzyNBRRE3s2m6wxv%2FGqdYKVEXYwOR6IEG9fPRsZHK0DLzB5SjYGIFmuaZ8JimuMsQFtqxNQAzDl5jY1hcOPhb0eXxvvy3HBfMMlZtO21HnET4ZDJj9kOKCC0wChKzkAgHnFYVIitlnpGPC%2BCycrgCEpODIecX21X7Vgx5hxjGzbtmpEvvBXKxWNC7k%2Fn9kNxoLLhGIKd0ui29plH10w8qE%2By6igPHeEW5H%2Bpyi54AsFxfLdKQZeoR0vmf1Rs1arBbHqcb%2FxIwn8rDxgY6pgEj%2BAwgZ5S4%2BIOaetcomcH9jqfgOeh1S2nNVVmEBXQgQtDfpBIr%2F2XBtY5PctZ7N3S7fz16jzkI33czR2MRNP0fx8PWrRrhgsnwIoUx18EzgBNMz%2FYSKyX29MefJ2WK2%2Bn6TDXbLczMy2pgBr75oVAus%2BAzjVThGTdT2YKRUty7V38y0JLzcprBCiMRJKUvaaBO6at1tNDH%2B4dwx3xt1LLNZ36OxLL1&X-Amz-Signature=2672c78c908cbdfea9b55fdf5e910083cfb04a6c45251c8f7ac46c30256880ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

