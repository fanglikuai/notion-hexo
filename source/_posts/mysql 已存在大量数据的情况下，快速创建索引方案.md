---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46663DDSRSW%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T060054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICb9OQZllM3vj5dLXQt5IH5vXumy2q5FdN6Pkc8N130YAiEAizGyrMPXgMJ7qGWalBfqAueq%2B31XPew0K7HKYJbhwHoq%2FwMIfhAAGgw2Mzc0MjMxODM4MDUiDM5aPMjexthEs0FXVSrcAzpFZEsgpy%2BXfOk8lxv42TVCWL%2B%2FpIdFo7OBw8I7QrYBersU%2FYYSnx83AiBXMXidYVOHAUbXadW9GXtxJFmRMcmHn07c09YcgV80M3lb2cl83tYZwjfSLwT5NvXjcatfe2OdckcjvyGtFAPSOJoE1nANP1FL6AjauSJr5NIKhESY1LRHRyUEFF1vG67N9BpaYdA%2FZYsV5hGvIbLWlwBVmi8w0cNyp6gWevwX%2Ba8NrOp%2B%2BcbYhyMrt4ML1G8XGbBpqOMOI92O8h1HmY1Qk%2Fre5OttbGPXoLdoBGFO60QaDQIYBWlWP2uzS7hmjQArL%2Fafq7c%2BkfSDMsb97AIGqJgAS4YS3mJh2XpcMlC7vAivw25vUCCNTOoSerLKzBptaFYf6Rq2PKGRF0dUsJHu0jbLt8Aq0xYGcix8TnkQv2ZhmYpACPWOkWu9dMy82Py6jVThpBFXzH7XzJts581COiUzQohyRXDia5eKg0ZiVg1rPSvjmPa98hifFLoUPVOjGdFYaMrPcy4aMLLFOs7dB5NA0yJ07ef8TVdBpV6iuqz7qxlGQHl%2F3JFYwdwsNZ7zfJg3dLmt%2BokV%2BAX7WQ09uNiesscLEQbim0kpqTr4CZvDeIuFCcadVYL9iG0B%2F5HoMLCWmskGOqUBPu2MfhPVfPgR9QHA2SqWxdbO5nR2zonwXTEgHVb1MnnW9WQ%2B9Q4EHyCCtTpUQk1peKAyvfCVVImWJ6FsnlXhnCDI4v2nrohjHmIQo8Gtzmz51ANsCf%2FGNjjOViz4tjRcZ2rEM16L2ldjIQz%2BWUcWpACsQOuB9B3Nunkg%2Fqw2e7Z4uNubcbExYL391lYUoxPaswhc3qcUVxbmGrlbRmSpg1MEeqsQ&X-Amz-Signature=2a5d85b3f6e8346628d8c0c24186b387e671c6071df586e16192f4cf3d4a6c8c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

