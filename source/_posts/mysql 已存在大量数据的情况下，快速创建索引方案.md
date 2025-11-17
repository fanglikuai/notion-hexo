---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667X36XURC%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T050048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDDNQNRG5YKOzcc0fR%2BviyjgNeXi4btqudwQWb8nqV6iQIhANXi0X3Dyg%2Bquq2lAomC9LqRvU%2Fl2I0wFqajs9nnkvFWKogECKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyNB2zodrtkxzMGaNQq3AP3p%2FOKxSPb6ah%2BMbqlSbJKPntvTDjXa8vIRBSEWgtAHMmAYPau4pf3GIYzXWXwaKGpI2FRo8WTjjdzwLmK%2Fw66tMQa4G2VAjBC%2BdLQPTafDB3EBw30N6unS98MTpkhdtGPUcJBh3Np7v0hoecKHagJu%2BB10wVHkrmKovn2riE2SuN%2BbysJ67y4ksTdxmJ5p7Vtq7ZTY9NCg4h02SR6R9XD1fuGfBAm1jNAduzmgMuANR33Y%2BsW61Uds9NhBOr5egZml%2BJrArJTly7H%2F6EuIFqCWvNUa48ah%2BMBPQAPH9Mn0UgRIxLCE4Ry0Y%2B6NaBqe7h5ppLJsD5JxgbTqaqPPXsXrEAc6XeXT986u4v93zN%2Bg%2BaE%2FEnMN5GKujRS9S%2BcZkstujTb2GdQGcCY8aJw%2FzMs055rAy%2FbeifPfe%2B1EmEM3GaJ7FVLxmRvUzqkXvXquYxkyxVbaMqCs8cJPl%2FtiHvoVIZW7c2muySUpXFCD23vxTEhT7F%2B%2FRqN9jYt6enqccRy2arHBKmdAT2nTXqxvnXcGQgwL2viTqOGQxwwm2yJcQFzpIQVc8t%2FP3LO56wdh%2FrtjpQsqurs972cNbr6IMYEl%2B0CRgZ8iGu%2F5hw8%2FhVr3keNh29dkEblf6EXijDJwOrIBjqkAYxCrzwNDYklXGVejsF6b1SpRiWn30R4f7uOLushPZGcK2aFa%2BCHKVoo%2BRZJVqQWpLflHvKM7qu6KSx6zUFvo6HAssokpvd16jpqmGKVhLLvybSgD31BlsD1fls7%2FjVqYWmoXoBP2tTP3F6ba0xJu7SM6vzL%2Fy2uPQxXbk%2FGDDSbea%2Fi7Pk9oflXIlQs4E4CQ7vLko1OTOMLIfQ9V4w0TxR5yyEh&X-Amz-Signature=b8f17c359456b069d0692ad133aa96e46a0918de58b2ebd6fce9923b3b424388&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

