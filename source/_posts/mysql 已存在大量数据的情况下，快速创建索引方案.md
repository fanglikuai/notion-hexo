---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JEI5GRK%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T230046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEYaCXVzLXdlc3QtMiJGMEQCIBTA495SEbqH9fqBudcujxJRr6URpFuKf0UF%2FZimhs%2BBAiBQPpMCx5eTnGQmZ%2F85AmwrCLpTuo47SKt2nCcN5viO%2BCqIBAjf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDks3Rb6w2Lvf%2BXK8KtwDTvTim8%2Bt4V1oDfn2OlOVYkfKZbIzDpwKqI6zdm8FyWjYyouf8szQRhIsYkFmkC9LoRwRd5HCs951H071mSPGnNdn5TRNk38Tl8fW0pqxLNW9tcc5gUdzpji%2B1G0bAlAWXao2RRhbG1Jcsoydfb0F8ce5eHhYrdA21J0IQJjq7Kz2rTjrIMHD9bHyTi1K78Z9%2FgqKnb81vZm0HXg%2F4cLBuAcm0GrVGXeOszbev9z02BJgqvbfmRVMa6WJiaZTdL7T5Z0n68sW3kivEY8oKML3itXMemnv6RW1%2Bjp5o%2BIlPzfX5ywUZZ6t2qoO0RFJxhxTXmAD7V1Iz4%2Fne4mwKXd3SzFZZ2slKZPbenUpM8TLj%2FElS49n%2FxsP8%2BTRqZ6HAclkPXhtc7OI58gl7AhafvnBecT61%2BAS76Oy3QeJypZO6EvDkr9KcbgMeNTMcwfyR9nj64qyUAw%2BJfIAYX1sWfAKlzunjXnT8LCZbHcHm%2FscSKUiWY6k3yR0xQ3IWd8DPVcs0gkBw9ARhca9waLJKst6aWJVLZj%2FILrQZUbgFnyHvh9YQPlMPJZIs7NLR7%2BGtdPRCKuyUkC8H3CijsjRlAIdKuY1XaLZtjYAkMLnzkjfRcvO%2FsqPTiXo9CHLWbYw7OSgxwY6pgHxeH8ufMC8Qk2eIfShUM6ISco1jsgDYygUWPRsGxQDKB6BV%2F1jV5TEDQG%2B4VQW6HEKAh9VidQ%2FnyHvKJk4XdjKSAeh6l0Pdyi4EIpuQ6QhyyzNANL78t5XC7jGh3aIhyctgyceMjdWoER%2Bof6hynmpxZbBim4G5qMxr4MpTdXrST6ncjMjGLRElH3c%2FwQnW8QgV6KlfR9gogX7jJKqlZS7YJK41e%2BC&X-Amz-Signature=156b911c3f4e4034ac410ddc59e8cb5e8efecf695584aca17ed1ac42fb5b3b36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

