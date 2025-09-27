---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QXOSBEBI%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECYaCXVzLXdlc3QtMiJIMEYCIQDFFPuurj0hy66zeIlOyysBTraVLvcvJxjNDsPSQEwe9QIhAJLuimNqyeLzZU5vTFoGfRvWIa4%2FXb68pH8EZk3CR3whKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzlJc7NNBHmCgMekuEq3AM%2FXpe5J%2BpmcuQxpbJ50aP1yYqa5QvPuQskzByiAVjGRMV6baHdsqsgl0VV0Kc92vRczPhc8Hepcq1%2F3RQSN2NODJlf3cWqsZKQwdQ62V9QV%2Foxy1gCkwEFy%2FPMNj%2B0AZzz4Tnr%2BVp4ngcifLvb1mhufJ4PoKV8yHqLRpIDAS6FOsNfHrKz1gB0oZn3gK%2F%2FguzbHffzmRsbV8MfZ%2FqQyA7HLmI4S8w1cd1pPV%2FHhDSmNks0rSRm6NGj4XBv%2FvEbeVgCAhsPsj%2FM5d7XWU90f12jYboAo3Kdm1BfTW4CFP6uP6YT4SpWABsaMJ7xxmmydHReK0TE8gEngwMeuM%2BObNM5L59FbEPIthpNJ7KH7DDvS1fPDMhwjiiD%2Bu8QA73qSxNiJfrNxxw9fAXRJCucZsI%2B2hJIyYq5unxQSH%2FKECLi5RhcJD7FM18VBbrXXpKllvtzXskeeKax9YyhohUTKzMBkEiBGvLJYq5mh15tfuOdRcLzgQB9s9mSF6QJ7sAe6%2BDG%2BPQWPG%2FxNMNurvZQHBVR%2FM%2FhOAYwC0XfJx336Hpp9Y7jKtFJpv%2FSyfxAm%2BxCM%2BGaA56th02bepCnAsibnszKRCHbavrl1cknjWvv%2FfOEERkmka5ZqJ7guwvZdDCzquHGBjqkAU4lLxJ0RSg7N%2BS8UNrj1sKXIn4KcUYza9W5uCq59TeVHV%2FnjqfTc4%2BKdugorc8igb2zrlXXEU2dS9v%2Bjvn9zRZxmAuVuYh0mVcKZv7KDelmXeKP9TlErgohK7ejUacESx2fHVcAuKx1a%2BYX8%2FYu%2F1E7gjxbPCEx6ZLp4vgop2u78yottodA0Qw5Xik0GhY7NJXleHFlQShndQ2K4itMUpYK9I7T&X-Amz-Signature=efb149333e660572f2524a3e37134cc8669a7d2f91fd53ef68e29bcf2d832455&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

