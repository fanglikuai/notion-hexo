---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663FUTKEKB%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCiDmAmcmZEpd8lClem5QUO9gGQRwqMZiaWl0vT8DzPtwIgCHQfM1msf2dI3vngPHm8pAVH1JaMfI9SJxMQJyD1FD4qiAQIp%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBttHyQbQ4VnsETWjyrcA6Jy33L6y7YNhz%2FquOvuxMSakykwX%2FbGB1pwwq6w8sWgh2uvJoJ2jMcFFx00BUjFyr8dh%2BsLBj8695ZE2EXz1l8mQftO6sbnmdyrfeN0jFcyHOqzWwb9BLxXxVvuX1Y6DbFwyruHuSh465%2BStHd452ooDavCPrmub6SrWhRZQ8Vb6%2Fz66vJMIWyMkiKntp1wSCg4MY4XvWxB0Fb01CJBbQEJNHxUkLHWDyMEnFspPTpbx8keY10CeP6tp8A09dZHYH4TdUoJCRK5tTkgE9jTVVnh%2Bughb4MXupQNbIoKkuSVrbXjA45kTgpX2bbeglZ1WLYxD6WnZ7KikUHujmjXDDN3rtE0aNyet%2FaDCST9xsRUGwN0yHyxphJz2SGCIaRz1jXM7UTjIZ%2BQEkY1A%2F6dzPfNC10SsJgWJfKGRBgzFxP%2Fq%2Fr3dXXLt9pGaXCg6izso0XZqsSRt8Z4xXHZdtpx%2BWw7%2Ff6YttYyocG4eGS8iygHDDrenxfx7VYub83bt4T5PHLmDg2s6TqqwzUWMPM4Bg7E82uUFbwQtOCDcAbh%2FJCNnx%2BduYxFYBvCFbjqPwFoLLO2r260Ijl7UWzVNDpvhdqxxdC5O6vgbZTRSnd8wNEYkx53HLcOTz04COQDMKKDo8kGOqUBTofNeuCZW1cdWvgZS0UL7VAnPszp6dg%2BBuMa1E1IP3lDQ3%2F8WL7jLyjKdkTM1yWWeIe2nld3CtDXJtWitffuy3UW7DAdkrw0yOWKzJ4oSTj4Ot0Rhe121QllnfV1AvQ8sSpTf1TlVlmh5iCvQT%2FdvOF94zr%2By89RnqkTeqqx7yoOQE5y5nT3F5Wj%2F74N0402hPbumO2Z2spIw3RoEPskCa0RNNN1&X-Amz-Signature=7238446df83d0dffff360b630fc35a8fc7abee888ee32ee4484674e7126e020a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

