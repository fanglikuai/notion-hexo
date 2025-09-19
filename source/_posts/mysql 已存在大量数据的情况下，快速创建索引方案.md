---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YBQRPIEE%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T000051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJIMEYCIQDLzo1jqRfP1uBl6nodE%2B%2F4GqoHqkcCxh47764Rtl8VIQIhANV0SsFyovG4%2FDNQ9cfT4poVnJk3jt7chwehaQKpj%2BJ2KogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyIPWS%2BeKOgQpXoZ7Yq3APTCp0JxAmVF2%2B5kWr8NIGCMXBMORWwxFcrqLt5GbNWlr%2B2%2Fys2hGr0rx0ZVcBAWzcpLkU2yPgdVqJ237FE59kVLwp4RD1mK8iGUQHaUfGLffuh22yefZX1EAk%2BP97KjYXGDP8i99Cfp3l8oM8TNNLWTJF6PRVO6OnlM1AmDmqzkwQBoHbSz2UriZ%2BYI2fhkz%2FLnIjgoEwfDYIghXVPIbHshVCprkL1yABZz6IVU4pEVM7IZNvDr3soiiL%2FDeNttL%2B9o6Tj1aaTvYpyRg0L2DzzNrQegctQnJqcu9nDWzdiw7KGXHJeYlETOQlx3jQcVUPz%2FDuKuAkvEw9Qz%2BCkIGm8VtItmI4XWdcpL211dRKJR7R%2B4u9QWldXGZ9DG5dbIoiV4T5zVaQ5ED%2FcaH%2BjIKTF8rM6rsQLsSDRmIHGUx85p26AmRfoc31Dx2mifnuKJatrLVCTdJnldB1Lq87e9%2BS67y8WJezOWKCh3lRK9c1Aacae0WQvNIWW0MY9i1f2VWidaPlXVHLK%2BQLAiiQtvmoKb3vWYw1lJ%2BNMjTfDJoH4wEIFtStVVgWhn7PDzNGC87m%2FWuxnwSHFRU7iKCMWyvZ%2BKXXHjHSY8qnsPn8m7Ki4qLYX6bvTCBpisNAwSTCDmrLGBjqkASeFHwdqnamOfInDrCyYQUHCyJBAl5009GZxCGW8UJiIg7KqOXB77GIgio8chhNbTpGEggRHAAgtzvNRcfhBvJ8dNELllrndmgN883b7qskYETIgHMIzcJMJdZ0nqviuKyFHfKYITXChsslsk9lNI%2FnwNEE%2BP5Z2vBClXaPakpLQTbgkBX5CsgnSCPMJ%2F25DnfNOYK5BHZIJV3Bn3IyycK3%2Fw4wW&X-Amz-Signature=c0e526f35215df432b8589249ea4772c806bb6b12ba0e652f56d23990077654f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

