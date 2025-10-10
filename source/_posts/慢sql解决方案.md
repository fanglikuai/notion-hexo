---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QTR4YFJ2%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQCpLyL1%2FV0iUD03ftgSvYCiweuLh0WUN1RiLvM4yH2pawIgHpNN4lWKXobcacB4zOWddtTTBZUceXu4%2FTDY0aaYxcMqiAQI8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKbbJe8zykNIIDnboSrcA7p7cMEz3kbBAT1duCby3BIsWH3v%2BbMrsJjEwgLd%2BSJQuQ%2BksVQHFWT6zFVwUbSBIY9RbNqz7KratHpKL4%2BAGj5gEO%2B0Q%2FsXzuF4%2BA2mkBnD%2BQJsH47u3c88hcmWab7PXfDXzZUwPFyFPoglBHxG0CIUe5XqMzEI5tvtDGMk%2FD6d7o2y0Ao1atM3%2BotI4jY98JVfAuFeyP43s7ZmFi9KUT9%2FAXzVZ%2BNY1yXREj0k%2BC6VQvOvZKr4x5OdzYXPnJ%2FsDb%2BqYHO5jl3SD6P8NqBeV1luwXeIwZEf0bwt2e4y60dQ%2FI1qE0G20OZrcecx0GbsQS9s436tc36unifXCkZqGO7%2FxFlQkM8Wwi%2FehJvph7qFKMI8nwsMm%2F5u8otTTY7HxrmpvJCmKro2K1BUqmMIl%2B1DoOar3a61%2BtYWf9M1LWo5%2Fb%2BlYeAQIKhXpEHqVsfc14noU%2FZcYNbCx1pB3Tf7CuaaDBmnaEtdBNlJOw8J5iO6me7N301AwlTU2XxTHXS9zAukrhG0S93nvrv2peHJHO3O6D4sBdqMHTrLMQBzRrvfgFyXZ4%2FoObTA%2FGCFa1LSf9wa96RDB%2FRSvybTKhSAxzjMdCJ2KEs4aMIIhy7il193TGZ491Ca00MWe3JPMOSdpccGOqUBHNFyt1lg%2FP2gp5tBIVB688yQ9nAoxZEBeYO2BW7Abe2bK7tttARsmgfmKFbatlEXF3o4zupHUuaNPFlGM%2F%2BD0%2BO0aKFSC7XSBoitkMigIHSqsc6P3EDVJ%2BQc3can6OuOGUQbRHNyiVFRda7crjYZOMTnhKqOEdv7UR%2BUPwYV9IEnbYqjp3Vmx6x8Hj8gAWF%2Bmx2uQwQkFMGNbUa%2FWMAjSazD7ai4&X-Amz-Signature=c808d0802c89b4b84bf2efef768d36d9000d134afc589e00e93fdae5f4a794d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

