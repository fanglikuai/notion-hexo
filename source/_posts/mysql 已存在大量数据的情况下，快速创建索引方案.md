---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJZYPGY6%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T150100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG8aCXVzLXdlc3QtMiJIMEYCIQCZrJ0fWNr72%2BUN1Ux3fXqC%2Bj1ibgZ6YNIcwuPuuraK9gIhANGG04RbPkwfLy6%2BNFVlKJwp53plpTyYoVJeMXd8pEdtKv8DCDgQABoMNjM3NDIzMTgzODA1IgxYw79WaUD1Vo7gQEMq3AObOHp1GK9yKQMdRiRa8qiFJlSi0dNZDX197xtCeZKa1eBBsjQiQ5BxaFT7Owu6j%2FFNbvjT7Aer0o2A9A9VTAzy6zZv7sojVw2GzA7enRDZSlCBpSis19Y0AHqXGZe5uzqTM2xsnaW4%2FeIY%2BEdqiU2AL9hFMIfwp3VqNh108v80Qf6hAqkJJOhF%2FoKK0BRg6rAcuAtWyZmS46g4r93kYQ5AOcxp%2BRSKB7VZFpaxCLMYkvxQLlYji2Rtb0UErW9BgGRpnWK2f5UtkwvcyFr1QiZPQlqtY%2F2iNLxkILdHOcP83%2BrI%2BtrNWzum5CDVoAW1sELBg3m8On8g9OruwYwGI6mCCg7hWSEURn1MqaT%2BuONtMqGG%2Bv6v5z0h9LVLl6D35k%2FNdeEkCA2QRRuVUmzqj9UEtaA%2BYE2u5gyePhsNY1wgeZo1MLMbE9xgLCq840DuVUV6xqiSqbhmZdjbZDIsX8Jyp5ndYmznVBqBES6f7cwPuHm%2BdzOpn04cjCO765HEFqhTELfqcWo%2BIjCKbtNyaQ%2BX0aBl5vCbXVZj%2F6P1MVnJDOqMFA4N5H3jOH25EYGZV7ubxg5cGQH5w83LxSEdbZfRunF%2BEezpfhyFweFwdI%2BgpiEB9PVaKJCINOFSMDDfudLIBjqkAQ%2FGd8oJ%2FJ6hi3EYOTw7ULamRcgRxTT7jxz8hq8vaQRA%2B6Ov8UADVC9wteP06llxrLfuj44bwaLY8wSZVgKLiM7cjQSck82f5K2zoHHvym61tQVwru7ta3SOrADv3nE2%2FzxfNiUPPcss3Z%2FsGFNh4hFtLhhQA4Fg4mXL6AKsmwkUy4LkpPKhjIwcKWvKve6VlhPiXso9L2ojxh2liCn3nrHl82xm&X-Amz-Signature=1280ab8ad3ff629c713ee437a04b70dcd6e02ba054fb5e6f48e54f1257c43633&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

