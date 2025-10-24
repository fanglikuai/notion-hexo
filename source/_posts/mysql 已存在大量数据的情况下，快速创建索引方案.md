---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVXRJI4Q%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T080255Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDWKxjJ3DKfjV44mG1cMeIZDLAnxzGwz2F8BXUwQZ1EkgIgF0SzDE%2BATVu8E5K0ak1RaJe6rIwsdsyMVrGviI7Ccv8q%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDGFH%2B%2Bb8maMhvSavdyrcA9WAVsfaW2BEXU51g3E0Uk8G5gVnJvephcQZMG0E8rWSVgd3CRgPJwg5GBcnNSdWdosTXxFkXNSKighkbGQ%2BYvfDOEmUbqvdguIS8aA7W0BXSFWEoQZmZdasWi7V0pff7uqnD1qONMyRRHNFpjfU1FgfPF7sPkoXGHgQQB6HXYxmReJh1sWV70o6W1gfLOfK7cxvv%2FIlR2DVMJyFThUfVeRGivQooG98xlJVBzz0Aw2vXmKDdDaei%2Bzd70qJhgSBAdWX1bJ5km9E3wkdAZZX2F42f9KNZBGtVCYZiOHtCav3IzTp4Drf5FPGSfY8bsf%2BT2s7ZvoLMB%2FZNpBLGG40w6vBu8W8A4hUADOHcwKNeyKFkYrytxT0ogcZdDmwZt%2FFTJX%2Bcm0GzEiynOw8GHZdGAE39q7xmzP0HQOdX1bLLUv%2F3F5QIG7gTbCHri0A7HyjqC6ELo7rJuR7%2FuDru1Ur%2FMoA%2Fg5zF7Xg4AnyGyaYxBee%2B2APmt8lBrJhWCGBYxvkSyRwwKtfyaf2LfQsXGiGRf%2BOwmY4xDir9cYKxyXk4x%2BK%2FhESd9lpDFfM8H5waUNjJnsz0pAddme93esHwMtexVf5yertMvHQ2trtxVRj5KA2kK5s0jboqGtWQ9KLMIzP7McGOqUBd2khd38P8PLfqvtE7zGiuoNRil1R8grx%2FApjg%2FhSndsaz12AmkAZzQF3nLQ5gJ4sYEWXNvtY655GznH7dqfbcdkWSvJyPrE7ev3ICGef2J4f5cQn%2FHSTqgNCFj6jfFEq6GpUV%2Ba1QDXMkCaLTb8ilzNOzvfV1KvP9otX7UrJ3H%2FCpgJE2tXIISqgerg5JQPRpfaVB9KRihJjLszSY05fKlGL%2FuI6&X-Amz-Signature=a49f0a80aefe0f8edf83369ba560bce9274133826001ab3976e8fd1ad9e46caf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

