---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666SARI2S5%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHjwcYi2fZwftqr5nSsSfvWXMQMeHSpKx8byklsOSOI4AiEAg3gkTl%2BSICfvF35zwThBwXOfsHkRTzRztR4avP%2BIIDwqiAQIwf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFKHjZxL4J26PtEJ%2FircA2nWhKPvw4SaYr1a6wdXhjnzXMNiBR9aFlxeBg1ju3N2gddB8mG3xngYNCLn7DGyFbX2UpBEOYtjWdfsy71UEegtXjk3ZHaN1ypJz0f21L%2FhGF5JNOOix9GVHLaIyS6%2FKLm5lp2k%2FUbC3t1FqmcH40AZI1BF01p8OoEiiHG8kLKHCs5LJOkVw9HZwo8RfDOo9iq%2FpIX6Cn7GDGwDlS2wuwIStpmhx9XTH4N%2FlGTITuZXNdg%2FhxeQKY5zDfelefcQ%2BaFrAeGQ7aVjt81rNrUP3wx6D3xF12Mnvj4elSbe4E6lzfd%2FmSg4ZB6UQ3Wu85Zq9FM5mkoUsj5E1XdbQiJ6OCXv50enr%2BMaO9vNjDb4CgSjNMS4TyccnGD01QLGx%2F2E6IXriD1PqJS04XekqA4Zlm6o7Kb2ODR0SPS19hULLZsokoxbeHo2YNG%2FwugcuYp%2B4HNA8Yg6AGXtWB8aY4WeF%2B8AZ8Ma8%2FRxtWMY9IotxA%2Bq7KKEwBax76jZuUYoyuQ2rgUu8k%2BrXlG0rA8TB8oFzjBHaHgiXEjIVoETr1hpE2ZvmA4LvuLKhlQr2bQaxg03gNbTKOCJ%2BXp%2FLpbRzvXILON520dcSU8OXy1zHEc%2BHyh87rTrl54%2F88HvMyWqMI7E8MgGOqUB%2BAob9GbYrkEUvPGwLQ%2FGfWJN4F4Cgy4UV0s5QsSKs1tw9rljsjH8Wg68D%2By%2Bw4R%2F7M0fKOkTXlGRhXNchjkQn5hv5vQ5vB14h2txF5Lwvjp8T1Hmd%2FAMFXZcJJBQVRDNlQVWvhyr%2Fo90VrA2IePcMx4WBcOGnZEFik0FsMxaeHRr%2BGIj7tym%2BzXpPko6itMsqXQ9VihXZedLpbwA9HKcz5t6ZWEv&X-Amz-Signature=0aa51c4cd46d8c705bc6344cfa407a8a9f814ec630f8fc1a30528eb5b8d516fe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

