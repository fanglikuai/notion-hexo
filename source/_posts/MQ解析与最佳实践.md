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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663BGC6BVM%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T170103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCIE3FYaBGpQlNuOBptzO7YhelyICgOl%2BGmvLO27Rdd92AAiBM6aY8iuEW%2BEn9jYMrzzScLiGaqHtr6iIPDEs9xlAGMiqIBAjB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMsev2Mt3uTHUXxoiTKtwDoA3Jm5LujxchwOjVR4WZbjn97jWPdr%2FxgLIfH6iV%2FWK%2FF5hYQW2bTpIA5J0TsXOzHBobyFz4VnHpGCIYMZ10nHQpKyJ6lGY1oqMi411VRwsThKkHfimLb8M6WNYyZGTb68BYHDOVT7iA9g0LBvbaZkziWuy4srhJ38VsV40pUcH2W4sPuulsAm5k1jgfOcXkFFU3pLc0fjdUWO1Ga9yRHWcCw1RCwa%2Fsrty%2Fh2Mmkfs8n%2BNbBnV17TdoUmznc8bBFJ82mOYqyDPiQCF8MNs2OvzIw%2BVYqS%2BAegjbQEVZ8JCRGlH8xRysRsijdfP6xMf9%2BPsSPfTPwnBEVyPSq1eyH6BIUil6odRR4DrPWWkz9sdMDr3Oq3QL0%2BL2B%2BxOtWcyZWueYA1J5xAU0nIA7V7%2FMgLbA5MvDOwbJCNJ4%2BQq%2FPHARXtLJ6iLOW2p9UMVhuD7g3CNFHxyFq3gVIoT%2BSke1g9lf2LtbC%2Fu4kfcKlDyNoboo%2B1pNZMJHzsT0o7lQt%2BHkP%2B%2Fg7pNN4fjiNNLewU0elfeYofg0%2BKPFiXjDZfHQ7MtagjUTDX0DFNmXBd7O9g7ZsVeDnskGWNidptl3y8dQ%2BlwoOmfYGv%2FE2WaocS2eO6e9aOo2fkgxOB%2BnY8wgNCDyAY6pgHxEem%2Fo0okU2bbzf86bfuclfkvzbcE3qxAIzwM9do%2FkJxerYTjFysE4gbh8m79nW4zWNDj8ZCAje5dP0TVaahthfMzTTQw6v9gqTfkWegS9hFUEg5aQpmTSDQO0kGbk16Up6n4Z%2BLAfB7%2B%2Bsk3nWY8ln9Aep%2FK2T9gA3dC2lCN35316ffVsI%2B4URtO8GJFz6vE%2B5TgHfSYnFScIGf2d%2BbugD2Vge7n&X-Amz-Signature=b2165831586764a5fe3dac20d685ced8182f0ebd34d407b819d5ef73576b889d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

