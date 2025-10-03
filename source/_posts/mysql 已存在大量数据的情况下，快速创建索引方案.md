---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TEP5YOQ%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDst0M%2FwsLZxb6nEr5UEHzJYL4hvdQ1d%2BJQzUHtyLbffAIhAIehHpFPM5mHUiwsvhl7qCLbSOnru4L2LJUUoTHbwk4rKv8DCD4QABoMNjM3NDIzMTgzODA1IgyTpG%2BeYXrXryms7qIq3ANJbNg2IyhcNXu6rFkDsfPaadJ1%2FY6ZpcMaV3kwNbgPARtWOiNDjEKqBUxdpTQciCWbdL%2BTxlsILafnYaCBdEIjSumQ%2BuCOQVRW1HVNqiA7%2BF8u3gl64EW9ukbm8oI8o5Vt3JlYt%2Bg%2F7Xb7q%2BMdpvzeLmF71ritO%2FmWAl3BXM0mMNFi9QFzMRtZsyBWZeMZrc2lVzj9LaAfEOHlsvFT5yj4Zl9N6h3ORavcIy%2FDnYfXWlkPGTvhC9OJIz0zlL1h%2BNiJXdJt%2BNMcIzMIo0rPS9WC6dhAOzFpCGtJld9Qqprvv3tlXUGOAVt32txguEkuYnxmlblwgyE85HQsCxtyv2Y5ThlXXvSmVyXgFwjjmFayCnGHM68rvOQ6M7EaNTUziGd9Z942A4zFCQ%2BxYx9pmRV3JdG38GLqBnznm88lRiXHVj8ueIGA0Hr%2BVTq%2FJIlHabw99gbuR88tbTuvVOIhpkCOe1u5Or5dS3byyKhPA6SBomW4TKIyXaAp9%2BwvSJVK4pu%2B1AAv3%2BleCHa8k%2Bzu4jmBmuPGhP358SaERsOM%2FDJrvBOTxHkFuFQXZsmNKPnZNctu98SICnYlu%2FcvTG%2BfFa0%2FSWcSvE%2FcxpZLCIcWvd6UES9I1%2BjZuRktzzvyMjCXrP3GBjqkAX31CzigmhpWZC7xiJdTviyZwT3qlWz27FFCT9ROA0pvI7VQYJ%2Bip38DcSE3VVtcNTKO%2BXqlkTj15VockyOJrqibKDYa%2FVgHqeReJHvqvXjLyG4p59pqpigbhVWGkC496SmdWlJ%2BBYj4W9DiIqj%2FqHQsUYE1vOuo4YbSH1SS4EmPNlYXppTz3eka9zetmVuWlICs3KpIVPatWbPAEkEyvRQOq75m&X-Amz-Signature=da51d5f32ecd6ab01558a08e00183dba891112707c0693cd07f00c6965619f7f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

