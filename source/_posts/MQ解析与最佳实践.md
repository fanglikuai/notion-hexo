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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466USCVHRXJ%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T130040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDqNgFDcoXQYATk7HlzSsUmUjU4DQXgHwQvtpANY3j9dAiBwAmoFfFxBS8gnskjtkeSm5yHG5RZqRhJqOxr2CWXLSyr%2FAwhOEAAaDDYzNzQyMzE4MzgwNSIM28n%2BC7j8%2BeVj7BL%2BKtwDi6waA%2FEeVLOEHvzjKXthIkNjWqzuEmNQiJXH%2FPc%2BP5FrluHwvrNBV4Qy4n8qggH1bN1iXRVgjvmr%2FIAkoMXo2A9b1FOVJQZ13T1DP509559j3XKJM%2ByQGiml02TJjihDWBOOn69XieJv%2FtoMtFY0LDRLC92hV0XODna2ZkH1a7wpcaW4h0RRR6KsybnREIg1yu%2BFu%2FGPsbJieBR77TeiQr%2BCi9U%2BCCxkEqawk5GPetlF2aQQIAaTO0Y6G9eDkKzh96RfYtwJp7S%2F%2FCVVmbJmZGqdnU%2FqZPn4GyQ%2BLGRnrHH6Cb8hQa4yaKxAJ%2BMD%2Ffm4AAOlVsf19JkcjyUrLUEh%2Bh37rU3xJqs0meCFSPCJPs6DX8dF14BmSgexIhu9fcd5Gs19kFYWFzSBPbf1Bw8w%2B4MMlNyRHm1ujMw6oiwtnUrmCIKvtRYzOVRkzp6yX6Dihb37Xfl1gEFfF0hAcTBBXrFiDQQqO9C%2BGH8916sOhnCM6HLA%2BwrzRbl9K7sFe%2BJPAskI1Phcw%2BMVQ9xvItm4LkhqOLy1ELTF%2Bu4uVqGcgR4W0q4BpyBZs%2B3B5nF8jaxx6RVHfW8evcSXl0vzj6yzXXkFYqa4dm7eUtfSwXwwhjRhKOoVIl%2Bwu1G8XYww36HXyAY6pgF43WJ81dPhdm9AyRO5YTlp26rXBXNWJ1DBC7P9W9jByEWv1Gdo8vxI2K0rsAQg0clweUj6qUBYVoE8qQzq5rJr2iMRUtiGarkP%2BhTYckaiL8ko6WUx%2B4UO5GFaKJsvYvb1OXv6WtdKjVZ7O7TWsCfAVbxP2ECSQOi%2Bq2B8GUiXNSr2C0BjVMu0bEIGw67tO7RpioQJKQi5pJjomW8ZTOzAdrlWOhBI&X-Amz-Signature=c3a6d12675fb2bf32332418540866ff7291a381398b76c7d26698f269520135e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

