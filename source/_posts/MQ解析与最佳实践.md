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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666RY6W4D%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJIMEYCIQDDJLx3%2B%2FdiVYDC8yYA4dl2reXiCX0pZSb5vgkm0DcaCgIhAMSENRoTI1t6oAt9lJWL%2BNgLv96eMIUWx%2FdiDBY95cKEKogECO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy3jAOKA5g37tOwSD4q3AOLrWd9uqrYE%2FDKZvWg4NMXrKZNtobHraGeLe5YuxKKBN%2FHsZRdRO2pWMOjoRdqI3Nj00I41Q94HJ2QwyUwdbL9%2BOlw%2F9LI0sMJ8o%2FAOB58y%2F%2FZ37yAm%2BDfVHUu%2FkxFeXxxiOoR%2BrNDGMORXf5ICEekV6SvOScrbWBdaUiVcOxIB3yoqd9ZEglW60Z%2FG1thA6Cl5gBywhmjCXYfs7cY%2F0xcqQyI86B%2Ffd9y4EozyVIAJ962exhQpXy3plKFoRgy5juJW%2FvdtjyenzOOeUG28h7UefpLkAPG5LjXEa1JlUTr%2B4pYI7VjVXU5NdZYlWKGkgiZg0X6IrIWScH5qPLKbfErPJW9V%2BnPO8QVIrISt8Dvw%2F4nRkZhLTwIFrKFTtN012PGhQI6EPB27dDgwvNHMsiumbC1vfina%2FDel4lVoMqjhd%2BPyOiIKsHvXhkqmVmluIzvIwpIy9XRhdmyXd9mM0hDz7i1OMzt2469NqV7dBQfqHq%2B23BRoFDG1TAqBJoG2q5deb1Gpud3QQQDmhvyN4e41GQ%2BiJU3lmNPXkL68hOqsJCwZTEMj%2FF%2BgemlYhMeOndITw6vjJimDRQOkwdmdtVFQrOWWFWPxr5Md2r80pRyteidoAV%2BhSegKbj%2BpDC%2FmfrIBjqkAflG7iNGnTF3iF004V2XEAzZEjoiSI5g0qcahMX7Y4QlsbUp%2BKmozBBJp92xPLrJp5cwleWAKAuCoSNPPEfsYBfA3DI%2BPVBX5smHCtocftr4P6ZLDKxHG586WoLN9ZVyQTAFvl2r1pFjdYyl6DA9EulOwEuX%2F4v9kh9jhhHhBntYMGSdB7EpbT7Oxyjkt8A0TEaJWwaQstjoavxI04Hi1oRUaUuI&X-Amz-Signature=86234f71a7982454164d042e9a95a3c18dbd3cb4db23d4805e7cc08ee3df7f65&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

