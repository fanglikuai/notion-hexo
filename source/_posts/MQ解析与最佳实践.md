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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663WO2AS3C%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEdpNCyZN0A7pQ1Ot7NX7Po2%2BLKJW9%2FE8TS3QCBa2%2BIOAiBy702cVrYmo4Ram4HpoAtdxHPW39fbNiLtaqHEVL4fgir%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMFLOjHE%2BVDbV0Uf9CKtwDL0E8wqLSryPSeUOuO73rV4yIlr3P%2BMAQsHudPBy6HnNSgalKccMxOJRQJaYfuhTmPeanVUI3e2GmijhTCpetRNL1EQA8cfH36P1I8vu17W6K5e5Jqqzska37xJ8W18bOBcxFLWE4LaI1eRZHcdQlWWr1uy6xABxXEp0xLg2tJWpN5QOsOT7xNQKw6mTFobTBU6IbBJo9NKAKDngzaRMDq9HWdg4O%2FabSyw%2BnNSOoyeTtVUJxMLsh%2BVa%2Fwz7l1BbkVIUo95xFtsoMnANCrwIOZcsI1%2BViTSjZnEqKWzApZWh5gxTmCCb%2FpOSjaI1D7FWyKLGJk2VLTFRVePiGipYOs5ioPkGUx0E%2BYe9WyT539cz%2BMx2EO2fw83huBwje1GU7KchJKnT4dW7rBtE9LU0weX3a7uew41d8shdGh4J3VfDj4au%2BnOTBm%2FJ1KJyhMeyWwcfBP3crkStigg%2FZV0ORwdFe7ebUlZuYHkkUvdpbwR7u25SvoG%2FeBYajRQ07iOIoXtXzNwQcfDrtmeOXRKJz8EEPw5JE6cz8wtJtND%2BIc5WfTCCfRVGFwx0sHzx3zOZzuV3FoyUoJnYvRKuz1F8day8HqnpcOPnXs2GZShCCd%2FE4Ls5dmH2dlORpRrQwtfbzxwY6pgHNbzbCXIFb%2Fg2S3%2BSMyUBy%2Fm03e1HT5JGoBztUetw6KlxpkmVQi%2FHz33XWhyUFqWMwEP2nVAMBdNnKO7Wt%2FaEMVc1Mlreu5UZSBQrOG8WZbCaPj5iqqbxSksnNmXpEBQmJEqK4ODeVnM7TNX6xz4ee0DJ9OOiJntciqJH3QO%2Fp574wpMf9QosVMDFvmVzwpA3IZWtIGxw%2FpAd4bU2wEXiafw%2FtdoR0&X-Amz-Signature=d7f2f1cb187db790f0a207363a9754b136eedd8a37e3dd6da4be248476df875f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

