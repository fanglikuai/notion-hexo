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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667BHF7DD5%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDISBXiPGPzMoTyh1fRW7TzVqKN75BJZsV6U5wdh90fNAiAhmtxumBgLzpdW%2FEObuS%2Btm%2BzvOY6SExD4tYiwGUWPOSr%2FAwhnEAAaDDYzNzQyMzE4MzgwNSIMkVSRsv%2BiIvdah6TNKtwD2ixj9tPk%2BYYf2dwa%2F9vDHAXhE%2BSx1RRXLralb5mvr%2FdsRDkXTywd21h4zh4z5CLyCcujs0fg9dBS3QLoejMmQCaIlhoZ9wzwi%2F6poeZpTQs%2FQ5rzMG%2FZHKCaYrIt%2ByWbu1gPoAQff6KP5Jmrv%2BIIeQDJMxQJBpTotyrfNsUZmCQjvPMlZxM%2BE1wkimuu%2B9smgOE4SfBoO7rhpPfe50Gtm8mp%2FR5%2BbVmJByGvetNgQHFRQ9f2KhMRu%2F0ZWkndaChTGD7jHLI%2Bfgy%2F9B7ZcLC2a8aVt%2F%2BtL8Sw0ymjsPj3uhbZZnB7%2Bb4%2BBWMUcql%2FU4QKAFLL0aWEBWPCC2wsZ10wQeYt7VUevSGYPqHh31CQgkBO7y8talfdVA9K%2BaF1i1mpsUiNyuC%2BPpt519Eny6uE7uR0jDm0X3nVFbaQF%2FEuDfdODib6%2FAv23TRhvA2M03PTdeuo4eUNlA8vO1Ozuk9iO2qJ9xE4M%2F6HLNpDG7gbafDLEjMIB4Px77c1kSfdtBTODvVHzYFJSmsposOdWK4m2%2FRAqsb%2BRpAprZor4rwLULQriHXFGoogOOq4VZmFTZuhJeOLrI9Rec0cq4xaraZdYZTaTHLvz0Us4yInyFL5iOO3i8KlZPhkM1UAmhEwvrGGxwY6pgFRCFuMYILBlVXehvgaTViJo%2FGdnnC9Qw%2FGMpQ0bUJi27QWjTZHm3ZhqXXV502CMgeS6PaXIk4IrPCO2OXzIck4IWyzChXZr67BvA8aaC6bf48GenLe54UdXJiohJnoepb43Ari368hD2eBjz4pnJlfo2%2FHmnr7W2Kz%2BxwacX8ykFMgVFATFXZr18YyARjhzUylc8Wduq8ZWqzR%2B%2Fm4W8%2BgzihffZEZ&X-Amz-Signature=11084c6fe931a62c087d5bf1343256e5e8d6791460d6ef1e56d6f021aafba3cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

