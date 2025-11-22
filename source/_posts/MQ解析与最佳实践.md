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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QKMMSS3P%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJHMEUCIQCpNmgI13ZSb0c6ymxng%2F53cKWj9CgUuhvrIbFzvHtqXwIgY36DZLOEHD3ExO6CJ8HvOANbg4d6ySa%2FbIVcIrdRn1sq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDA%2BL0Wdyb9WT%2BNUAIyrcA%2FtP%2FqcvgVVmnmaMLymjgD0yRcz2z98V3uEx2HSFXXi6obd6HoKR4fgV1nlC4yeYqilKEy6Ruj0JCGcReEUI8CAqSA7EmxIVppGLvESmZ5W9Gcm76sT5JAHFcQMuTYH301LBXAsTUPMxVem3ek5XXttm3psO8ghbHfJFiZ0woEYYYYS6ZaL51lS5YDEh4QtU8R7FDrG2hTx919w%2FICIQ%2FgX4qpPhova8UgoDtR2sk3EchUA%2BfXHmWr%2FOm%2FQrQZQTY7EsW5aPbcXARQz5XjPOooJNMpTkXEc9cZ76eIN%2FevKhV%2B7X1xpaV9QJoh51GSkirpfb9dpkUu3BuxMAxmT6g2%2FFuo1kvnbQioYiVz7kAOC845oGjZ8T%2BivlPEyj8%2FAECR6mlgFZ0kDKgbmCPvp0gn%2FnO7w0Vi6JpvoUYaXj6Ntge1tQWeffWA3JeflVrnXWDMVZkRxrc85O9Vayop4slpFZt%2Bh0D2LcAuXzSrW3rLlPw00BogBLcl4LUBGuovtY57BG7pqm5j9MKzSSSnIk57EyM12xQ%2BnBXZe5yET4PEMvkuD0z8%2BddN83%2FUTrYPFOQ%2FQ%2BjeEb2NpYyqRh%2F12xQlcNNFf1EJUdk6FsUEhFnPhQY%2FlUamb%2Bn9OXRVRWMOjkh8kGOqUBxd2i0VUZF4et0zKulYPPkQql7l3GcsJ%2BaClnObdhDMEekXT%2F4%2FG5i7PRE3mUibWtRShPPLgehsRR38OILykpr6vROZoUob8LlMKto4fWiR6y%2B0xNwGPCl%2F96C9S9ZDUsEmVkpst67ff02o8BhFW5%2FMZXiJOppK3fuU04griYGb3J3VvrY66J8ePL0TTdp9tJh3QCrFlNKBJ3csr6eNNGuArUmE%2F0&X-Amz-Signature=43577710ad95771374b62a49bfc5e3f40314c85e3e042e6fdf422ced98c0247f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

