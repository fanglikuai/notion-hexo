---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YZ5HITMB%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T040046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJGMEQCICVs5pme6x2opQ%2F6HE4OcwOVjzi9HfWgGvWAogWPRtM%2FAiBEYweW%2FbH1BE87x%2BJJRzQwkjbg%2BGk0Fu42vGRYN2SxoCr%2FAwgVEAAaDDYzNzQyMzE4MzgwNSIMlp9Ofo2yxz07YmUvKtwDyPRlAKsQpBeRjTrx%2F%2F9km9n1kvVRzWMPJICYgwx%2BzWZ%2FZ13kCL4BCZs7LhEVn2AARWR%2BLTpN%2Fa%2BHrVTxVA7Ep816JdO%2FxAROxXL14RPWeNQa8R6RgjArAW48iKeJu0arjL216IAF7CNuxHl1Bsky%2B7Mmm%2BhSjNa5rw1bOuEs%2B6xYlchlhaSsl3rBLiovvQpvidySCqPIUKr6QnJrE7tw9WEj7GqzKdz7CKOwisN7dETm8bJMlnEvKK3WX4mbdvlNL%2Fwqd0%2FKmMqlDUlnabT0Nb1IvTc3cplM1%2Bda68IHtjXMxjY0AmcFgoO7fd%2Bx70BaR8ZCST3kRcYqLwA2mNERHXsfodALmGPgLobZBEylpQ7GMK9T3%2F%2FHPrGnHhY9NxxEM5zj4ZnCfL%2BwKLfhDNPaUr2duKKihODZnYu4PQgsZV3RH5VhoL6zGQcGWYQE%2BWDCn%2FW4BZqGAaPeuHd8Fss9vgby%2FO8L%2F1wjQB4p9IHGfRc2yzb1qqlMB129ySYEQdAyh8UfDks5trGNu04wAr8cIxbKRGX4LkPSJDiF7gd84uG7XZciR9IjGb09Ih9nK4btWUZT6BvQQpIGB8pTmHvEP7tll1vWBWGr7rqfuzcHn9vR66YPtfJ66O3pdywwr%2BXKyAY6pgGxraPqJiOo6XOz5Qfkif0P7oM9Pce7jB8piylLP%2BQQ6%2F6mLmeYkWEVgewKS44jLkAcIgwtNQqYLsUoG7WP5bWNU%2BNH8iG45na6OKoZMpVWO0Nv7FQCdvj2oOavfsG3QJCoBqDK1z%2BBWzBujN5exRm5oaocYmaOQvcoPO2LEphO%2FLnc5%2BVb1R7N%2FewyyEbPA0Dr5gdt3HPGP%2BKLt9agLA48BgyxtVeU&X-Amz-Signature=b5fe238e785780b189035f3646b1039973a9007be614566af0ed465f1ff7cee1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

