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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XHV6EAGU%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T070048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB3zxrdou64z7Oo%2FfBR0AXnoQm6qM3pnCVGAM%2BevKBIlAiA0vS6bp8R3BsSlgNBOPvVuBefw2qD5LFk0jRMidzjckyr%2FAwhuEAAaDDYzNzQyMzE4MzgwNSIMaG%2Bog8d6JlNT%2FPyDKtwD9OIFpBZxDmvO7A6JLPRfjteTUpncrwmFZy0xhmfTBQYc3dghOehBfvon3iFtTkbiGIrDhcrbo2R02KvmVMcMFU4PsCstpK%2FvfHb%2B5hNJ0%2FQkydYTTf5ny8p3UPU4NgeOdk94sTdBMPt9H5tKnhE4id9eNFUMe8bHIG15K7wLP3a%2B8T9ZzBK5FMEXTJOoPCzQNjjnqdmueFrCGh2pkToj9rkzKN%2F%2BYDVBs2D2edjgug%2F%2BlDHGFuyg%2BSe02CKevf5lBfY6VNjEIRSwFV6NnxzFLlCB%2Fh8cXOtJXDkTvjUL3WaUZKQrVjlgGqdc%2F1iOMCsrWJtvbRI4vtoKJF32NzlODE5BTDp2CjPW75GNyuItPmFBJeSJs21uoC7OAJLQIuEve6Vf8uvwPMlpKs9nlEautKw4JSisD1b3ujfWdoXQcHEXfLyL7qLWXT7fh4nxBOph%2BKPyblWep5wt4Ipt1q2o4smJTvh3mZEyVgwr9%2FgRVeW7CFk7xSLtVIaivPCmbMH2kVRHcCeZYLddw1Gh%2FM9czghgWGSxTiOreDA8FAPqJptSooYWFGtZrVoNRvCwUx2TZAngxg4GAdrKmg4lLzbxsweG29FDOqo4m3IO3yCq67oG1SNKPz%2Fvyi5ozZsw%2FobTxgY6pgFQo8Gaq8vqHI4WmAZdu0kgSM%2FmxMVpX24vtQDtPaZ%2Bwfd6bU5zCID%2F36yySf%2BOiw8W0snlXoFL%2BQMJOej40QgS%2BTfEyL5xpbqTS2vNhPyxYi1PIZQZDlm6%2ByyUuuxJRrZ8L1nnok0ILm2o2L5pQ%2FTlgsTnrorVPxivOzHFXZTa%2B0hwrBjK49WT%2FipQeGyo41WCEmfwzVTTuES9xcNlUfnscs%2BQidqP&X-Amz-Signature=0ef017178652345054982e57cf529855d934ec38918cd66c2f402c8a33376e0e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

