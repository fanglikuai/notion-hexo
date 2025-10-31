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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WIFQCEQL%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEMaCXVzLXdlc3QtMiJHMEUCIQDAyKWJ84JBJRdAqdtsgF2oxx1BB5HIIAuSoLXuTDvFcAIgffjbjbfADQI7sV0WXCRWystKewRE09lMaWSd%2F%2F0Y248qiAQI%2FP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFOESXv3sbEmtbWZkyrcA6DUVfVE9TGd0l%2BD3ZLXBGISiI2az0DlQnsPzNyk36v%2FBAdR%2FXyKSnqovcCVrg6cA2nAtKqk%2BEKtZ8lKbJVIUee7HsmH6QoyHY7NYt88pvy%2B%2FIFvGtENAlDBydXbWrt628QK4OnwjbQhnWgK5BX1tSOyDcwEgeQS5j2usNAevJEyIx4mHFlLDaQHmRUzaT9hTm1i0M9SjNQnV6ehCIfcDO4ELrnzecgttwPxq4JI34hK8iweBC7SQRhCnibpVU20ypLD8oJ2gXIzRMZd8nKNV7rbVdE8HXMXJ7mn%2BKSxgLezHdDFLxWj7n%2BIjXNtKdW8%2Bql7EXbcB8frH8cRmua8IzYBOnzzW2DfwCrudAG17PJBsHO58YFymsx4Dg0fazifhcHsrlpztArm3sSLpbopgPyoKLmuYbF8iLoAtC4KIKn79gB%2BOnWJAkEd040Hx4cZ%2B40MoSN9g9q02gRg%2FhDgmRRR7LZ9hrmI6wym%2B%2BhgHmcbfz44DqHZUrmxSvqX43Piq4NndUDliezBMsDpfW%2Fy4EzzqZfN7L54jTezfS%2BJ11ii0bKs%2FoT4CiqyiViUfanByLdgpNY1DKN0Hqa9X0C%2FiAUFFtAf6v2kioyFXsH4Wx6igX69s8TYMGaTgtyPMJjKkMgGOqUBAYrLC81yhwttkWk68vZXMo1M3RnF99Airupsr6fkP8RmvCLApk0%2Ba356dLM0HU%2FhM%2BwJ79LrrUhrtVM1d8UZvW5guKAgMt7CMDCh0QW9YJLzdX9R8nYnGBOyjYU0pud%2BsSlXxMGRlugEEX1HRpnem7xMoph63w%2FW1t6LguswsSbTttj9gseqLcJVbseMBZuGKk58uRwTPaH7oU%2F5oidvF58ch29V&X-Amz-Signature=0732a3e1e3548aa29ece8efd9f854d6e63fcbf4c15804b185bd090e46416b6b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

