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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMCF6LGI%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T040053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC2WM5pDNlm3BFIcGA0INBjy6IiIdGsi%2BUHqBnhfho6mAIhAMn6IyTpWdxU%2B%2Fis4eOUmxcrAm%2BSOL2DGpSQcaIT9NQTKv8DCGsQABoMNjM3NDIzMTgzODA1Igw%2F8Bx02FOQfXrlQGMq3ANiKlChBrckFM5Ognc9NqQB5UFRozEAkj5BQDz8yafJr5CAtN3t49GKZ70ffgIdjxNzaVyzBvA%2FtGY4uVAvtiFZCgBCy190sa7v2i8WWxf6Zrlruj82WROMDIH7zYnOd67gcA%2FBwvEIMhBv5Wh2mxu337eo7DVI9GI%2BO5XffeuUDTAkphFCiAln6wdRA%2Bn1F9GL9acmJDvXLdm1kPEP%2B7yuZcK2PQKhdA7fLqx5f7YkzL%2FbpEUv5aSzVSF6w199VWVu6jTP1ewdz85kNFdf06oMzoFe2OMhPlERI6vnMxxVApEkKI4j2fqMBq1ggzAMTegqVFCwYmehQUoWLU%2FAYGv2CEXYIo76lvuVr3phDGpcT4Y9%2B7O08IJh6XSSBK9WyfZpWXZ1t9iAyhcYBfrnLY13AoAiIjv9wP52LlTIz%2BiYcpS1cQaJU6pd0PchJOEqMUvHeu21raeCg0rfvffSvO1dg5hbFqxz1Qx2zdEz1GOmr9ICdIUjTx6QceZWq2n%2FHF5X6Aisi0yqMvy88Ns%2Fs6B94lTMB1MYWX0aoJOL2yKximINYr3hTzhG5uZlIQjVAB8RZVYmWw5cQMHUAg5%2Bw5%2FbZExsudFIRvEdfaeyUUyCPluGYZYUXZeg40qmLDCHibzHBjqkAePG10imlOd%2FgthToLhgmVcScBp6rIMVFVVq4ohZnlQb5MFTvkUYOr96tNJ6of96m30%2Bi8aDZ1LCB19yirsHfpZ0t%2FzQYFh9p8kFZ89XOoWpzacg5fyjG8EBnrhCxiifZgVcuxhjIFWFSZGUx%2FDqwFo8BelvBsrF56j0XcO2iTJEQQ66TPmilJ9MWmlLJ737e%2FHyac2xtK5vO4uIjHqLCRvWB5rn&X-Amz-Signature=e6e6e0783eebbe91e3c596730a70a12977d612fd5f78eb1c0a6936b508f688ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

