---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZHDGZVX%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJIMEYCIQC4faF7NLE55%2FuTd5mR%2FrVjciU7fqwP5%2BbwVhgR%2BpkuTQIhAJm8sjOi5uVft2o8v88BKfRfQOjXejKU3e7DqM0e9OYuKogECP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy2714cC6eaY%2FQqJo4q3AOCcfYpRdXV%2Fw7%2Bk%2F%2Bc0yKYfNh1Pw8O8HO7nurwvXimIN1n%2F%2BZAJa7%2BLQMExQLGDeqiry1XO14SYAoOOCtQcC8gPoVjeusKVamToGQNlHAn7W%2Fah%2BpGf14X726lME06wvMyczbppew5DbUGhEzauEgrsziiQ%2FgnBhxET6cR5ylgb99NuwzLkgOxdxCTeEBBCyFtM0lEwTxhKsaWTZD1NEOa8IMLODOMepjphxsvQYGng0DJHja08aIEVQ1SgIm5byz%2FPwWLJ5rloyy%2FKPZGfArfCuDnKy8Z32ZyEFvOx0j2c0aGxJL8%2F%2BMEAakMFUqU%2FGkgToJ7hr5eFeC%2FZQJEZXmPnmTeIY9YfUxI91FVGKU%2BvihHGASTobEaqBVycaM5Ix48ADt0WI0TeEfPeV6izocCqjytMc5gTSMbeftECLGgpRzq4mZ3V0fgFgzyZEKo41EgBOJNynsQJKlGi1ZfEUs9s0DNiEJsH5tPjoPtplSoDTJqTkvf%2FVQl5EOP6jSnYdkL2YEBqQDk8A0qRqQRyx4bjs7RrlFXw0mGc5B5mDm%2B3dm72GAAmR3zBpDS0sWZ6OkzQPBb%2FxJMFlgA%2B2YqusjfEWVThbMn8LtCgja6kwu5xfwhJ5CQvRRj3AS2qjDHxKfHBjqkAZ01VAQAZA6ozOOViBoighFBq9sn4u6K8PoGyQ53fT7ZcvdTjQ%2F1N3HgEj4JGFwYeSUpBiOjc7IJ9HbtMZb1AAnT9VzIQAF3CQGIzDDCLVFUYv8JBSCenbRH2BdIE4kFDtikeaeskL5ZxdW0eWn1pBCmO3wmAp0k6WIX7H3twPAsPtBx2DCbvOfmM9v%2BwXqDrRbTutg7mRkVtc4xwae72p5TfXHL&X-Amz-Signature=b1448ba9588c531ebb0d4ebc9a09669144ec0ab21a53fffdff293ae1df9fe072&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

