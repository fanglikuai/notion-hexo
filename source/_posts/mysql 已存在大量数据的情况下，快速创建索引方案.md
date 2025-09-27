---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCBOF4RF%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T020038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIB9we0hhFrNU0k1fWsNWV6YXLuufXT3CMvJY7%2F4h46xjAiEAobqK76bqywEGGgczLngXSy0DyhQjvccgVdPX0sQuCysqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOalcfIHHv0u9LTrKircA1SXUVZglrDatbb6ccZcsdwsiVc6Lp4Ro8Ddq3NkulgqkxnyUNsuccKefR5UyWoHbaDHHXzjpcKrIoad9oCMUeSMB20q2MVIfS47lueM8t3Fv496WbG2Gmq1ki3E6zUiX5wJgRFmlJ4MTDQjHsGuT5PZTMWrgmeT5TTsQP3634bGeCfCFv4mw%2Bp6%2FWDKUNixcqTGKLExLqhLefG02GGWhRqkAiT9sbMCKhWgVz8ZYLHR8hcMobHjRNSNwZbw3N79I6gTlP3NwgIEiJAvkO2MdHkFJg5YZklO35cUIq03QH5APMobBLcxqCIWboHNBwxdVM5TgYWeSWK6ALwD63RH1XsPS%2FrE6BX73e47AAdTKJ0B83iCtvaAqfAq9%2B%2Bbg5fxidl7o5nI%2F63vNGGDy4nHZZ3eA0eAL0gpPxCHpGhocwM6YhxWMheV7LJcFAnow06A7nJJ1DeLNFMZ6%2FVAr66v5Ak94iw0tMFowFzSgv8IPTtAUqxuPrBnYkqtdPkhRy9v9AVqM5VIStZgL7tnRdV66KzsVg%2F9RykYZg%2B9miS9bVSBcsO6ST4%2BaWpYmSCYFJy0lTk46f0y3bq0flPN3YLRX24GJ6nq0NiQXIgYse1GHTUPzns1E4EsylBt2KzGMJvx3MYGOqUB7lStwUczK9t7hZ4zth3kYCWtnnJwUsTnUWTN7Tg1wSsY4p8ujCWIwHyq3LEOP1Riu%2BToXZt3jBZcF9hcg45gG4xyqKhoFaHkXegUvgNIyDLiaatn5vF05J47pF%2FSYuQ5Ykb23ge5GGxXX4Hs45sKtS%2FM9JyDgfgei6pJoCcbLQEkFUpUvirKXoQxNAdPMjzmHzWhIgjX7r3HIezzM5hqBqRIJz6d&X-Amz-Signature=632d9e59fb7ecf39429202b83500a4905523b7c17260978b3e54ed29a7e23201&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

