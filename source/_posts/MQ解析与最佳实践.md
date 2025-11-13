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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665A3YCIV4%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDNDJBrtmWB25SrvmM0rV8sv2R8%2Bn51J%2FkFEnchHVie9QIhAIqjZQvI4xrXVtjXIkngwN7Gdfx9OurQCXvSmZCaD1A0Kv8DCEoQABoMNjM3NDIzMTgzODA1IgzBL46mgqbaO5SIvyoq3AOIK%2FjF0OhHzjrlYPOJC9ebNrBbgQBtJWmaqg7Z%2FbbnDRS8FtDk2pIvBgmt0FcWzSOn266FgR9m7WVmHQeBmK3c78rFNPY%2FiMzFAvTVCabHDDpnYUJ5cZBUI2ANp2a%2BTg5B9UkYtLLKC7rv60HbMOWo3Veq3VEEW63Z7DFGnJm2G8EvpRgm3TehWiaI6GWID41XadZFFcEL1oJvGIBlLaMalp6RkWpTBlpVNWH%2FDV4WTwEEHjr%2Bz5JQaYzNXaSClDM0TMiyNMlN3OLE2ha9GrSspCp5pPKZoiKzQ3JHdW6McYFacksyFx5Tmbo068PQRird3trUIh3dPoQ%2FHIvHBp9xSHWBNQuULyPaWhuB0MrpiLxRuiWaUu56%2FDdzmAnPsU1Vk93rjBt0GRFyAZY%2BdsdNwxANj2oI%2FsjtOJxjDuDWbb6vWy9uWyMZziW5yabXowJfODBlC3mFnyTMBTkSRP1iODWGunbmma7i2Y7pylCo0n%2B46%2B0TOGmPBXXH%2F6yr4VsCOA7Os%2BgasnZnISMpFD46AfJvJAnneRtRmJSfM19AJc6hk45R8YYBG%2FTLZzmKHI9I5olo%2FbvC3GRrS%2FJ3LY48pqPBc9uwYVIrgWiEk8A0A3P%2BFkIuDyRW1%2FmW6jC3w9bIBjqkAUltBoLtxwZcw6RR%2BcoXAbaHfmlaQqm6v%2BQ8sglAuGuVcY8IKfc1AtoHuRtnQM7DmtK7DlLvz%2BAj6heQCa1P9RMOu8QL9KHI9ly6G4eGx0uf7Y5LIG4s1D6z%2Bjde%2BUQcQo2lAE5r4%2Flu5gxaUdbMs1Ju%2FoHa3j%2F3fUKvXOYfmrMfunO%2BqJL%2BqDkJiDrYf5HIdjiUrmKw%2FNAD9ljE1NKzIFaRaM3H&X-Amz-Signature=85d0a0e1426213b5b19253eb190bdbf36dda35cd39d9e2a35a1730d2faf4f2b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

