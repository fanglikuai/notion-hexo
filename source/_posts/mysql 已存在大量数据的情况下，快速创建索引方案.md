---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R6KHIN7P%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDqugIpWJwb8OmuHLXLDXf9CwqX3GOmejYJP%2BUk1k2FJAiBkIMyDuYbJ1QxiV6LFKqA30Am7hDAlYCYLu2LrcXJ5hyr%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMtOmmmcdqbDCw%2BQrEKtwDL12RhnKlL0Zy%2B7urnktujJyidVei6W6VXZ7IVHq1klalseCQs2RGyI3qHnBxBqtJBHVtAoDUfVQWZq2GEB40O0z5frN%2Bsl9gEADVzXCjHsv4f8HSbWfBVoLZnOgBA%2BtiqTM18llIO5dCL7fUEMXTTStZMIbVuEaLZfx1UGcpwmIrmBSjVW91ztIu0VfL4A9gC4F3bZChewWcQfk8WLZp9uNmYvpvEOgH%2FGyutScYYsoh8k9KGC6El0LOxUV5OXLn1WGmasOOQI0JjQjm6CEmFBWa0OF4bRasdbixDclI6tnmgCcB611romnl1pg423N4bRxcnFhfKJ%2FLFzd7y8nQ54S%2FKY9%2FZzabAH%2FhEAwJH0jW4pPXftElFwhwLhmywetFcIHwxMgyY0565344ugs1ZFfby7zSAs09FOvjtmN5yDjUi9e%2BcHiilGFmHraiy24YNvTQcSp4H%2F7%2BRp91hJkR48%2BPbAkNqQMfdpRE4Pxp9m7DsPqdDiB9h%2FlTNM28wE09tIPh%2B1O1hV%2BNYdinELT%2B3ib%2FU1PCSphIZsZLvuejqPhjPAMJR%2FoeNfs54%2BAkRZDknUUHU1jH3wS%2Fl5BX6ZGZ6zjUwS%2B4G8mA%2B2RTC9nuIIlEKLYjybneKHn5tIowgpCFxwY6pgFMoCOgOXBUclaLg%2FqIzhRtxUtYUoh7npIUBgDiI%2BrQBULSnmZSObR%2B5w8ud0FKBx0Omzf3iPNxtDR3rF%2FDhtIGQHxdEB8Awhag8%2BjXxDNP3IdmWpCI8TDys19mBKla38%2FYEwKfhaGneJxaR3jAOKxvByfcbWKtQNlWiK%2B1p96c5xh2iQCyz0n9jar%2FnEkF%2FznOfrWLSC4n0ugx6yfgiS0IWGSv2iEw&X-Amz-Signature=fe8643942319c4ecf0d2b1a14a8a8b59a06b81e41c11ea39b2261d58c1e968a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

