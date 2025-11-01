---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WZW3FTOX%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJGMEQCICcdogTzK1PD%2FUqpSEbe0gEs6UEUfyqU6opetner9j4VAiAEJqnmwSHNgF10nTyPtH9ctI5BorXIJUNe9xnKWyFckir%2FAwg1EAAaDDYzNzQyMzE4MzgwNSIMWdkuM1bU74JzxKdZKtwDcRHyLx9tfGI33jsf5m0V2hoP5B2thheQ0YrtAAEzOg2D3JmXH3g5gGPKzH0ufocT%2FjRNU8vX1OHNrpORrkNi%2FuvOxfjUQ8iMH3Tjc6EwPukciXKM%2F%2FEyXeXNmXqDl7hrv5464bPPEvC713dgUOtaTBqQDqg%2BFc%2F4Vd21d6QnwqMxmw2dZdNmWULa7oULuPsDuEW2bdgf2cuIOqt6vZMc42YoJQNEfCd1ICK3K%2F99MKlS1RGmPAWfXDXQ%2BBLaKp%2BFr2Nrq9gP%2B5eHKbTxg6vfjj4YyVRKzIzOhnGeMex6uaUln8%2F2faoCgmXtjfArbygtir8mbf%2F6ivwuIelLQzdzJw18alPZFTRMIPjLhDeRwFacIdsy%2BtJV5Jeby2oJPZ9xJuWPYdoQPU1WAIoTt5W6N8Wt2G48xTWpvXajplV629P5aNKj6U9ayCe8sgqQGaiKMTVVO5m%2BgG70gAdIyt59wPEbSM8At2%2Be8LxQsfBKnQgucXapL54bSNNNdDP9vVENrGM1pumx2VJZFfC8aMO27EktivDFSIk7Cmjkg3H7QoEC9Ii2hv5bzHWVkXSj701%2BJYTtNhj4BVHLD2KIfI4NrQqcQDH7xf2fr9gWnLNgdRpt%2BS9hSO9ieru%2Faocw88KZyAY6pgHBOqiGF%2ByojmEykAy3ceGIYvhh0jMenSCE7lz3kLsXcuzfpRtbJtN9%2FMzIW5mrL%2FE69Bk%2Fih%2FZIdZdSLdtVpZ%2Bg%2FTzGg3TzFCnUbFnFw1M0WmwCP5E%2Bu8v05HX0mRy4e6bdSRcXCbxFFzjz9YLlfHB%2FfpAa1lAl3XGDyqYduGoKNFev%2BGNR6vw6hERObS0GFxZXSgd56CE1bLPsSI7xGLAp2rsUvH9&X-Amz-Signature=4c5fef2c99be466e6de11b37b93600c7921d10349d68c88ebdaf7f1839b233cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

