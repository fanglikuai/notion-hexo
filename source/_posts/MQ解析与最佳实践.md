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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XJLSJWCY%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJIMEYCIQDc%2Fflo00taVOfLDleI8gwWqz%2Btf2%2Fcteg4GWLvaAUMuAIhAK8OB61aWzTvVxUEFsiMTnv3uPtg14eoY4L%2BCwD7b50PKogECJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw9NMgXchCsN1vsnA8q3AOWfaGAYhStN79GruO3DIYUQO6dRlzN3wCsSygwlJX7morTY%2BjWqoK%2FNEgm%2Fl4Tad95iPl9DgNF1ycYQfN02ye1ygjpp7SJWQF46f6ti1GVy8g64eqOH75Gwm%2BKujL9CLP9eOWO%2FZ2xBLNbNHhKM4CIP9fU%2FTRYIcCAH0jTNKaDtDnzuUZOGcSYaNnguWj5oxQvip%2FHgyJJKMwdoyHPk4Hwk46w%2FUQ8E0Da%2BICt%2B33C8x5pafmyxncJ36fLrAAuan0ILiiEJGUJmtFPffS2vZHJT3dTLVGMwrB2TjIeY2y%2FIdIKBgI5TBqvtUSwTqESUGBt%2FpKm8c7KBOv%2BFVFPQoQdA5mj3nMQ53%2BUitPbDWFKMB2%2B4s2U3zApjFNewqmOebSpqfGLEkwDSAps6Wt2bWoBeMZQ71%2B0d0tDdkgT3x0tA0p1mMipsbMg05%2B2QgzAU1P4vVHWuor8UOrdeN4SuVT9JnzK5l5NlQGxV9qrOX2bQB371iHQDTR8cYMVfHr0%2BIWBagG9pjBJJqPyP7RND9ZBJG56OY%2Ft%2B%2BwdQq9UL84FXuiIYAEGJPSoy2ltV%2FgN0FTFRC42Zvr%2Bxav8Nfjs9mWThkJNukJcp4SnaeONoJTGTJ%2BeugYvsidcGqlgRTD0vqfGBjqkAa93djRxCN9004MpC9Kcp8kZi6Ogjm8BEAgAIS4dK1bKu%2FALgXr4xugE2whKUypxBuEnkD3juVUeIVQQM1wHYYWvY3p593ekgkws7kLuxApLNKawMTQDUm0lpC2mcK8C8DA0Ls15r7ATVhgKhMeiMtEPO8ebZu%2BKZ2k7AmSEcXNXIsqE6kKuhpno7RV6cjH681yVCASvHMd9Az6DDo%2BZSXvRbrEi&X-Amz-Signature=f4fad87b020095e5327300da219a514a88c6c1d3005b2a2d547414a2bfa0a263&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

