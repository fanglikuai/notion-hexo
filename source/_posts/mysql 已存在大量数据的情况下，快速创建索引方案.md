---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCMDPTA6%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T170049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC17y1LfvcEJ91Ek%2FzAKcJ1fSf5xDVoRwMQhoZuglydNgIgSqx4RpzfGrTY89TZtYJUaHiLQqWeWsm4MUJmDDCqkmQqiAQIqP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBV8KXmER%2F2J128nqSrcA%2FenPiCueQoFJ4HPom%2FarDB3NUEqDjeyTLkwF4qK5CIy0a%2Bhsfn2Vmd1szOs2RUm53lJCJVVqJtb5MEKf6aEzVKZyGeYXLPV%2BuIi918DMYhf%2BaKLCQWp4uQe2hwE8xIZrQpap%2FifhluaDsk6SXiFAPODL8cf6xmCDNToItnl5CvifHD8i4nqD8DKJ4u1pu0dbhuk2U0K81zC5RvlnJRtXV%2BRjFFn53yKBX1tkSIBLVnXCAcuuoyuvz7M1xBfZ5w0SO1dBd3RwAXq8FmAHwAzfBwU1IWdHmv%2FhlFKScIhOyjBtVJtMxXDP9IQ3mjQdg3QBgR1rid255AGN%2BPP0tVEYqCEbZGwO2fLA4kOdFpOL1LXO%2FrWfMbXr7atL8BWSvwadLAUM47VmFXBYG%2ByTAP4R4%2BWOIL3Pq3fqp9OmK6qR7RPiEn1zxF%2BZaaGFFSkYFK28rX0vkg9XF%2BbRzl4DfSSz8P%2ByfRWD5daYAPID9G9ED35HC3uucpRx%2FudF1KAWPT49FYaCjeg7F%2BGTZ0ZIgORRdREj8NX3OKasRK%2F5YbbyHcYnBEhgkENzGcXbOJa%2FzBA2WOJTy2LXU%2FG5vRHmZIB5lsfP5AaxOjPqpOeZpkBw6Kxd1K2L7Z%2F83K3t8LhMMa2yccGOqUBH2E86OpfMHXkFj01abGPhIKQulTjP8DSiKLZttFS29M6o4Ts5xeUgP01m3ui334qZIpzLiondMRBPq5ea0maI4xbDS8s1xQ5h73khGoLQMAhv6pdPwQl9GusIHMN9KXB4QciHKTHMVjhcxB%2F79xmzE4KWEM3Njn77%2F72eW9SBIQHOTSWM0QKUMKI22SKhiH0Ot8Su9dhXVa%2FJu%2Fvyqb5WHFRjXxn&X-Amz-Signature=3e45f985a465d0afed8813cd76f2b522ec5733525bf4ead7cd9fde9df971fb84&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

