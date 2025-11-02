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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZCADVYM%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T080038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQDTgI3%2FBDc6sgkz%2BEjIrTDH2pOD6SJJIUX0A1RvoXQ%2BHAIgaEAc0JpCyarsHSoYebcd2oiezbvXmvWTTgYgqOjPjBgq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDNmkEvtiHjquWOm0XyrcAzVBXqTOUuqsPkWG2URwPCSWxuWArgdn2YpIJaRJ0w4MAlrh02qtO132wu9j2zzS1pQIKOb4LYZBolCkReAZKcMb8unjzkd2MWfwYkO6XjY1bn8pAO0oeFlz36LBMpGLCLIk9P5Go2JuKyb7Aoi%2FhYCiAKyzja5b8HZjFPDPS%2FNdOykja2J400uoE1HERsF6x37Ju%2BL9jtIM2zaqEgXJKDjZmW2rVYXE03dp864oapM8kyYmTTUj72QecQLGb9opYD%2FOtFMGGhdGU%2BqUY8X4j763prWmBCECbzNpQo1xv82gpPUVrkHAE4gGRogYATJfELR%2Fk4O0vK6NBt%2FNwu4E0xHHVDpgQa7NGlrG6iFUV8rQGsf98AaTopxp3m8RQjHBvKqCWhK%2FT25VusRJ6iq87FoyYvuLEkYdJ%2Br4OnIPqGplcG4989NDl16xIw3vGGowEo8IGFf2fJEqI%2B8Olf%2FPukYuJEraQGsaHJemhKVCwrdGrrOaK%2FsejzhMQOAqXQ7rYA%2F3STvjP4rlyJ5xhETa5ULEqWhdQAWQCQLUO%2BmY1yfgpYp145PYqLOKyhSq5YbWkkU9WnzF%2BqwmkcHXnEzjj1rwX8IEULzQrdenecm6wEEGogWxJju%2BGc%2FAGk8HMM3Um8gGOqUB2EIP6cvrVFCRkaH5OPQJvaqRhx5NYwkrNKhmCGvhDCozknT4sDfYIvb6KgM5kEhacwwXfhjcKB6M8dNsJeUk2dBILmbgkIgE5w%2B39ZTfsA2JXxZ8gyTWjEvai%2FrunrbXGmLn6iiPPxVAE2dkNkt%2BAevJDVDgfTWZLir4rNYhww4s3SB%2FzG7cCeL9XcRzRZz1BF96rTap0hC6sGs%2BJMDIkhcl1qSs&X-Amz-Signature=c45498898915d7837cfe7469ba8252e525eae2fc08f3276262be3177df4cd239&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

