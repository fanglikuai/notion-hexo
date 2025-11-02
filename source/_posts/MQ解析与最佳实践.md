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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666VCDJGOU%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDAzLArQ15PfjLFVE2cCZU%2BvnxTb3Xfl%2FLoFOATxiA3ewIgBhykf5wrZef7%2BwPQy341fDXAjK4eNdzHCEFaktG6%2Fkoq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDARmG8AmPJE1mHQXFCrcAzSDURgOLJup5JoixQXkw%2FHi58rMu9bgdvbXAOsMxu1ndE5DukGyYL7ggUfCoIixvMyNooLsLJp9HhSl2xA32vrxcgJBE6xQ8cUJ1lwmJ8BEhUw6rAlqS1gHokO%2BPwiN49O%2BBK89y1fL27VxM%2BhasBgBywrFXscjs1KBtGKDh8kX%2Fu%2F1iRV2yGi9%2BQwTQJOYEDK71CNyDDGra4AoCveQafORkxb8bMB5T1PpmpP1v0UBtXvQmQTq3i9JSJmrvnTqmZqH7V0cQDkSFi9Ayg%2F%2FTwqeTMpyq9WAqn6m7nThFUBCFBspXxw%2BUAtFtvwVowklA1oLSyjDcrjrKIiZpVgbe4adqRYq%2BmM%2FNtjWQZ2Upjzxoz%2B8zJxmqImc%2BwtogX6XkdHskf7Wkc9NfAonpFALaMuaxhU%2F4p2iQD9ueN4P3UymHZZsZW4i1Y7Qujm9IyO%2FRZqPULCPVl%2F0BB98hD1Dq%2BsalXu1Rt0fekvAKeZ70uV4s%2Btk%2FMtUGEtjXKZygHjLIyvOO07DqAfSZZGQ9z8c%2FeJIy6UiD%2FtfPpPNNDWk%2BIOZYD7tBtervFrCbsR5OfCETaIXLWQlMJ0SwAzfloJf4Nas%2FYnvGkfTFfNAohdTkmslPx8OGppYqj8OI6T3MKe7n8gGOqUBshIg%2BRnRVzRsZL7CAbAdx3tHT89H5cA3Y1tsWLOd5789f6JM54IcckRLWB9g3D5viFe47nzye7tarA58fVB7Iyv2po7nmzGXjKwBXdyd8sgXkgS4Set0GOe52EQ4%2BNepm5WUBvNg54F27wCPSIvlxqb1lDHVCPjqZNn0l9SJOYFn9PUDUhRo5XChir1Q09FofJuFyu5cesArbWiXOXrwjgB7WA2I&X-Amz-Signature=615ce9ce313c228ea5d829b4a46fc9e081ed92ef11561d929b5eebac3c3c2904&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

