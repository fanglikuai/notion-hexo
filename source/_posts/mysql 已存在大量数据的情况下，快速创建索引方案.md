---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4FGLX63%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJGMEQCIDyQ%2BOJbYWAQoOu5a9mrSu9111cGefZDiuV4p3t4AHDgAiA7G8k6pbhFcfvjDgBdM8Gcztwhc9cGSZ33jZnDvjFEnSr%2FAwgsEAAaDDYzNzQyMzE4MzgwNSIMCeYvbbSHd%2FbixWfUKtwDb9EwimqbuoBc2vS1WcCBQDfCD7kWgx%2By3xZThnBjsmT59a2YU0v9yo1NYik0VG6C9sz8c%2BwTtlLXWcpyJv91cCtK31iZbjXracUJUnbLYg57UeLRqA%2BxbSXBigTO7QZhG8wcdq2CQ2ca889U1AECreyYW9fYq5Dl5uhzxP6JKtLuxgv5yzNvmbeipabDMnQEeMUuukQMJ4%2BKtlXaXy5%2FKGp0OQWBtV7dRbtfsMCIFFpPS8yF1aXETWH8mFYDPuNbMSRgOdTO%2F6R%2FLz4QaF7hZJ3vbezaXXFgjYTAfGqZI1wu6X2BnQ0TXBruotQ3pvBXbHNsoE7VwUqNGQW9WX0ebq%2FvktJyS9N8TgX%2FaA414fC35aJ96xrjFLFhQdMK9nsmeCrTA5UtbVSvqUm4rBhyCkMYGcg%2FbV3jiBdoxbjSLzPLWKzjLdCxwRETBGuMqYVqnqvgF2PQiWlzdNhaTA4kYSn8PJn67OexrFH1zr8No%2FwjvBSi0P43D1je7Yu0NWopYQn9FORlSxSjwfGJZ8C4TU56W4rkbdxEq5ELYx2lm0JsJMqi3%2FdFuzvdGhSphQZJOgxDOUFv75JTDELcZvw2lCK3YhEEmJuC0ytJX%2FphXLM4TC48m%2BxI0q6eGzUwzYCIyQY6pgEPUhjLtK1A2e%2BCj40HWelIbf1xz8l4fjkjBnBHeeKLenJLOlpAxLd0VxuZ3AYkEVvEmGnF3WFhr0JQ1f9ZGMLxuBBr8FRjGswSLS%2F72XJHNhLCCXFX0gKvqMbp3dw5obJREsT2KrNy7vppiyOU2IJMolRP48GUgQtkozUAVXXuP1bqTsyz%2Fk2AUOpCPEF7k9KYI%2BD0k8MFlHGIjLk46EVE%2BWzxe1cu&X-Amz-Signature=179552cf6bf2130ef16de6086eb3b5119a88508e98d6b508372441fcaffb59f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

