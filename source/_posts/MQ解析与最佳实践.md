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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WUIHPF57%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T120057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJHMEUCIQDSKlBMpJ37wWinKhNsl22d2VPyRL4Od89kFiwLhaq%2FngIga%2FHMbqXC9C5YLlOBNvdkiFFCGPwMz%2BwEw2tFD2iFHHcqiAQI7f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDO63TmJx3ARNRL4C4SrcA1ZdBJOaZ8BN1OYb%2BBtPI1zkEr%2FtCVIOGzGU%2B7DdlMu2CZ0%2BEobJLAGOpw6WB7lFDS3WnORxwY3EngIOen3GhZ2%2FenRelIyEOHoUn%2BuyUh88yuOXKB%2BcwSFBhSOfYxwQqVVT%2BEOAM9S3%2FadHrQNGAxyVZObtjjMLpOkDJvNpEp0l54IIX7jZYfSX0TTeKz7DqGSt0xeiHMeQahn2LQQt601dC7qE%2FfIeEamhB0q7cGPGsScLPRcWwZ%2BS%2ByHJQhT0sItlY4gsp%2B5aJgrB9x6j3IIKA3KTjXHLhwkafkSVW6ZX9YhAJu6nQ1cF9IUPKfJ5Lu62W7HTDlh7fC7B9fxI9u6X6YKBj8t6pObUVhfjEMuE0BwQoj7xeKi0yIDsmz9boZu7mduPFsCbgedW%2FXMj4SYDmJZVSZKeuA3e0zmYCzUVpc7zBdz8yySvSxMozV1oOP3oMhDVU2cJ5%2FiFAOipV%2FbyNKReS7G4LO7usBOd4zNIeTRL7ag%2BesJPxJDnema7pyBwAR5AmPwx4gnWBhQPi1b3Rl98DsbhiVtCodeHnFVMSRShQpUmXFYn8OdZWcwl%2FrLuaIP2KeBdanTuhgUTHrPY99Rb1YhfIg39Cb3GvVtIEZeUEhhGMoH2KvMzMKDmo8cGOqUBAFqeuh5XMWZs52w62e2TMDJmkcOnPh%2FGEAnagDmisGhbV8xHZvWlc8qAI8Nuxy5ElLvL7s5FQK2b%2B%2FtDLzJCjbJIi6Fmb061QG0jm53MRyjxA2fyX%2Bh9y8twSVf84QlVOSKPaPegsaPmiEefwbg7j2f7DzuIkgU8F1zyx7Y33aV0v0Hg0gWj8ljYZJIq1f0LRMYLosM7KOaX7nDe8wDNsq1cTDJk&X-Amz-Signature=f93bafe2b3204645176fd3a15d59674d444d6671a441b0bd26b4c10afd03961d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

