---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SN5RANMY%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T100040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQCGEq%2Bjcudng5uETDbAuoFMukIzvIvidz%2BciSrk3HsmjAIgYUqI3eF%2Bij31vJ5jKepRtd%2F6E4J%2BfieIxISFtdGoXcEq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDNDD8kBxg%2FCqiGkHHSrcA9JRxbAqXwpYNOBmOZhcIFrNZsg5upPQZ6gKKYwcfRSJl3FvqXXw2aYkSPihjN1jj2UAl2ik1xg8GWbpSlE357LQykiM6qqRIzh3obZder%2BKu1Xm2a8OLvTeKPP9xwhu%2BmCQhzacZRT0g1%2F%2BM76HKu0%2BxIHSy7p8O0LH%2BSnaGXyL8wim06bWkcvtk7CBesXzmlvUZmd%2Bis223FakbjwjSL4uX9oWnFxOfhrKj8iPTzx%2BVP7Rm%2FGqXDfnQMY5d%2Fp5qaaQa4fV3FSyTWgLy4Ob7%2FO9n6%2BYcHmfQuuQGd7sJgyNGMr8ItyR5K2uTI3mBiK5w1puxDp1cdTO3wqNRrWYMsPgbV1ez7%2FznHkJXK%2BcPwLaNA1J%2B139iSSU0fnVRWVS5DYootXfx1d3g87uE435Qpx6EjfUDvqh%2FxjNXUHHguDOFPJBCb0COT%2BRadXPRVl4zhdkpUA5gVByXuoOGHei6N1lQVlMOJ1mEITF4n2mC98VBpUP4YLI1ohviVeUEtYtIYYeKSnQbbhZ%2BU4StAqs4mXR6wRmMnfR6cl8alX3PiZ2Fp0B4I%2FoXbZvgpW9thc15cMHkHqac4dTxEC17MKlweeSd59umi6MV7s65xnDYWeQRKkkq5260a0vblhPMNTTm8gGOqUB8EDHoVqd%2Fmhz%2FY%2FZc7K7Fh2Sk%2FMPBF1b5nWNL2gNbBAiKqG4amXm8qqxjEdzUMl3k11IV8uKrPXp2ydrMP7l%2FtWSRL%2BQZtmkBAsh4W6hHOkJMxsatw3HSNguWZdsREKEfwESlwbMzFkkuVcso5z2BH%2BwlcqEC4S4Dl7LopeW99W7IM6Iv%2FQS4aERPZQjNw4iZmeNiHbpHvfzu5m3MsddMPG%2BrZNc&X-Amz-Signature=0dda1de409d0f5ab7ad4b7c4f9f9be8e32eccd77185b45d090b87e7f74cb6189&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

