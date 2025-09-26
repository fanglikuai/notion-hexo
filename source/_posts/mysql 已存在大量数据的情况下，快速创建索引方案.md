---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VUE5342A%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJHMEUCIHEOlYTlqnM9ryNzdGfLjW9IeZDzRtBGZaolAX8p68mjAiEA3La6qxz7qkrdUoGVeVSWArjm16MqJkRXtOTNdpWCEEUqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHsU0R6P0Qriv1oCZSrcA9MK20qPijQ04u1wlHU4dB6keJk63CLxzJ14trb%2BL07VsF%2BYBbHoDPHN3QN7Gu6SivAr9hW62g8%2BlVMU9bRByM%2FmLTMqkCLQx8Wb9d606cP5dj9NdmgY3vYznE%2BuW8227gAMPjPug41nFxUEJ%2B%2Blk4G65adzOElmbvmq17dwEKZKqO%2BFYDo99Vzp6f5yt%2F2bnbDQm9nlZymT472W5RKu6IrE%2FW8rtRx%2FcRTWEF2ApRgOFsSnrSay7R6%2B8n%2FhgablpTYmm6s4DU8AFsFKC3hWIHoDm2I%2Flj8dHSRRXGGveONRQ%2BbT2GcS%2FOIqGny1mDhZW7KmS%2FxitiFPn%2FqUxhx6AHzXBkCJ55r5KnF2SJuCNM9B0WspOBCGYzu3itru6ScXgSuv3N41XO%2FxVrXrgzh7%2BEx0Sa5%2BvsdRU8fuDvxaT%2Bjar3lIa9njpxJT1%2FZ2KyTVt0gBdB%2B6MIIU4UPAqhi0updbvpZZu9jXQxESt%2FkUyxPrUQvZPnft%2BM%2B0LeixCeCkdwAH853K%2B%2F84K0XhloDRME5JnijIbeqefhqKRzKG89ElPVQo7c5undePpNkUrBnhSbc0VUo6ujPA281C%2BTos%2FJlp2Yl276fOPxlDNjA4M%2FQA8Uav36%2FoXhTiWekVMMCf2cYGOqUBhOpVDmMIAwThsuLjc5z4kG%2F1q4tBXVOT%2FGTD%2FdtZHKxSjOVjAVczkShY9X7JioayCdF80YH5GV6PGRaMy3oJZFLhDXkbTCmwPbzDrLxkJCVclE8PN2A6zmnmIROOkkgRArdUIyrvUbPhMzv3qD5NgunHofLOBv45U99eFSctGNflomfopy0kPBH4XuErH7vvFrvO7ro9vO6gZVJ%2Ff93T2SlgxBkl&X-Amz-Signature=1783f9c0bc91ce31f2c3ebdba45bd9f3f0a908698b4c762431fc140597a90ed5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

