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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SAC7DFQZ%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHcaCXVzLXdlc3QtMiJIMEYCIQCB7paNpxcNjCLWZB37GehxsZrzDX65O62KVvTjd6NZZAIhAOXciaZVtZHEXyJ%2BzhqSwsRjTNls%2BXi8Ha%2BoQDyzPmyBKv8DCEAQABoMNjM3NDIzMTgzODA1IgwugsB1nDXvoIJhtWIq3AN%2FT%2BdFpahsXNx%2FZQrRJ%2BZzjilF%2FS6qipj%2BJhwlGu%2FmOe9y1bW64aBSn7zsj0CCtXdxDl8qSyTV8sO9AP%2FjUvwLyN71xXoGB7uUbMN8R7dsShFo%2F929XVmbgTcnzOdYH1cOoDIRtr6LXvqiPapG4Jmv%2FtJrw4jSFGIdpfbiZNd%2BmhoRIo7%2FiZw%2FOXSJT08N72FfyTH9n5R1hVYk906ky%2BW3ELRTwZJEo9UCifV6mF2D6TD2vGO%2BZrJ4GXHiPaPFppnDvXg6fZc153cTRXd8ZK69rpdwLCvdKqXCF26gBNjyRQpNXCcQJ58e47FHISpZxxM%2FDSAjJs7hGYrW5JtAQ8KnMw51BEY8Z1viuofMdnHDmBSmp6GG%2BMCZ5LhzPVqcygR%2FxpR6yT7rp4mvJGq3jv%2FAncrx9YGM0qi9gc1cI1Id8T15lJ7gTYg2jng6kiK0FxKaGtUJuG%2BLFnfhe8u45udD0AgwE356nFxpMYeyQeVlWlnzGjLm2zxC%2Bvjj1oQKUiRp8sL60%2FxkOO3Gwz%2BmIMoqEHp66wB2j7qxcVpX%2BxOJ93kHOIGBR%2BOYBumQSb8n8CSazSIhbzt2Zmzcgh8g%2BhaU1fCQhQxhPex9eWYOUIjkfry9N1yNX2OMjD%2FdXjCWu4zJBjqkAevUo6YkTHn%2Bd2uj3iO9A%2Fu%2FTKeicqV9g750mZ1kbx9fji%2F0vhp0LUIZhm45SuvvYrOrWiO%2BZKhtSEy1qo67glcaW9p7RebvFmewrJqCafr2JiqlrTyo7PkuYTAqCo3uxnaAlYp4GlXDiVENdwgCZIEoSCBFt72QQb3HFqQSj1XwkroPlYsmoekq%2BuPFkTBbwE3kweqQyV8HCZBxZCi7wYyKS6PF&X-Amz-Signature=1127c05716eb8cb8f78e5a2e1a19c31f6bb9d284d1ae1075a1d764b01b3786d5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

