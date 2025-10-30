---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q2GTEQOK%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJGMEQCIBy%2FUS6kJr6EfZgN7hB4c2pjuzy0TVQN8iIcKVzWC3LcAiBnWZqcq%2BEls6W%2BNR1Pwf1CXh4H0%2BkUP8BOuj%2FSUSgKSSqIBAjp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDycghE%2BkQUrda45JKtwDicsfEF%2FAoMruYMqDOnyznTatTb4udYiDYnl0sXunD1TzxmeDU%2Fad%2BgyJRjaIZg3kLbX6zzU5cOJA4PdsbYBUOSJBnuE3GJ8AgKiAy50rVeESLDf6%2FpILn0UgWv%2FncLHFUhKU2vhf2Ul6AkfHYqL86jY1NXkkzv%2B5ywdxKm%2BwD5Soc3aP9fCj1cygXfOHM2nAv2Uwo9bo1nQs7Jw4S4lmy01x12dSAa7uJM%2FvskifLc%2FRl0Hzo36NR8EwoVC6N7czjXEYR5P7h255exXqJWP1ZZz0mJBTMORVXM9XCU%2FP0Fw1aJDXs4HjJmRGDczK4KpXBDL5YXc8sf269fVlAqODMnYp0hKmgIRzfAzWVEYzi4%2FybU8GS1CgQBqu7SsALgd9oI852bQaQzCC3ysIUheeh3oNM5UFaw55O%2Fs4%2FZOpSkpYZKqkACWPtA3VvFRKuj%2Fml083lWR1QqB8OBGMebC8JbGqQ1GZwqJS4HM3BKzKyTVvKg3FE8%2FoSvoj5ekn%2BzsctAkpmGsiPmkaG%2F5U7pb5G2swIQ5QvFaZgpUY1nPk57jw646DO1CaWgnYxtYXkmsaeOPs6m8dh4NDLdkQDXCuKhs6a6IwwQ3zjTjhC6q9Au0%2Bu8a4PQFjuzpg4Y8w%2F7SMyAY6pgE6RR%2Bx6CgR%2FruVOi8s3B7CEVf9MoQZplkAQfPZBWGBdusgzq42xQYqrIUFo%2Fh4lVIzevAeWEknnw%2F4oAajdbitA2kSSo4%2FpeGLx2jZap9MpI2irlwTaJXEP9ILK%2FdepRJWg8cxzlC%2BTERYgW%2FqNm9cwfcQNCX3lMyz1sEY%2FhFekxX8a42sXWHiZ1%2F6ZPTdE4RVwgLMeuT2AdOhBtq2fkBzXZT5j8dY&X-Amz-Signature=e374b49fa73e2f16a56e823a4a7f4d41e6d1b892393bef9a8ae71c4260091d9b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

