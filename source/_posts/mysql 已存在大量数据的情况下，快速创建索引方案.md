---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3NHE2HW%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T140052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID6jMcCFSvgbqAgP7eXfPa48zaOlO8BmFXhEoxj7nYKQAiAkRthYEwXW9XtiiP8TgM7qZrEqPRpvWpLByNf8G8RE8Sr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMqbeyB4Hj1T61GGaOKtwDxC5GbXvnNBh%2BcFF1qFmPxdM7zRd7FuGE8Z64Wxi%2BXh2zPZgjm9XMPY87TKlOF1OjUpzV4GaiPKrfC252I4CQb9mD1IAGr956UypyECXm5vNv3RxWEHMcUc2CdN79qHAglCwe5xCAOYESGdXsGeoOhXmzw05rA%2FutlD1rymHn4dpSMziRbtbqJCXNVirJsVcj5HQvy%2FbphdU4DTV2TBQQ7Dfq3oTwLeVXgJZvpb5iI6sU6Up13KoK0NNuNyoiC964d9p3oOVLKyselVGRBA6BVNnh70e7zNg86pJ%2BiegzwQyYZO0m4fYVTX%2F89XpjWwOYS3j4q%2FkT0uNH7rHcrPJMETVXiSehXGNrMs7Ts82zlsrFX%2Fs2bsmRUoFkwupBRyOY8qeFctDIHL9RzV4tkqOqPO5WuQV03Z%2FajktULZI%2BfgWfPUuAoLmy96D76qbb2cnL5Rer5XnoiqXhyp4HYOLHzceHVGkeT78pGAxXoSq%2FviIGIfCaLkCwdyXjXXEwBRVKxOB%2F%2BTDUghq9nip9xo9t9T%2F1ItGPzMh4UqbHrGyaRh2lXjBeOQGQlVCJnUmh8aduqjRKFTLpxSt0nnnAkrZPPNQQfahmzBgBuazmDZru60%2BNCkcxNCk086BdEhYwzpyJxwY6pgGW%2BYhgdeNBpChZj0bC7s7w%2FEfbrqwsjXHlIXBTP3Aeb2iT1xFFGa31nxORWvPSJ%2B25durEGaZZkuWSbKiWe5CMDc%2FZUiqP9ic%2FlbF6uhZGthgObvKc23VoLefVnjLLQc0AuSfIOQTtpORDpWAQ8nexn3A4DWw2JuKye5p9aSFbhkjtoFOzGKKsRMtR3kEnF3Qu85w2SwKRITGxoyL6CNibK5I1aGkv&X-Amz-Signature=dd6d5bcbec6b51b752b77428c45fbfa087f4851b330aa6c6c3d1fea18d6611aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

