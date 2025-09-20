---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664YFVVJMG%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T220041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJIMEYCIQC5fY0Poujfea4DpuByQreTwjE8osayFOB%2BrFWVzv6N2QIhAI1tQjniAQowgqJHllflDS%2FZg9p3k49zJGAb47tp9WRNKogECPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxT2QeEfz16Z6BLtRsq3AOsoDmy%2F%2F%2B4vQmAf31QwBRh8iEMjiZ7wBTjyaXrT1fe7t9jSWJntWs%2FsGFeFY9vEUTjB2XhT8pkqs0N4VMSbhd2c4CP%2FoZmYfwXhrVtglP%2BlLNuMdd0wKtucCGKDz0m%2FA9BG%2BcO5i4HEuFSEmELByPwWRLxAQzNzHbD2r71uRYyK8NiHOlkgOgksOa%2FXjAiqelKMEiEFnus10O6aAzHWx5ZITNMck185dJugLW0W2ZQvE5W6zvMjlgrPAk1FpK5Lrc9wAapESbanweE61h9KWAc8MULaOxQ6TXElMmeThTy0sTSmU050Rpze8SfEqDZ%2BvG%2Ba8PzhAd5FrFkeQUrEPXp65r8jnMXi8By2U2939yRZd3UNimUaVFhtrGhI0Ax1tkdh3s5mKXBjhzbuIvZC7dCCJWtRVFjjQFjz8X%2BS2oORSV%2F7%2FAXEN%2FM5pQ0I1fjX4BVR4vRNKqvwgok0wlwv62Ctvw%2B5s5yjpFZ%2B26CcIWS5OeRrqwb%2F7UZ%2FJAPFqV0ePd371yT3eHkavRBge1tF9CskAneJ9dWc%2F5BjPL%2FG6SffhEB%2Fi84Su052laqM2jz10q%2BcKInNG6YpsEDDd4aY0AD9rZhvZrRPat76KAGhRi%2FpC8w388BFCRwbRPTtTDIsbzGBjqkAX9JBfRtm8PrZ654tq9iM9ZIDx%2FSP3mBEQNfWz0zHhv5K5knKyjAn8Me9qJOOYiYPNlkwVoq87TvTomjmvHlGvD%2F42gzAaDVmICBpbKRqeyWZjr8O4ZcI5kLpkcNtLu2WPq8bgqZMBqgStf5c0vyvfk6%2F3XDNUdcuHPZH%2FmVLxr3HVLTcLeH4nhiOU3gAMNGQQN3n0l3YIBbdgUynqCqVeFWF2w4&X-Amz-Signature=68a830af0434f06c4f3d7751bd45f74eb4c82bbef24a4fd90e2ab6af809ffe77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

