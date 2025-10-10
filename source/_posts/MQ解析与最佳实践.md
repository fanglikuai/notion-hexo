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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q4Z3PKOX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJIMEYCIQDHCvXRL5Wayyue5cspdtu%2FlKeaF2hjKkFxSwJKzIYkhwIhAOx%2BKKzhEqOGwqhah2Vdp8mzRAB16cQn%2BXkO1SPJ%2FEhXKogECOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgygL9mc6UjCezj9WZIq3AMFhzhF2McEBO20xJgwRqRgytMM0DrdJzvbaO2TJdxtlbXngIlajafonunhUbQ71pdu6zLRJY6vDItDHAl24aAdapqZ0mUZt1DHgiI6ZuE5N4ziOLhSZcGQAxjIwkfN9%2FYzTc2orqAemHR%2Fe7EGxOFySQrZ8lKjPHoWj13MOQYdPah%2Br0fUgMLdou%2FHotSwejCnYryLxT%2FKZVGRIUBcoq5jrLRC99F33vfCEw%2F4WEAJhXicMMWW4hjzRkFZUSyrdNJluqLf3630%2Bg3Mf8cm7kRqrzcxBEUgdCLPwack1lBKUwkEjAPQUdfVUeyl0yLo%2Fxs7Fn0zo1jISupwMdS4MB9l5aHJenMW5wNV%2BLPjcg9Ag%2FljkGekVBpm53bYeaR8F%2FigKP2fviEoZqQ89yV3cXuUbHTZ%2Bju0%2BELg4AQ88o%2BD7PokbKSMdpkuDk8aJLLhrOf4sukmQRM8sN05fsPyYwScH%2FMo3mRPJLZrG%2B8UHnGedYJVkg%2Bqv3D6nL2KBSaoxmkJqhchrJW5%2B8PEJRSJEqolBjkLu56cht3g%2F6ksT0rdeaIMNqbF%2BQNavFNFKl5jqrZrEDwLG07rHcsKg2Qow0gJDmy0J%2BsJAJjuxbeF9HKWD41OTmfOrkkOjMMEBjD1yKPHBjqkASL4xhHFyYrt2PDJ0m06x2ej79OFpundt4vH1DK1WA6Ddf1hYrhUQpAjOOt6D1t7AXrNgdJRgMxycYbdS%2FlKJr6CN4jjW%2BWYM7SJ%2F9Z1r9r%2BIQ3LKiR6i05HaC2Kku5%2B0cycYNJ%2B4dg5YBGXx3raOsVGm8B6faypWWwFuPN1w5NzxUISBn%2FWW9%2BgGjJU8rgm9SuMsyjns97%2BbqU4KbLZTbdwuFLY&X-Amz-Signature=0b7e0d8909d287c4ee76a4f72a1a37ba228ebb7f8a5c3f9b86a0cf043236994e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

