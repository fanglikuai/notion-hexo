---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662U2CP6DS%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEUcRib5yN62JiHvtLZc21q4gAAgAVXpafUh2bncCuMeAiB3JYXJ5hZdsLap8zWTZFRB%2FJGp%2Fbfs6HU%2BwzdDg2%2Fpbir%2FAwhxEAAaDDYzNzQyMzE4MzgwNSIMKCF5NwxauAUDNjLPKtwDm3gDQAqXvjjYBXWNJ6tpAej6P6r5KOeWt7L1rWMrmggs6T6aTXgTMAIK98oYQYIMdwbILS9w8G1sQlP3OBFsUPCzMzQiwmYDU3N2NcpN38IDQ3M7bWIscbQtfhw0VSUoxXAnMvo1wnMl%2BSgGJ%2BpkVoZ7ApL8IqbNYb0wO3YFUBb%2FkBGzqcoPihV0HjRZoMkbr9SFpFBVlpY3I6XGM%2BsnAN3eotVeJBzTD2vfb357575vTkm%2FrRLp4vFrcHxWJFLwgBl0wH1WqSpdub2VdQdjl%2BbrCWbL5iJIlBBMQMj8wtBAmklDaN3I9DnhMYPG%2BoLHS6GBY6ISF8nRbKJ2BJ99ElGP5fQ3gn8kJPyirebR3uzYKvejvVs88mm3wtSeh6YD6uFD7Oc5XUYEAAookVm6j0pNJVl8iK2HF5n0fyZKzK3mwHCFHELClLufw1wNn%2Ff4lRfYvyBR7fDacuer8Pjw1S8KcdRF9mt3HhUFz89QT9pmvRfMPpKM4n3xNc1wB3Z9zfXbGyMaRdwKlsBQvgqGJiiF8DsngDVRo61BwZAR7dK7Ll9ToswFsW067pf2C9x2DMbFW204QPNav%2Ba2JL%2BYqE16ZJgyMyhcca4kXI9J2qREgp7bQ7e5Od86M4MwnKi9xwY6pgGEh4dw36%2BHF%2B6jAXrIA%2FSvMSmJVNuJfy9xEQrhriGgdQ5Eqk%2FP%2F3Vy1eK%2FfZ9%2Fe97XXDdELI31MPJQCsT%2FYGz4S9guP4F%2FmukEdPkJwTjAKPY1pvluc%2Fq3e8Ae5Uib8BjaMXYzph8SUI3B4XdMdGjg3CUNtnMDxXtjX8dKqwqwIOfOLY%2BIzxZkvtk8TWwvlPb94H5Faiml6YD7CDmbz8MsKKoYq0ST&X-Amz-Signature=02f45099ee19c265e6fff28baf8b7a325477047c1cd49516a59fe60bd47d03d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

