---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664562JJEO%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAgp79sQIHKp9T8El4BnVQh4PxfK0OimXCbplFdBD79nAiEAkJmjVHjI30sLuP5NepCdT83BksXbIK9hg8042NucTgMq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDIAjZyDZh6aN7sUMlSrcAwA0NZaNpvLbiKzqGA%2FAL6V%2BMhq8Ngpvx97v2QgqpJA15zlW2XaPcV8qXW6STlAuqbwgrD0T4nzuAoBpI49pZko7xA1Vj9cAI2YBKoT6CHrB6f7XsbO05%2FatPqEhKxPPXkmIrdE1VVHuLsc%2BI0iePh%2Fig078VmAv%2Fz0qwTyDJX0oVhUe9Ax3uWMBL0FojgM57SA8%2F5o2SlcIMULR%2BE2vkklRg1GpWj7F40rbe73W%2F59fFZvI8X6QgixOWM1nCnXfm%2FRxl6dpoNQ1oDG5qoBuXVE1hmTP87sV9lasZ%2B%2B620q12gwMqa6jyXLxejNVajjSbLBvSw9X1uwijZxx3bG63IYaoJDWFaa4uYtbDN1e3QS3JuAZ9kOkZlVTL2yD1G5UIHn6hL8%2B9QRvCLxEdlz2DMIKup%2BqmDUPox8rcar4yCo9UkYw7USRSfzVL0TDsonsHtcbg2Pmx4DFiEnLkEBCMbjB%2B5FurwP9lIPu9CIzg95ThlFvzzdkvjNM5VrwAnpKH0%2FSNntzGrmX4ffa2jP4%2FPVsBYJR%2FtFQq4Hgw1QZW%2B3h8dL4%2Bbh%2B8WMoA5Hwm6Bea9SOTt%2Bv8TcIR26%2FvjJXGQa0gMf15xjtuvG7xyxKJhQ0RNEQjCnNQWw%2FkXMTMLaT%2B8YGOqUB%2FTDoZ6xltUaal%2BZB1DanxY1rvnIpwSwdy57VnDte%2FAlKYheVUGnQIZwujMeraV706dXHA0hIGbQsbWFrYHUQxlkcgRGbgmVZu%2BpxcVy89wAZBxlF9zFQL6dzFg9CACnLWzzx9Gz7yGBzbei3mzxCElYgXlX881o4NXo0Q15JLmKUovIer2DRRdzTRfLg2c2BBlss9b%2FRoKzsIupjpoPGFkH3qEO9&X-Amz-Signature=b75d127f404586ae6ec18c074f75148b01160ca7e8f6e1bf5ff75eb90d6a4a0c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

