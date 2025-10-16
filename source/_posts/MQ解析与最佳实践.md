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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662UTA5CV7%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T210051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFM2o2LFXeg2b1mHl2h%2B8p3vWDNgePMOusTLAstD1UctAiEAnePFRL%2BOMSV3r54lI5KkaCPAllHYRI4BW6j1HIpJF6IqiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGy0%2FTt2XK05ZrjCsyrcA1JMKTn8l9eCdqUJ8AADFNblQ3THRRmm3mPKNyKSmUIIxDxzaTGCG5Fr%2FjDyPS3C6CHZ5q%2B98smPaHqoUKjzPqo3sdcjM3CcHF892WlpMb1TL%2FEkexCpX%2FR1bVZ%2B0AuuPXow32F1qbG42FGLd1UaYxnnelZP%2Fvbuv3xxC3C%2By8AojC1jjZw0KiNb%2B%2BznaQB1v%2F3mdvUrCIzhv23CE8f1aYdJFHwIWvLP6OGKT3AOHfRM7J%2FLM1wNLbLOM%2FNQZ6H5VBeMMmyaUjhuCy9mcKfM9IgQ13BfstjYJ6qJOxWPvBQA2zPAPmvOHl2uIb3ZsI0miw%2BMzvkMGV23cFk3s9h9zMxkpiHoBWZZbO7xqJ9vHLNUltzfRqEAmBD3%2FXH%2BU0I%2B83YORX%2B%2FXnOGsiFV1YIufNfUhH6wrQO3CZ0vP%2FKQps90VGAp21zA94YFJDn9eWu2Z%2BdinPALjUou29ZkNRquCOGpIVYvy5PXppX6iiJrnp6jYja4vDur9HO%2FfzlLApCYYyrXnL5JC2fiE4RefCKZPRCG68r%2BtcMxKwtEbBGF36qoRzWypfqePB9TZ3xUCRTGOTEAeThwz7190qSZ7OneFQmruFvphtfI7c9oa6ykeaQWPRALhYV8uR2ahezQMLq4xccGOqUBvIKLb5RrsZJ%2BZGRNCvtrKUPbLo4nRHOITAEIWtNjlbQqkEp%2BsXPCkbhRnCbzEGzMTyYwV9f4%2FdeduXk%2Bg6GDcToP%2BjxNnmgaMHsrIK917oKuxqvjm99puEoNsCRecS%2FupHFFn%2FffTs4Mime3N3K0iT66ZYgLclFx%2Fun9lI5%2FtDdq2Pz64u%2BoEQ7c7oWJtUUkkLDIoaRmxw8%2FXoosDr9sNjSoS1iG&X-Amz-Signature=6b72f1598b130c49d7e33f2202e21e4cea69c619572a8a34a2e645a7b5cea302&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

