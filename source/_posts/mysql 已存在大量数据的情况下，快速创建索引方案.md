---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ULLCZIET%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJIMEYCIQD9PxMAcfILWRCOGHXfagzqSitYSftvq4jYRsOEQuh3fwIhAOHit2RpPUCpvR69skE%2Ffm2C%2FWRM1%2FxN0WB65vW26zwsKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwwQeERlCaD9u1wDNgq3ANqQrV6punJBHuVJFnqCdRnnxzDnFjnEbQjM906jtBTMJlwrxjSznn0Fa2c3qTyDS0Ad1%2BmaMB%2Fnsco9Rrco3dWfUymwRCXDnL5UNPzyk5Mk2uzcHvHLt1wopBbL0Nw3mi7aaTx6k6nYT6NvQ2imfUfb%2FCSOyA7AbE2pK7%2BU0Iuhdc%2FLA%2B1BlIEZ3S67q5zqJRkXCNUQ%2Fv7kVDw2V%2BnSFhTDl9kEkX50Y9dg8zEuMDtz6cAhrvQ0ZrqE6Lk4S1JN4eCVB8eXLtgtea%2FlHzb6gSOOIOWZsFoT9PFuGjbqO6AtyaEqxQZ7hrE73A2oewHYROzLWk4R03ydtuEUa1sHbJOAom2eO3Rfe%2FGo6gJevmpvOcw0EZBEorFahpPioCh%2FBYaM1zwx8qOIyeIdAjQBqUx0geq7TRb%2BT06ZiBUOrQQfqUyqDkNTn66rKaRUSEhTo%2FcGo%2BQyqAOvHcTUgs%2FOssHqqGLTqKRzkoSMfEUKSkB9vSIiVbjs49UlF%2Bcyn%2FC3FoSN1RWKQPVDCcZFxuNDTFxwrJSeLeXkFNJrNH2DgMPbNKBgBnuIq%2FKjX%2B86OBa87E4OYbjmHVHq0isFFzSnBTBzAcAyEA9gjC9Rsv4DqG%2F4%2FmB5buPAehMljvsdjCt0LzGBjqkAZTo45BCzYRZQv%2FJy3tj7DGxQLjD0rowUMy5QVsMvdYqxN8rr5%2FpOWy1qME7e5Qp04xoTAogJJ3a1sQsbDW2IwYEtNRrl9jfjMy6RoIGn9QuUNr1vh2GfoFRu8g5rJax813ZWTZYQBLBMorNvssWzCcRi9SKoRtm%2F%2FCd6sD9RBiV6IxvvVsrfUCSzyh6SADtRY1R340aJClpgpcfOzbA3OuubKUC&X-Amz-Signature=b01993d15fa852fa68f98bded0bbcfe4ef1a38d55a73d9834e58a610118a53f1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

