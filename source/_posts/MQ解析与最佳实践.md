---
categories: 整理输出
tags:
  - 分布式
  - mysql
sticky: ''
description: ''
permalink: ''
title: MQ解析与最佳实践
date: '2025-09-04 08:00:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVHR3CCP%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJGMEQCIDwk1s9FGPNnVncnjy2V6UVhi6uhOIuS8yOF7aj0ejCWAiBWBNfwI6SmrzWflgHV%2BW8bs9dAahY6%2FJva9wTKw6eVUyr%2FAwgKEAAaDDYzNzQyMzE4MzgwNSIMDu2H2PdlPN7sNzu3KtwDihe5trfvQhiHFA3G5ZD2kB%2FGY1cqs3MdLNf%2FF7SI9Sip4Fcr%2FwspcdWWMHvXtc5ElT2b6706JAy5lS1GZ7MzFH6EDb%2F3%2BDRStjVh7Vc%2B54hlUBzvcuxIyn%2BbJGPafB7CXlo0bqO07bnUsmfQhH9ns866HqyTRuj6rurYWdTP7o4BS06xaMxKwDTqUOpPePFEzOrJJZr42TbNNDva2%2BGnwgVcUvit6N10%2BzE2FQxTz7G7u48dFUt2MYfotmjTtcIU2XDNoWSTS5ABgmHeflUUbMP4eeBltDE0d%2F4eINz9%2BBJR2HV1ywYkP%2B8HpVGGCXEf%2FdcWPY1mTul4DalfD7TRvPSSrKMqfuFw9WxiTYikiuGlhPW5eq7MY%2BmoaGYUpNnEtrykxHUZA4aZHk6WAmcLhLSdoyS8tAOQtRYYB1rGAnKPZacBkYcaeizcs1hLz0ruuqqJkZcWvj5JW99NXBa1%2BsE%2FmTupcCJVgjnWy%2FsWnzfRmFygEq%2Fw8kcVxmNWULLVjfp7FGQyxqxhgHz7l3PfoaAoL38k87lIsiaukT8rh56xy%2BW42R8NnGx1SSUXd88kYta%2BeAFA5c3FAVNN3VwQHy%2FVpvLlzibavKLS2eE8vpMWb9PqvptuQMBxHkIw4dqAyQY6pgFv8UUnKPN8gsr50VW9lEHSR4ppei04FOuHz5nxc9M6cNTBNsKg9SudlCxJTKiAgL2M5C9aCPVQTA7Jxyjwa1%2BwGl4WxoDk73wpJ3whQ8x9p8SCeETIQvzjkMDmM69gt8EpPSFYWJcB0xySl4GKIwLTFbj6MIAKQ1HbWQGbQsDcT%2BJ70fi19tS8C8dfAesvnhc3qovQutlbIC4cYECBukknVFdUd8h%2B&X-Amz-Signature=7eab58d529ecb0265edafe4e229496f899ee91fc0cce14a7f05d805a2071528b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/c8962001455e38177108499d1e1e605b.jpg
banner_img: /images/c8962001455e38177108499d1e1e605b.jpg
---

# 丢失消息


## 生产者丢失消息


使用ack机制


异步监听：

1. 成功/未成功发送到交换机可以触发一个confirm-type监听
2. 交换机发送到队列会有一个publish-returns监听

**但是一般不这么勇，成本太高，丢失概率低。一般都是采用日志/邮件记录，手动维护。**


如果使用定时任务那些，成本太高


## 消费者


手动ack


应对：

1. 消费者失败后，将这个消息存到redis，记录消费次数，失败三次就直接丢弃消息，记录到日志数据库中
2. 直接false，记录日志，发送邮件等待开发手动处理
3. 不启动ack，使用springboot自带的消息重试机制

# 幂等性问题


原因：生产者没有收到mq发来的确认，后面本地定时任务把错误日志中的消息又重新投递了一遍


解决：


redis中增加唯一id


# 顺序性问题


使用一定的策略，如取hash值等

