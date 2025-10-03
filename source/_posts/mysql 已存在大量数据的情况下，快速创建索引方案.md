---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665IT2KLBA%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEKf15HoMDxjdOtqjmyld%2Bd%2Bw8xFc7v5d5o3l60QPbR0AiEA74n%2BbAGMkpHUBFbTM5KvFTdaP92xWsVaF0OedXPHiwsq%2FwMISBAAGgw2Mzc0MjMxODM4MDUiDCiKoyX0e0rjP44C6SrcAwTR0LFVCpCWVbzsRKuNks2%2BsjkNYoz2t58pWnA6dQxgpnYfweWBVUVB5IjqWmmJ9TZ7D1TWuDzWTrYD4GS2AIUbcrYWINeAziIRUMoaeBchJCGfUbGeSJQo8qfn26LLWZW1FdhYJTUUjo0l9Gg1tZ1b0xpi3fF3AkQf62qJb84cvdqkFY86z6MXaDmGf7tCxUkaAJJ4Henb72O1IUJK78P6W4gjLhKxUy4AIndiPPB780QtZJ4MRQGYxk2SfX0rg9mctNHWCyM9jHTE9U%2BOS4lrnU2aJrmEHEM6LET4FVbNdHT8TXghyMxrh6%2BY0hj3xFmbOWS8wj%2Bisl2RR4Axi%2ByfHAPJS5mUKlo8vPjKA4qoIJE%2F5mN7vPirf0V0nEEUy9dujgF%2BBrAsaMpandBP4EJB9XIY93Zv89a2ig7RChDfXalwTxEfaufJA4j1Bn1k41QXQKwvrDPeixMC9ZDMLGfUHle76ulv29KJuXRnaYfuiEhA46IxUWVC9YMt9r08d3V2OcGTBX%2FpbLyUAxpXtyEzqrDEmUDIMvM53gPGUiW9U1VXFgF6f7nvx8fyM%2FtHH8GlQxHRzT0oNdZHVrmH3FnFbBwvCpJuJpXZRk%2BxoCqpiRAuiDOe5sxW4KHEMODS%2F8YGOqUBkdGvixIrd7%2BUkiQyIlJa8pEhJ857U2KNIg9kbgvl7YoEAtPiXs2W829bg2at3oVluuACn%2FFZMSjomnawYxnbSGZ%2FzKzrzp4R1WVHC9kOM5vV6fyru7sGkyaOd103MQHcg3Y0II8R6gtTTKaUiKzgrYev4KLe6IlTTcTobPlqnGjlkGa6UqqjfcAPLIF%2BjMGphnzBwYmFypQDqC0ZBCrHruddNBZj&X-Amz-Signature=d7e35d06f72237767f25399d9bf0f8ee8b9b1d277184b43d74769b3f9111f4d7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

