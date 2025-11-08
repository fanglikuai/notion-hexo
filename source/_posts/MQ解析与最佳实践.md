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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c9835214-316f-4bc0-80b9-279807294da1/934905.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHOLJQ2L%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T190042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJHMEUCID%2BuNytqZkqKjR9WDxU4tyvxy5WfYD0s5ZD3Kj4hFrlKAiEAy2zKAYe46DkIgdK3poT6cwxlBg%2Fxweoi9jy7blKN4SIqiAQI3P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOdj5PbGqoRjJtn6fSrcA0I%2BeVSxCZLGbOkzcr0ec5IClVOoWrwSpqKNw%2BLcMkLw%2B7D3TQL47qzOl8ZMGm3DT1xtjytBBEPBhoInpCIF7IXUudgyT8deChw8xYlN%2BH9anDZXsD6DCpg0Drwc2lghnUtOmMJsN3HHPq5cM4bnK9kQYmvVhcN0VREdS5VMMxDX2foz72BNaGCMhu1bpOBQ6yUvqCh2eqeLCk6R9BOer1HMSui9jOj8zRwBvALbBb10YSCgWy%2FdCfznnPNQlCYQrxAUOVDaVWYMk3%2F9jbMBLv5q%2FAO%2ByQgRaeW8gcvTYPtBB5goUKySTnHbuBf4JbNrgux%2BVKDCYZSsSUQqP6RAASxr%2BMxpD9OzpXnvy0aMoLkxt4QRGGcyYMavpIpiSCTVFVRVfMzRd6LLi53%2Fsi1BW%2FHvym%2FiGA9spILqE1evTOvWTz6wb6Osz9e5USwohfrzdV1LTNjHPwBV4U2NUwDStVPLnAsABOelZzBoxird3YIUwh5dq0y3Fld7oQCHgt5TrMJhTpVH89HrAQYnFKGn5IUBLcPKKDgC9b2NGVRN7JHE0NuCzyV5EnbhSZz6vaEfk%2F0sc4QYIw7SI4lGk%2FQmbd6xHQU%2FzMn00u7Xg9G%2Bda%2BQl4Hu6mZ7ZiKXOk5TMJGavsgGOqUBxbtGNiAkqYI9MSCBPvzm0MsJCQIziFhXxUkTfEiuH%2BpY5NE%2FYHCwMhhVKYsgo8sse%2FOmk53gMn8rfVzdP5gUI0zQcmkjWOUhUGf72lVF0kxD7S1svbXjJsys9mz682VFjgjFvYJgNbvdbaS5IjrjUBBgaDEIMd1gaAZwaN0vHpaNkbybRFGpATSo%2Bhqjlxrw2%2BoC949MYE5prcxhQjTklGFWvUKO&X-Amz-Signature=f7ffc52efd326ec44189da2a6d83a79d32e2e8c3265ca35d4296b75ceaf50202&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

