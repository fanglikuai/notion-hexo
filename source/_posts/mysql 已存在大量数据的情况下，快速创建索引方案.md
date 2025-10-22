---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JK3W3D7%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJIMEYCIQDxZYrjGzkuN%2FANjhhH5lv2LwJt%2FdKUwlEqGvP1sV54NAIhALsV8NGKGK6P29uT1K%2BGE0hAYp0weIpA5wv5%2BZTDKtNLKv8DCDIQABoMNjM3NDIzMTgzODA1Igxzo%2FMQ3yLNHjwObwMq3APC9vrvA1L3vNaCHUAu1NxO8QIQTIIet6%2Bb4jHryrZXpMFEr90i785eFZgMEF%2FLjtSrrpdyFG6x0sGVrjmRl5ujd3sNiMcFFivM%2B%2BW4sIHmSKvrmVXY22Qn27JgniVkEvqSpl7DkKe8pVee%2FGlEDxc4LMggkVBeTy%2FevqAst9Y6c9A68nbd9Pc2JqMaPhUnCIr6aVr5r9BrHYNagyrlA7xcJDHJUXEPiJAdqZzIs%2BXocurX2H4emBCWv5NbJD1tLqtrlzhIFMF6tuqF0vvKbXaSPgpKtaUZFASJdFv6vz7djv0qtlsHceKtxPdKgdgmFu6fJKXFMzdhF2zt6BHjCeVxAjL%2FJpezrnXp%2FYI6w%2FPf6D07P%2FYAjP5oUpE4BhSShXEDUaPTMIm8aHOHdZ5vpi%2B6XUUIpjqLduqWCgx9VhO2c9vUYz%2BQ7VRH%2FprPqY%2FByKUHFoxuwKREHgy9yGyoKmmKRgTzFqyl4FURZHPslpvlgJGezW5%2BoZOjN%2FiPFQIX3Ba6RQECKesFG2k060S%2BoctVf1iXJFrJyTs9a0yv9S5iJHcQ5KTunzK7siV5EYhPZmE7xxlxzWgn4agj2Q7X%2FV6LTtzobAXZHBUxI%2F5GtvsIZFE49f%2BuzVP6IrDxmzDileTHBjqkAQSZ3bLcZ9P1HUnKwk1%2BfOh9GUi7wBoySiAQE5uicc2RuGcqu41DEnBMLTUx9AhiDtLzO5G%2FYVc7zCEwHd5jpscG7GFvmbVYEtpqEM%2FewfuJ9Uo3id9PD0f%2FyTw6QgOqV0QCEtL8QPQUsuRjSCkRcQ0JlBytpyM0sStvhUCq0ifW5Ha6nwyft1A2QuYLjTqG%2FS4FdSEegfCj4BuNtiBb5hWyWLdT&X-Amz-Signature=f337513650629757a12b3fc059983f59b5f2b6aae49ec7c4683f8456c0d14e7a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

