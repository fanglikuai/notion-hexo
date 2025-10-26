---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XAVU43BX%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T050055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAU9M%2Fyt9wJ%2BkRGyD4wphAIICg8Rh0XTyjPzGjpu1YlPAiEAsP%2B9Cece%2FXcqWQQ4S7ycCu63GIA0C%2FoqPIBBkGYbSn0qiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGEWiH6lg5FjMpGpfSrcAzuDhYCTQIf9kxeespYdk4gM%2BOqVoaSrSNb6RVn8p1kWz81VZ2YaDkv2lDLWDLjjfBRc93t%2FvjFIi%2B%2BMtOEVewazfJ%2BFUudRZ6Z56TQfmJzOKWdswomJrYsdcBMwN3wkT50IPqV%2FX5asdQhtobbdNG8aqO0JXbqRw41AbQWcntuxTJcQXf5dKrM270mH4piyqafhIVd6Gex7PzwSy8Zqt8QVCu2HUTgh750Rd9ucKzj9%2BkFwGtoi%2BFr5fwU7schoTsqtI2vGgI6h6KqAi5lCrJibtrr6zcJ9CmYSi%2Bjomf8X2xlA5%2Fo6hUGFgpg4%2Bpqu6MjRMdUGorjGK%2BK45u6D6gvC%2FTmFeueHVnKmppwDHzXtI4SQSdEUGxg6cr%2FEdtjUszjyW0P5bQxrKBtorkuTpq%2Fz0AwkKfBbE%2B8MoqDmF6DuQU8LO8zXAYEoA9QkV31At1tTOwo0wnnqQxXXT0zOhb8PunxXk9816PS7w6cPaoUHWD5Pf3bzamxdrTBqSWgVtDAy1zEjV6iORZc2PQpRYZRDIUhmkucno7RhY0serzn4wbb5akjLeiDf9c4qZQzbWgcqtLnk6PjKf09sdDQeZynvTDWwr6Xj9M4z8HR1AT%2FL0SkBMq0mmhCpDvAkMKLw9ccGOqUBdY%2FLjSFPoRuqObo0N7M9KgeMfi1uBpmB1sm0vshJ32zXaFrOKhZXQ10IWuTh1SXhiL9pU1r%2FJPB7WTrIBEblKz90EU4jk%2BufEwIcKPz3GZuzpgzT4T1ksBwL0se9twwAAc4irucltiqwYdYVAHCCEOtLix8dpp7Hz0lhfUXbMGe19n5AsFG%2F3a%2BZzrf4wSYYPu3Y05DmkXoFeENnVaS7Z%2FINYA3y&X-Amz-Signature=f12bd464758b14d6f532eb64454801b6c02c39211bda562459c06cd56c288a6c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

