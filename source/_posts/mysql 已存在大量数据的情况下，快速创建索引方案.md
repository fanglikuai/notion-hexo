---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GKNT52U%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJGMEQCIAnQ1qmNjCIPLO%2BRKbR87YtsOqf5DA8erEHMF6e7I%2BUGAiAdyR3vwQOcaqcqVJJ2YAoP6zXEJ%2FMGEyglNh9GI1%2BgKSqIBAiW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMBvo2EYAaRGhU1XeuKtwDfphjbuYloK2pkZrySGxcEgboxEv%2FTdkf7fOO444MEURVyPurShIiMQoG6YBWOm%2B8LyoK9cGZ%2BGloQ2zflB1inNreLP3y4mJLv41LDb3tH5dT%2Bh0evUsAsqwjA9hOO1PztRlSs8tkI29at4WxX633aIKbGMa88XYTr2EtlAdPzswe5KDdGXWnEV9THlurCQv37qLMuF1BkRkfKZxjBe9nPoJseRfkF7KAiW54fjCbr5duW4s5lDvAf%2BXoCPozuqBjfsDFPJJdo%2FGJURUDkoSIPRd11qK%2Fwv3ymtvpNhjyFNuQLGVPZDHOdgbuXHPZdUKv%2BgqwBQhvbPZQchbh0GPDWeOOOkozio43L4FW%2BryAhpZiSTWqR3WlDOTJ6VF1%2BFuMXi8zxG45kNXnJzzgQu9DpL4VTBvO8YppfxUlCs%2BNVbqVj270m1Y87yCwgmQvBCRWEkbw8%2FBSMW9BbC70x1rgbq790pDHH0gpC8AYYRT02dqge5F6nlP0dQKGOj3Lc9v4oOo8EPN2vq244VlgbJI74RLSGDPKKeaYG44NFFhmr7oCRyTiKrbD7i5gAIhnGrRSaxkWelRH0LImXkCdLvPBl1BVNF279W0AfiuH0TsGz%2F8iTfWsb%2FLeEXPRKFEwmPfbxgY6pgEEiFDaRcKUv8UOfs33TzBsO7cLT2X%2FQtkkoCeFhLI2JTfkvWwINMzqyfHXTbIjqH4Uco%2FPIUFH%2FmHnH2J6kDOWSGnv3G5VwEoit%2BcJIv3hZ7CFFsD5dtOkqHYYHbwPgwe9iduo7zQpijrM4N%2FQMyz5Bzmebvd5H4eGVaHuaAEBZZZDhi2HQZZB7Yq1E7ncdttUJWn6xXyeVPfZqzjSryEae3OeRSqc&X-Amz-Signature=c9dc6e615de6b4f9e097f0229386e3a606ac3ec6224568dee7b5e33f5e9e3e94&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

