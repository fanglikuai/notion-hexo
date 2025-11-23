---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SITPB572%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T090056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQDYYQ2r1Affc2N38Bm7xfXup2idm8HFjx6TNMi2SGhV5wIgWm7FtwupgUSs9Hb3Y%2BACzQrPAiUPyRDMFBBIsUsUd7sq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDG6sgehEWA%2FwE6bpICrcA9PK7%2BIfQ8wuB3RRMBss1kdFFkNc3pR18tzTwB12nf7nLvuDiLfgGgbeMB9ykF%2BwPMPC36hEnoAxiaMqKddHPZUy4l3JE8A57FvV9jdcEqYoLPE4AWXPSHR79088RqWRMUR3D5hvU27Eh%2FKOA5GXo1KDOs5iUTODjA%2B7CcuoxjwQMBx6lTWcXCW%2F0%2BrPoPvjq3lQRIXLeyQI%2BVq6SIijB7E3eSt5b66VCoRVyEqacqNrSdUIrXMBPtwZoykp9FNLrnpdShDh8DhIvOphXHpSg4eRW%2BOzeXWfhqWYu6QkummZnLvDprYhlHz%2B%2BRGu2hxNkg10J3q27gHTF%2F62b1ffMnS22HUwhR9wyno6s3otjaERlTggaudNmid6Qt7eL53DxCJEK4LjQc9NzSHy7T202h32I2pR6R6ZmJPLOMMSc%2Fsn5VfI%2BDyQTZoBGKgho9O3L9of9F4lcgGyQyzIsIrPnz428HbWrdVyKkU%2FHXkBnyGQiPW%2FPLIcfDmtZgfdP5c4QY05AxHBbPmOUGMOATt%2FKRFeejifiYilHFydOzpZ057TAzHDLQ1RDiMvm6gELLZOZK%2B2RhhQ3vlUE0WOO6zoOryLpkhod6tis3o5JNUj0NVyALHXOBZUmZ7%2B%2B2fmMNeXi8kGOqUBV6wJSTNu1jf9MgD%2Fu6zH5q8rFmSyt3Dnz%2Fp1h6XiyHgDrZFLQ4KvvjlqaWl1Uo0CO9i1k%2BojqpTbH0wChyssPxGqQ1JsMSbNF%2FDfSTQ6mu5JI2BYEccDagoH%2Fpkjx3fWmA69FaxnyHef%2B%2Bl4NF9VAYy6D4Rkhb9FQW4sXQd5l5MusrST%2FFv48puU%2FWMhJnOYAWT9cDbDxv5Gq9IhJlaE39ocgdNi&X-Amz-Signature=27e82ec028e9f842296b64026c59b02c1329f9994599295f62d62d9e87ad43ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

