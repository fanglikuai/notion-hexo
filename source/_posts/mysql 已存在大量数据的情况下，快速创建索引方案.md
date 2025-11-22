---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PKZXDGC%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJGMEQCIErjMkwTodcjcIXg3HEaiL8BY9QLIR3MhK4hGuyr1BU0AiAKXNuAwuHZCKFFqHqr0TKtkY1gsCO%2FXS8N9mAh%2BfwSBSr%2FAwgkEAAaDDYzNzQyMzE4MzgwNSIMGcLHRHto5OSRF4tGKtwD3WEdk60Hk%2BFCcm21KgHgL4x2dYTMPNxixFe2zXlK3Cu7RULwo8s6ZSgA%2FxV7iN8nnz64tQf7fsavUg8JZT3DOVCapGaLSo3d8HEpjX5zsUpd0Jeg75Eo7vvqQhRKfxnpMUc1uHkBc3JsdGf0nfhRtWZWQjcyaXZKv7QWPSZ%2BH2ZVza0hlhlfk6Qo6%2Fox7Zcz8VDAwwd3rNfiAyqrs1Hr2LxizFrrRNic8%2FoFYF51vGK3BEU91iOHSpGv6tDsWpSxu8CL3Fxs1sBVISED6JJNpNyBBPvLo4LiNIWR0FICeryFYFlhPZ89h1%2FwAY9uCp6AbMGexqaGdaUclNwUT%2B8mjsFPjulX94XvJ8Oh4bjtKDnKfTJP7DzyzXSVzlIvqUVF4ywEnJe7HfCI5RpO%2F%2BSEyyEpgJ3ZUPuHLV3Ll43Uwln4caYbcVFtpWSn9IVQTsz8bLJKASVJAXi7O0GnFbbJ9Q8VtCleAOjwLp%2FJLaJQT0B0IGG8WGOwyWvRSmmgChbAOuT3%2BZo1%2FfqkNHvAiRzXeOuzpvDtT5e8b3AbpCKpayFHMCrahdei%2BFH%2FCQbYtaqQbVLhy%2B1EULJP6V9fB4t5UgtRNWkHuMUuHbWze5DZYjfAsqXvxdDntvhM2HAwiaKGyQY6pgFP3aG6H4H6i3t4VHXkXIQVZnr8Ogb0rdK4ogzkrsu6q%2FcyS4%2FtxnLey8iYhBQNTfC8beX5losltAynXWlHuwuUTtYpGyskxd4uiSOyPlUAIvKXw1FavptLaqJ4C%2FM0kkmiyM7mGymISKVkWkhGa%2FAxUwyBqmGUZd3ORwtDQLlrPRZN5zhRg5VEpjpeF8HLYgJpm8hcNT9vqU62uf9DqXNuBMa81ab8&X-Amz-Signature=a36547ec53e69cdc9505100b92fa1d7067ca445428d6974e9e052bbcff84e998&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

