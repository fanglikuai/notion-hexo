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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VN4VL35N%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T050038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDQiGw%2ByZI8RIfl%2BC%2FLwA7K%2F2mnmC0tPkhFhTKaMcPzIAiAuZKv0jVyhY6LiDcCv3XFyTToy%2B47K2%2FOWJHASWSlDjyr%2FAwglEAAaDDYzNzQyMzE4MzgwNSIME%2Fn%2Bpiz8OYhOmtRYKtwDj9RF%2FRwjw%2FY8eqfFRJPB%2BGgJ25vQqEXD2dTMRQ1oIJEYemntDdtL6j8QM3ejQvz7SjKb1NtQRpJ7gduz4BopeRNj0HHtuYxBUkdiD%2BbQKpLZ9YStYldjXet3sICkW37fQfpIwaq58erSjfSIsMtQwct4EYHmLKE8oyjU6wOSLr%2BQBZ%2BRR5dLJwJa4Hcm0r5H5YsIfGtMdY%2FCBv7gEiMn59UC4CPfo6SxufufqW%2BRwXGeoucoqmoaq1Uqjq8nUrmfiyno7fxdxuqTOeQIuBgRmJG2S51dbvVOYUm03AZlQVlWxQ1VF4wG55oRpOd3xZtvzuxK%2Fiqs24C7w7E4J5Mmn7SYa8cKnT5JQAD9c%2Bv1HnG2%2BTIgW%2BCUkE1KMDUR7iGmc%2FsarVwPxAumfJewd%2Ff3VDNuRvYjBeGM0X%2BasRx1P81FDaTexuszaSKic2yOBz5CZGyVxXpD7B8av4UiuKq%2F3gCO9BbqIy0i%2BCC0%2B7QIkczi2vU9%2B2se1Sccd4FMo9fI8w%2F%2Ftuy3vJMteW2rdWOD2vI6yk8pknv3FiGUQg88a7Om5YbUeLJAU8omj9Ab%2BMs9Wbwt%2FJC%2Fg%2BDQV57V%2FbgGntM0XgBWq7g6dsVsJ3avn%2F1a8je9AtiixmzcUrIwtvz3xgY6pgFYjDwJnElGtf0Dy0ix9s96Kwapta1BprPJXLypZR%2BqjD3G%2Fob05dPF7F9i27F8TEeZQcVmUzXUONYhWVDXTXmJShmQH7i2m8SPTv9ddyx9SSfof6TFpvrv9%2BMBfrrLheGud4JGcrvA6xmGmha6q%2F4UC3sRLyQXpoiTIGheh5okuDbSDTp8DkjI2xeYWZXX%2FqX0f41vB%2Blh%2Fsn831nOf1%2B%2BnWw05vgz&X-Amz-Signature=d6f8610810216e3d74481093c95ebac18dfceb592174a009b512e8e97bdfefda&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

