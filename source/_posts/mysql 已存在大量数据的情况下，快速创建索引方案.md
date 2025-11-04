---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T2Q3E57D%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCWn2mcYNNYeF8HPJBvgpcCgcMBGTz4BFo6Qha8uSJrLAIgAYCJBMz03vDjkPN8H9rR6foXOU%2BbgyvPibYPO5z3j%2Fcq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDMTti5Q6IoZNnxTy9CrcA%2Fxk2PsSqqYkgk1gilV71JSiLAAoeN2i9UcJk7XT81z5ZjbsOAXfWEwYkK2ZLTyM%2F14irw10IzOVQIJSPG8g45zxfkewDQ2wHzKkWIIRnXVWZWqldL%2FduisOj8zOCrBsr4Q8d6erCubYacUJnFE77ujMgjyhwRD1lKYLjhrDOnpiU8rYnf0i4NZTMI0v37L1V9JLYVi6oYf%2F7PE%2B%2Fc5L4ODsyThs%2Fyw%2BzXZun8mq7htkauNAZKGPJTwDBP2l7wGfrO1y6b7SAk%2F5toONUbh031%2F9cCwtCmM7kAqLSMqmkRhF6CLVhHN%2BboZZn5%2FHykTzTFmwgMSTZbupF%2F39YKBlvYW3NSU36hWtHFdMFrpXNJvAHMMz4CQVjDtWnhA4u%2Bkgr2S%2Fsn3ryi4nDXmTGk22GmWT1FOQg9tkWnneaYIflwUEk7m0MUYgs3DPPmZ4mRlj0pYUr%2Fe0M6gI5rTKyf50dhXHdE1%2B%2F%2FM%2FfO5LKXvnLJLXZgedZAYeKe7chfjBvwvMaQ8etstFKlXCE0jF7kKL2J4eyskKOUBFtPqR1xS7MbbUig%2B7MXdCKQP1sGFSg7xf5PImEMZGKJ5et%2F4GKhlyXAToLo57ocyA6DNhatD7x5Fn%2BsZ7QPO07FXF74SyMOyvp8gGOqUBGAQ09aZoRVgja%2FAeP4YdmMRJmlwysm1ntCyXATrAqxzzlvFEkzqu%2FleqCyOJI7M4Ur2Mwka9yXo9Gy2WZiE4hRROc9jTIXKiRU35T2TqALCtQJNO4%2Bhg1qQr9nwE5uk6DCiVIeMVlN%2FSLBrwrupJhMGADYpGP4bSoXKxrBXq0sHfdo3DlLkQoh%2FhHMiDHengr%2F1dKAppEgzWmxdvYV%2BY9MGjHEA4&X-Amz-Signature=9e917e64dedc4ab802735cca4a3923aaaccd48d55ed3337ec2b578b33870081f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

