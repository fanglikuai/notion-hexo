---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCCUXM37%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T060041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBWqav415LYSKRgJyQRSrIUniPo%2FKtErS%2F5jBAMy3zlQAiBCwV1YDyyK6f1phkW%2F8l6HxQ0uzOK3gX1S1XuT00TVpSr%2FAwglEAAaDDYzNzQyMzE4MzgwNSIMpNoUvEsI46%2B4ULSUKtwDodENnDiXuPUBCvbvwtGA%2FD42IXmXk%2BSqFn0f4plWElWHcR%2BI24Ie09U7WCkhIeAbGZ%2FEy1802UpFImpdeEabk17%2BoCRZCPiIhXcjpSnzT2gcT0%2B94eVmfwnrHqE6nU0Szt6l46VwQWSidvZsaq4%2Fex15Avam%2FOyDZAbqmRQeykMK2POpLuwKrq7OHwZdzocf0RKFAOyGALckZ2vyALYcLa%2FkPRoqT1IYhzzgFYwAvEcO5bBnE8z5L0WPB6qb6b8FEwTdYhbcsP8M0rRjn7eoomF5YLZKnh7Dt2R2Z0fVzdZ%2FNMiEK1sVJvP%2Ba1JF9t%2By1TW1jzjxxho4tNJzSmwAXs1e3IdeH6%2F07Ge427dhyHiihbYCsYwA%2BVaXHvdHPmkHS644TZNmcJ6kJAD9d0IQBcNu8hZa33LtrX2XFSVv4ThzDfG35AtZCI%2B15FSTziz0%2F3yhc%2BhPWOl3SiE9%2BxxOW5ua7e16zksutT%2FXfuvnZolXAlE062BApP73i5SXPZ1ApsUy%2B0W8PVEmo9TOBLUsFHuM4T%2BHt4Oy%2B2YfYF2524fq6%2FFCLjUKN9wZF3yzG1Dm3GJ8V7S18wroVHSpciODla1bCRpIIQqlPmQjKM7CY66GXpg%2FT%2F6OioC1roww9JLDxgY6pgEZBXfye2g8tYlMcSIRU7GP2ik7vIsdnpnvgUGxc9NnR4%2FOWNINYPaV11MLaswv7k1WDKmC7RwwQ%2FM2ShQHm4Kc0q6ZxxwCweO3%2BIfYXNCtDwu8ahXiXnuCnpSTUS1TbpOnS2HtTnzJMkDMM0pCvnRpn1CJ1niSUBUnOJQTAax0X6O8Jv3Pbskm48lBmJapWJotx4sFtppBXwQ0PG0qJRM7aMg3wYV8&X-Amz-Signature=e04deca4c52f787eadfae2015e239ce285b03d20738267efa9914563020cb821&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

