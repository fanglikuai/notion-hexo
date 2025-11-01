---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 已存在大量数据的情况下，快速创建索引方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/988b7974-cdc2-4e9f-acbd-04f3a00dd49f/63190571_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46622DZSCJR%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T060043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCIFILltTsQMdHonUYygXtuG%2FcYGqhZtaZMrKI0tToTKLtAiEA0%2FtODN0AnxIpC9wq6Aiy9ij%2FQ0ew5uZc0v%2B0lsnuxZsq%2FwMIJhAAGgw2Mzc0MjMxODM4MDUiDO1PTwXU%2B%2FaHnKsWfCrcA9xpDb20sKiIs3uEVp6Mc5bO%2F6mLIrWGZ193EKsY8Kx58CyWYpbN4V6cpCGIpOZw5FhwmjudMc6Fa0YLwFivYdaVwcGKWUHf8r1VJgbw%2Fp3qxLMoEz%2FPMTcpuO22dCrLF5W8Bb6ixX9sXQnhFZA6NlODejicazpmDSQ6ufu7xYyg8a07E0efkn0NfMIrGr0Dy4wM988uHEPcwEUAWZ9QtAGRB0IIyFhX5OyhnIHOvySr3MNwwuk%2BvGnPfFHx2gpAzdtM6J3xkUfFJthxqkrRBmZeI6BMhhXvbMqPqjUrahKd9DEs7b5uoTXkucKXqFChnl6ghpYEaCGzJCqLkd6EhQy0aO1pp6Ox5KzPB3mzjAHuR%2FsDaWpJ7ZhR6Bsr7NJdNdGBNGWC7OQHCWU9Iso%2BwQ6fVqvMkaB9%2F%2FlffyCHPoTEN%2BFySMocG4t8VJSabdtiFSnU1Ly5RATvaOkZ6KnyjeKRm2yz%2BDAYh%2FcIMaHPOtIk8tyWW8VasOJZd99%2BW7T%2BGceNyuieQ07zrQVph6H8p%2BSxcx4OT4C0vEa2FCTmzbYS5OW5lu%2BIUj4wBrt9el2WWgbpOfiIwM%2BFAE1yzoSXF6QCE1MaHZHov76PYhjwtd9yS7%2FKmq82Dlw%2BLyOSMMCtlsgGOqUBuUSdeK97xXQnvDrT2kDKZQlAGpPe%2FM5lPTq4PFD%2BbEfQjfJDQHsFi%2FifKP2BNXNZVOm3UTuWx%2B5qHSUxCwetR9uTza1A59POTga6nUpawEZM1JK5KzNua8X%2FQay1T%2BsnNc5vIdGeM85y4BnYIVsTGTVHYrzuSgC%2FT%2FU%2FqsolteqqYoaVB55xUmpJLZeUsGOVcyiUargCQrpGiI7DDVGD7Xy94y1b&X-Amz-Signature=05b37cf0aea8e504ddd1d274903a62b8c249168859cb39c15a8ae1217ef10fe8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

