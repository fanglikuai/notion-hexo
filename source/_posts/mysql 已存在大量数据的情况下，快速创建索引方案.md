---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SFQJ7J2Y%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJHMEUCIQCOCkuWwwAYwc2%2BtoTIPOZI6Ju6aU3lEj4P93KlKkP78QIgAVQk1ZcIhiNv5SYXmBDEu8yQDhUEGafKRySBIMxIjMEqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFlB2AoIMOx9L87tuircA0sfQJGQvccxWfBKR0jU8w0bFRMFQwfeRam8TdGsNqxKmIzkAMF1UpyTKuVIEiMoG8WZf3jA%2BB3gXwHvQQ8uLkrefskwjSFvylnTjtqimPyuK0i4k6SsQtGBf26beglub6A%2ByHNDXiHRWjM%2Bdi1srx%2FQkuSPrbHthFkhWJIhVW9xYpf3%2B4fznbQIO2zpZkiQnROUVegKyeZ%2FwqoRqT5LnTWXeRWbj4RN0k0dZSRkHzZbtwCzcSK93MukqxULqUZIN3TvJFQeaUqBHaA3NFTTJgMVazpUjMkhx%2B%2Fxkyu38vobCbQrRpek0vP%2FEvgO3TubwXSMJKVZqqjC9ApJ2uovABdt05yD42rak%2BRAfUHEiVW%2B6prnh3sxfTwaJskuaEwxxNNFKBvfzdx8cvYRtRDSYxdwM1W58goe8DtHinJmpHo3RYeiiNbi3VTsQpUn546k6fR8VAHpzRD8jZGxSYHOMd%2FKsM5RJ00tkarMFL8dV%2FME%2F3IGtScssSaK%2FNiIXuTzAeZhgBXjT5N2UdcS21sh3jizNetV%2FYIb2ps6iqvbgXxPkf1hl6ysqESxgEAhoRaJM8PW64qhYY38qb9pfQDvGWiAa6ao2WFn%2FRBeKuLR0KajibZ6VUgh42jyGBHGMOSk7sYGOqUBrGLfC2o4qoJgbW0t7pf1VtOvLnYthuowklA25IHQMWVQ%2FPAVC45zmdMw5l9vJmCGKCmrrhc18Q2qxrL0O5pPmESUlcT8KLD%2FwmzkgKT7WJ%2BZUXUNllyCqkd%2FVHZOUQhKb0tKV279JCrIZmaOvrRMXWApQsFo1fXhLJWc8FqPgW2BWpJYs2WG%2FvpIkle2uwcmMiSDsO590%2F7D%2BHVPTg3xeG6UKo5j&X-Amz-Signature=4eaa627691e1c37a2d1cacfd6a22d2fbdf6b310b57512513b278d7be70fb780a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

