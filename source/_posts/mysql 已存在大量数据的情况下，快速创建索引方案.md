---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ANXRON4%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCuXfc3%2F3Aar%2FtoT9nIhpJElIF8JpKOcDhHVEyB%2FHwrUQIgZFtWA9Ix06ajo9Jdz5AHLDJbliwdbRMa%2B%2FKjknr4G6UqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC9lFG9waWlwksVxUSrcAwhQJr%2BV%2BzY7b73vfO82FZ4RgvcW7fZJIZ%2B%2BjpaaluxNTbgqvXHdEH4waNK0baB4vJuSGR3oOji3w13%2BM26mOkJ9GoZkUtNDCcjKujDRFBAQalYLhnw7ESgmTZGGoAioiAeiIT%2FJdPKCUfqgoxTdSV9Buh6qqqYAQZ5jcnowztVsW7tl2I3Zu6FmMHT7gvO2V1sHO6RFeF5n3saUG75fbY6LGrfPbpBtRJQ9YMqQuU1wxMpSNa4AIfBmJxtOIfjUTt6CqJyRXPl%2BLhNtYifgMtSkY2z49XgHEc8z2qDJSyzSd4TBWuqQu2hiZVQx1L0e5aoSdwghzxXjlEqWXmj%2FNfxA4rhyKknmpCjquAWTs3R1aIui%2Bu5yvENh74bTDNRIl8a1FiWega5wuuc0OjweLjoEa1cYL0c02paL9NgvNEuRD1B%2BNpGZAXydNN8NOzEdKNa202HlKS33i4NjwAo8W3J7VdQeWzvaGnrQtuyP4c%2FwTonbFYnJjkeXYecg4%2FEd4%2F52llG6wdzEiYaP9jsqeO9fm0dKLt%2BRmgKJSAhOsiJGYfFk%2B1Euj1p4W3RUddlBnqUoxb1VEnaj%2FZu5mmSVgYMSmnWnOBSYtDi%2F3kwru%2FGxUtK%2BZWH1rvvBvaMtMMz%2BvcYGOqUBqWBcPHGmdbHsEkBchDqTLEpMPfUDLxe52Be27uv3m1vKa5HSBvJXDUZ7GbJ7OKyB%2BShl3qE%2By3VAJYRXLQtHNDn%2BSuairgJc5nNBfF712%2Fn1B73b37gUAriSgBLOlEQKSgX38ErD2D1ufBNRq5Lx7ugMPAs8Hn7gjoIX7Md1A2pZjq%2F6nNckgFoj93tGT0a5beYEEnNMNST%2BW7oc9DSV1pXJCw94&X-Amz-Signature=298a7bd0c71291954273e013f52cd0ccb7884c59ece863ff105e930f7b1d60cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

