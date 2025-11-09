---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VNXKRXJY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJHMEUCIQDJpcMH3I9EdyX0d2E8dPmK5FCbE8ZS8CmhVO3MEYyu1QIgBFA2fLorvOVqWodaBH0CwsS4mPZCJ9%2BxkapyzCIGIjgqiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHNxOhuOHWCtrfSlWSrcAzpYjmvRCIzeQrH7BUReOzSZnbf8gdmbYCxf1ul6wq%2BmOSY%2FN4aFeCqBQP90MBIPPt%2BZ8V5NYSMtajOxq1McO4HiNROUeECFD%2BMjygtUksjEfa1rv%2BzHfrfKsCr3f%2FO0%2FRLhtk9vt9BrlZQ1eBpW8WBZVf0gYXIG1j4axRwcdlvzEEVBL5n0V5NsmCghThCHYuFf4HyOtkezsQ3xbuzC0LoRJ8vdScJusb5zc%2BaOezV8fvdhZzsB%2FtZH9BTqK0%2B3xlFYFX45W4nsVF2rmphjJldeOwSq67jB1kj0HAdnyuly98KHbs7iDHjKj0UNWew8XkREQ156xr2ObkgQk3L0lA%2FldIaz3vWxq0JChFUM6DqWu8FlI7Ub6Vb3yZBucPLUc5ijAoRI3ogJJkGrLiCfnZ9FaEEmCEflAP3%2FYxe%2BfQAs%2BAgg%2BKYpf4bQx4l0SqjH6cI7LnD1GaiQUfdAP8YgapawNo%2BZVbx8g2RziaTFf5Tmu7XFJIPsRVIf2bcVRftup4fPfUY%2F9yhz18oxHU%2FBPCv9KTa6u1XoS%2Foy0NtUy6RcaSdZPoePKOzurspSUCumzUv9fO31zxWwM7JV%2FbJNSoOqLhgWohlDmOVCrmuE5cLD452QgOIs7IZWWVgSMM%2BAw8gGOqUBU7fdCaUgm%2Fv4s4AVLd5iMIrghHf8xVjRfWl2g5pBB3dxYR8ByAIh9OzqXbQDJcv1bwZGyHVWM%2BqnBnHai4PczZQnPezjYtivLzfZ4W7tXPn6HXUBQYx4pFh6cYZAMwkAXOGsft5ImWYKAWg8lNmViuk61%2BQoCrWSd2EcStYKv2VtwaVr9iDBdjgt5KK%2F2p6w76%2Bf3Hp0m79E%2FjtJHQlCQNxny4xP&X-Amz-Signature=3df63be039650cdcd1043118fa33802fb717e9ee50387e86bc8bc2c4c3f4a870&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

