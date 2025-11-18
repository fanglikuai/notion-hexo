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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662T2DXC6G%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7adOUJvnX2vo0RI62eIsMOxaBJtDOheADcYyAaqofgAIgcFJb3hpVD%2FKTqLFMtATfYfmlOcK64XxAob4kE15yp8MqiAQIu%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJrIGe3%2BaLWz0X8UUircA9bR8DokJEBVLNOZD3s09ooNnDjE%2BUfvGQFG1a6uwkvecPrre5GzMXNLA846567ZfuKe0p8nOWmW7S0s5c1wLT2Jwd4p3C4KpmPGk9z%2Bo%2FUo4dsCGMyWQIdtidT82sXmmOAGpiS%2F3aXIoXgfym7uFuKPcl0Ye%2BdlR1B8%2BIJMoljkrOVKoqb7YlphLRCfeKBfcY%2FghoIg%2B83HZcslkHnj5RImmZ4vrQ97h%2B0J5iQ2ao%2Fm4Pnlt%2FUpkyzKHIp09CSJFdUW0F8GcOFeieWszLN97OQYYtjRkqIkccvo44lHt1PecA%2FkMhe3U9%2BMBqoQb8bJ8BurlzyAmW6UPfhgG2%2BSjWlgZDjh5rkgwObVt62ko6tRJxIc5w11BRCxgm7PZXGkW10kjR54PXT7Q1ANCzbQGdsqWCHHQEqOaE2B7Zc0M3TpZnbw9r1KX6c%2B4DpQb%2BnzT4BcfVsy6B7vyJun0fjBYY4JEJ1ZHRnPUJNLPbMPuWHTfHJ9o8ehAiPPcgRDjE9ZGQxDS4I3Bu7usN6IkkIN4RiKgSf3RyNzQBjn3PoFSSgy6INmvZeck9CJEnM%2Fhy2XbGz2QBqDxoy5Udgi7cKB9GS1Abh8KsulPVXO8eXnNE6bVfwviTrcQjpBSV7TMK%2Bi78gGOqUBFBGge%2B9feeBrx6JAEgpkwVXdEfpLQ7bc8XARIA8p%2FvQlOu36%2B0njyrOHa6mIQcqJWJ%2BbznMDmMPVNK9B8fThQsmD%2FEVtUH5zdMsvJOxYNZ79eg9bJjlfcQztbL49pESSEHx%2F3AvvAxtV9gagqQHBfo4wl%2B0jSfaNezIFZo89L%2B9cb2rOodZJ3xvpAWaN4cuURkBIvCTTfgD5KQ%2BsiJaXctlYD08e&X-Amz-Signature=cec9d6ccd5afae8a77938363a6df31adbb17bf36e152132c9bc80f9966094f03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

