---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UVOUVVWJ%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIA5Z3kQhjw%2BRXZo%2FZm5NHp41duiWOkAAVRBkAjqfBmgqAiEA4biouI%2BBuoaKrzc3Flcw2MzIWbxIGq7kg0fL0L2nHFoqiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLwoIu4KxU8u2F2vVircA%2F3kWqTVTTJ7OtAH%2F6Rrex%2BN1uAtaNUy0OMDdnT20aeMnFlP8TQKpQ%2BwVHex2XcbZ9KwklKoXE1oMFNx8HgbHU0bD0pSsBqad4FchzSni0%2BeyZkcGVqwlcr0gWGCpBPbEwAjNC4MyW36L55T2zSZ%2Bb%2BHlajsViyYA8MO1KykH4TDt5NbKr8UeAzecJy4uLzmvL8PacoBzFrF3QKXOwlR2NJ11Y7PQ19LjVHZKrNlU5PmGJQvR1ioticybMOhSYXGatyXvbEPuCJ9Lk9iSY9voWsul93GN7%2Fiz5jUV%2FcAFutggT35GedHZpcmUFGvbXo9ON2Dc9WVnidfvNDMtvnWUdzD0Amepni5p76h91UrUf1RS1Laa3f49AejHsL8xdPyL52QTCynAt7LgB8pvr7deqL81XeiSSyKs1EABrEjbIcU7Kl2TkfF8PcGUGKSJ6sDzv9DjmBK31qc81z%2BPvwpi20%2BRyhBuWEXasXrdishqWP0CRzJyiv%2FfvKX7bICmyO0RxSaX8usAC4ehTKtVZbOQA2XNKanip%2BHcmBgGjNjrod8pm0Z3%2BVdVYwKtchEu5fk5H8PznFzM8aH%2Fe9N82l0mOivCs6lu3EmB5r%2FYvKzLx%2FU0Ofiu3JJaSSgPwaaMPes%2BccGOqUBIDBAj%2BSyHqWzira6mt9As4rUoolNYA3Rzmce9upY1KDgS%2F70Q%2FssfkYVZNX7nx0Hp8%2BGy5FYyadL2S89z2wahVy9ql91WOoHmcGYWUcwAQ819lyo%2F0PzKspTM8W5xXqryUBYoOAMmo0Q2xc3MJ69RzO0wIq8FBdOEwJT9uVdGd%2FDJrJgL74XC7lyP7WchWLwwjhfSw23jTZadHwAKYFRcqM13xbi&X-Amz-Signature=dde82bb80cb9a0d2f20a969b6e3a419eda00860b2e4b065a5ddcf831583792a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

