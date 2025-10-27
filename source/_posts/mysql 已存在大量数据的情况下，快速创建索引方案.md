---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIQUDSJW%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDRKxi75tXMheJrLS0QKbpLpdfe375%2BAWQcz2FHDl0iGQIhAOfc4vGQKpGByqR0XlaMk%2F07pIh7ZNNWLouHbO8prJKMKogECKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwy2Gb38awfSTtkPbkq3APu6mlkkIlM27sIMh0GZ8I5tlLgGh7C5aJ86dqfCpXS06wpPpmjlj5chk1SrRxFq9CI04kOfhMkWWvl8K68jIpxqjd4or1Jpk2j7F27G23OFdp5fgD6x0AMUoFsmEbkQdY%2Fk7Ta1H7UNKLYh5z%2FVR7D12K2qLGiWs6m3EOWHKtGxnLe6IkRhydehh3Dflf4PpMeEcbLt2zTv9j2ZxoiYX%2F3Fu7yS3x5DRXQzBFsJum6RWOBB5MmrmBtN5TL%2Fgy45iIih2YUOy7zCjJA5IJErDM9T1ZN%2BR4C1wfzfhFRG1XJDGy2oA3fFSHDY%2FTATwPtN8TPyusiR1yI6Iu%2FZ1Y50EoYn46If051QcK0xHEk6kV6xLoKPwEJgwfQkmReJicuzBS2zLkgm4GT9UdHIxoolVkqPqwsc2Z1Kx79%2BOyCLN6O6pghe76dieMw%2BRD%2FxxFvVsLu5avdfKqWftVzSLe4%2F%2Bh57wZ4AAuH03LjV%2BvGZOGhtdBxDPiFHVvMQjYobTIDCCGBJu3mbDG7%2F18eAmf3GuMG3bHQwm0kcnRX0Y7J6rJAS9UphUr2803u8byNddN%2BYPYPzlTUTYv6XYWdMgwjlwDLGlEgaVRFpCVbV5G4rafFCg18uABi4gsXURKtXDCw8%2FzHBjqkAaIBMDuZDwPX74D2y5qbkLXNHId45E%2BK1%2FV7SLymiZBB3hFlO2es8FGyLyiEAAoXfdGZe1CjDQ%2FKUnr0qHtdAMdW3FH2%2BhHxtWNH8VS25R%2BFD4jOnClG6VkUSyMusHJLXHGPFA3P%2B8SJljhlVNv00Vs%2FgD%2F0DZrYpp78PA7o%2FoTSXLdTWEvwgzWGZPaHXwCccQdFVQ4YXddbbF9t%2BGpQ8zM%2BbX%2Bv&X-Amz-Signature=83a9c082553b826506decbf46b0918d852e7e706f6fcafe0ebcd38ef41256e79&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

