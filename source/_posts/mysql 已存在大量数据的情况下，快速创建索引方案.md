---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q24P64HX%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJIMEYCIQDH%2Bc%2Bo5gKh2Hq0pu5%2FGVXnaDnvmBB%2BP1wZC10uS4VzeAIhANipd%2BauJ%2BBu9Sfnbr51wBMNXm13uYQT53%2BrkyRcmCOTKv8DCCUQABoMNjM3NDIzMTgzODA1IgxhKrCnGxFzhRYMIsEq3AMtajddmhz03TCimcn4y040GhZ88QCA98wkErWQ%2FwBjh4OF4S%2FalgJ5BruJurOeh1tri3lJ5OQFD7lMydfY2mas7XOs4jtNjzzwdUlrPsrihtvTayYYeTQ80urTssExHJxpwzDBDtwZmUq0yHWBWaHJMffyWVJjm4ULnJWN6t%2BUksNmQF9sifnILRm0l%2B18WnfmAA5iv%2B9Auo7rjPUWH1x9RcLsBstIQ8Z4UqVgu1DQIWHEuX2E4F06h6ZlzE5OnqeY8Dl63kDvtLNQYX8PZLtL1ayeGvWdA3Go8vzumfox58xWgbABwcq1IvCEuuk8MPPeNcnpsUb76euX%2F18oGQnIgt51R%2FbrL3n0qtP%2F5rsAd1RbY%2FWj4C2Y%2BEvBjCjgnhFQHPCs0G61l%2BbBij829ixp1aTtBUCNnNvrJIIWe2uR%2BH%2Bgifo2LXaZLEnNvlxkRhKkPFAOSQdDmnZFUC6Bh0y9prO7BQjBAYY5ongCiXM505CrCaXAkkMZnwJRmwoBe3cMzDhJ5POZCs%2FcWGyReIQzLBnbV4QnHjk7B040UcEDmyuKfm%2FjxcLVZCua6TCmk0zdFRN6JLhwfAoxs4nZgzlPoByCUFgGwslG8SyNBePHCpSbPsVvXaj1a0s%2F8zD0p87IBjqkATnS%2FKh6npjU69aUO0OiuU1wcTmKZbz4J0kdr2z1EtfwNc50Wpky6yJaltE4J91vPIDUUKMthDhEWnFno74U5V2iE4omVB2bXt1CeCFD7KBbgp9cTXitg8%2BUIHNqIwQazD9mgf8%2FMFAoamCTdVADMsc%2B%2FQNV8stWdVooqQPRW99LuDn9TN8OpeQRg0qpct6GiHL7nIegmnp0sunYyCRBTcDICVc9&X-Amz-Signature=7ad822b809b1aff358cdd4a80f1d386b6a5d1a9c2fc80ca97a6ed26f1cfa278c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

