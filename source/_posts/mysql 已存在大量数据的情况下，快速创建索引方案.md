---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7JQT6VN%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCh%2FNN0VHopUgYkOzfddziOF8ofea75p1VWq9y2pEX9TQIhAJuOeGuXvmSU2ey5s%2B4acrZWkr71v2vqJmY8Yb5hqHHxKv8DCDEQABoMNjM3NDIzMTgzODA1IgyAGdQpYnpcnf8r4KIq3APg3MfAFpd1iBpMgy3%2BP14kJA6cL2ik%2Blf8b%2Fcvv2tmkWo4P2gjKkMNHb54NjiEgfdraRmPaSvF%2FYM%2B1upwklNMAq4riNjejw9YABuyxEK%2BpIFSyJKGWBcG0Kh9rYSNrq1Oe%2F7k9jBg3rBUdE%2F0kPrU5M8X38IrPORy4ceZlTZjlW4eENdLpaXKl3k8fVSETVkv7ZPE9ZpomrSmjZtLkccu5Ag5H2zbhSf21dyZEu1W6mOmeQ8eKezi%2F%2BkiCk89600iTqnKWyOhUobYHYAawFaU8HThUEqA8GfR2Rx9cfZoU5io5VuLQcsmmhYV5N9ROieca4d2j4Vzlf0fKESBHt7D2jfb6Wh%2FXv4qkIycTUMiu9plhNe4oKrpNEL8uWDyQ%2B7J%2B5s%2FDdMmSMmrViSMh0hTZFF0kcQSSZXzgiTMatngS93rEntEuVYMoe%2FkGsiLSWo7eT7ZvrSqyhQS4eD6tMn6cXW5bkJaeOSOcBNjyorptaXMCoGnDM77TYtD2IDUunmusXyYuCjFR0gmB0capRMm%2BMdLdTDoy9mjyypHBt99mKfxEd2Q2gasNq5C7lSSfcrU35Kn%2FWFnXRO%2BRSnAvmobmf1oTruGsHupEbpemvUSD3yDDRgHwpkcSOmJKzDo4cXGBjqkAaZCw0emJW1o6f8WZLJSe8FvOrGDQIETjSlgTHndAlDJQqJofbOURklenjYoqX607LCFu4nDDxCSnwZoT0Ob6soW2QPKvxj2j5gV2vOqIk8zSGJD9lEZMzeRUd0kCTMAQ7S1zZRdnvfU1Fn4nJserJ8dXu9EqlNxWf3ozQ%2F9Bvo5sBq4CMcl6BOB525CInkaH840lCcpXYZE9XPtX6zyOcNkOX7q&X-Amz-Signature=89af5d1a5d98699a7342e8230a40714b3fe7d1ea9f6de782e40ec9b94da13422&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

