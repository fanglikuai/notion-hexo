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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665TBMIJFL%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC3%2BeN4tr%2FcsaRbvkxaEp3OpBj2TAS%2BxhxJBBJEnddFSAIhAJa4iOi4qZM5HhFigZrmMVCLjJKD26OdTIGT2ELkRHMVKv8DCGgQABoMNjM3NDIzMTgzODA1IgwmqoUWrwckq%2FG5Q%2B0q3AO4pnncWly%2Bl6eLE58cHbgANX0vRooLxS44LAoazgZZWLBTIqYrrQPCfcam1YBaJ%2F4uSmpz4zWIAA4ptE2ydtfJYxcO8uA48FIFwFvcVAMhEmjtSVn93VkK6jqU4cXAiu1FT1wpsIX3GIV4rGLVLutUGDN3LsAASKnaA0isU1iCZuxUywRZ1MgtSbF8Uo5BwpKwMMieQq6T5jJ94SPrMLENiR6RS3XDRs%2BlG0y7sgS4uEF5wH3QH89WmuAn9aM0nVIhBDc4ferSuC%2B23Vap996OPX2pFRP6iAR3cBIPW0Osc4WksV6yeeZnJC4010dYMeOtSHRTgDOljldQM%2Bha6AKI7qtT0YokPNiFHKQJDju7iu1OVdOuYdHnKio02amR9nMsAclthoSX3keOHbPM7ClCwet0zfP1cAekdkJVQI%2FnpDSHgCF9zHKI7MnVANYM64w%2B8bXI2V52YlZzEgSjtdiCor636tROREtqilnDqpfmraGWtIaa3ojx85d1rdkk5cI8YbnLo3GBN0fczGEVmTnjWjfoADsrd7%2BMSxEB7%2FlQIH84nbXG78XUrvqbxo%2Fsg9ZtKTXo95bKjFjwYIzw2yq6JE5MoALCTsb7TtFFxYIRlirh4OS1%2F1WzKcAAUjCf59HGBjqkAQ2j4E00pJAqnP4tzp2%2FaoPMxRy85lY%2FanU2%2BetH0GxxiVV4tUzO%2BMnpyOztBfXbwJeY7ZKgZTMUD4d4oZtik2yg%2BYmhsxlfcyd0vyj7BJg2IKnCym3muZegx044QlMBa89bQyZfveRAAlOFFvB8URF0iBScQ7z61ffhNe8mXrKmmPJ0GivQnDKJK3GacGImsOqbRVpzo1Y402AZiqfq5N3kO1Vn&X-Amz-Signature=479c1b3fd3f3f2086aede0702d7017d1171a3ec9f9d11c536bfa6feea6122365&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

