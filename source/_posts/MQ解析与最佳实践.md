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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TBLEU2QE%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC6AE70oyVncerAJFH1A%2BhrCSay8tdT5bDm7wD1YclzEQIgXTfw%2BiTYKLOrlUZTZ4Ml9WeCLllIetV3kWnQgDGlQToqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMEIkycKS1xQRrJp%2ByrcA5dBxJfnf4TM1ansLkjKgOTWwvZ7bpUHaAfMf7gHoElmouFI9tZSkvg3849%2FvJoibfvWeVcCwUzeLLgZ6W6JSfwLtrz62h%2FxEpBVJJ%2Fvz7VF0NdoP3b0%2BqILYdHyOlw%2BNYg9FCpQGIr0GQFuSPNGLJnDFT8H0f%2FN0aFKNtU9oJzxKAHiE0AYNDUBr4spaUy4mEzNc66UAOFwsAVb6VU17axaUG62Y2bWKmjX2PLxolC0Yybi9HEpyYgFLm0sXpk2i3CjA5ftiYKpntVwjt%2Fm0pPSoHoCQapkSQkkr6m%2Bo1kWtVUU6rbnEHYyj7obDuRCnQY4JnliULU1n%2BtpaQxT11fyMy0%2BRG%2FgYlIFYD1xih5CkOQ7ZqzG7dEv%2F4stoV4V2EAB%2FsPlfDr7ZEIhK2bm3zZ6M7fGjzXyZusDwcpjL%2FwSSP2V2dOlhM4KipMBUAbWo73tfJOZZ40H%2FB%2FGWCMkcbqT%2F2jbWc%2BM72b%2BbwemNT4U2KBYtqghWHd8mz1Y%2F%2FhkKL0xZeBrsRbqkBVYqsLClqdSFGPpKFfcmks51d71MGO5Ea9SorZ4SMMiBHidiELwuEYtIgrSQ1q1uoy2%2BaXhu%2F7TaLqrfrLSw3EMvokVkA0RPMQzlapPPBUyhrpkMJOiockGOqUBil6O8OL9I0IprE0CB4VEnct52raWez1Xq%2BOVzLEgze2ii87CpCD1tH18%2B%2F1%2F0u%2BWQzF5%2BualuF6uB2wPrc8u1ioZtRaFldmRamhEzYLxFGjHDC0%2F%2FPrddkOd0HnZy8%2FwJkPaXd5N75PmxviJdacHr%2B3sNv6YXRgVydAnB8VcIzNTJ4cvlyHoDcbYc6UZgSUndlhD9W%2Fk9M78H4lLjNmfLn6p8FYR&X-Amz-Signature=3469ed8ed778bd2f33a9e3106640dddc645773fb25dc268faa09aa5f9be06c06&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

