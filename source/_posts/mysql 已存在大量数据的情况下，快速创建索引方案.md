---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663BBGHAOV%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T120046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEKvsjjoO91M0WGo1stJeIOdh7jXR9cDB9JYGvwyxHI0AiEAgg2SRnWoZg1%2Br0%2B2rm%2BAax1FQdEhStUxCaPyk%2FhjVj8q%2FwMIXRAAGgw2Mzc0MjMxODM4MDUiDJa7%2Fz3gLSE%2BIXaQsSrcAwQBsMj%2Bn1kFozASqHcefWkPC9bYtHbTvJ7fCv4%2F6t8GKsRyW7qtF5w5GsVOXIaLx9nqrXO7UdvXIG%2FSueOPiCDJSngKu%2F98%2FzI4CM%2F2ZXFV6Hh%2F5WwAzAWKTv462MxrP797L6HWzwz6aMJB0fE%2FsmUsj4gAXuqL4UDP8Mxck4JB%2F99bEIHhiJ3jSI084n0z8ubaQgxHOqDTuT6KyITCX9rmtmBc89qlRV3Gjya02Xc5XsWVOfUWCtwcyohGQ0W7D2OkUDsGawbCO%2BopuoWwzA%2FCqzS34VNb6dhoQYsDb%2B0%2F54K%2Bk7BmBS5A9sriLFz2VQCymfEFs7bjY3CJ14f2gyirsPJWcyyTJv8XHGTDzivikvtTGVf5edAX3E%2BMFKDEAcoLLbNWIjdb7x70w0EUxS9K1eMpuDvIrgOr1bH7WFdMY1zOvXOUFzaz7ih3DjFik6k5pOJa5LtaTrt6B5tOss9WeAz40HLLeUGdFN7CUbHPZUZ4qNDtW7Q%2FkBNVTDTjg4uL9c%2BxO0y%2FdGo9jwb0H2YKsTblqJgy5zefNZ%2FseImmpKUVNj8Je0bH2XKz9EC74peVpWbMoqXwDnp%2BMiqvwEuDWM9myrIemSvhVnFByCCD%2FfkBglPE8bVHYNekMNeiosgGOqUBgxOljVC%2FwFa7jF9K%2BlavnBiXtV%2FqhmtVVJhWmU5a0l4i8OcAxmGK4%2BaDEmQ4CmX3qDQW3JhAESxB37FRFo8cHAFWQXyk4PzU7v0tRD2oQdlC4fTz5RJypzjx3PU4jGr3YhKG0qIqGXsMgPM66ztPOwgalIWXo5klC0YHuJQSotQT6zscokZoxLevWy6dYCieJvZBOIlAjPprT4sOu2LByI7JsiSM&X-Amz-Signature=19a1afe158bb9a2126af7e805c6d63f98fbd4c540f91b64792c366e1ffd83a92&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

