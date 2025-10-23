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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/afeecfdd-b53a-4d0d-8dc8-7f6afe626350/anime-anime-girls-Mori-Calliope-Hololive-Dino-Art-2304478-wallhere.com.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666B7V7AGV%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE3VWj2rZ5EuSch0tJg9IJ9QIFXORxlHjSM1xhmS%2BH2FAiEAlRfPivtJv7nml4IDSZP8TAFbVqKxVfSf9tWJEMw0sB4q%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDHa76j%2F8HDL28Q7VDCrcAxTbl43hv9kOJtn5%2BcCWku4n7lfAkEeAOmLjfu0JUCobRQYF9fH1yPPdXRYYv08QJNK55Kl0TrOSK9Wr4KRt7gDeADliv%2F9LxIv2C9XHEw36Qg2os3MewCGoxlj2ZTap5ppPOnEpK7GiAxqTzSJKRfXtoPqqbKfOS5pynp6BMiyMyY%2FEwupl1QRVNgg4QJiBsHnEgcD4i3cyohiMYHN3rkOpWUdMNyH3F9ywWV7hlp%2F12sdrUePtnlaW%2ByVqm6JadqyinD68g4A01YT1ANeLFdZZWSTJLE9SctU12xjXdyJmXRHRB3JBKaaP%2Bd5wylMt4iAi2u5q7Xp%2FCDmNmalN%2FQ3GM1FbOHpTV8CKF5lKGpt8UxGYdPgES0dmedIsazHG6NjfR5XSrtcsemyNxgg3YVvcSJju1myCFqBQY29aIa%2BEs87yXoW5VPX2%2B6eHPX%2FowYflCpe0AFxIUnJxmVLjp0bcq3F39M32jZkbUpGi6%2FmzcrBFULb5vEhXRmQAUMHoOmfEmXMJUkEJK5HEcWr9k4fmuGY3JZVwZ79JSLPLkQA9Kz5XtfZWJ2ZCDFIiYQKv8brOuOJ9t68W8wSPPWWIhwFokt14YWcYHqqjfGgoRcQzH2Nq0%2BWTjc6DJx5uMIy%2F6ccGOqUBaKpl%2FprCsVA86vI7PSWq6Movis%2FiFdZCuZ113AlcDjbauEE3nqpsr0EvkJfgUZeEdJy79Jhi8ksTkvYp%2B4Th7wvSgmmFFQCie18azJaJ1x%2FT8sSu%2FaUFRDI5WqD6AjfVLePYU00f7XMzvs8SFjndh15mZrHmrAr3L9K8YBsWbAcwqBibB%2BNAL4pBV9ZjAJaNHdaJAS89Mcrp09ZRRJqBmrhVc339&X-Amz-Signature=a063dcec135f40f7230a59be8f40234c3fe549bce92d6c644df7e04a4a10efdd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

