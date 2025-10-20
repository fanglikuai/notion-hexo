---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DPAKRQA%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T160039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJIMEYCIQCcB9cd0EKnjaupmEOIxJA4uveMLjs0uZtHs6p7SYWhTwIhAJiUp5UHhxSn4yyGxRMy9%2FUwS4upnrlnNwBwy5Lv9XiMKogECPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwH%2FRzla1Ixfle0T7Mq3AOjhXRNLq9fLq5M9XjsyV6b8JdYDitDSxva1pZov%2FQgCgZD%2Bz41%2BIaTOxgAtEELNbfJyFaspSMRU0%2Bjp7wm37OPNE1AVYtmwZix927TL%2B2KDgWRDoxrj4OsmDmmGh8TxPuiEk8vxI7rs91ZDa6Jeur8HokL4etN%2F5YjQ8RRqO0xcEqqG5z4K2NZGhFSdx8tGetf6J5cLtMNKRyp4ZJER0LhufKU%2Bqmyj5%2BBbfRh2UpAV9R5Yd800Ud1xXndRtUy4X6dmURjMTXPCmBYFC7aTwHLU4e64cjC9QtQ%2BsezJvZHRDL08IX7lxLfb8togkLr4wEJ1VuB4%2F6rqMTQ6LDfWYkbAO7lch%2BEcpzTyWTbfd%2B97kSXZARKH8QccEnvPo65DolqFfnzsWBiegdveJZ5XIAGGC4Lq4vrhaki3Z3NXQ%2FoXP%2BqTxnQXRRfeFlmg1QEKtxSd5XfBCWn9%2BVIkvSwtlVxFA%2BISDc0EbtDap6xeUcx4xxvckpSyy3rX92eZWYsaI%2BZ4DQoUVMyuHUENrEZ72ZlaeGMM7QlZmZio4MT%2BntW0QlOjHQI%2ForfqEcHqMPgQ%2FnXd7otYn7LoVihLROAHe0AdM9YqjHEIRwsORhAKPUoX1DUOWHi8pJkU4O4cjCxt9nHBjqkAV7U%2BXeb7a1nhL9ZJ%2B6HQlEeJe57m64T95A07UM6Udt56GbYe%2BDwZ9SB%2BzZR6SOAinPH%2BAccr5I2K5pfcXYS0G8I7XX39vO6ySpPyj0R8SFgufXFEUGr0qKfrx8cFH4FnnsvRejIyYq3DIOMRehGAFkppgs9uFRLQv0t79KIHOlmqz69OL3JdLZ22na4tnfGph4x%2Bi4bTicPmmI%2Fozk7kCOLy%2BaY&X-Amz-Signature=434c321175511c0f6fcc90c5c592e0ae47c296745a5c40a3cb2ef5795a483996&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

