---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X5HQ2SCA%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIE5PWwUeyezTY1qEO8hvl5AwDt2lThh5NgbDWIQgc%2FiFAiAV9AQ8StsUVHZnwdusRn%2FqP6RYJqW4mfufPPOTFSBQayqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMq8MxMNs%2BcJYl13RRKtwDeSVsnYqo6wLNZjL0T8NQUaRe5tdNXG%2BQH91rh%2FkEAmV0pyXQ%2F4eaa9h1AZQgiSEqNUyK0AttOYBjVAu7cHHXAFmapwCYGi05eIfKbI9Ng4m7eY357q7UPJaJ3675zt%2FJspjYRNEgDwLTOuiZ6aigzhQoVAXytaV0ywtvDpjAxUmgC0ozOzzTetZCY5EAzs%2BEFGoMradZMxNsA%2F4XboUus2NUzmWtehmL1VhGC%2FYyJQDLsMGVPxmAiIbdEQr8%2F1TVYYRsym4zM914X5vrXyVAe82VcEhJ2yVnxOyUExrSTBLzQjw6MsIVS9KU8qMXRZqNsaSzHrZJi0XUe%2F5HNlkRPxEBeFSu8%2Bgb9ZILFmq%2FD4%2FRMRvIQGcylRKtWbxnTzf4OfD%2Btlwsx1E9OoAti%2BSulq9vtNyW37gx8LIR0jgDTZdxGdg5FgsbGqUZH9Whij5UYU6pFLWm2doFYOoY3aXHRjKzICx2NyBQkWACZYaSaslfibYh7n3gz7i85bVsTkDa9QlE1ljbMDrd1gtOOHEhX5TopMykXO7D1b%2FV1gOE%2B%2B0FSXQlknyZvH%2B%2FrY7ce4klNuVHZth6%2B9Ecr7zWlEou3KDcty34f6N9yPYekuY1tPEZ1DdOGbv22BmmOi8wheXQxwY6pgGibbSmXrCacGGPhTg9o%2BLotmFu%2ByS9%2BAWnmOHjwrAH39HzFy4uucqFFUOw6%2F%2BAOMVX9whrxg%2B8i62iDMH%2BLlMfzctQrIiMiX%2FEj3A0zcpNF%2BsTbI1zu5sV79bza7n7kVnqTJ33MP6RXbf5zGhGAy9v9S1yFQNzGLtUfQD6%2BoOpTA%2FcwsXKhmagvyr7Vx%2F%2BwbEVPU3E19mc4pqqUF5y1G2w%2FACHcRjT&X-Amz-Signature=0b2cd5ea02f8ba7ab4ae1017e58e137abb664f63a13591c7dd82db23c8884da1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

