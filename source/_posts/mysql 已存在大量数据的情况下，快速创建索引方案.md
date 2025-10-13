---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SX6ANYEL%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7qGZUhRsuqPNsxr7%2BdhmaoqR5E8vSqq4rig67r1dHlAIgN5fPwCNn4HwAZGRKPt3mKqAPJjGD6AQZAbeo0V0Ccokq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDIP5Du7YfFO7BFGU0SrcA98Dx32FIvX1%2F7FScEjvhZipfY2fiINs35jJ%2F6YEf9YGVkK2KsTq2lFqAPbdbQw1W7kU0YopOeL6kfUbgCTceA1K%2F952AtNoPWnq3DqWv5Udm0nMGS0Xpg2EIGn%2FR2qIFZgrj9xLtdUoM9NQx%2FToLAVXOVhPPYtoDItyLvEZMdPMEynrlASWZkcCvZlZ%2Ff9HpxdQIGe5ZkuvR2WCfl9q%2FO4iwtqg2s4WEvM0axI9GqRNFc3fme6PW5JSyRARE3q9unW7ytbr4KNiFZJj%2BjmuitemDu7JOLGnrgJ3NcGjx2K36TXlP8%2BIXYx4Z4DnZEwKrz2MF2d6NxtkTIIjUl8c6S6JgSm%2FUV4k4mfA4DH1YKfeEUZyDtWvowOois%2BdXz6TE2Dvg0N95cNlmp5hsLQLKb%2BD2lYDwqdcsugZlCNmWQCiw4PYLMdXqHWVZna9kVfSGHhI%2FxDbOhkCHJFIAp6d%2FzTVcUPtSp6cDdINupGf5ifvNaNFgYvPUIApUXfK0zTWwA15hM0NlcMwhphg17sPibVEBshN0rEQ03MRSHjSBMZy0B4k15aVrPGAaMaEq8GjBHkGnsdroj3tmYZYNBp0pAHj6BbJXRgy%2FN888dXq%2B1KKaV1ugTCcAtBqUNynMMrztccGOqUB1Kb6M4Atzcxw9d2mCtdDUrT6msPsJBO6QEHpn0mXoOQI9EpbxQ5rgPWuiAZ4w83UCmuOAeJoJQW9Dgqxukh5vWxO8ViihbRIaVmloNicOR82kbdMsIE0K0iJGxF3%2BMV7JNJTt1SN%2BQlfEVwVf%2F%2BnsRCUSkxPmZ6icxVec5gKYGeQ%2Bk6f%2ByYr4uwbleXcd3cfC%2BD3dfWIe9D%2FdIbNRBdmOGS%2Bvojt&X-Amz-Signature=f048139f818a7df58da8d0739f97d3e4906fde260700193c7144cb09df913bb8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

