---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666CVJ7DYS%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJIMEYCIQDfg8VLiDvFh5IzCtI61Lv81oiWIEILD%2Bj8Fm%2BBZevhMgIhAKzckQ2KeIYkhuxbCI3ztUKgR90vCzF2odRfS%2BrbfFnNKogECJX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxBDcOtTvJZp0Sri%2FEq3AMJgiU6dOt0c%2FT6rHb7aB4yqK44aGGzLiM0oHjrRmKkxs3WPDHO4B5chqXrM9wdUtySxaEbxVRWTaNiQNsenlhoRT%2FUmVYT8jXjDTU8YuFiXlRNXFJbFU821ZCy4%2BCRFIxJSgistCq%2BP0EZd0NTVp2OgwnZQMbULK8i8FqOmTCrwtlH8%2Bp4woa0W6yU2TnmQP8FYw%2FgGEYgajrbcgwrI%2FDy%2Blq2JncSO1FOanXrAzQKJ1ZZe5njU3Dr063yEW%2BMYaRTRwix3o0kgyZRDarSqyWdrdU6seA%2BnBjuP%2BeKfYo%2BFwAqxdTwFIvSAWpEW%2BqIR2Mvr1tpMC%2F8aHlxhsBeGX2mut1lET4CvZvqQOlLDi67iYlajleLWqUso9wN7rnixZuAIWdK24MuP2YN2JogdwHmnDMsV5jEePRiwkapSuSf%2BcizHs80UZjGN6DHdpvywD8m90%2B0u5a4Qp8npVtcirJ1IHhdgvo1kom%2FviKDPuAQg7HRH%2BxqkFJQh55MW7txvhB3eUKv0Br7gt069D0unm3sXY5v0ZED6XBOIr%2BFLdXQOlGVZsvKsrVW6EuRRLcR8%2FhhnPg4lxN4Au8xnpKid%2FYHHFBE2TP59BfQwOQLImhxKpZ9ZurtuEE%2BDX1H%2BDCy2tvGBjqkAbhXefOUCmUzQXNFKeB9I4AsZpFyh4t9EAhl0h9U5jT69%2F6IW437IIho3bSK%2BZycpFwIPhyozp5RIznI%2BbI7nLylzggogamhBpVT4f%2BNfhT8XgzGWFuyfBbAVz0R1pFaOy35FXC4hCSvuECb58DvidOdpDHZz4yYozwyHE40xZKXi5eGMFxsXABmBPq%2BaMXtKeQcWQhHIoP5toBbQzVyRpBcKSDk&X-Amz-Signature=9cc123a7f79778f45014e8665866b4cf024b28c1e99d43779b3fba86d2867190&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

