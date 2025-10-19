---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NYNPVCI%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJGMEQCIHYMJINH5NVIY%2BoUIZiRl5UnM%2FdlVH4t2kd1QuiBGE3oAiBApNgZi0tLZY%2BeUd9lJOHCWU6%2FeySKZNJvsAE2yKRMLSqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2vYbYW6QAn1y8xzDKtwDenZMwk3eARnt%2BLVU%2FBNpqADAJy7TRtk0slvp3MKjblFPI62Nzo9usKIDAz5UlDDh06A%2B0avILaYHDES6omjFaScVSleJzoHUSLJtPn4gKRrHnKGA%2Br0DYf3A8Y3%2FRnKYQNmNEnnglQ8CeQ0G7GMRiiahN9dRGxqsxbxCg8tEgIcpcCQDwqoguw51Y2hP5R01b%2FgO9a2FOIxFHFud%2BPVDcWf21uWWx9dogsOTUBYOnxSBA%2BN1azn7jzLmkFXBCmGR24MaGCQgpL6EU34fOTD%2F5xOiy2KmDR9aP5rYYc3uoU5p3wZ7XVLJjWbqu8R2kmzwlrmY%2FG6DnG%2BG%2B9bhRqMud1p%2FiJ0Pbxm7cWr5xpdUGgJIunRV6jo%2Ftow2%2B1xFpGxsvqG48rQ4oKsDFrt%2BKRnzLI9NJoIU%2BeeGUpGpj6K05amtLxEZxu99JFo%2Bqj7CM%2BeNoq1V9VRiU9a%2Bb7TtZsk5JLY6Ohn5ajqgkYw5U9c6fTU0I%2Bnij0Rzsdih7SJyvErlqwfJWjced5%2Bkhw3zp%2BGXiTxAk2joGKcNI%2B57QhOPp4BDECAoSY8DCwXDnaet8%2BTqdX%2FAHcrJQ69EevWSk7IaiQf8Q9HNhsozVNhywu75rtJI%2BGJFoACCt7X4aJIw2tbUxwY6pgFXsg9RlG0U3xF9fnBJ0Ph%2BchKkK0jZ5P%2BhG%2FNyDt0MpWYuMssakX9j2Cw1YnkoHPaU0vC6LeOGCIykCI%2BToN3RgaLuo9Paq9C7Ne0wRE1BEnlIg1WK9fa%2B7LMN7YqnSGbLn4lGaB1VKEEMaPd8aoz%2FUGBxhquDsLebHedFSrNfw%2BVaZIffcyuJ3tD46t6DDVcZb15sxu1ALDucYu3LxVfJaDA%2FAKd0&X-Amz-Signature=983e55488c19d7c3564cfc57191f07220f1ca952454aa3ef0766e5580b37716f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

