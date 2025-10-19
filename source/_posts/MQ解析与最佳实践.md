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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z5S4JIEG%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJGMEQCIDWJ5JOZQjSvJhp%2BIIE%2FRygywqImWvUlrXmh1YEIbDQTAiBJULsr844QsalM2jhm2lF4%2F9xKfBINcVq%2F%2FEcu3uGSbiqIBAjV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxytuFaddy%2F4K9SwzKtwDKT%2BSB42PDZgTbbysfq1BS4zFCh1ECoOgcyo%2BAAMPuM4qdx1e%2Fr5jY6BH%2BVj5BbtsETnbe6%2BrEqemnkOLFdxwGqBh7c8HXlYcQQJZnuvlkSC%2BtGS1VhZuCSVBx4k5cw0z5FDgGHihIOXNHxK1E2x0cuu82XviCysqwKg38gY%2F2a03lvWfEu3mE2oRd33rPvv431oC1XTpzxUP837%2ByVI9OqYzqc6UZeZaG5dmeTC3UNcRwjxkJD38R5Cq6yy9H%2FM6q04vOGub64twewBcQby%2Fd6T4LS26uEmOJKwVUp07xsSPMY5tlQM4%2F%2BaWqgYZti9qsrdZwCctB9%2F1%2FuiG2irRmysbE%2BRoAnREFZf6Dvvr3ogBdAqrbed1D%2FdGclF4PwNNNnjxsGRvVg6N61ldfOZtFVL%2FUb9WHpMhjitQVBsnNXXYSTDlDk6SuPZ3HA%2BDu1zQAjCIjHw2%2FeWRZegH7X%2BYOrsi3ZBaQ%2BpjpKI3QkqSW%2Bsv0DG58yGz4wo5HCA3jf9vlFWc5FVUbtasXVrrGzCk107KyqTyizY9pYThyFPYAt99SeUZQD8dYQt%2B7cudjXsGQdHInzvWuXs9rh2n6c2fw2ULL6H2VNzsdp%2FVefXXy%2Bv8hfVMcVRehUPfVNEwxabTxwY6pgFJZ4qSyFfSqRA8pN98OVXEMDY57xD0%2FL4klh35NCsuS6VtB9RlmRRAsVAvl5A6EDglkiNF0stjvGOcDJGjn%2FrxAiYT7OM4wUli5pIx8uC5Ing5hVjpwA8XeIh43OFMW2sjXz1EPJITgIjKOouUUGL27L22jfyDDfX4bRx%2FWgqJQajJI55wjXfGodIZCljD3q0SF3pblyFsGB1bI2Yg8wbDH9YMhyYH&X-Amz-Signature=28e2a3554bdc20b404079d575a251033f9afdf236d6799e6aa3c41c6b6c0fa10&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

