---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVGPEFQB%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T010128Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQDWSnm50rBvKvkemt0uoWuGflr0F%2BOzxitSyTFP4J%2F6ZQIhAOtJGrHxcHXeDcTdlT%2BaiQBUJekYa0QMy8rv1IEfF5AkKogECMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwtDgnWsFEaOEPgXK4q3APWhEtIF4r%2B97w7kwjegdneIRr62MULLwGfwIta4wxf8iLyToeNTqqOKqbxZJdJJEwfZUyVXcqdg3rtmodIILBt7JezOf3Ak2hmL2p852p76oTVlIJsQO58%2BrbO0ROJfiO6bBK9NoEo%2BeDJGYKv5CHNPNpEVN7gmgQ90pQntYSccJ7YQfjExwq1jmJkwjJaxaS4QXnnATXJyg2HTi66dXJf6xOtm5%2BxHVU5nNpkkX00RmJHus4KTzfPoxS6l15wt3Cvp%2BhNrm2Xhs6BzjRT%2Fkk%2FxCDH669qfTAqN91jqEdB%2FcxgFHtO31UMiOiKZQlmka1oVlp24HUzPDHIE4FghvRnN8I9myyHwqNwSrm%2BwIdy5%2FYhUsZZzMl5%2BiRbuoBtPPHPH%2F9ML%2F1B6Q16JdCq4mz2XdiqECDjXXMVxaucIR32XS6KriDsSfHg2ZO6%2BtmyKrG4UZfHdpCLNhH53jCWb53Db4SxN%2F6whFo1BbRlAGE3Y44wewIPMTWcflE8cOCpyk4Btru0V8aujOn1nhZ%2B0E0l%2Fs2A3cXCfTmDwGjVgXE6Pv29wRDZZ1GO9lC%2F%2B6fGCVeJQvLVnawZAanc9ZR4lYgaU43d0TUYmeqbPrdXJd1uhFpuuq3Q3hg2ldFWTzCcm7rIBjqkAR%2FSpXWt974c6xS%2F2eTVy9LWmlkPwO5Z%2Fx6%2BBnKXPJjgMLffl3TW1epdjvzjlW2JSCly23aLXst3aRP1qEvq%2BHIdOCx%2BDYOAAPIP5DLWU5RrhM3RtNMgnebnu35FKy4cQbM%2BFAGS86RE3%2FzeLcO1qmJ00oFrLLDTCohKDIPToTUtnWYdVbpGj9NHJLnjgi5fqT6VIruDKPA2jWUJa1tEXtSNySv7&X-Amz-Signature=3525656345db2b70f74be0069c921f6ad454b452b1f4767270203cc4c9e7af35&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

