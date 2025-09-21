---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTZEANCU%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T210037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDKHel0Np7vx91I7eZYZiKqDaj%2BqMX3HKZ4hT9aBEL9%2FAIgAfLpGoxveRl8o%2FWoufqiGe%2FSwh3iiqhbwerlJRqme18q%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDAPbfGWy7pKlldau7SrcA9Z6bPDXnin1PzcgqK2KDR%2FecT1mmfp9GxhGf14skV2twgWzjsrh8JJXaNThHd1WFdG15frGOVJOYdRzk%2FHRKa2DPiDUlK6zUjsjYBTtFVxGJBtNOkdx9TdSj%2BGc91ZG7db425QfYASdLMRqk3TWCHfnv8GUPm6BdsO76vt1EOhzEhzXLZ8edZTuNJheM0lqfnQqXnzUwmQlBRqP44fUZfF374B%2B1u4rKafZTH5LZXezYIX6IhFavrXVKyYkRqs7S%2ByBVSAzu7f2sNrZ%2BVaVBAvGLSF1B1WUJYHadz6G%2Bv2i0FPRplcUm6aT4JCR0GG9hr0Xf1IVvAqwlA7luQHiOo%2F2tSlaAcz69B2hnoa7Wmr9O2gP%2FwksNsG0rMhktAoiIk2c6At3HNP0wlEnfODqqInm%2BiViMGvTVsiKWixVGMC82TBZ19fSj3JUIypcpW2ubs2Mi9uHVAgVock1QVJd6xYBeidbmmgq8CYlOqdPbW7ca%2FuqbSVr20tg3nN6XcopdDe%2Fnt1bqpHZm25akXcHDAokpr1fKn%2BSyGjkGxGcdlGvCs97VxYpNUfquz8CaiqOswRSNAPJv6VXOOGNu3%2Fc1ZbREIOOj4eP3MUCKG%2FPG5x9v6GYp6kSEuQN3MWUMMDpwMYGOqUBqgEWp%2BGcepU%2BfsOW0WMFkIZBwJpCvKTVKWvS%2BztoYGP%2BddjZCrVC3t1lISpSx4XfkMdROId%2F%2Bma8aL7lqgNc70chnmntAs2wOMSSM0YYGWP2C29lZ2mMpNRWaoQPWnaHUQ3IQd2l7eggQ98q4FxV4TtGCEp8MfMT18u7HXRsvoWiVK8rYxqKPk4e6FluyjwrprAOiVAJgobhk6WS6SkySYhB1AWg&X-Amz-Signature=c8a7326270bc4908f64a8b993843015c73b48727bd8238a851cba0ba16c02058&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

