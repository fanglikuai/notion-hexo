---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q65T7QQK%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T160054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEsRTfupXA2ToHU%2Byl3ezRbExnkrywn%2FU9u2ZmJ3CKeLAiEA9wIcJ5kHBiJdvUXEmtjt3B%2FluK%2BzgByM8D8eWckq0vwq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDHHqeDrkt6V4e0fuASrcA9AE0RaZ4FbcaIc3r%2F3Wpp8rWMd%2Fm8%2BDRYhIgsPz8q%2F8vq%2FtjvYZr0AaTiWfpLZ2EU6o%2BtrTKVso6cgmObJYlYEwn9cL4YxdNbP6R0c%2FcdA8VEHEUyqLQTkoBCOKXH9isd%2B7GnhWcBU7kgh1ipIQ%2FyOdvHGkvjGC8uB0aZgpJb%2FJ0GUeOcddjcucA56Y%2FQivkTByr7JmiBikIl1RHCUlLGVlJ0K0GmsFrs2zs%2F4%2FVechqa3kqQZXE0IHbOKceJGX8iVbVv03QlllIrVuQDjDW8sjO%2FfyAI%2FAHXp44s16EdWTeV4rO8%2Fe1sI0DLPTWvpv1W%2B0Sv6YjLeWFbD53%2FWYEJOhQYQ3J50P86315FvPPDaKu21FWkl1%2FZJzt4%2BMNx7IoBP2MnNX1LZm9ZwjHJaCrwkNhU3i6iu64ekgJGbH0dTZLPbxhuARX6QWe055aKkX6cluUctgvhNXjLQqd6aehAH5F615BWa48SeBP7WOS6yCKywOy3tYVlE6OXLk47h6dtjITqYIKjSSdrUD1f1FJd3of6W0jIxjj1oFl8jEXSgMasphxYVMXpjOpjHIvzZUZgzy%2FsJ3cgO5hsZz09hkb5kXSlbKOsNTdyW4abjxO%2BW6ON732U1FiGbSCgVpMNj6kckGOqUB2D9c5SGdDdF2te3I3ObsGZyN3Qdkk0mjRf0Caz6h32VKIVCnVrmFnIicRUurfFumTyyAN4nUKCvIJYYabN%2BIVf8Q21VW9g%2FObInT89DbPnwv8CL%2BWct9ViBvtLv6OWJMwWyb6FbKS2e99a7U1wVcYu%2Boj1N8Iol8tA6RoFucJTTkbR%2BTNu8q0Fwa3RiHbp%2FZcDpedsCarMIeuPzDq7xWYts3G1Or&X-Amz-Signature=fcb49dd3a4a312a2684676f5e2698a6cc4a3c36b00d37c41fd9acdd42f3a9701&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

