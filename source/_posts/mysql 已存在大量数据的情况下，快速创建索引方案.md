---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663SZLWJDQ%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T110050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBZZas9VgP%2Bkdcn2kg7vpUYNGPxZMuU8oOcXP%2FbQESI7AiEA%2BaE3RSN6QcHWmapQh1AKJ3TM69Vth7WhSR4DYnRLD3oq%2FwMIaxAAGgw2Mzc0MjMxODM4MDUiDLQalkTrocyCpon%2B2CrcA9WeT9ZBt4gJS%2FPVby3hTXdxGUgCOYedhdmprdeIIHCzvBb2UBLWcHYvhEzkg8m%2F%2Bjci569sCFYXm%2BUK9GPU4fL3NaJq%2Fz2fb33Ed0l%2BW%2BHDryAIKv8pEQAdLcMWzaGRnLvra0VsEf%2BF73t%2FA%2FHH3Zr5Ke8e2n6elzgBQZYAj4Yzvum%2B5tcu%2Fu8HaxUd5uH6d6Z1Eztt73J3SwTGyVLbkHA5LYoV3thslOZBqVDBV%2BwaI2xhgGZDahUFFbnXtsbBrqAI88GbusZqsNNwo87Xtz5ZdhmKWfsHLCZYRV3e1Sv2UCkdxxHidunlJzQ5G1TWjMD2QCxSjUE%2FAZTNKZRttmXM3rBciHroYr6wRjxFOTjFDbWl7qEIM1VGDAw80Pht6PLx0wZE2%2FynwhqrIFjr7g7iOptVVMwdnUADoy3sosDNE3kGcGF3t8K2ht7cYISIoReslfeD9xS6%2B17b2uV1oCwRddkn200tgyDrhP303szComad9yZakzPbaSla1rxzMXzj3YwiMhP4iGOZmiKMNjMrxsHPtdGWqeIMwlWdkZ1f9zgbGCyIwrx9wLYM7D%2FlSx6MsEMCZOfe3uucpmWN9UgN1ufAM%2FCk7Qs8CIAC9TXKhdoWKETpONp7Ab7mMMmElskGOqUBJewfJ3GwfvBBeIdyiGnZdlFCges14x7muDcvl3uu3l3a2WLoSB%2Ft%2FjCPAkRVrT8hvIIhZ0x1%2Bi6IA15KL%2BrDkDZfuNoPJBpaA7XcSc3%2B%2FlITmy05ujasa3hAP3%2F8d8CTh8fFDcjQTeXKGl5VGhz5KsQY40WyCKgH1kWiM2SF2V4%2BE2Yr7cGfN6ENgJdzA3YPNqg3M5rXSSWzJDAghHO1ay7sWkGq&X-Amz-Signature=782a38e8c2b5dda476ff60c823185d24a2912d4f7656c5dbab95bd9ebf262667&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

