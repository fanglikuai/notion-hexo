---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OCE5DVV%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T080056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQClpqPeSTkWTf81h1YwzM9amJy%2FRt4asf9Mlrv5bD8ZoQIhAPy3Das9UUMgf%2FaUsp0Zfq2YMLSqe1ZKBiIJNTXywx5oKogECNj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyHfVlOuIWxVZIGP9Aq3AM3vdpZb7Bx6IikC3%2FM2Xn8RT2AlZzRlnvCrbWuosnbgXUnFyFsHVnUkFjoapbAnnBsLJvoYq5djR8adzs5feyTMIl79%2BpXBA6iRg6hJEf0YlGoGebDH7sXURUt7aPj6TFWsmL82U0GJYO3fH6xdy%2Br2lsmR5Q9oW%2Fo1eCOcjjddMtG7fgNlGIZyQdwvJbYN4K73k%2FKXwLkdeL%2FCV4lJ43tffiqjxg2XaesVjExyJCWvZv8ABSsAFGkxv1d8pMvb29QsBj9uZTGkber%2FKpG9XaGMqrghoe4BRy5nghrKkrSMtgvEgFzE8uZB5KMffTjx%2BC0DGo785S3S6%2FtRrEy%2BNx1KmIKjGK01b8k946qTj7F5tuWpHnN7To9Jtc%2Bhysw762z%2Fyd8T5d%2F4ShTXbuM1xJY2AqHCu2UTkXjwQnwiV4a%2F39TuslVcvINQQ1bMn%2Bog2Ut5m5705TwY1QnixrihmiBNPMgBCGmLK5sGaN4Ymi48eC6gZ7zKruj9CMcRAC%2F00dqqA2WEXvQfxlnLM%2BKKJUqcgwxn09RndEZ1bXm4zpEYF5rDDw53WYhu8XjHjTHKZZ86YhpJVagBfaZ7z7R5Bt4Tyt%2F6326cbnyiSSZ0PNrzwUCq3TbR2C2zV9hVDD%2F0vXIBjqkARnbUGYt98hqB2aM45MubTe5DZ8iuBeVthXCfOhUAemB5QC%2B8z%2FDsERJOZa3SY3gsnH6TYT0pL%2FA8vHYjyiTD5zsYO7cp9qxbcZKZqFTRDUD2uZNXvDIslkVKtmqy0ym7NeIad3Rraa11NUYTSDfdm5%2FBDcXVzd9xJguK0dg%2FDd1qLFqrs3iiYr6Yz2CQby2VF6ohIibg2eQP84Y41LFqB4PClft&X-Amz-Signature=a7882c86114b82abe8be4680553245b149a3f765c94ff45ec926afecbbf91e9a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

