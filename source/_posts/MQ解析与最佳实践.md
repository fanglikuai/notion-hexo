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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664X2CO2CL%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBIZu3VE9KuvP8M5k9H3gX%2F1xVWWdrK7hWDfswfuM7hVAiAXsnfdBXuAMLOBLpgvIIrOcfIYjmoo0r5UsW6NUbJnjyqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMfrWvkCBVifEnx1KUKtwDETuTbWe7ENrBXHvHZFwVwps%2Bqe5urSrqzO2OUr266CpNO6wXkTglH%2BLhDdoi5YI6J2ZQQVZxrABs5INiL4TAuse5HhPleH6SvpZUZ550lmFCo73KXXix1T0Fk7%2BgilQRRCbd%2FXOAyBKjnGua4ZTXDaSChDyx2VbjqqGVmDqj%2F6vkxMMXzzXlOvpJpA6OvAKgsAZzRCZ2LGEZD1JEcvD5Az6pXWSDh2KcFE5TK0TFsqicQMbG44me2GpufZnMBYI3RCVCUjs0zKZZsRGLpN32SWTYg6jaVVuMgO46FubImP45cnar%2FgY4hcDMYg9ZSQUFxMiqr3yrA8BMn0F6L2KDpOho1b4gMEY%2BbXTaMb3uVS0rXqF28t58fgwJiwuR6MAfSYeToM78gHPjg%2FuN3p4GhMMknimHIVvzDOBMhOu3YHLi%2Ba3S2V5E43%2Fn7tS58EnD4yf1hClcRE6lFOviUK6QGyOtKdXxUv2dyaPCogQhi6mCVH8yCla2uBxezaS3hBVbLSvZUHSZo04Y7z2FSe4wBzMlm%2F93eUaRxwMsI2I5nf%2BiM3tgLnxG3%2BNHRryIQljW0ZqfM279KVnXUjcfBTzL2bDzb4sZOWaO1nTmTU424eedt6lJfWLlkqju1mswmrukyQY6pgGFFT2z8bn%2BFGfUJXCN%2BftPU018Du%2B9lFT0oSAP4IDiQk%2BXE%2BNvp3buwNIlwm%2Bb7cJuNiKhZIzwT784XAUWlZFe4N7%2FHoNMocSLbUmAMcHcHuzqlua0OyFZSu%2FWZc7EQiPrXGWjDR9oAKqFb4ap%2FE1Napa2nnjrdI4l8xljOlzXKOwQzenXpUv2zvBLrwp4Eb4hQ1rTapbe07u2jQmeRvEUVH57rjvS&X-Amz-Signature=950d6a4bbc8689f7ae2f72f078d6cbc6659a4078bbf0b4b2688a419f1cf26fec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

