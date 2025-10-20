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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIMTCP5B%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T220049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCIAUQLq%2FsI%2Byp8M%2F%2BNG2GFjSMgP4jI%2Fhq6kwLcwP9ieJIAiAAt54JIjX0P28IhQyD70PE7YDlIk4iPYjKtyueMTRSkSqIBAj2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMwT8xFa8OjIs2K295KtwD2wz%2BWiTD5iAtK2OlCIJuZshM7J34HSZFW%2FW4R3BM59VWZC8Q1dJgf2zGF6bgzP6ao6KevSsPlPwqf6qBCiOpon2lQX47HyQNYOInjfiKX9VvY0vJZou7yXyUMYyWLsaXQNTciRgmYbCcF30om5MA7oPP%2BN0y3C12Qnx4Xi82Ex8T2FVrFi6J5FGGNvohLX0xKSbuqRER7yYHeqr0Ms04Ah9WfpTSL5%2FtWp%2F7n8Dxli0iAUdTPji10V7JkZwCNcYffcbvp1kcv6snXHjL8Mks9Up0T98HcPR2uM%2F123gPULnXVZvmkfmOVvG83eiN%2FBdwj7G4bK5%2FrzeC%2B9S2t0EcGdFFsK%2BkeOHvnJIlmzKZVQAMk6c7WBfmFUyCKniz8ykLj3K9zcDli%2F%2FrbvigM0gm%2FrBNDFR0xa7FrY1%2FJBG6TYuCIazBvg63ZpDGVWsCvF%2B6p0%2FBSGd6CH%2F0NfVdj1ZyXRgea3BK5WNGLMgrPopgwsODn8fmiC1NqpxPVGAnqfWhvwl0vVgjjpJwAY5LhIOI2zOlbSvpDOtiEC38HUA1T0uKD4dZki6rZ08esi6%2FHc%2BI3M01s8KrDBWxYdYYa2pTkgKusf3XpxEPWhAU%2F%2Br63dPnbEZVm6J5QDQJUdswhMDaxwY6pgGSFYSWYhfu4NqOelkaKCuOvIm3n%2FLuavmRhRKMwTWBaCmJ96wSoJVTEQio3JCjAiDRUr%2BaKXWGhUa34dkX5crhsbGFk4xWcBa%2FmuOOYtJTrKHJutj4f1UwwJfFmbFPTABk2mhXdvLZxduvbSIP097JyFRYrmj2vEhRh8KVUJYsmLMqV8f5O9xV5Jq7J7gPLhqmeRyFyTJS79%2F%2F3RIg1%2FhoMZBSXZac&X-Amz-Signature=fbdb2104300ca1d910d17f1d47a00675ebe3c701e91737e04024d1b9840c97a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

