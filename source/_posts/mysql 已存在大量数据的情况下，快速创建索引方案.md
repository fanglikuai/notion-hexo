---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667ACUSTSC%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC7tQHiUK7pC%2BC4MBiBX9qrbxtj2AV%2B0yNePvZhP9dkkAiBufBdsABck5nW3eN5RpnUgMSF9W7G41HJsGUwFeaeS5ir%2FAwhdEAAaDDYzNzQyMzE4MzgwNSIMZGc764a6V4PRcbC8KtwD1Cg%2BVs5ZAhIw4%2BlYqVSK%2FSnsmyszyUGHKF3Vp6LKQ1E5bYjgCT0vF3h%2FDlUzIp2pCPEmlxfwdQpVK1FSBj3Aa0NNeoChisGhGGvYK9k9TQm4Xw4eWKsF4vAZk%2BLU7N5sC6qFvGM1DfBx959iBIJW7GO3K%2BQ%2FiLTyOxDm35vQg0qm%2F6ToEAGMyx83Y7krMZwufMVPAuSqdGGaZaRTPqeWnmNIqEkFtnK%2BQG2Gcy9aHcYFMe7VrYHbWPoUr0E1zKqNYJMmUoT%2BucS8C%2F7YeA5XIMk00rCqRCt1RKrX7kJaLfeTNH94P0%2FGUhQ0twB3RMg%2FtrXb3NaNJYky33HOFMbZwZPVOR6vi4JRaThyx7AaKwavzJcmqyxYq8CtOxYOKIRMr%2B7o6MbpYjDdVcSlQsySjyllyCR%2Bszq%2BXJjt%2F8D0B0Ycm%2BxAPVy4sTkxIcnbBWTHhKIUOHYuHzCkyG7xTPncuI113cWWmcKad6nINWsY9a%2Feo3i108rPjLuX578d91eufGrnIHgtt%2F91yROUynsaPGVvt2L1vZBq4XWhfSpCMUq5ZxWgd%2B88xUEsAvO6ZnEcZrnkH9eFRm7CW5ybDhIIIUtOR7XSXYcWfAFVywepwtFOL43%2BNRUfvTktRsMwjMftxwY6pgFz10eIDl6DQzncsHFqyRsnpe3C3J2NOqyY82QbdwDn%2Fbt6MZOzdYAJfNVwvB7f7pld0iAIzgWNG8uCy6uYeoJ4gltM2yfJSPKTR9CH3PSgE4wEHvexb02mavqMnWb5RYx4CgZDf9W4mxVA7q5Nye%2F0iG423U0iCgrr0QN542btkLQmEaF73PzQbYvpwo1vjUcuRifVvg%2BfhWQDs2cRMn0ce2I4%2FLW8&X-Amz-Signature=46212a4998c04ece718f5fec4c6f965692a24a95ef88dbc5ccf7a62aa1e9b99d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

