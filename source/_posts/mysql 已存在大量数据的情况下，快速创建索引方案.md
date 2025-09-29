---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TH5ROVTL%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJHMEUCICSgz8mylNQv%2BKA0rzRg2x3UCqGkz2R9Zq%2BCZVhL04wRAiEAn%2FOy8lNibW8UkQ1zO6zmsGCRhhbMd6hi5bbXS6gMF90qiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGngDjpED1SK3konYyrcA65J4lO70%2Ftk33wXq1zR0%2BWJLTMyb0RAInNfNnaEf576hXjFb2MTtL0q%2B66SBMZIVXC%2B%2FeNlQhd%2FQButZUxSoZPxmHqXbtGDOG3jPzQP7dTLsuHJEp8Ht%2FHNVSX9rht4zgBH7diixfA%2FqXREBXftE7FvnO%2BM%2FxgobN6GtumMQfr46Gdlvxp9xVGaOatRXYxXArK1K7cUvq%2BuXyZqSFgKbqcmoh04kPtDFOwkGmKSHdneY27pCBr8VAvvg8FfznUZVp4FqhAEIXAVpmWa5pkbdNY6rpptUhOYxaicrxa3daMROspXQJ6ukLtUmgumnlvWB5gQVUXSdpeawfLxvhf75LRM%2BRen6MhywmocImj3L2RBbWrtwP23FAmORQ6rOdWiy3wgoSf%2Bt9pNepsA2vDq3DgqT098ADHLxqomEvbXr9V6s9ZOkmtT2lzaN45Wvwhd%2BndIdU%2FGJDI5mn5rVCHAai%2B%2BG8Sl%2BNCm%2FXtEF4LxFnFcuynoLHclJB3R%2FJuqmZwT0qYOZM5WXKVxx03QnCoF4mFInNJ3aNmv7MjSX0%2B6jBWiJscMq1L1BUrAHWfcRGBw7%2FTDEYeTNUNSPVcTbt6xHn2AvuXRrxY%2FxuXZC7mUXO2us05i1q4Yv3%2BQT1D8MJfX6cYGOqUBmJKCtOETd9CjqJIyQ1sMM4CAOU54ZU5Fy%2FFkwEhq8r1i0dIk%2BWZcK1mhCWaog%2BZ49xf%2BA7ZeN%2BR3tgahNL1hV09Nn%2FZoo%2BJRt%2Fi14XZ1yUcnNq3sfRnRG%2F%2BMxD0vB44x1QGQicImlZwD04pUiTy739CKfpg7FptYWlK%2FHgd9g%2B3cN1E22BIlU2QfcPb02K1JA66y%2BHjybR46HaHeVG1VNwhNH21x&X-Amz-Signature=4478be730437817d38d5e28ba7bb1dd74d82761abc969889000b47807f1d4d83&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

