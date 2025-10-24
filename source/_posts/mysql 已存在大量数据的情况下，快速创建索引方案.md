---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMA3SS3L%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T090134Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF7V80cxeTePp3HhOi4PkBrQe9TN3QPex6P1MDHTLOFcAiEAh%2Bn4IJe8dyQQY9kxwzruoqc9Q4yfFh14jLChX0sFvFsq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDJ25Hu6TjU5%2FaYX3KCrcA5i12u%2Bm7XqF6PKvpyMG244I6RJistR7huAb2G6QDdEYawCtboi%2BQ6mf2I9RSvNTR3axKJa2vSR6FLNqCbYQb7tSfD2C5cybuOq2LDzBnnKmmXH1qRfUXEgimLXyKlho14ZulfHOg3dc4uDcZhYGJqrCRbTD9XATeRmfRCdGSzFIuCj7GLmKQZNmsXjlmz%2FZeXIH%2B4DOkaZwVPiBUanWDuiRPe%2F4a6KTfX%2F4CXrpoAGHq%2FueaMURNfcRIqTiok5Z%2FpCaIg5cgg2pBqmPrPezmdj%2Fm7mQt36f7NAiSZskEIa6ZO%2Fsjl8G2ego2F8fz1YYPFCcIt%2FZ0JmXd4tLNxvHO6KmNqDGzuuoHHSZkX21DzU4zUt8TwIHpGuea4Gv6jPm4afshdaE%2FFm9Wp0GGy8CrFJ11vGDVDvHAC8Bq9M8fewVwixab6DmX%2FaR2mMS1Np0Y2%2F9xlfS%2FD6Xz9%2FIcdLBoqxlX7lqJ%2FA%2BwM5Qe7HoZql2oNR5b%2BfTm0ai6S8atKnO5vzKBB00Y4FAgxAkanRIEiq6Cf8U1JiSnkKBzi0qRhPFfX2I5T%2BfXI6y8VVfz9tliaKsO7IS5ATJ8H4RCd2Py%2BvJeaD1pZ3XoD%2FKbiSuqh93jAUPypaTXQJKVmqUMPjv7McGOqUB7qXWW0osjEu9cslS5xU5FWLxB8wOI1r%2FkbAI3T3OWhQHSI0tu2%2FsoMIQa47FY0n1WsZbYYtAnEWfCkAlT4n0LoKORYflkXqzuUMFOGnFvxwxvfExF0tTkeA4UON3Vwmwj1JKXFK8nHk%2FylrV2fEfDLBk1y4Db1hX3wqf1ZW2scgLdW%2F9PkwpxR%2BsU6bPLj4dJLbUnVXy%2BQxIw4GlCzZPZ2%2F5wzlC&X-Amz-Signature=364c4e5943d156b5f7ea559bda71a95b8380b95a82756bd98caefee9c14eb66e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

