---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SYR7FWER%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T100104Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHcMrbjshJU%2BpjCn78HBNndRWaG0K4uidiCKwn8tKS%2FWAiEA8YGHNTJwD7%2FChel8g%2FmS8dPg%2Fp4jyqQcgBRmYeG8q5cq%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDC2V5esG6cy%2Bvmz7tircAxGVxT4nkgbPTaqyAXpWykj16yJOJAUuuSWbsfEuT2HEgnxuYVRMhKFpd6U%2BK9N3j4AywyZIq2G7Uqxztl3dbFtU2GwDzPSzcNbyZbnCX%2BB%2FOEKGtopRsNbE9z9ap2%2Ff4al9FmBMQ8y%2BjagHzWe%2FF%2FXATsmfewhyGlC1KTih3iQeiWWxDHMO3oTiw8My2mvpYI7%2FIUHK73Lnkwn2kSk0jMAxmCQjfjlTB4UuEMGOgz5jgiSt061A0r8bAhtEiuxMHdMMqugSMtzGKYkhG5qQuws6qEioKjQLBkzUuhA8eJc25R%2FpTNVfIwDqxkKSQTSmva41u%2BcS7Rh9c%2BxJa2rnagNdkxrxgxNUZel4LB0sL2V6geFqoMwSvp79pBTBvNAEdVua8EMrHAGalGWJzqeOTqTJJtW84ABLPWnyjaJUDt9UJsxvzsTF%2FgUgUb4lygdYCsPobx3TNADtNcNdToOuaGump1DJCY24UyWz0wZCziqccLL9YQBFXlZxVlmyEnTeg4c3Qx2qb2wm7BpFkmTD813dg%2BW9BrSPVxOr15WcTc0RCRQXWi6y6r35a8jGhnYdTL0xA5eRhCqxGWuhB3kpIKe82rltaB1VoIBr60AmHXwleMqutSo2IIffgFiDMPrZlckGOqUBktChLWl%2BzaDEOrs%2FJJZeJpHpPbfx1aPdJt2o6EPipYQWfrmhIpHul4q%2BD3040Bff5Ml4DSCDeTa2wugnoMD%2F%2FSPQg0oBk6N8k8WvBOQ08TD%2Br%2F5aozFl5Wn8G8Covs1JlUr1uedHISRgWNu%2B3tAKPyLxTfTeDDpTj3%2BgT%2BWWa0lRqCZyLhc%2B%2FnVFOKHeHiNb6n7eV5pzPExdO9Dds%2Bs%2BZmyy8I7B&X-Amz-Signature=aae403a3b128f2f4330b35bdd22e701661aa657ec56038ef93ccad2697b89e8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

