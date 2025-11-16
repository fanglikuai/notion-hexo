---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNLQNTN5%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGeOGFKs%2BGYEbv8zif0T6%2FbxLyYaVoMhcsrvBM9h23m9AiBAgFIYq3ZYGz3c0ekWdH5054A7g4GDrbgkOeSb9wal3CqIBAif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbjd040Y9kDEQHHQYKtwDXl3OJq2VRovpEgfmC21G9XsAGEeqGxsVSruqATLMXqNtzimB28%2Bp7xiCBqqPxqBooFSPxERQEOGs40a7tJeZb1oIIr5Ub8Oj1T7LFlytURD0BmsAlUMMZhIMVofMWXOD2jMUPKtatOOo%2BDe9kCNhchs%2FzF7RCdgaSDw8i5zxILidCzQIu8e6CEagC5vCgUBExR6DqekdWzNSaFvySrlw4%2FAV2b0KHeugWH8ykigBVmOaO2vOnMqAWRL6ksOh6FIDzv%2BGX1XnjC8X4XwyaZTkd5FJrjSyYx5vBhJUstQXRIQsh%2BVDr8SVs9vzWA1ubX%2BU94%2BFm9IT3eyGMjyKVpPBdZ8GcrR4bY1YW9lR9la9u7%2FylEZkifJ5lzhmkfNNtrnedE%2FtX6hyNnSuHyK9QROLlV3uCPyOmKLT2g81FLL4lT3f4FdewvkJQoxTt1ROW917e70zXDYcyPp9i86PhJkZQV9Ll4%2BvtN9ksA1NysyOe0u3VA%2BPtTlweWccqKhOGoJqbyoX87XvlASBxmM%2FVMR3mIasvOBrzy9%2F8LR5nJxkPbKQ7Ogv4yGDwrdStjpps2o9HQt2RFAEFSu3K%2Fzq9x%2FG8bX%2BETWovLhy5OChqBGHJP1uXO0ao7kNvCZnHTQw0ZHpyAY6pgF4NYbzgYqZ%2FISo9I3kRiX8%2FSJzypn%2F19jxwyYCPGcIs8yQUeaFgnryuwFSqqelh0n%2BmeuVNCFznbH%2BwKvvtmrKVXhq%2BHxseXP4pRJT3YgF0gOvUxlCc2%2Fp54o7Y98a4JGE4SAt%2B38ebmbK0g3YzaZq8W2kioSX7CfyqKiVxc4vLgFVRpHZQbwKQTe%2BNsAg%2FCi1EXKYZBedsOxB4L4W1ZKHb8OY1yI3&X-Amz-Signature=926439a7b4eb3da6caf80dcbb4f88f0fbf9689958f2d0dd00139646db8e008e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

