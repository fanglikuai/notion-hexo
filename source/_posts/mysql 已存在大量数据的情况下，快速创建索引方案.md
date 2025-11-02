---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666VCDJGOU%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDAzLArQ15PfjLFVE2cCZU%2BvnxTb3Xfl%2FLoFOATxiA3ewIgBhykf5wrZef7%2BwPQy341fDXAjK4eNdzHCEFaktG6%2Fkoq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDARmG8AmPJE1mHQXFCrcAzSDURgOLJup5JoixQXkw%2FHi58rMu9bgdvbXAOsMxu1ndE5DukGyYL7ggUfCoIixvMyNooLsLJp9HhSl2xA32vrxcgJBE6xQ8cUJ1lwmJ8BEhUw6rAlqS1gHokO%2BPwiN49O%2BBK89y1fL27VxM%2BhasBgBywrFXscjs1KBtGKDh8kX%2Fu%2F1iRV2yGi9%2BQwTQJOYEDK71CNyDDGra4AoCveQafORkxb8bMB5T1PpmpP1v0UBtXvQmQTq3i9JSJmrvnTqmZqH7V0cQDkSFi9Ayg%2F%2FTwqeTMpyq9WAqn6m7nThFUBCFBspXxw%2BUAtFtvwVowklA1oLSyjDcrjrKIiZpVgbe4adqRYq%2BmM%2FNtjWQZ2Upjzxoz%2B8zJxmqImc%2BwtogX6XkdHskf7Wkc9NfAonpFALaMuaxhU%2F4p2iQD9ueN4P3UymHZZsZW4i1Y7Qujm9IyO%2FRZqPULCPVl%2F0BB98hD1Dq%2BsalXu1Rt0fekvAKeZ70uV4s%2Btk%2FMtUGEtjXKZygHjLIyvOO07DqAfSZZGQ9z8c%2FeJIy6UiD%2FtfPpPNNDWk%2BIOZYD7tBtervFrCbsR5OfCETaIXLWQlMJ0SwAzfloJf4Nas%2FYnvGkfTFfNAohdTkmslPx8OGppYqj8OI6T3MKe7n8gGOqUBshIg%2BRnRVzRsZL7CAbAdx3tHT89H5cA3Y1tsWLOd5789f6JM54IcckRLWB9g3D5viFe47nzye7tarA58fVB7Iyv2po7nmzGXjKwBXdyd8sgXkgS4Set0GOe52EQ4%2BNepm5WUBvNg54F27wCPSIvlxqb1lDHVCPjqZNn0l9SJOYFn9PUDUhRo5XChir1Q09FofJuFyu5cesArbWiXOXrwjgB7WA2I&X-Amz-Signature=254d1185fcce3ca308cf0b938c3ef029bca9364fb457e6ca3284f3de440276ec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

