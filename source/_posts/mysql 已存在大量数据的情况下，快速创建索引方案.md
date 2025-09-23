---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMDJ27RA%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD991i8FfC03nmPADTRRyM1H55wyWVeo1G9dJ%2F2IFgUXAIgVaaN4SywtJ5bVrM%2FKxTsTDyDSgERBeooP1FKK6hip70q%2FwMISxAAGgw2Mzc0MjMxODM4MDUiDF9g3J57ndoc8z43QircA33wxkgF2lEn4PhG0wSNc%2BFd6Wsq%2FG7FR6IaWrVp0o%2BrZopLkP52FFdhahicLmCvdk3H83VVPOZY5f44ZdRL9v%2BZ%2FmiLQrbC3it58M2DGNDj4QLBsGbQCIKnfnAWM9Q57ePai6QHwis6CwOPQL2kjuiiUxvxnqcrTscb8BgP3tX2TjtgArqtYYzeGomcI2XNQrABcD%2BuItvKjRAa%2FqIqKH8oOHlv3gTmKk8iSyE2qxLv8hhUuatEDzpUdTW5BinMk2DM4qVSJBj1b5Df73371ejpp%2F3WgU7bTpzVtav69Ud4JwJbM583ePCRVT9zgYph1h3i2vC1Vi9rIBh4ippM1SDcs0YtHRUcO0EkBeuqmN86qt2kDN0h9INPaL4nQECqXDY5OgXGkAKvvXysiSGwq3zkBJhTXgH9OOYK4L0OlawK1dIokVMkxm7UxJHy8oZtV5JHQLbc0xKKDpmHxtoDEpEjXUFjfVVMy9PG3Uxb4oZ1RDcIDPwimEjsg5sOawSQHWNcj9%2F6ZBFDuYq3PWv7ejKuWBuyGi4yNE%2BvytI3NHdZyI7jU2ebXmeZzIMpjI8zgXMjL7GDTMVXs2by01%2BaBAzy2Vh1hQbsp70sr0JLr0e4UOLa0gkTvYWkdm8uMJzEy8YGOqUBpjrpOHVXF7uV0ev3HiYpn3AJOSCdazIA2FSm%2FVopZN8k%2ByYlkww%2B0JmR60%2F10cceE7jY0gX8V%2Bffby6d1WcOQSwPAz8Q8f9h7vt3v%2BX2gq8%2BlquJatFlUw21qK0zx%2BGNK433DeYPWlZ%2BwphfNBVGjUOSX%2FVaXUyimIJp2T0nw%2FDzW%2BajZItPpIpzI1bQ7iEKPc6cZoeUEdHr8xbq%2Bdv2h3Ef0nmN&X-Amz-Signature=6bd552729e353cbb2fa30c4df63e664e9133fbb9d8b1003e85fdd33602438b20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

