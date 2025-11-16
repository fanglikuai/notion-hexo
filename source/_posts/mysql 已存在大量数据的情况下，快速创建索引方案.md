---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QI6LMTX2%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGcNfWSZHH5ihrC6PHAxUX2pgu75vlI0CwKe%2BbmImcIwIgMQaJlXt5EGf4Pdqheiv53Od02zJ%2BHjk1M4tbD9VD4ecqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK797oZhWBWCDoYXECrcAzxRXQDuo2Er6QNdQxMe8cA2tqIafZ4pqI6ox958dLYYmVKOJkYy3cGmfh0uXSBwqbErOqp%2BwKilwHg2tDvhjwsO7nmqx0HlQIV5yvp5PIJF93WEk8DGjpmODu9OZwZpaJEcBZySmEEZfC7Jgl6cVvLNEDD1ShLmvkSGPgR1vGggiExFJJ95RxLiwSN4jWED36JoY1xKWZt3coziwT9Wrl1EGAu9K2aTcH9Fag%2B1S9J916LVOSU2b28brYKyh2J7A%2B61VH5wSV4VVleDkN3A99rAbEeF4LuWEbX2IUM2CO47dlbw90W0LEB4O8Uew1yxIT16Y0Sf9FBhy4B%2FJFrs0%2BR46Wl7Y78xcEOi3fURO%2FKNrnbDACLbrSXHiHc646zFtyCj%2B1fvYUGoLyqENG%2FnXm%2F3r8eCuDpRLZRyAc01%2B5OkGcwqYR50%2BbYxDvlNNqm2zYXlikqGEoDFqn7dhn00tYUfLWI7MaFcBywjcDVfeXS6iMpgxWa8yngoj6AZ6KtW6NBQaQTzV9l%2BNHuWKiIOSse8%2F9QbPTMXcw9B6OY4%2BzOrK3wG3W4ck2UHOl5tdP24zADvxTBKckQgVq3Vi40%2FNZ2xW0Vf4%2FsTBTrFb2CC7dJjzY8dsrIRpnJdpBVzMIrP5MgGOqUBEsGGIfesnEHo7kZVEICL%2BmSNNF%2BKGe8x9NxlQ7DUCWT6RygNpESO4OXDyAWjXrc%2FFMaVSZkzPOCor5bfFXcAW2EZsxQBucrGW76aXcPfbl8ziGdnyo%2Fr4Nm9JNm1%2FagdeLmbub83847hJ15ospCCB4VpHUrlubIhBMBG6ijg7EdO6r5jyO6gWneMVOilu3IkxbLLTpPNNqba78qUgyjEYgr3TKme&X-Amz-Signature=bbfcea708f142c03ab4a635d76c74f1304bb9b00f17d4f685c9bde5cc5887061&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

