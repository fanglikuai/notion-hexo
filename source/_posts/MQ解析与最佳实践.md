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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XM7FVJI3%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T140048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEhSOJWSW%2BWMvDtpQrusplmv1F0NFFygmad1zrnGlWbfAiA8nWP5n115MHRnjiwkTjUXizngKVxFMfATUvzGO3dVdCqIBAiH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYux0JB0F%2BsJk%2BZSiKtwDkQK53byeklAjFf4GCSJ3xk5fXKP2vviEP01Ork86Ula3uydfcc%2FbrFNvXdjP4bbcPXHKdi5x4Rw6R8rfykNVv3Wv4%2Fl2vvyEa7vfUOOXpyWFQ2YjSYPn8nzdauptIVTpJXhEoHk%2F0Vibs8gUZ1tyA%2BcqTpwXUX2Qu%2FpvMNbLqIh7Y2OU89jgbkmH8BVV4ux%2BFYyjtatp%2BFW3pOcmG901EeTPiHUrGr1nCaFlP%2Fd7lXk15fskAAwd2zBeIS1abaBwZGOCxcdyXthYEZaMwOGvq%2FGLfGfkYXHsHKZYxUg7krgRnHquiyuU9cBY7U0q5T0Ko3v1k8%2FOVD9rv5rbhxX%2BDltfpaiD5ezhUW9TqcAvv2OOBvVmKdXrG7YSep8Zem8ZIv53mWFaNGoV75G09WhH1qPzEgZrvfhHcmoilKbbz26ifMHhHvIkSghYH06gvAUfBgauq35Ol3kH4K8S8jhBdrYgoSf7XfIRN4AWzPJSVV6wfGhch3GqkXAWxuv6ZSG06GgGuKkW4XEHo5WCTpkjJmJ1rtMyuL%2F0Bbew5q%2B4Ep5I7M13FXiEEVCkTICWF3uZhJQd6XWV%2FnaF2HR1pbLtiYk%2Bq%2FmspezgF%2B3wbteoTXOwThkL%2BhnN%2BEYYggAwy4qcyQY6pgEK%2B6peOfXlzbJ0cV55tLTC2%2FbbBgB4C8kTqEz%2Fx9XxKKlIr0qfSOZ5%2FQERzou2iwUp%2F%2B27FjiRiaf%2BgVJ5UznqiJjyLqS4PhEtVEcEsuUNJSOvXziP2O23O1oZ4cXFJWCcfpu2TzzoGlglBRr8znf2k9z9O37%2F7jTBqV378LS%2BFHZV%2FZlgQuVdzitz2DPw2zMzM0ZVGI3zShqw4qeHUuQZkhhTyF41&X-Amz-Signature=c2cfb44745ff549d5eca3ac139b5067b4b258bb69b4bc6b6514b69517c9ef7cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

