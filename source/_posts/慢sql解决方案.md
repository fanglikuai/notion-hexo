---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UESUJ42O%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDKZide%2FnRARIJz4MVmD5AKyyMdahIgIcE4GIzOg%2FiJrwIgLDGHdn0uXuu1gKQh4HBkvHvxQhfz2iRB8hRTdd9mXvkqiAQIqf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKOsy3hXQEzJY6F7myrcA8hk1rgJh%2FYX7nqD8aSa%2Fu4rrzOIPZxkjF%2BEHhH9P3zjBhv7%2B0eCwI4F56T%2BkhWX%2FGb9tyGpfXuioPOGPA4MOhXgNMeSfbYtYv2TJn724EmNOGP72ZMQoQqkv0UzhoE7tb%2B%2F1aug30cgLY5RxWOW%2FwxvAcNg3nO4VrraQB9DH2fvX%2FIp76LrVQmI7nOtZCV%2F7TIhlqXpdHpG3AbvhfYhrBZEmmwrs1KMRnYnxvJYyzpNVzvsVMhsOHHzd5XkXl2tGNRo9MrXz8WMc8%2BFTFO3Xg0ymi6E5ehahoyzXSDRBvBfzQbFN9x%2FfNe7FZS89RbI3JbQJ4254fXY3cKXspWtiVdTVA4BsKEH5XsoJgY%2BpkUkX%2Fzb62Zy6bUc3FysFWzogpommoruuDUyP7BKHcwq4NsVXoVyKBaTFhgfNrFlbwebYv9CZexmxa1KZ6kyGE8y9OLf9mhGqaCERlzlSK6kD7f2fHiIoMFoxppOM0Y%2B%2BjOva%2BsANdc2iOkQhk1ECK25e7Al0KVxUC%2FAJxq6sKLNx%2BRVotc23OE%2FFqcOTAs7UPyZdLaEnRTX5w5Uzl7UH84MvNKFg6ruMCNFr4%2B0rHqFI2NoqhS6VJoW%2Fs41TnRo7F14165%2FvlKn2g0zq8O2MLGi%2FscGOqUBlsl8zKPzFZcuDwJy1Po82CyDFFi0Qu8fN2pzVCBIdJ87Gluvk32XYZeEAnCT%2Fnfd%2FSV%2F2VxONMn3JKcj9V2rQqkrT9H0%2FjmS0Oi5BC3ffj6gnTf1SDDVn%2F9p3sGujjDSjlgPSHixyBlPjUlLWKjJwqDBr0Fk1%2FDMr4j7E%2BNRx0hr9rbLV6%2B0xnkYatwnEFk4p1Lzxfsn4S%2B9klXjv1%2B6HVF7a6yK&X-Amz-Signature=e65268e08bb212cf672f01cc5938f0cf272aaf8f5565cae023f94eba6287a728&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

