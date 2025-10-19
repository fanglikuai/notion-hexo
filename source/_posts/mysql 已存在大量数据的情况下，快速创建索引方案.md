---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X5HQ2SCA%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIE5PWwUeyezTY1qEO8hvl5AwDt2lThh5NgbDWIQgc%2FiFAiAV9AQ8StsUVHZnwdusRn%2FqP6RYJqW4mfufPPOTFSBQayqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMq8MxMNs%2BcJYl13RRKtwDeSVsnYqo6wLNZjL0T8NQUaRe5tdNXG%2BQH91rh%2FkEAmV0pyXQ%2F4eaa9h1AZQgiSEqNUyK0AttOYBjVAu7cHHXAFmapwCYGi05eIfKbI9Ng4m7eY357q7UPJaJ3675zt%2FJspjYRNEgDwLTOuiZ6aigzhQoVAXytaV0ywtvDpjAxUmgC0ozOzzTetZCY5EAzs%2BEFGoMradZMxNsA%2F4XboUus2NUzmWtehmL1VhGC%2FYyJQDLsMGVPxmAiIbdEQr8%2F1TVYYRsym4zM914X5vrXyVAe82VcEhJ2yVnxOyUExrSTBLzQjw6MsIVS9KU8qMXRZqNsaSzHrZJi0XUe%2F5HNlkRPxEBeFSu8%2Bgb9ZILFmq%2FD4%2FRMRvIQGcylRKtWbxnTzf4OfD%2Btlwsx1E9OoAti%2BSulq9vtNyW37gx8LIR0jgDTZdxGdg5FgsbGqUZH9Whij5UYU6pFLWm2doFYOoY3aXHRjKzICx2NyBQkWACZYaSaslfibYh7n3gz7i85bVsTkDa9QlE1ljbMDrd1gtOOHEhX5TopMykXO7D1b%2FV1gOE%2B%2B0FSXQlknyZvH%2B%2FrY7ce4klNuVHZth6%2B9Ecr7zWlEou3KDcty34f6N9yPYekuY1tPEZ1DdOGbv22BmmOi8wheXQxwY6pgGibbSmXrCacGGPhTg9o%2BLotmFu%2ByS9%2BAWnmOHjwrAH39HzFy4uucqFFUOw6%2F%2BAOMVX9whrxg%2B8i62iDMH%2BLlMfzctQrIiMiX%2FEj3A0zcpNF%2BsTbI1zu5sV79bza7n7kVnqTJ33MP6RXbf5zGhGAy9v9S1yFQNzGLtUfQD6%2BoOpTA%2FcwsXKhmagvyr7Vx%2F%2BwbEVPU3E19mc4pqqUF5y1G2w%2FACHcRjT&X-Amz-Signature=a6ae46d380bedf07b592f34c2226fda35e1190b07c93b27ad211fa55265cb79c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:56:00'
index_img: /images/c34f92fd7edfbc072452166489949590.png
banner_img: /images/c34f92fd7edfbc072452166489949590.png
---

# 原因


最开始设计表的时候，没有设计好索引，等数据量多了，查询效率慢了，想再次简历索引。但 数据量巨大，一创建索引，数据库直接崩溃


# 解决一（慢 但是不影响系统使用

1. 设置mysql文件导出权限
2. 导出文件为txt文件
3. 创建一张临时表，与原来的表结构一样 `create table text_assets like network_assets_blend`
4. 导入数据到临时表
5. 导入完成之后，将原来的表改为其他表名，作为备份，将原来的临时表改为真正的表名。

# 解决二（速度快，但是影响系统使用

1. 直接备份数据，导出sql文件，（这一步几分钟
2. 截断表（就是清空数据保留结构
3. 建立索引
4. **将sql文件中的删除表结构和新建表结构语句进行删除（重要）**
5. 导入sql备份文件

# 解决三（保守一点


就是方案2的改版，额外创建出一个临时表来存储数据。

