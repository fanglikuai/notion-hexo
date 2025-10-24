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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646BDXTEV%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDrFoB%2FmefvkiFK%2BKwlAM%2FHIfmhiitLSFj%2F7eHX97rrHQIhAK2%2BW%2FZuzeQfdREhtMmOEZrUWVJphX4ooS6Mqs3Z%2FDO4Kv8DCFQQABoMNjM3NDIzMTgzODA1Igx%2F2ju2lb8H7Lg0IZ0q3ANB9GuZRfe%2FskRIQuuWdeiVqxWFtrrLXMDfTMnD6odPIh5QscMB%2FruNEcqYuLARmVOIOckCPIY2Ftwr6aG2qCODNNo7%2FDjF%2B2kU8IlcdvlYSh2MkjQhw7Oxv9TUa2%2BWu8RRBw9MAlmMwVcN18Q5XIuU8Y%2FUCCzDeF4eOabkPykxg3wYyu2LYnSuc7QvZWsgIiT%2BSK%2BzlE2LcdYI28RSFQR8lnZFxhqxpq6hc2TScugiy8mE2J6vHLvAhANbDgcRCegb%2FkudT%2FZsTJSGipncxi3AreeoLSE%2Bqu72zdgxBjkl7%2FOp88iLAEzMW256DPICA27rh36pqCC7HOwCB5JxRzJCHyzFLgvnMBAYbxliZJac2ivtxCIlK0A3m92PMMYwc7c7QqjurB0HoPMvV9sz2k362voQfa1mQm76V2BuZLudpw66G%2FbIzB3l%2BNfEPCS%2FUQ%2Fb3IP9BtV5VlsAaDIVcjZCP%2BzvaLDj%2BA095jljva05O8Je8dnGoZWk2yiuep9jP3GRVFoccZOzbUoHrxF4Sl%2F4JgmALBP8FX%2B4lFZQlyRrZgtyPHkqgMoF905GxldmexZMzAK5PQprGvxBnd4YcEFEzIgrgjK2CBUO8oFbIncl0TFxuC7g7CGjrQMGBzDmy%2BvHBjqkATTxL4y6hD%2FK3FCz8BBqKiga1XfLeYZjGC39dJ5khdS%2FqJFsB1%2BJXRX%2BNbFbjABOHH1diQxRDvOOLc01F8cguy5GD8mdxUczz7DYWEVntYMtCdCBXeLZsgMCUTcruxM6ojI9sMyPNB%2BDujMKISuEu59HibI1Am02ueF51uApnj5%2FY%2FZxMGoGmI5j%2FuXiR8wF0%2B195AQ6AulmrSkl4%2FC28Ykus37a&X-Amz-Signature=ed6fd27fe91594df3a8aa02589eadb67351d2b0a19d79c9761faa7e846bf175b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

