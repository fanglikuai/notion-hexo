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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SAQDN4BD%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T050105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBRQ7kPsdf3d5VTWYgSSMVDi429pHrRgkHGIrOnLZ2JPAiB60qM1Veo1ejKRCPVW115RkMnxzo5smjTStfdJ%2BK6WNiqIBAiS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYy7b%2FfuRf00hkvDvKtwDuGP2Y0Jbo37Uky0pAcbPdGbFd6oGcegMben0bdp2lT%2F3njKnEUE5q54CHBs7LDXuS3od1tJ2QHkJwrmpyaV%2FkgwIX1uKxbVpoarVCOOhHA2XhG7GH7OmeCfVV%2FYhh2LPjueFtY6pv%2B0wmzOPRlPC6rqGKJtigc5gNpcbLJ6Xb5rII9EP2%2BtH7hqm1LUcKG1%2FJm1uA6Tb%2B6WgfHWXZAxRPq5r7i7hWBsHn1j0nOammitnl0H8Pxe9%2B3hi5gsW82d7JFQjy%2B%2ByGNeUMhuzmIoxwl%2FZgvaQwScwGZQoDpEOie3nv6R9l%2BeVN9TwIJRQGeNSgRGzk%2FpidvHZqwlSNPeVT8ro1m0m1%2Fh62DVl5HGkbUoned43v0KiZGc1qmon9s9bo%2FDwY8Tr6V%2FvDpj3EWycBBJB5Vk0amZpwnJAheZV3bnupZ4S4NpRwyC%2B7I34LOmirN0IJTzLM9JG5SPWZ%2BoAuNy98CPoo2%2F%2B1S3L74JtG7z0oY7xBqSAc6gQ0iRz3%2BbIJEA4M6qza6aEHxDy8VSl%2Bds7PamRCvpy6fOHW25Ex9AJNy%2BXMcq7%2BmDW4V7hjDG9xzztlrU8iNEd1%2BsqfrYDU2Wx1z1K1bou3vHIfYD%2B3eguJ0Ju0ZPFL%2BktZoww2rieyQY6pgGnJ%2FEwUFBRUsla1IQ%2BHH%2BRwSCNC4HHc2rK4%2FHhkUS%2ByAMyJY9z8%2BDPT1hXp4xWeroeGPzD%2BeTStSEbqUmf3JXaKPFrKrei1gH3vCI6y5ITOe4gP2kvHlYK8g9cnF8R4CT0Yp5P%2BrTS0IMBe%2FtvJFSKpj7%2BrPe%2F9Yz8V4AWYzgdKM2iV8MzrfQwe%2FqSu3aGrqHbf%2FeDQtX%2Bu92dEnFWAzNg1lNFtJMp&X-Amz-Signature=135b734a39add482a5bb84c931deb4d3905c9e172b075dd9686fdf8c52f87821&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

