---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GC52MUV%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T000035Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDQu8Jg%2FlXgAz%2FUoZ%2BuxPiPixWlnmw2oAKOzWwhFbyypQIgWPpONnb8ibodOqfy3WJMZkO1OvTsRUJBfMcfHVoIY4IqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDh8GXFaMbDic%2BepKCrcAxjTgYgOgEout4O6AcB8FpdsRotfZbqIgAWRp%2FPfW98q0l8yfM0JLVe%2BUImyfPkYJCBMilnaoAJMHdrRBNXtGvGmLlh0JQcPJgukkg27CdChKpKqhoAMDE%2Fpjswij5TPmXp%2B6VLdQffIG%2B%2Fz7Nok27SmzlF7F6NwLEZ33f0ZBZNDeDEliMbnVHbWz85V7TX8v6ZExQjIUWEP7Y7BbDxLM17KN6A37k2W4u%2BffNio4oHgrmHzVYMwvX6bT4cFfbEOAYvBreQYh9Ps%2By%2BFx5p7y4dekj0esVsHJPuDw54ZEQxH2nsJz4T2J0UzAz18TzaEq4iveUVPTCcK%2Bmyc7LwYZfBcrYhoU%2BFa0O4DI7oTdaQX9gUUOi1CaHUHB7yn%2Bl9vreUAWVlwslBDkSoqZP5AgZQ9OZemB9VnUI3y4aYbUEt2eqrkPyqDmGvkKZtWC7TaXNbhvpbgvqd7cVNkMIGk9TVaL1qX5heh5zgq8sKK3Bm51uqcMhH21jprXRtTdfAhhA130KLjHNqBgCkdT5qsM0lkF6XIzCXM%2F6XOTyLRl4tfZXUT2%2FmyE%2BFJpadL66%2FOvIhNVb1QYrxCOKzJR3xo1yD6cYcfZcrzV11muvequqVsxtdu3v5I5aWDMcLbMIb%2Fi8cGOqUBxizIpSa3YNP3n7dqJVV7xTWdOPcGmBWA%2FZHAPFRARDROaZvx%2Fo%2FfXUH87LwZZmyxwp7qSAVXWTpG6zBNv7p67UCBENTzL78INTEGQH73vaZFGd22nRdG57%2FDD12zxYxDXHH1uJeNmft4z%2FcvFf9i6O5Hq0jg3rBarYJVBen3P0jKK4ixSaztQLQanWG5d8MJqeu4YLi5N9ZT9juLJH%2FoS4%2B8AFBJ&X-Amz-Signature=8b59e042f01786bd78b47089942894b0ecdec4ba48d78e15fa6fcda19303c416&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

